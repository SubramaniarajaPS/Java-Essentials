
---

# Java Date and Time API (`java.time`) Revision Notes

## 1. Introduction & Design Principles
The `java.time` package (JSR-310) replaces the legacy `java.util.Date` and `java.util.Calendar` classes, which were mutable, not thread-safe, and had poor API design (e.g., months starting at index 0).

**Key Design Principles:**
*   **Immutable:** All core classes are immutable. Once created, they cannot be changed. This ensures thread-safety without synchronization.
*   **Clear & Fluent:** Methods are well-defined (e.g., null parameters usually trigger exceptions) and can be chained (e.g., `date.plusDays(1).minusHours(2)`).
*   **ISO-8601 Centric:** The default calendar system is the ISO-8601 (Gregorian) system, used globally.
*   **Domain-Driven:** Distinct classes exist for different concepts (e.g., "Date without time" vs. "Time without date" vs. "Machine Timestamp").

---

## 2. Core Temporal Classes (The "Local" Types)
These classes represent date and time from the context of the observer, **without** a specific time zone or offset context.

### **`LocalDate`**
*   **Description:** Represents a date (year-month-day) without time or zone (e.g., `2023-12-03`).
*   **Use Case:** Birthdays, holidays, paydays.
*   **Key Methods:**
    *   `now()`, `of(year, month, day)`, `parse(text)`: Creation methods.
    *   `getYear()`, `getMonth()`, `getDayOfWeek()`: Field access.
    *   `plusDays()`, `minusMonths()`, `withYear()`: Manipulation.
    *   `isLeapYear()`: Checks for leap years.
    *   `atTime(LocalTime)`: Combines with a time to form `LocalDateTime`.

### **`LocalTime`**
*   **Description:** Represents time (hour-minute-second-nanosecond) without a date or zone (e.g., `10:15:30`).
*   **Use Case:** Opening hours, alarm clock settings.
*   **Key Methods:**
    *   `now()`, `of(h, m, s)`, `parse()`.
    *   `plusHours()`, `minusMinutes()`.
    *   `atDate(LocalDate)`: Combines with a date to form `LocalDateTime`.
    *   `truncatedTo(TemporalUnit)`: Truncates time (e.g., to minutes).

### **`LocalDateTime`**
*   **Description:** A combination of `LocalDate` and `LocalTime` (e.g., `2007-12-03T10:15:30`).
*   **Constraint:** It does **not** represent an instant on the timeline because it lacks an offset/zone context.
*   **Key Methods:**
    *   `toLocalDate()`, `toLocalTime()`: Extracts parts.
    *   `atZone(ZoneId)`: Converts to `ZonedDateTime`.

---

## 3. Machine Time & Timestamp

### **`Instant`**
*   **Description:** Represents a specific instantaneous point on the timeline (UTC). It counts seconds from the Unix Epoch (Jan 1, 1970 UTC).
*   **Precision:** Nanosecond precision.
*   **Use Case:** Timestamps for logs, transaction times, converting legacy `java.util.Date`.
*   **Key Methods:**
    *   `now()`: Gets current instant.
    *   `ofEpochSecond(long)`: Creates from epoch seconds.
    *   `toEpochMilli()`: Converts to milliseconds (legacy compatibility).
    *   `plus()`, `minus()`: Math operations.

### **`Clock`** (Abstract Class)
*   **Description:** Provides access to the current instant, date, and time using a specific time-zone. It is pluggable (useful for testing).
*   **Key Methods:**
    *   `systemUTC()`: System clock in UTC.
    *   `fixed(Instant, ZoneId)`: Returns a clock that always returns the same instant (time stands still).
    *   `offset(Clock, Duration)`: Returns a clock offset by a specific duration.

---

## 4. Time Zones and Offsets

### **`ZoneId`**
*   **Description:** Represents a time-zone identifier (e.g., `Europe/Paris`, `Asia/Kolkata`) or an offset. It handles Daylight Saving Time (DST) rules via `ZoneRules`.
*   **Key Methods:**
    *   `of(String)`: Creates a ZoneId.
    *   `getAvailableZoneIds()`: Returns all known IDs.
    *   `systemDefault()`: Gets the system's default zone.

### **`ZoneOffset`**
*   **Description:** Extends `ZoneId`. Represents the fixed offset from Greenwich/UTC (e.g., `+02:00`).
*   **Constraints:** Range restricted to -18:00 to +18:00.
*   **Key Fields:** `UTC`, `MIN`, `MAX`.

### **`ZonedDateTime`**
*   **Description:** A `LocalDateTime` linked to a `ZoneId` (e.g., `2007-12-03T10:15:30+01:00[Europe/Paris]`).
*   **Functionality:** Fully handles DST gaps and overlaps.
*   **Key Methods:**
    *   `withZoneSameInstant(ZoneId)`: Converts time to a different zone while keeping the exact moment in time.
    *   `toOffsetDateTime()`: Converts to `OffsetDateTime`.

