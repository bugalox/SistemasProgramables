# Oracle APEX: Binders Planning

Use `f172_page_35_binders_pivot_fix_v10.sql` for application 172, page 35, in the original workspace (APEX 26.1). Version 10 supersedes version 9.

Export a backup of page 35 before running the script in SQL Workshop > SQL Scripts: the installer replaces that page. It retains the original workspace, owner, application and template identifiers.

## Change in v10

The generated row DML was passed through legacy `p_attribute_04`, where installation reported ORA-06502. Version 10 uses named native process attributes in `p_attributes` (JSON CLOB), including `target_type=PLSQL_CODE` and `plsql_code`. The generated source and DML are also built as CLOBs, avoiding the previous 32767-byte PL/SQL variable limit. The save logic, row locking and concurrency comparison are unchanged from v9.

The named attribute format is used by Oracle's APEX 26.1 sample exports:
https://github.com/oracle/apex/blob/26.1/sample-apps/sample-interactive-grids/sql/sample-interactive-grids.sql

## Validation

Static checks passed. Installation and runtime behavior have not been tested against Oracle here. The installer parses the generated region SQL and row PL/SQL before replacing the page.

After installation:

1. Reload page 35, edit one quantity, save, then reload and confirm persistence.
2. Edit two different dates in one save and confirm each quantity stays on its intended date.
3. Save two successive edits to the same row to verify the returned row version.
4. Load the same date in two sessions; save in the first, then attempt to save the stale row in the second. The second save should be rejected with ORA-20011.

Uploading this file to GitHub does not execute it in Oracle APEX.
