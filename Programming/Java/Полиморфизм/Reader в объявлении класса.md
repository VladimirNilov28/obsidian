---
создал заметку: 2025-03-26
tags:
  - java
  - polimorfism
  - FileReader
  - BuferedReader
  - Writer
---
### 🧱 Почему используем интерфейс `Reader` в объявлении:

```java
Reader reader = new FileReader("file.txt");
```

---

### ✅ Преимущества:

1. **Гибкость**  
    Можно легко заменить реализацию:    
    ```java
    reader = new BufferedReader(new FileReader("file.txt"));
    reader = new StringReader("some text");
    ```
    
2. **Следование принципам ООП**  
    Программирование "через интерфейсы", а не через конкретные классы (FileReader).
3. **Универсальность**  
    Методы, принимающие `Reader`, работают с любым его наследником — `BufferedReader`, `StringReader`, и т.д.
4. **Поддерживаемость**  
    Если позже нужно изменить способ чтения — не надо менять объявление переменной.
    

---

### ❌ Почему не стоит писать так:

```java
FileReader reader = new FileReader("file.txt");
```

- Жёстко привязывает код к `FileReader`
    
- Уменьшает переиспользуемость
    
- Нельзя легко заменить тип на `BufferedReader` или другой
    

---

📌 **Вывод:**

> Использование `Reader reader = new FileReader(...)` — это **стандарт и хорошая практика**,  
> особенно для гибкого и читаемого кода.
