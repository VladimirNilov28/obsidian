---
создал заметку: "2025-03-20"
tags: []
---
## ✅ Исправленный конспект (Obsidian-стиль):

---

### 🕒 Описание

Каждая страна в наше время придерживается определённого часового пояса.  
Временные пояса могут быть **неявными** из-за таких переменных, как переход на летнее время.  
Ко всему прочему, при работе с часовыми поясами в коде, некоторые вещи могут сбить с толку.

Ранее Java предоставляла классы вроде `Date`, `Time`, `DateTime`, чтобы работать с датами, но они были неудобными и не учитывали зоны.  
С появлением пакета `java.time` в новых версиях Java (начиная с Java 8), были добавлены **более точные и удобные классы**: `ZoneId`, `ZoneOffset`, `ZonedDateTime` и др.

---

### 🧩 Резюме

#### **ZoneId** — представление часового пояса

ZoneId имеет две реализации:

- **С фиксированным смещением** относительно GMT/UTC
- **Географический регион**, который имеет набор правил для расчёта смещения

```java
ZoneId zone = ZoneId.of("Europe/Berlin");

// zone => Europe/Berlin
```

---

#### **ZoneOffset**

`ZoneOffset` наследуется от `ZoneId` и определяет **фиксированное смещение** часового пояса, например `+02:00`.  
Это означает, что значение представляет собой фиксированное число часов и минут, которое показывает разницу с UTC.

```java
LocalDateTime now = LocalDateTime.now();
ZoneId zone = ZoneId.of("Europe/Berlin");
ZoneOffset zoneOffset = zone.getRules().getOffset(now);

// zoneOffset => +01:00 (или +02:00 в зависимости от даты)
```

> Если страна использует переход на летнее/зимнее время, то ZoneOffset может быть разным в зависимости от даты → требуется `LocalDateTime`.

---

### 📆 DateTime классы

#### **ZonedDateTime**

`ZonedDateTime` — неизменяемый (immutable) объект, представляющий дату и время с учётом часового пояса в ISO-8601 формате.  
Он включает:

- `LocalDateTime`
- `ZoneId`
- `ZoneOffset` (автоматически вычисляется по ZoneId и дате)

Пример:

```java
ZoneId zone = ZoneId.of("Europe/Berlin");
ZonedDateTime date = ZonedDateTime.now(zone);

// date => 2025-03-20T13:20:31.068+01:00[Europe/Berlin]
```

Также можно **конвертировать дату и время из одной зоны в другую**:

```java
// Исходное время в Нью-Йорке
ZonedDateTime sourceDate = ZonedDateTime.of(
    2025, 3, 19, 14, 0, 0, 0, 
    ZoneId.of("America/New_York")
);

// Конвертируем в Берлин
ZoneId destZoneId = ZoneId.of("Europe/Berlin");
ZonedDateTime destDate = sourceDate.withZoneSameInstant(destZoneId);

System.out.println("Исходное время (Нью-Йорк): " + sourceDate);
System.out.println("Конвертированное время (Берлин): " + destDate);
```

Также `ZonedDateTime` можно создать из строки:

```java
ZonedDateTime time = ZonedDateTime.parse("2007-04-05T12:30-02:00");
```

---

#### **OffsetDateTime**

`OffsetDateTime` — неизменяемый объект, представляющий дату и время с **фиксированным смещением** от UTC, но **без привязки к региону**.

Пример:

```java
ZoneOffset offset = ZoneOffset.of("+02:00");
OffsetDateTime date = OffsetDateTime.now(offset);

// date => 2025-03-20T14:27:48.603+02:00
```

---

#### 🧪 Полезные методы `ZonedDateTime`

- `now()` — текущее время компьютера с часовым поясом
- `of(...)` — ручное создание даты, времени и региона
- `parse(...)` — чтение даты из строки в ISO-8601 (`Z` = Zulu Time = UTC)
- `getYear()` — год
- `getMonth()` / `getMonthValue()` — месяц (строкой / числом)
- `getDayOfMonth()` / `getDayOfWeek()` — день месяца / недели
- `getZone()` — возвращает `ZoneId`
- `toLocalTime()` — только **время**, без даты и зоны
- `toLocalDate()` — только **дата**, без времени и зоны
- `with(...)` — создаёт копию `ZonedDateTime` с изменённым полем (например, часом)

---

#### **OffsetTime**

`OffsetTime` — это объект, представляющий **только время** (без даты), с **указанием смещения** от UTC.

Пример:

```java
ZoneOffset offset = ZoneOffset.of("+02:00");
OffsetTime time = OffsetTime.now(offset);

// time => 14:28:37.728+02:00
```

> Используется, если нужна точная работа со временем без даты — например, в расписаниях.
