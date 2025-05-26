---
создал заметку: 2025-05-22
tags:
  - java
  - spring
---
В случае *Constructor-Based Dependency Injection*, контейнер ссылаеться на конструктор с аргуменамы которые описывают зависимости которые мы хотим настроить.


---

### Как Spring разрешает аргументы для внедрения зависимостей

Spring пытается найти подходящий бин для каждого аргумента в таком порядке:

1. Сначала по **типу** аргумента.

2. Если нескольких кандидатов одного типа — по **имени** аргумента (названию переменной).

3. Если и это не даёт однозначности — по **индексу** (позиции) аргумента в списке.

Если неоднозначность сохраняется, необходимо использовать аннотации вроде `@Qualifier` или `@Primary`.

---

### Примеры кода

```java
// 1. По типу (Primarily by type)
public MyController(MyService service) {
    this.service = service; // Spring ищет бин с типом MyService
}
```

```java
// 2. По имени (Followed by the name of the attribute)
@Bean
public MyService mySpecificService() {
    return new MySpecificService();
}

public MyController(MyService mySpecificService) {
    this.service = mySpecificService; // Spring выбирает бин по имени аргумента
}
```

```java
// 3. По индексу (And index for disambiguation)
public MyController(MyService service1, MyService service2) {
    // Index 0 — service1, Index 1 — service2
    this.service1 = service1;
    this.service2 = service2;
}
```

---

Конфигурация бинов и их зависимостей черех аннотации:

```java
@Configuration
public class AppConfig {
	@Bean
	public Item item1() {
		return new ItemImpl1();
	}
	
	@Bean
	public Store store() {
		return new Store(item1());
	}
}
```

---

`@Configuration` — указывает что класс являеться источником определения бинов (можно дабовлять в множесвенное конфигурацию классов)

`@Bean(<custom name>)` — используеться для указания что метод являеться бином. Если мы не укажем пользовательское имя, то имя бина по умолчанию будет совпадать с именем метода.

Для бина с областью видимости по умолчанию (singleton) Spring сначала проверяет, существует ли уже закэшированный экземпляр бина, и создаёт новый только в случае его отсутствия. Если используется область видимости prototype, контейнер возвращает новый экземпляр бина при каждом вызове метода.
