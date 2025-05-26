---
создал заметку: 2025-05-25
tags:
  - java
  - spring
---
#### Опредиление Spring Bean
В *Spring*, объекты которые являються основой вашей программы и которые управляються с помощю *Spring IoC container* называются *beans*. *Bean* это объект который объявлен, собран и тем или иным способом контролируемый *Spring IoC container*

#### Базавая реализация класса и его зависимостей

```java title:class_decloration
public class Company {
	private Address address;
	
	public Company(Address address) {
		this.address = address;
	}
	
	//getters setters and etc
}
```

```java title:class_collabolator
public class Address {
	private String street;
	private int number;
	
	public Address(String street, int number) {
		this.street = street;
		this.number = number;
	}
	
	//getters and setters
}
```

традиционный метод

```java
Address address = new Address("Higt Street", 1000)
Company company = new Company(address);
```

Это метод классический, но в случае когда в программе десятки или даже сотни классов мы иногда хотим поделиться экземляром класса во всем приложении, вдругое врмея нам нужен отдельный объект для каждого случая и так далее. Это очень сложно и урпавлять таким нпстоящий кошмар

#### Реализация с использованием *IoC*

```java
@Component
public class Company {
	private Address address;
	
	public Company(Address address) {
		this.address = address;
	}
	
	//getters setters and etc
}
```

далее конфигурация класса передающая *bean metadata* в *IoC* контейнер 

```java
@Configuration
@ComponentScan(basePackageClasses = Company.class)
public class Config {
	@Bean
	public Address getAddress() {
		return new Address("Hight Street", 1000);
	}
}
```

Конфигурационный класс производит бин типа Address. Так же обращает винмание на @ComponentScan, который говорит контейнеру искать бины в пакете, содержащем класс Company

#### IoC в действии
С момента как определили бины в конфигурационном классе, нам теперь нужна реализация класса AnnotationConfigApplicationContext что бы поднять контейнер

```java
ApplicationContext context = new AnnotationConfigApplicationContext(Config.class);
```

```java title:test
Company company = context.getBean("company", Company.class);
```
