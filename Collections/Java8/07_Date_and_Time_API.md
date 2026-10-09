# Java 8 Date and Time API (`java.time`)

## 1. Overview & Core Features
Introduced in Java 8 (`java.time` package based on Joda-Time), the Date and Time API replaced the legacy, error-prone `java.util.Date` and `java.util.Calendar`.

### Why Was the Legacy Date/Calendar API Replaced?
* **Mutable**: `java.util.Date` is mutable, making it thread-unsafe.
* **Poor API Design**: Years started at 1900, months were 0-indexed (0 = January), leading to frequent off-by-one bugs.
* **No Type Safety**: `Date` represented both date and time (and timezone implicitly), making intent ambiguous.
* **Thread Safety**: `SimpleDateFormat` was not thread-safe and required `ThreadLocal` or external synchronization.

### Key Advantages of `java.time`
* **Immutable & Thread-Safe**: All core classes (`LocalDate`, `LocalTime`, `LocalDateTime`, etc.) are immutable.
* **Clear Separation of Concerns**: Distinct classes for dates, times, timestamps, timezones, and durations.
* **Fluent API**: Chainable methods for manipulation (e.g., `.plusDays(2).minusMonths(1)`).

---

## 2. Core Classes Summary

| Class | Represents | Example Output |
| :--- | :--- | :--- |
| `LocalDate` | Date only (Year, Month, Day) | `2026-10-09` |
| `LocalTime` | Time only (Hour, Minute, Second, Nanosecond) | `14:30:15.123` |
| `LocalDateTime` | Date + Time without timezone | `2026-10-09T14:30:15` |
| `ZonedDateTime` | Date + Time with Timezone | `2026-10-09T14:30:15+05:30[Asia/Kolkata]` |
| `Instant` | Machine timestamp (UTC Epoch nanoseconds) | `2026-10-09T09:00:00Z` |
| `Period` | Date-based amount of time (Years, Months, Days) | `P2Y3M10D` |
| `Duration` | Time-based amount of time (Seconds, Nanoseconds) | `PT8H30M` |

---

## 3. Working with Date and Time

```java
import java.time.*;

public class DateAndTimeUsage {
    public static void main(String[] args) {
        // Current values
        LocalDate today = LocalDate.now();
        LocalTime now = LocalTime.now();
        LocalDateTime currentDateTime = LocalDateTime.now();

        // Creating specific instances
        LocalDate specificDate = LocalDate.of(2026, Month.OCTOBER, 9);
        LocalTime specificTime = LocalTime.of(14, 30, 0);
        LocalDateTime specificDateTime = LocalDateTime.of(specificDate, specificTime);

        // Date manipulations (Immutable - returns new instance)
        LocalDate nextWeek = today.plusWeeks(1);
        LocalDate previousMonth = today.minusMonths(1);

        // Extracting components
        int year = today.getYear();
        Month month = today.getMonth();
        int dayOfMonth = today.getDayOfMonth();
        DayOfWeek dayOfWeek = today.getDayOfWeek();
    }
}
```

---

## 4. Timezones: `ZonedDateTime` & `ZoneId`

`ZonedDateTime` represents a complete date-time with a fully tracked timezone offset and rules (DST handling).

```java
import java.time.*;

public class TimeZoneExample {
    public static void main(String[] args) {
        // Current time in system default zone
        ZonedDateTime nowDefault = ZonedDateTime.now();

        // Specific Timezone
        ZoneId istZone = ZoneId.of("Asia/Kolkata");
        ZoneId utcZone = ZoneId.of("UTC");

        ZonedDateTime istTime = ZonedDateTime.now(istZone);
        
        // Convert existing LocalDateTime to ZonedDateTime
        LocalDateTime localDateTime = LocalDateTime.now();
        ZonedDateTime convertedZdt = localDateTime.atZone(istZone);

        // Convert between time zones
        ZonedDateTime utcTime = istTime.withZoneSameInstant(utcZone);
    }
}
```

---

## 5. Machine Time: `Instant`
`Instant` represents a specific point on the timeline in UTC measured from Java Epoch (`1970-01-01T00:00:00Z`). Used primarily for application timestamps and logs.

```java
import java.time.Instant;

public class InstantExample {
    public static void main(String[] args) {
        Instant now = Instant.now(); // Current UTC timestamp
        long epochSecond = now.getEpochSecond(); // Seconds since 1970-01-01
        long epochMilli = now.toEpochMilli();   // Milliseconds since 1970-01-01

        Instant specificInstant = Instant.ofEpochSecond(epochSecond);
    }
}
```

---

## 6. Measuring Differences: `Period` vs `Duration`