### **`OffsetDateTime`** & **`OffsetTime`**
*   **Description:** Stores date/time with a fixed `ZoneOffset` but **without** specific region rules (no DST awareness).
*   **Use Case:** Database storage, network protocols, XML messages.

---

## 5. Amounts of Time

### **`Duration`** (Time-based)
*   **Description:** A quantity of time in terms of seconds and nanoseconds.
*   **Nature:** Represents "Machine Time". 1 day is exactly 24 hours (ignores DST).
*   **Key Methods:** `between()`, `ofMinutes()`, `toDays()`, `plus()`, `minus()`.

### **`Period`** (Date-based)
*   **Description:** A quantity of time in terms of years, months, and days.
*   **Nature:** Represents "Human Time". Adding "1 Month" accounts for variable month lengths.
*   **Key Methods:** `between()`, `ofDays()`, `getYears()`.

### **`ChronoUnit`** (Enum)
*   **Description:** A standard set of date periods units (e.g., `DAYS`, `MONTHS`, `MICROS`).
*   **Function:** Implements `TemporalUnit`. Used in `plus`, `minus`, and `between` methods.
*   **Special:** `FOREVER` (artificial unit).

---

## 6. Partial Time Classes & Enums

### **Partial Classes**
*   **`Year`**: Represents a year (e.g., 2023).
*   **`YearMonth`**: Represents a year and month (e.g., credit card expiry `2025-12`).
*   **`MonthDay`**: Represents a month and day (e.g., birthdays `--12-03`).

### **Enums**
*   **`DayOfWeek`**: `MONDAY` through `SUNDAY`. Includes methods like `plus(long)`.
*   **`Month`**: `JANUARY` through `DECEMBER`. Includes `length(boolean isLeapYear)`.

---

## 7. Formatting and Parsing

### **`DateTimeFormatter`**
*   **Description:** Main class for printing and parsing date-time objects.
*   **Thread Safety:** Immutable and thread-safe.
*   **Predefined Formatters:** `ISO_LOCAL_DATE`, `ISO_INSTANT`, `BASIC_ISO_DATE`.
*   **Custom Patterns:** `ofPattern("dd-MMM-yyyy")`.
*   **Usage:**
    *   `date.format(formatter)`
    *   `LocalDate.parse(string, formatter)`

### **`TextStyle`** (Enum)
*   **Description:** Defines size of text for formatting (`FULL`, `SHORT`, `NARROW`) and context (`STANDALONE`).

---

## 8. Advanced Temporal Framework
This package (`java.time.temporal`) is for low-level access and library writers.

### **Interfaces**
*   **`Temporal`**: Framework-level interface for read-write access (implemented by `LocalDate`, `Instant`, etc.).
*   **`TemporalAccessor`**: Read-only access to temporal objects.
*   **`TemporalAdjuster`**: Functional interface for modifying dates (Strategy pattern). Method: `adjustInto(Temporal)`.
*   **`TemporalQuery`**: Functional interface for extracting information from temporals.

### **Classes**
*   **`TemporalAdjusters`**: Static factory methods for common adjustments.
    *   Examples: `firstDayOfMonth()`, `next(DayOfWeek)`, `lastInMonth()`.
*   **`ChronoField`**: Enum implementing `TemporalField` (e.g., `DAY_OF_WEEK`, `YEAR`).
*   **`IsoFields`**: Fields specific to ISO-8601 (e.g., `QUARTER_OF_YEAR`).

---

## 9. Legacy Interoperability & Chronology
*   **Interoperability:** New methods added to old classes to convert to the new API.
    *   `Date.toInstant()` / `Date.from(Instant)`
    *   `Calendar.toInstant()`
    *   `GregorianCalendar.toZonedDateTime()`
    *   `TimeZone.toZoneId()`
*   **`java.time.chrono`:** Supports non-ISO calendar systems like `HijrahDate`, `JapaneseDate`, `ThaiBuddhistDate`.

---

# Code Examples Section

### 1. Creating and Manipulating Dates (`LocalDate`)
```java
import java.time.LocalDate;
import java.time.Month;
import java.time.temporal.ChronoUnit;

public class DateExample {
    public static void main(String[] args) {
        // Get current date
        LocalDate today = LocalDate.now();

        // Create specific date (Note: Months are 1-based or use Enum)
        LocalDate birthday = LocalDate.of(1995, Month.MAY, 23); 
        
        // Manipulation (Immutable - returns new instance)
        LocalDate nextWeek = today.plus(1, ChronoUnit.WEEKS);
        LocalDate lastMonth = today.minusMonths(1);

        // Get info
        boolean isLeap = today.isLeapYear();
        System.out.println("Today: " + today); // ISO Format: 2023-10-27
    }
}
```
*[Source 503, 504]*

