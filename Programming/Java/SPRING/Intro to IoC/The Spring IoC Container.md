---
создал заметку: 2025-05-18
tags:
  - java
  - spring
---
## IoC Container
Это обычная характеристика фреймворков, которую осуществляет IoC

В Spring фреймворке, интерфейс *ApplicationContext* представляет IoC контейнер. Этот контейнер отвечает за *создание*, *настройку* и *сборку* объектов известные как **beans**, так же управляет их жизненным циклом.

#### Виды реализации *ApplicationContext* interface
Для автономных приложений:
- *AnotationConfigApplicationContext*
- *ClassPathXmlApplicationContext*
- *FileSystemXmlApplicationContext*
Для веб приложений:
- *WebApplicationContext*

Для сборки *beans*, контейнер использует метаданные конфигураций из:
- *XML*
- *Annotations*

Ручной способ объявление контейнера 
```java
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
```

Ручной способ объявления через *AnnotationConfigApplicationContext*

```java
AnnotationConfigApplicationContext annotationContext = new AnnotationConfigApplicationContext();
```

Когда вы создаёте экземпляр *AnnotationConfigApplicationContext* и предоставляете ему один или болуу конфигурационный класс, он сканирует эти классы на наличие  анатаци`@Beans` и другие 