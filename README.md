# Dates and Times and Timestamps, Oh My!

To conserve my sanity, I've constructed this table that tries to capture what I believe
to be true about the representations of time in some systems of interest.

| Semantics | SQL standard | Oracle DB | MS SQL Server | Google BigQuery | `java.util` | `java.sql` | `java.time` |
| --------- | ------------ | --------- | ------------- | --------------- | ----------- | ---------- | ----------- |
| a point in time | `TIMESTAMP WITH TIME ZONE` | `TIMESTAMP WITH TIME ZONE`/`TIMESTAMP WITH LOCAL TIME ZONE` (see note) | `DATETIMEOFFSET` | `TIMESTAMP` | `Date` | ? | `Instant` / `OffsetDateTime` / `ZonedDateTime` |
| a date (year month day) | `DATE` | none (see note) | `DATE` | `DATE` | ? | `Date` | `LocalDate` |
| a time (hours minutes seconds) | `TIME` | none (see note) | `TIME` | `TIME` | ? | `Time` | `LocalTime` |
| a time *with* fractional seconds | `TIME` (if supported) | none (see note) | `TIME` | `TIME` | ? | ? | `LocalTime` |
| a date and time (civil, no time zone) | `TIMESTAMP` \[`WITHOUT TIME ZONE`] | `DATE` | `DATETIME` | `DATETIME` | ? | `Timestamp` | `LocalDateTime` |
| a date and time *with* fractional seconds (civil, no time zone) | `TIMESTAMP` \[`WITHOUT TIME ZONE`] | `TIMESTAMP` | `DATETIME` | `DATETIME` | ? | `Timestamp` | `LocalDateTime` |

"Civil time" (also called "wall-clock time") refers to the local time without specifying any particular time zone.

Standard SQL has a `TIME WITH TIME ZONE` type for a time with offset but no date.  It doesn't seem particularly useful.
Java offers `java.time.OffsetTime`.

## Notes for Oracle
- The convention for storing a year-month-day date in an Oracle `DATE` is to set the time part to midnight.
- Oracle date conversions that don't specify a date use the first day of the current month. (!)
- Some sources recommend using an interval type to store a plain time.
- Oracle's `TIMESTAMP WITH LOCAL TIME ZONE` is a variant of `TIMESTAMP WITH TIME ZONE` that "normalizes"
  stored data to the database time zone and uses the session time zone on retrieval.  Presumably this only
  affects display and not calculation.  The database time zone (`DBTIMEZONE`) is usually UTC.

## Notes for Java
- `java.sql.Timestamp` is a local date-time, but conversion to `java.time.Instant` treats it as UTC, as does `getTime`.
- A reference time zone (typically UTC) allows treating absolute times as civil times and vice versa, 
  but the convention is not enforced by the type system and is likely to lead to confusion.
- My opinion: `java.util` classes should only be used for interacting with legacy APIs,
  and avoided in any other context.

## References
- [BigQuery](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-types)
- [MS SQL Server](https://docs.microsoft.com/en-us/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?view=sql-server-2017#DateandTimeDataTypes)
- [Oracle DB 12.2](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/sqlrf/Data-Types.html#GUID-7B72E154-677A-4342-A1EA-C74C1EA928E6)
- [Postgres](https://www.postgresql.org/docs/current/datatype-datetime.html)
- [`java.time`](https://www.oracle.com/technetwork/articles/java/jf14-date-time-2125367.html)
