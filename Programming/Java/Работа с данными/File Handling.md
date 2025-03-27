---
создал заметку: 2025-03-21
tags:
---


## 📂 Java File Class Methods

Ниже приведена таблица методов класса `File`:  
![[Pasted image 20250321214326.png]]

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

### 2️⃣ Write to a File (FileWriter class)
Используем класс FileWriter. Иерархия этого класса происходит таким образом:
```java
// Writer — базовый абстрактный класс для записи символов
public abstract class Writer implements Appendable, Closeable, Flushable {}

// OutputStreamWriter расширяет Writer и работает с байтовыми потоками
public class OutputStreamWriter extends Writer {}

// FileWriter расширяет OutputStreamWriter и пишет символы в файл
public class FileWriter extends OutputStreamWriter {}

```

- Используем класс `FileWriter` и метод `write()` для записи текста.

```java

/*
Для объвления FileWriter используем тип переменной Writer.
Почему объявляем таким образом см. -> [[Writer в объявлении класса]]
*/

import java.io.FileWriter;
import java.io.IOException;

public class WriteFile {
    public static void main(String[] args) {
        try {
            Writer writer = new FileWriter("myfile.txt");
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

### 3️⃣ Read from a File (см FileReader ниже)

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

### ✍️ Запись в конец файла (режим `append`)!!!

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
### 📙 FileReader

FileReader это абстрактный класс, который используется для чтения символов из текстовых файлов. Как и любой другой наследник класса Reader реализует все абстрактные методы, такие как: `read(),` `close()`.

Основные принципы:
- Класс для чтения текстовых файлов посимвольно.
- Является подклассом [[Stream| InputStreamReader]] и работает с символами, а не байтами
- Подходит для простого чтения файлов, особенно в сочетании с [[Buffering| BufferedReader]]
- Использует стандартную кодировку системы

Иерархия этого класса происходит таким образом:
```java
//Input Stream наследует класс Reader
public class InputStreamReader extends Reader {}

//FileReader в свою очередь наследует InputStreamReader
public class FileReader extends InputStreamReader {}
```

#### 1. Чтение одного символа 
```java title:"Characters one by one reader class example"
/*
Для реализации FileReader лучше использовать [[Reader в объявлении|связку с Reader]]
для большей гибкости программы. Программа получает уже созаднный объект reader.
Для гибкости передаётся в параметрах Reader reader
Может выбоосить исключение IOException
(class FileReaderExample)*
*/
public static String readAllCharactersOneByOne(Reader reader) throws IOException {
	StringBuilder content = new StringBuilder();
	int nextChar;
	//чтение по одному символу и передача его в content
	while((nextChar = reader.read()) =! -1) {
		content.append((char) nextChar);
	}
	//возвращает собранную строку из прочитанных символов
	return String.valueOf(content);
}
```

```java title:"previous class realisation"
@Test
public void givenFileReader_whenReadAllCharacters_thenReturnsContent() throws IOException {
    String expectedText = "Hello, World!";
    File file = new File(FILE_PATH);
    try (Reader reader = new FileReader(file)) {
	    String content = FileReaderExample.readAllCharactersOneByOne(reader);
	    Assert.assertEquals(expectedText, content);
    }
}
```

#### 2. Чтение массива символов
Можно так же читать множество символов за раз используя унаследованный метод `read(char cbuf[], int off, int len` 
```java
/*
read(char[] cbuf, int off, int len) где cbuf массив символов куда будет
записан результат, off с какого индекаса начать записывать,
len сколько символов пытаеться прочитать
----------------------------------------------------------------------------------------------------------
1. Создаёт **массив `char[]`** нужной длины (например, 5 символов).
2. Метод `reader.read(...)` пытается прочитать символы **в массив**.
3. Если **успешно** — возвращает строку из этих символов. 
4. Если **достигнут конец файла (EOF)** — метод возвращает `-1`, и тогда возвращается пустая строка `""`.
*/
public static String readMultipleCharacters(Reader reader, int length) throws IOException {
    char[] buffer = new char[length]; // создаём массив символов нужной длины
    int charactersRead = reader.read(buffer, 0, length); // читаем символы в массив

    if (charactersRead != -1) {
        return new String(buffer, 0, charactersRead); // создаём строку из массива
    } else {
        return ""; // если ничего не прочиталось (достигнут конец файла)
    }
}
```

```java
/*
1. Создаётся файл `file.txt`, в котором, допустим, написано `"Hello"`.  
2. Метод `readMultipleCharacters(...)` вызывается с `length = 5`. 
3. Ожидается, что результат будет строка `"Hello"`. 
4. С помощью `Assert.assertEquals(...)` сравнивается результат и ожидаемый текст.
*/
@Test
public void givenFileReader_whenReadMultipleCharacters_thenReturnsContent() throws IOException {
    String expectedText = "Hello";
    File file = new File(FILE_PATH);
    //Так же объявляем через [[Reader в объявлении|Reader reader]] из отличной гибкости и удобсва
    try (Reader reader = new FileReader(file)) {
        String content = FileReaderExample.readMultipleCharacters(reader, 5);
        Assert.assertEquals(expectedText, content);
    }
}
```

⬇️⬇️⬇️⬇️
[[Buffering]]
