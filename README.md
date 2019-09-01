# Dates and Times and Timestamps, Oh My!

To preserve my sanity, I've constructed this table that tries to capture what I believe
to be true about the representations of time in some systems of interest.

| Semantics | SQL standard | Oracle DB | MS SQL Server | Google BigQuery | `java.util` | `java.sql` | `java.time` |
| --------- | ------------ | --------- | ------------- | --------------- | ----------- | ---------- | ----------- |
| a point in time | `TIMESTAMP WITH TIME ZONE` | `TIMESTAMP WITH TIME ZONE`/`TIMESTAMP WITH LOCAL TIME ZONE` (see note) | `DATETIMEOFFSET` | `TIMESTAMP` | `Date` | `Timestamp` (see note) | `Instant` |
| a date (year month day) | `DATE` | none (see note) | `DATE` | `DATE` | ? | `Date` | `LocalDate` |
| a time (hours minutes seconds) | `TIME` | none (see note) | `TIME` | `TIME` | ? | `Time` | `LocalTime` |
| a time *with* fractional seconds | `TIME` (if supported) | none (see note) | `TIME` | `TIME` | ? | ? | `LocalTime` |
| a date and time (civil, no time zone) | `TIMESTAMP` \[`WITHOUT TIME ZONE`] | `DATE` | `DATETIME` | `DATETIME` | ? | `Timestamp` | `LocalDateTime` |
| a date and time *with* fractional seconds (civil, no time zone) | `TIMESTAMP` \[`WITHOUT TIME ZONE`] | `TIMESTAMP` | `DATETIME` | `DATETIME` | ? | `Timestamp` (see note) | none? |

## Notes for Oracle
- The convention for storing a year-month-day date in an Oracle `DATE` is to set the time part to midnight.
- Oracle date conversions that don't specify a date use the first day of the current month. (!)
- Some sources recommend using an interval type to store a plain time.
- Oracle's `TIMESTAMP WITH LOCAL TIME ZONE` is a variant of `TIMESTAMP WITH TIME ZONE` that "normalizes"
  stored data to the database time zone and uses the session time zone on retrieval.  Presumably this only
  affects display and not calculation.  The database time zone (`DBTIMEZONE`) is usually UTC.

## Notes for Java
- `java.sql.Timestamp` is dual-purpose: it is based to UTC but can also be viewed as a local date-time

