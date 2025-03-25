---
создал заметку: "2025-03-20"
tags: []
---
### Описание
Каждая страна в наше время придерживается определённого часового пояса. Временные пояса могут быть не явными из за таких переменных как переход на летнее время. Ко всему прочему при представлении часовых поясов в коде, некоторые вещи могут сбить с толку.
В прошлом в Java  предоставляла такие классы как Date, Time, DateTime что бы заботиться о часовых поясах. Однако в новой версии Java появилось куда более удобные классы, такие как ZoneId и ZoneOffset для управления часовыми поясами.

### Резюме
**ZoneId** - это представления часового пояса. У него есть 2 реализации:
- С фиксированным смещением относительно GMT/UTC
-  Географический регион, который имеет набор правил для расчёта смещения относительно GMT/UTC

```Java title:" ZoneId for Berlin, Germany"
ZonedId zone = ZonedId.off("Europe/Berlin");

//zone ==> Europe/Berlin
```

**ZonedOffset**  - ZoneOffset наследуется от ZoneId и определяет фиксированное смещение текущего часового пояса относительно GMT/UTC например +02:00. Это означает что эти числа представляют фиксированные часы и минуты, представляющие разницу между временем в текущем часовом поясе и GMT/UTC

```Java
LocalDateTime now = LocalDateTime.now();
ZoneId zone = ZoneId.of("Europe/Berlin");
ZoneOffset zoneOffSet = zone.getRules().getOffset(now);

//zoneOffSet ==> +01:00

```

В случае если страна имеет два разных смещения - летнее и зимнее, то там будет два разных ZoneOffset реализации для одного региона, следовательно требуется указать LocalDateTime

#### **DateTime классы**  
**ZonedDateTime** -   неизменяемое после создания представление даты и времени часового пояса в ISO-8601 календарной системе. Пример `2007-12-03T10:15:30+01:00 Europe/Paris` . Объект ZonedDateTime содержит состояние, эквивалентное трем отдельным объектам: LocalDateTime, ZoneId и вычисленному ZoneOffset. Этот класс хранит в себе все поля даты и времени. Например ZonedDateTime может хранить значение *“2nd October 2007 at 13:45.30.123456789 +02:00 in the Europe/Paris time-zone”.*
	
```java title:"ZonedDateTime for the previous region"
ZoneId zone = ZoneId.of("Europe/Berlin");
ZonedDateTime date = ZonedDateTime.now(zone);

//date ==> 2025-03-20T13:20:31.068233486+01:00[Europe/Berlin]
```

**ZonedDateTime** также обеспечивает встроенную функцию для конвертации данной даты из одного временного пояса в другой

```Java title:"Convertation from onte time zone to another"
// Исходное время в Нью-Йорке (UTC-5)
ZonedDateTime sourceDate = ZonedDateTime.of(
	2025, 3, 19, 14, 0, 0, 0, 
	ZoneId.of("America/New_York")
);

// Конвертируем в Берлин (UTC+1, но возможно UTC+2 в марте из-за летнего времени)
ZoneId destZoneId = ZoneId.of("Europe/Berlin");
ZonedDateTime destDate = sourceDate.withZoneSameInstant(destZoneId);

// Вывод
System.out.println("Исходное время (Нью-Йорк): " + sourceDate);
System.out.println("Конвертированное время (Берлин): " + destDate);

```

В **ZonedDateTime** можно передать готовое значение используя метод **parse()**, например:
```java title:"example"
ZonedDateTime time = ZonedDateTime.parse("2007-04-05T12:30-02:00");
```

**OffsetDateTime** - неизменяемое после создания представление даты и времени смещение которого в календарной системе ISO-8610. Пример `2007-12-03T10:15:30+01:00`. Этот класс хранит все поля даты и времени с точностью до наносекунд, а также смещение относительно GMT/UTC.

```Java title:"OffetDateTime with 2 hours of offset form GMT/UTC"
ZoneOffset zoneOffSet= ZoneOffset.of("+02:00");
OffsetDateTime date = OffsetDateTime.now(zoneOffSet);

//date ==> 2025-03-20T14:27:48.603169650+02:00

```

Методы для **ZonedDateTime**:
- **now()** - выдает нынешнее время компьютера
- **of()** - ручное перечисление даты времени и региона. Пример приведён выше
- **parse()** - позволяет передать готовое значение в формате **ISO-8601**
- **getYear()** - выдаёт год
- **getZone()** - выдаёт регион
- **getMonthValue()** - выдаёт месяц от 1 - 12 (**int**)
- **getMonth()** - выдаёт месяц (**String**)
- **getDayOfMonth()** - выдаёт день месяца
- **getDayOfWeek()** - выдаёт день недели
- **toLocalTime()** - выдаёт местное время
- **toLocalDate()** - выдаёт местную дату без часового пояса и чего либо лишнего, например 2007-12-03
- **with(...)** - позволяет создавать копию **ZonedDateTime** с изменениями ![[Pasted image 20250325122914.png]]

**OffsetTime** - это неизменяемое после создания  объект даты и времени, представляющий время, часто рассматриваемое как часы-минуты-секунды-смещение в календарной системе ISO-8601, например, `10:15:30+01:00` Этот класс хранит все поля с точностью до наносекунд так же хорошо как _zone offset_. OffsetTime может хранить такое значение:`13:45.30.123456789+02:00`

```java title:"current OffsetTime with 2 hours of offset"
ZoneOffset zoneOffSet = ZoneOffset.of("+02:00");
OffsetTime time = OffsetTime.now(zoneOffSet);

//date2 ==> 14:28:37.728127810+02:00
```