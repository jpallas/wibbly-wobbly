# Dates and Times and Timestamps, Oh My!

To preserve my sanity, I've constructed this table that tries to capture what I believe
to be true about the representations of time in some systems of interest.

| Semantics | SQL standard | Oracle DB | MS SQL Server | Google BigQuery | `java.util` | `java.sql` | `java.time` |
| --------- | ------------ | --------- | ------------- | --------------- | ----------- | ---------- | ----------- |
| a point in time | TIMESTAMP WITH TIME ZONE | TIMESTAMP WITH TIME ZONE<br>TIMESTAMP WITH LOCAL TIME ZONE | DATETIMEOFFSET | TIMESTAMP | ? | ? | `Instant` |
| a date (year month day) | DATE | none (see note) | DATE | DATE | ? | `Date` | `LocalDate` |
| a time (hours minutes seconds) | TIME | none (see note) | TIME | TIME | ? | `Time` | `LocalTime` |
| a time *with* fractional seconds | TIME (if supported) | none (see note) | TIME? | TIME? | ? | ? | `LocalTime` |
| a date and time (civil, no time zone) | TIMESTAMP \[WITHOUT TIME ZONE\] | DATE | DATETIME | DATETIME | ? | 
| a date and time *with* fractional seconds<br>(civil, no time zone) | TIMESTAMP \[WITHOUT TIME ZONE\] | TIMESTAMP \[WITHOUT TIME ZONE\] | DATETIME | DATETIME | 
