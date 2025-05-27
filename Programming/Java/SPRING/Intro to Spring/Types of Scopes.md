---
создал заметку: "2025-05-26"
---
### 📦 Основные типы областей видимости (scopes) в Spring:

Области видимости определяют жизненный цикл и видимость этих бинов в кнтексте их использования

Типы областей видимости:
1. **Singleton** (по умолчанию)
    
    - **Описание**: Один экземпляр бина создаётся для всего контейнера Spring.
	
	- **Использование**: Подходит для статических компонентов, например, сервисов или репозиториев.
	
```java title:singleton
@Component
public class SingletonBean {
    public SingletonBean() {
        System.out.println("Создан SingletonBean");
    }
}
```

2. **Prototype**
    
    - **Описание**: Каждый запрос к контейнеру создаёт новый экземпляр бина.
    
    - **Использование**: Полезно, когда требуется множество независимых экземпляров, например, для объектов с внутренним состоянием.
	
```java title:prototype
@Component
@Scope("prototype")
public class PrototypeBean {
    public PrototypeBean() {
        System.out.println("Создан PrototypeBean");
    }
}
```

2. **Request** (только для веб-приложений)
    
    - **Описание**: Создаётся один бин на каждый HTTP-запрос.
     
    - **Использование**: Подходит для компонентов, зависящих от данных запроса.
	
```java title:request
@Component
@Scope(value = WebApplicationContext.SCOPE_REQUEST, proxyMode = ScopedProxyMode.TARGET_CLASS)
public class RequestBean {
    public RequestBean() {
        System.out.println("Создан RequestBean");
    }
}
```

2. **Session** (только для веб-приложений)
    
    - **Описание**: Один бин на HTTP-сессию.
     
    - **Использование**: Полезно для хранения информации о пользователе в течение сессии.
	
```java title:session
@Component
@Scope(value = WebApplicationContext.SCOPE_SESSION, proxyMode = ScopedProxyMode.TARGET_CLASS)
public class SessionBean {
    public SessionBean() {
        System.out.println("Создан SessionBean");
    }
}
```

2. **Application**
    
    - **Описание**: Один бин на весь жизненный цикл приложения.
     
    - **Использование**: Подходит для компонентов, общих для всего приложения.
	
```java title:application
@Component
@Scope(value = WebApplicationContext.SCOPE_APPLICATION)
public class ApplicationBean {
    public ApplicationBean() {
        System.out.println("Создан ApplicationBean");
    }
}
```

2. **Websocket**
    
    - **Описание**: Один бин на WebSocket-сессию.
    
    - **Использование**: Используется в приложениях, использующих WebSocket для коммуникации.
	
```java
@Component
@Scope(scopeName = "websocket", proxyMode = ScopedProxyMode.TARGET_CLASS)
public class WebSocketBean {
    public WebSocketBean() {
        System.out.println("Создан WebSocketBean");
    }
}
```

---
### ⚙️ Как задать область видимости бина?

В аннотации `@Scope` указывается желаемая область видимости:

```java title:scope
@Component
@Scope("<scope type>")
public class MyBean {
    // ...
}

```
---

### 🧠 Важные замечания

- **Singleton** бины создаются при инициализации контейнера и остаются в памяти до его завершения.

- **Prototype** бины создаются при каждом запросе, но Spring не управляет их полным жизненным циклом (например, не вызывает методы уничтожения).

- Для областей видимости, зависящих от веб-контекста (`request`, `session` и т.д.), необходимо использовать WebApplicationContext.