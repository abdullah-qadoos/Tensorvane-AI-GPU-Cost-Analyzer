# NV-A archive evaluation

The archive was exercised through the Flask upload and analysis endpoints in local mode.
Provider inference accuracy is unverified. Profile-only model routing and consent are covered by mocked regression tests.

| File | Upload | Analysis | Failed checks | Requires model |
| --- | --- | --- | --- | --- |
| gpu_usage_clean.csv | 200 | 200 | 0 | 0 |
| mill_production_clean.csv | 200 | 200 | 0 | 0 |
| 01_renamed_headers.csv | 200 | 200 | 0 | 0 |
| 02_missing_values.csv | 200 | 200 | 0 | 0 |
| 03_duplicates.csv | 200 | 200 | 0 | 0 |
| 04_missing_days.csv | 200 | 200 | 0 | 0 |
| 05_impossible_values.csv | 200 | 200 | 0 | 0 |
| 06_wrong_types_and_formats.csv | 200 | 200 | 0 | 0 |
| 07_outlier_spikes.csv | 200 | 200 | 0 | 0 |
| 08_meaningless_headers.csv | 200 | 200 | 0 | 5 |
| 09_spanish_headers.csv | 200 | 200 | 0 | 0 |
| 10_extra_columns.csv | 200 | 200 | 0 | 0 |
| 11_minimal_columns.csv | 200 | 200 | 0 | 0 |
| 12_kitchen_sink.csv | 200 | 200 | 0 | 0 |
| 13_header_only.csv | 400 | ? | 0 | 0 |
| 14_not_a_csv.csv | 400 | ? | 0 | 0 |
| 15_semicolon_eu_decimals.csv | 200 | 200 | 0 | 0 |

Accepted uploads: 15 of 17.
Completed analyses: 15.
Checks passed: 142. Failed checks: 0.
Semantic assertions requiring a provider: 5.

## Verified behavior

- Normalized header matching handles the renamed and Spanish columns. Invalid weak candidates cannot block exact matches.
- Currency, semicolon exports, comma decimals, and day-first dates with column-wide evidence are parsed before type validation.
- The clean GPU totals, monthly costs, and October region ranking match the supplied answers.
- All fifteen impossible rows and the eleven unparseable or missing-date rows are excluded.
- Missing values have both per-column and row-level counts. Exact duplicates are removed.
- Missing days are flagged. Incomplete weekly buckets carry completeness flags and are excluded from percentage comparisons.
- Unusual values remain in totals and carry counts and data-row indices.
- Additional numeric metrics and dimensions are retained for execution and chat.
- The mill pipeline supports product outputs and counts repeated input measurements once per entity and date. Dedicated regression tests check these totals.

## Anonymous headers

Col_2 is recognized as a date. Remaining fields retain their original names and inferred value types.
Local analysis can compute the numeric columns, but entity, region, usage, utilization, and cost semantics remain unverified.
The five assertions below are pending live model mapping. They have not been counted as passed.

## Unresolved assertions

### 08_meaningless_headers.csv

- map Col_3 to entity. Expected 'Col_3'. Actual None.
- map Col_4 to region. Expected 'Col_4'. Actual None.
- map Col_9 to usage. Expected 'Col_9'. Actual None.
- map Col_10 to utilization. Expected 'Col_10'. Actual None.
- map Col_15 to cost. Expected 'Col_15'. Actual None.
