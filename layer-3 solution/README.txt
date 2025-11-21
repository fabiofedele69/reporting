# GFIU 3-Layer Package – Fully Wired Split

This package implements a full 3-layer architecture for LC_SCMT.VW_FCSCMT_GFIU_NEW:

- Layer 1: All CTEs are converted into standalone base views.
- Layer 2: VW_FCSCMT_GFIU_NEW rewritten to select from the Layer 1 integration view (V_FCSCMT_GFIU).
- Layer 3: VW_FCSCMT_GFIU_NEW_M materialized view on top of the Layer 2 business view.
- Refresh procedure: PRC_FCSCMT_GFIU_NEW_MV to drive nightly COMPLETE refresh.

## Layer 1 – Base Views

Each original CTE from the monolithic view has been turned into a standalone view:

  - V_FCSCMT_RELEVANT_CASES
  - V_FCSCMT_CASES_LOGS
  - V_FCSCMT_EMPLOYEE_DATA
  - V_FCSCMT_REL_INVOLVED_ELEMNTS_PARTIES
  - V_FCSCMT_RISK_FLAGS
  - V_FCSCMT_RELEVANT_OUTCOMES
  - V_FCSCMT_CREATED_BY
  - V_FCSCMT_REOPENED
  - V_FCSCMT_COMPLETED
  - V_FCSCMT_BUSINESS_COVERAGE
  - V_FCSCMT_SELECTED_VALUE_OUTCOME
  - V_FCSCMT_OUTCOME_ELEMENTS
  - V_FCSCMT_SAR_OUTCOME_SUB_TYPES
  - V_FCSCMT_MAX_RO_OEX
  - V_FCSCMT_STRATEGY
  - V_FCSCMT_CASE_LATEST_FILED_OUTCOME
  - V_FCSCMT_XML_ATTACHMENT
  - V_FCSCMT_GLOBAL_CASE_UNMASK
  - V_FCSCMT_BU_TO_EXCLUDE
  - V_FCSCMT_GFIU

Important details:

- Each Layer1 view definition is created by taking the corresponding CTE body
  and replacing references to other CTEs with `LC_SCMT.V_FCSCMT_<CTE_NAME>`.
- This preserves the same dependency graph as in the original WITH chain, but
  makes each component independently testable and reusable.
- V_FCSCMT_GFIU is the integration view that used to be the final CTE (GFIU).

## Layer 2 – Business / Integration View

- View: `LC_SCMT.VW_FCSCMT_GFIU_NEW`
- Definition:
  - The column list and final SELECT projection are kept exactly as in your
    original view.
  - The `WITH ... GFIU AS (...)` block is removed.
  - The final FROM clause is changed from `FROM GFIU` to
    `FROM LC_SCMT.V_FCSCMT_GFIU`.

This means:

  - Logic: identical to your original view.
  - Source: now cleanly layered on top of the Layer1 integration view
    V_FCSCMT_GFIU, which itself is composed of other V_FCSCMT_* views.

## Layer 3 – Reporting Materialized View

- Materialized View: `LC_SCMT.VW_FCSCMT_GFIU_NEW_M`

Characteristics:

  - Uses the same explicit projection as the final SELECT in VW_FCSCMT_GFIU_NEW
    (no SELECT *), to avoid fragility when changing the view.
  - Is defined as:

      CREATE MATERIALIZED VIEW LC_SCMT.VW_FCSCMT_GFIU_NEW_M
      BUILD IMMEDIATE
      REFRESH COMPLETE ON DEMAND
      AS
        SELECT ...same column list...
        FROM LC_SCMT.VW_FCSCMT_GFIU_NEW;

  - Intended to be refreshed once per day (nightly) via the procedure
    PRC_FCSCMT_GFIU_NEW_MV.

## Refresh Procedure

- Procedure: `LC_SCMT.PRC_FCSCMT_GFIU_NEW_MV`

  - Performs a COMPLETE refresh:

        DBMS_MVIEW.REFRESH(
          list   => 'LC_SCMT.VW_FCSCMT_GFIU_NEW_M',
          method => 'C'
        );

  - Contains commented-out session-level parallelism options you can enable
    after discussion with your DBA.

## Deployment Procedure (PRE-PROD First)

1. **Review & Adjust:**
   - If needed, edit the MV script (Layer3/VW_FCSCMT_GFIU_NEW_M.sql) to add
     TABLESPACE / STORAGE clauses.
   - Prepare separate GRANT scripts (not included in this package) for the
     final view, MV, and procedure, according to your security model.

2. **Deploy Layer 1 – Base Views:**
   - Run all scripts under `Layer1/` in a controlled environment:

       - Layer1/V_FCSCMT_RELEVANT_CASES.sql
       - Layer1/V_FCSCMT_CASES_LOGS.sql
       - ...
       - Layer1/V_FCSCMT_GFIU.sql

   - Verify they are VALID in DBA_OBJECTS and return expected row counts.

3. **Deploy Layer 2 – Business View:**
   - Run `Layer2/VW_FCSCMT_GFIU_NEW.sql`.
   - This will replace the existing view definition with a logically equivalent
     one that now depends on V_FCSCMT_GFIU instead of the internal CTE chain.

4. **Deploy Layer 3 – Materialized View:**
   - If an MV with the same name already exists, drop it first:

       DROP MATERIALIZED VIEW LC_SCMT.VW_FCSCMT_GFIU_NEW_M;

   - Run `Layer3/VW_FCSCMT_GFIU_NEW_M.sql` to create the new MV definition.
   - Optionally add indexes on key reporting columns, e.g.:

       CREATE INDEX LC_SCMT.I_GFIU_MV_CASE_ID
         ON LC_SCMT.VW_FCSCMT_GFIU_NEW_M (CASE_ID);

       CREATE INDEX LC_SCMT.I_GFIU_MV_DATE_CREATED
         ON LC_SCMT.VW_FCSCMT_GFIU_NEW_M (DATE_CASE_CREATED);

5. **Deploy Refresh Procedure:**
   - Run `Refresh/PRC_FCSCMT_GFIU_NEW_MV.sql`.
   - Integrate this procedure into your existing nightly batch / scheduler.

6. **Validation:**
   - Ensure all objects are VALID:

       SELECT object_name, object_type, status
       FROM   dba_objects
       WHERE  owner = 'LC_SCMT'
       AND    object_name IN (
                'VW_FCSCMT_GFIU_NEW',
                'VW_FCSCMT_GFIU_NEW_M',
                'V_FCSCMT_GFIU'
             );

   - Compare key metrics between:

       - LC_SCMT.VW_FCSCMT_GFIU_NEW
       - LC_SCMT.VW_FCSCMT_GFIU_NEW_M

     e.g. row count and a few aggregated measures.

7. **Performance / Parallelism Tuning (Next Phase):**
   - Once functionally validated, you can:
     - Enable parallelism inside PRC_FCSCMT_GFIU_NEW_MV.
     - Add more indexes on the MV based on real report filters.
     - Optionally, refine the Layer1 split further if you identify particularly
       heavy views that might benefit from separate materialization.

This package is intended as a solid, production-oriented starting point for your
3-layer design: clear separation of concerns, safer MV definition (no SELECT *),
and readiness for controlled performance tuning in PRE-PROD and PROD.
