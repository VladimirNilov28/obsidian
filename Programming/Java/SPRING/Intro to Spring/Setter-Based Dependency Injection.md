---
создал заметку: 2025-05-22
tags:
  - java
  - spring
---
Для _setter-based DI_ контейнер Spring сначала создает бин с помощью **безаргументного конструктора** или **безаргументного статического фабричного метода**, а затем вызывает **сеттеры**, чтобы внедрить зависимости.

```java
@Bean
public Store store() {
    Store store = new Store();
    store.setItem(item1());
    return store;
}
```

Setter-based инъекция позволяет задавать зависимости после создания объекта. Это удобно для:

- **необязательных зависимостей**

- настройки объекта после создания

- ситуаций, где нужна гибкость конфигурации


Можно комбинировать _constructor-based_ и _setter-based_ типы инъекции одного и того же бина.  

Рекомендуется использовать:

- **constructor-based** — для **обязательных зависимостей**

- **setter-based** — для **необязательных или дополнительных**

---

### 🔸 Сравнение с constructor-based injection — как выглядит код

#### ✅ Setter-based

```java
@Component
public class Store {
    private Item item;

    public void setItem(Item item) {
        this.item = item;
    }
}
```

```java
@Bean
public Store store() {
    Store store = new Store();
    store.setItem(item1());
    return store;
}
```

---

#### ✅ Constructor-based

```java
@Component
public class Store {
    private final Item item;

    public Store(Item item) {
        this.item = item;
    }
}
```

```java
@Bean
public Store store() {
    return new Store(item1());
}
```