### 2. Time Zones and ZonedDateTime
```java
import java.time.*;

public class ZoneExample {
    public static void main(String[] args) {
        // Create a LocalDateTime (no zone)
        LocalDateTime ldt = LocalDateTime.of(2023, Month.JULY, 20, 19, 30);
        
        // Apply a ZoneId to create ZonedDateTime
        ZoneId laZone = ZoneId.of("America/Los_Angeles");
        ZonedDateTime departure = ZonedDateTime.of(ldt, laZone);
        
        // Convert to a different time zone (Tokyo) preserving the instant
        // Handles time difference calculation automatically
        ZonedDateTime arrivalInTokyo = departure.withZoneSameInstant(ZoneId.of("Asia/Tokyo"));
        
        System.out.println("Departure LA: " + departure);
        System.out.println("Arrival Tokyo: " + arrivalInTokyo);
    }
}
```
*[Source 1621, 1903]*

### 3. Machine Time (`Instant`) and Duration
```java
import java.time.Duration;
import java.time.Instant;

public class InstantExample {
    public static void main(String[] args) {
        // Current timestamp (UTC)
        Instant start = Instant.now();
        
        // Simulate a process
        doSomething();
        
        Instant end = Instant.now();
        
        // Calculate duration between two instants
        Duration timeElapsed = Duration.between(start, end);
        System.out.println("Milliseconds elapsed: " + timeElapsed.toMillis());
    }

    private static void doSomething() { /* ... */ }
}
```
*[Source 1072, 1525]*

### 4. Human Time Differences (`Period`)
```java
import java.time.LocalDate;
import java.time.Period;

public class PeriodExample {
    public static void main(String[] args) {
        LocalDate birthDate = LocalDate.of(1990, 1, 1);
        LocalDate today = LocalDate.now();
        
        // Calculate period between dates
        Period age = Period.between(birthDate, today);
        
        System.out.println("Years: " + age.getYears());
        System.out.println("Months: " + age.getMonths());
        System.out.println("Days: " + age.getDays());
    }
}
```
*[Source 1073, 1528]*

### 5. Formatting and Parsing
```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class FormatExample {
    public static void main(String[] args) {
        LocalDateTime now = LocalDateTime.now();
        
        // Formatting to String
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd-MMM-yyyy HH:mm");
        String formatted = now.format(formatter);
        System.out.println("Formatted: " + formatted);
        
        // Parsing from String
        String dateStr = "25-Dec-2025 10:00";
        LocalDateTime parsedDate = LocalDateTime.parse(dateStr, formatter);
        System.out.println("Parsed: " + parsedDate);
    }
}
```
*[Source 570, 1467]*

### 6. Temporal Adjusters (Advanced Manipulation)
```java
import java.time.DayOfWeek;
import java.time.LocalDate;
import java.time.temporal.TemporalAdjusters;

public class AdjusterExample {
    public static void main(String[] args) {
        LocalDate date = LocalDate.now();
        
        // Find the last day of the current month
        LocalDate lastDay = date.with(TemporalAdjusters.lastDayOfMonth());
        
        // Find the next Friday
        LocalDate nextFri = date.with(TemporalAdjusters.next(DayOfWeek.FRIDAY));
        
        System.out.println("Last day of month: " + lastDay);
        System.out.println("Next Friday: " + nextFri);
    }
}
```
*[Source 631, 1605]*

### 7. Legacy Conversion (`java.util.Date` to `java.time`)
```java
import java.time.Instant;
import java.time.LocalDateTime;
import java.time.ZoneId;
import java.util.Date;

public class LegacyExample {
    public static void main(String[] args) {
        // Old Date object
        Date oldDate = new Date();
        
        // Convert to Instant
        Instant instant = oldDate.toInstant();
        
        // Convert Instant to LocalDateTime (requires ZoneId)
        LocalDateTime ldt = instant.atZone(ZoneId.systemDefault()).toLocalDateTime();
        
        System.out.println("Converted: " + ldt);
    }
}
```

---

> For more detailed references : 

[Java Date & Time - Jenkov Blog Lists](https://jenkov.com/tutorials/java-date-time/index.html)

[Java Date & Time - Java-8-tips Blog](https://java-8-tips.readthedocs.io/en/stable/datetime.html)

[Java Date & Time - Javabrahman Blog Lists](https://www.javabrahman.com/java-8/overview-of-java-8-new-date-time-api-java-time-package-tutorial/)

[Data & Time Coding example - Java Concept of the Day Blog](https://javaconceptoftheday.com/java-8-date-time-api-coding-examples/)