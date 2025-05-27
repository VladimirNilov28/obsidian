---
создал заметку: 2025-05-27
tags:
  - java
  - spring
---
Аннотация которая помогает уточнить какой бин нужно инжектнуть в случае когда допустим при использовании @Autowired спринг не может поянть какую зависимость использовать, приводя к ошибке **_NoUniqueBeanDefinitionException_**

пример большого количества зависимостей и не однозначного @Autowired
```java
@Component("fooFormatter")
public class FooFormatter implements Formatter {
 
    public String format() {
        return "foo";
    }
}

@Component("barFormatter")
public class BarFormatter implements Formatter {
 
    public String format() {
        return "bar";
    }
}

@Component
public class FooService {
     
    @Autowired ---> NoUniqueBeanDefinitionException
    private Formatter formatter;
}
```

Решение @Qualifier
```java
public class FooService {
     
    @Autowired
    @Qualifier("fooFormatter") <--- уточнение зависимости
    private Formatter formatter;
}
```

---

Так же существует @Primary, однако он указывает только на зависимость которую @Autowired  будет использовать в первую очередь при неоднозначном выборе
```java
@Configuration
public class Config {
 
    @Bean
    public Employee johnEmployee() {
        return new Employee("John");
    }
	
	//выбереть эту озависимость, предотвратив ошибку NoUniqueBeanDefinitionException
    @Bean
    @Primary
    public Employee tonyEmployee() {
        return new Employee("Tony");
    }
}
```

---

Так же в имени объекта можно уточнять какую зависимость использовать при использовании @Autowired, нужно использовать имя объекта, совпадающее с именем бина

```java
@Component("fooFormatter") <---
public class FooFormatter implements Formatter {
 
    public String format() {
        return "foo";
    }
}

@Component("barFormatter")
public class BarFormatter implements Formatter {
 
    public String format() {
        return "bar";
    }
}
```

```java
public class FooService {
     
    @Autowired
    private Formatter fooFormatter; <---
}

```