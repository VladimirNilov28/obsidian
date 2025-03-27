---
создал заметку: "2025-03-26"
tags: []
---
## 📄 **Java `Files` – шпаргалка + зачем это удобно**

Класс `Files` из `java.nio.file` предоставляет **простые, мощные и современные методы** для работы с файлами и папками.

---

### 📥 **Чтение файлов**

```java
List<String> lines = Files.readAllLines(Path path);
String text = Files.readString(Path path);
BufferedReader reader = Files.newBufferedReader(Path path);
```

✅ **Преимущества**:

- Меньше кода — всё в одну строку
- Удобно читать файлы целиком или построчно
- Работает с `Path` вместо `String`, что безопаснее

---

### 📤 **Запись в файлы**

```java
Files.write(Path path, List<String> lines);
Files.writeString(Path path, String text);
BufferedWriter writer = Files.newBufferedWriter(Path path);
```

✅ **Преимущества**:

- Легко записывать как строки, так и списки
- Упрощает работу с текстовыми файлами
- Можно указать режим дозаписи и кодировку
---

### 📂 **Работа с файлами и директориями**

```java
Files.exists(Path path);
Files.createFile(Path path);
Files.createDirectory(Path path);
Files.delete(Path path);
```

✅ **Преимущества**:

- Всё делается через единый класс `Files`
- Удобно проверять, создавать и удалять объекты
- Поддержка абсолютных и относительных путей

---

### 💡 **Почему это удобно:**

1. **Краткость** — меньше шаблонного кода
2. **Современный API** — `Path` надёжнее, чем `String`
3. **Гибкость** — можно работать с кодировками, правами доступа, буферизацией
4. **Безопасность и читаемость** — лучше для поддержки и тестирования