* **`Period`**: Date-based amount of time (Years, Months, Days).
* **`Duration`**: Time-based amount of time (Hours, Minutes, Seconds, Nanoseconds).

```java
import java.time.*;

public class PeriodAndDuration {
    public static void main(String[] args) {
        // Period Example (Dates)
        LocalDate startDate = LocalDate.of(2024, 1, 1);
        LocalDate endDate = LocalDate.of(2026, 10, 9);
        
        Period period = Period.between(startDate, endDate);
        System.out.println("Years: " + period.getYears() + 
                           ", Months: " + period.getMonths() + 
                           ", Days: " + period.getDays());

        // Duration Example (Times)
        LocalTime startTime = LocalTime.of(9, 0, 0);
        LocalTime endTime = LocalTime.of(17, 30, 0);

        Duration duration = Duration.between(startTime, endTime);
        System.out.println("Hours: " + duration.toHours());
        System.out.println("Minutes: " + duration.toMinutes());
    }
}
```

---

## 7. Formatting & Parsing: `DateTimeFormatter`
`DateTimeFormatter` is thread-safe and replaces legacy `SimpleDateFormat`.

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class FormattingExample {
    public static void main(String[] args) {
        LocalDateTime now = LocalDateTime.now();

        // 1. Formatting Date to String
        DateTimeFormatter customFormatter = DateTimeFormatter.ofPattern("dd-MM-yyyy HH:mm:ss");
        String formattedDate = now.format(customFormatter);

        // 2. Parsing String to Date
        String dateString = "09-10-2026 14:30:00";
        LocalDateTime parsedDateTime = LocalDateTime.parse(dateString, customFormatter);

        // Standard Predefined Formatters
        String isoDate = now.format(DateTimeFormatter.ISO_LOCAL_DATE_TIME);
    }
}
```

---

## 8. Converting Legacy `Date` to Modern `java.time`
When working with legacy codebases or database drivers, conversion to and from `java.util.Date` is frequently needed via `Instant`.

```java
import java.time.*;
import java.util.Date;

public class LegacyConversion {
    public static void main(String[] args) {
        // Legacy Date to LocalDateTime
        Date legacyDate = new Date();
        LocalDateTime localDateTime = legacyDate.toInstant()
            .atZone(ZoneId.systemDefault())
            .toLocalDateTime();

        // LocalDateTime to Legacy Date
        LocalDateTime now = LocalDateTime.now();
        Date newLegacyDate = Date.from(now.atZone(ZoneId.systemDefault()).toInstant());
    }
}
```

---

## 9. Tricky Interview Questions & Answers

### Q1. Why is `SimpleDateFormat` thread-unsafe, and how does `DateTimeFormatter` solve this?
`SimpleDateFormat` maintains state (like `Calendar` instances) internally while parsing and formatting. When shared across multiple threads, concurrent operations overwrite this state, leading to garbage outputs or exceptions. `DateTimeFormatter` is completely immutable and stateless, making it inherently thread-safe.

### Q2. What is the difference between `Period.between()` and `ChronoUnit` methods?
`Period.between()` returns a decomposed year, month, and day representation (e.g., 2 years, 3 months, 5 days). `ChronoUnit` calculates total duration in a single specified unit (e.g., total days or total months).

```java
LocalDate d1 = LocalDate.of(2025, 1, 1);
LocalDate d2 = LocalDate.of(2026, 10, 9);

long totalDays = java.time.temporal.ChronoUnit.DAYS.between(d1, d2); // Total days across whole span
```

### Q3. How do you handle database persistence for Date/Time in Java 8 / JPA 2.2+?
Map Java 8 `java.time` classes directly to SQL types without `@Temporal`:
* `LocalDate` $\rightarrow$ `DATE`
* `LocalTime` $\rightarrow$ `TIME`
* `LocalDateTime` $\rightarrow$ `TIMESTAMP`
* `Instant` / `ZonedDateTime` $\rightarrow$ `TIMESTAMP WITH TIME ZONE`

---

## 10. Quick Revision

* **Immutable & Thread-Safe**: All core classes under `java.time` cannot be altered post-creation.
* **`LocalDate` / `LocalTime` / `LocalDateTime`**: Zone-agnostic representations.
* **`ZonedDateTime`**: Full date-time representation including zone rules (`ZoneId`).
* **`Instant`**: Universal UTC machine-level timestamp (`1970-01-01T00:00:00Z`).
* **`Period` vs `Duration`**: `Period` measures date differences (Years/Months/Days); `Duration` measures time differences (Hours/Minutes/Seconds).
* **`DateTimeFormatter`**: Immutable replacement for `SimpleDateFormat`.
