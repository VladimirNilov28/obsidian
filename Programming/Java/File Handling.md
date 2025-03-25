---
создал заметку: 2025-03-21
tags:
---
## ✅ Что круто:

- Четко выделены **основные операции с файлами**
- Даёшь **понятные комментарии** к коду
- Используешь **реальные Java-примеры**, пригодные для копипаста
- Указываешь **ошибки через `try-catch`**, а не пропускаешь их

---

## 🛠 Исправления и улучшения:

### 1. Опечатки:

- `Obj.getName()` → должно быть `obj.getName()` (Java чувствительна к регистру)
- `Reader.nextLine()` и `Reader.close()` → должно быть `scanner.nextLine()` и `scanner.close()`
- `System.out.println("data");` → это просто текст, а не переменная `data`

---

## 📚 Обновлённый и аккуратный вариант:

---

## 📂 Java File Class Methods

Ниже приведена таблица методов класса `File`:  
![[Pasted image 20250321214326.png]]

> (Вставь изображение таблицы методов сюда, если нужно — могу помочь сделать Markdown-таблицу)

---

## 🧰 File Operations

Основные операции, которые можно выполнять с файлами в Java:

- Создание файла
- Запись в файл
- Чтение из файла
- Удаление файла

---

### 1️⃣ Create a File

- Для создания файла используем метод `createNewFile()`.
- Возвращает `true`, если файл создан, и `false`, если файл уже существует.

```java
import java.io.File;
import java.io.IOException;

public class CreateFile {
    public static void main(String[] args) {
        try {
            File obj = new File("myfile.txt"); // путь к файлу, не создаёт сразу
            if (obj.createNewFile()) {
                System.out.println("File created: " + obj.getName());
            } else {
                System.out.println("File already exists.");
            }
        } catch (IOException e) {
            System.out.println("An error has occurred.");
            e.printStackTrace();
        }
    }
}
```

---

### 2️⃣ Write to a File

- Используем класс `FileWriter` и метод `write()` для записи текста.

```java
import java.io.FileWriter;
import java.io.IOException;

public class WriteFile {
    public static void main(String[] args) {
        try {
            FileWriter writer = new FileWriter("myfile.txt");
            writer.write("Files in Java are seriously good!!");
            writer.close();
            System.out.println("Successfully written.");
        } catch (IOException e) {
            System.out.println("An error has occurred.");
            e.printStackTrace();
        }
    }
}
```

---

### 3️⃣ Read from a File

- Используем `Scanner` для чтения содержимого.

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class ReadFile {
    public static void main(String[] args) {
        try {
            File obj = new File("myfile.txt");
            Scanner scanner = new Scanner(obj);
            
            while (scanner.hasNextLine()) {
                String data = scanner.nextLine();
                System.out.println(data);
            }
            scanner.close();
        } catch (FileNotFoundException e) {
            System.out.println("An error has occurred.");
            e.printStackTrace();
        }
    }
}
```

---

### 4️⃣ Delete a File

- Используем метод `delete()` для удаления файла.

```java
import java.io.File;

public class DeleteFile {
    public static void main(String[] args) {
        File obj = new File("myfile.txt");
        
        if (obj.delete()) {
            System.out.println("The deleted file is: " + obj.getName());
        } else {
            System.out.println("Failed to delete the file.");
        }
    }
}
```


---

### ✍️ Запись в конец файла (режим `append`)

По умолчанию `FileWriter` **перезаписывает** содержимое файла.  
Чтобы **добавить текст в конец файла**, нужно передать вторым аргументом `true` в конструктор:

```java
FileWriter writer = new FileWriter("myfile.txt", true);
```

---

### ✅ Пример: запись с `append = true`

```java
import java.io.FileWriter;
import java.io.IOException;

public class AppendToFile {
    public static void main(String[] args) {
        try {
            FileWriter writer = new FileWriter("myfile.txt", true); // true = режим добавления
            writer.write("\nНовая строка, добавленная в конец файла.");
            writer.close();
            System.out.println("Successfully appended to the file.");
        } catch (IOException e) {
            System.out.println("An error has occurred.");
            e.printStackTrace();
        }
    }
}
```

---

### 📌 Что важно помнить:

- `true` → включает режим добавления
- `\n` → вручную добавляет перенос строки (если нужно)

---
[[BUFFEREDREADER AND BUFFEREDWRITER]]