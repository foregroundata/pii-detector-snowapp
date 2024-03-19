# PII Detector Snowflake Native App

## Supported PII Types
A full list of types can be found in the README in Snowsight after installing the app.

## Functions Reference

```
pii_detector.fn.detect_pii_in(column: varchar, types:[varchar]) -> [varchar]
```
This is a vectorized function optimized for detecting PII in columns of tables. Pass in a varchar column as the first argument, and an array of PII types as the second argument. Even if you only need to detect one type of PII, it still needs to be an array. The function returns an array of detections, with an empty array in the case of no detections.
```
pii_detector.fn.detect_pii_in_nonvec(scalar: varchar, types:[varchar]) -> [varchar]
```
This is a non-vectorized function, useful for scalars such as varchar literals or variables in stored procedures. Otherwise the usage is the same as the first function.

## Example Call
```sql
select text, pii_detector.fn.detect_pii_in(text, ['PHONE_NUMBER', 'PERSON']) as detection
from (select 'I like ice cream' as text
union select 'My phone number is 404-000-4444' as text
union select null as text
union select 'John Smith is the manager' as text);
```
Output:
| TEXT | DETECTION |
| --- | --- |
| I like ice cream | [] |
| My phone number is 404-000-4444 | ["404-000-4444"] |
| null | [] |
| John Smith is the manager | ["John Smith"] |
