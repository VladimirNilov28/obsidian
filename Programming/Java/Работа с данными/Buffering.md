---
создал заметку: "2025-03-25"
tags: []
---
```java
import java.io.BufferedWriter
```

**BufferedWriter** — это класс в Java, который **оборачивает** другой объект записи (например, `FileWriter`) и **сохраняет данные во внутреннем буфере**, чтобы **уменьшить количество операций записи на диск**.

**BufferedReader** — это класс для **эффективного построчного чтения** текста из файла (или другого потока ввода).  
Он **оборачивает** объект `Reader` (например, `FileReader`) и читает данные во **внутренний буфер**, чтобы уменьшить количество обращений к файлу.


 
### 📌 Зачем использовать `BufferedWriter`? (писалось давно так что не забываем оборачивать FileWriter в BufferedWriter)

- **Быстрее**, чем `FileWriter` напрямую, особенно при записи больших объёмов текста.
- Можно записывать **построчно** с методом `newLine()`.
- Хорошо работает в паре с `FileWriter`, `OutputStreamWriter` и т.д.

### ⚙️Объявление BufferedWriter
BufferedReader может работать в паре с `FileWriter` ил чем то похожим. Для объявления BufferedWriter требуется объявить FileWriter.
```java title:"example"
//Creates a FileWriter
FileWriter file = new FileWriter(String name);

//Creates a BufferedWriter
BufferedWriter buffer = new BufferedWriter(file); 

/*
Замечание. Если не нужна работа с путями файлов то достаточно объявить 
`FileWriter` с путём до файла, а если нужна проверка файла или работа с путями
то следует объявлять `File file = new File(String name);`

Можно передать размер буфера в агрументах если нужно - `new BufferedWritter(file, int size)`. По дефолту 8192 символа
*/

//еще один способ объявления более правильный (см [[Writer в объявлении класса]])
Writer writer = new BufferedWriter(new FileWriter("file.txt", true));


```

### 👾Методы BufferedWriter
Метод `write(...)`:
- `write(...)` - записывает один символ в буфер
- `write(char[] array)` - записывает символы из массива в буфер
- `write(String data)` - записывает строку в буфер

```java title:"Пример записи в буфер"
import java.io.FileWriter;
import java.io.BufferedWriter;

public class Main {
	public static void main(String[] args) {
		String data = "Some data for testing BifferedWritter class and his metods";
		
		try{
			//без аргумента true файл будет перезаписан. Пример с аргументом true `new FileWriter(file, true)`
			FileWriter file = new FileWritter("output.txt");
			BufferedWriter output = new BufferedWriter(file);
			
			output.write(data);
			output.close();
		}
		
		catch (Exception e) {
			e.getStackTrace();
		}
	}
}
```

Метод `flush()`:
- Нужен для очистки текущего буфера. Этот метод заставляет записать в файл все текущие данные в буфере в файл назначения
- Нужен кода ты не закрывает поток сразу( например продолжаешь писать) а нужно освободить память в буфере
- Нужен если ты пишешь файл по частям или в цикле но не хочешь закрывать поток каждый раз

```java title:"example"
import java.io.FileWriter;
import java.io.BufferedWriter;

public class Main {
  public static void main(String[] args) {

    String data = "This is a demo of the flush method";

    try {
      
      FileWriter file = new FileWriter("flush.txt");
      BufferedWriter output = new BufferedWriter(file);
      
      output.write(data);
      
      // Flushes data to the destination
      output.flush();
      
      System.out.println("Data is flushed to the file.");

      output.close();
    }

    catch(Exception e) {
      e.printStackTrace();
    }
  }
}

```

Метод `close()`
- закрывает поток BufferedWriter записывая все данные в файл назначения

```java title:"example"
FileWriter file = new FileWriter("flush.txt");
BufferedWriter output = new BufferedWriter(file);

output.write(data);
output.close();
```

Метод `BufferedWriter.newLine()`

- Добавляет **новую строку** в файл (эквивалент нажатия Enter).
- Автоматически подставляет **правильный символ перевода строки** в зависимости от операционной системы:
    - Windows → `\r\n`
    - Linux/macOS → `\n`

---

### ✅ Пример:

```java
BufferedWriter writer = new BufferedWriter(new FileWriter("file.txt"));
writer.write("Привет");
writer.newLine(); // переход на новую строку
writer.write("Мир");
writer.close();
```

📌 Результат в файле:

```
Привет
Мир
```

---

> Используй `newLine()` вместо `\n` для **кроссплатформенности** и читаемости.

### 📌 Зачем использовать `BufferedReader`?

BufferedReader - это класс который упрощает чтение текста. Он буферизует символы что бы обеспечить эффективное чтение текстовых данных.

Основыне фишки:
1. **Быстрее чтение** — читает не по символу, а **блоками** (буферизация).
2. **Метод `readLine()`** — удобно читать файл **построчно**.
3. **Кроссплатформенность** — корректно работает с разными кодировками (через `InputStreamReader`).
4. **Гибкость** — можно обернуть любой `Reader` (например, `FileReader`, `InputStreamReader`, `StringReader`). 

BufferedReader пригодится если мы хотим читать данные из любого источника ввода, будь то файл, сокет или что либо еще. Лёгкий ввод, позволяет минимизировать [[IO|I / O]] операции благодаря чтению блоками символов и хранением их во внутреннем буфере. Пока в буфере есть данные, reader будет читать данные из потока а не на прямую из директории.

### ⚙️Буферизация Reader
Как многие Java [[IO|I/O]] классы BufferedReader наследует шаблон Decorator, это заначит он требует Reader в своём конструкторе. Это даёт нам гибкость в расширении экземпляра Reader функциями буферизации.

```java title:"пример объявлении"
BufferedReader reader = new BufferedReader(new FileReader(FILE_PATH));
```

BufferedReader так же имеет удобные инструменты для чтения файлов построчно

### Буферизация [[Stream]]
В целом можно использовать BufferedReader для любого типа входящего потока данных.
```java title:"пример объявлении""
/*
Да можно испльзовать вместо Scаnner scanner, BufferedReader намного быстрее,
но лучше для большого потока данных.
Для простых операций по типу ввода имени пользователя
или ввода чисел лучше использовать Scanner. А так Scanner сосать!
*/

BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

```

Можно использовать любой тип входящих данных, вообще похуй.

### 😱 BufferedReader 🆚 Scanner

Как альтернативу можно использовать Scanner для достижения того же функционала как и с BufferedReader. 

Однако там есть значительные отличия между ними которые могут сделать их более или  менее удобными для нас.
- BufferedReader синхронизирован (потоко-безопасный) в то время как Scanner посасывает.
- Scanner может анализировать типы данных и строки, используя регулярные выражения.
- BufferedReader позволяет менять размер буфера в отличии от Scanner у него он фиксированный
- BufferedReader имеет больший стандартный размер буфера
- Scanner не выбрасывает IOException, в то время как BufferedReader требует его обработки
- BufferedReader быстрее чем Scanner потому что он читает данные без их анализа.

Для простых задач Scanner выглядит очень хорошо однако для построчного чтения BufferedReader сияет. 🕺
![[Pasted image 20250326223739.png ]]

### 📕 Чтение текста с использованием BufferedReader

Ща бля раскидаю как создаём, используем и правильно закрываем BufferedReader для чтение из текстового файла.
#### ❤️ Создание и настройка объекта BufferedReader
В первую очередь создаём BufferedReader с использованием BufferedReader(Reader) конструктора

```java 
BufferedReader reader = new BufferedReader(new FileReader(FILE_PATH));
```

Оборачивание FileReader  ахуенный способ добавить функцию буферизации

По дефолту буфер имеет размер 8 KB. Однако, если мы хотим использовать больший или меньший размер буфера можно использовать `BufferedReader(Reader, int)` конструктор.

```java title:"пример кастомного размера буфера в 16KB"
BufferedReader reader = new BufferedReader(new FileReader(FILE_PATH), 16312);
```

Оптимальный размер буфера варьируется в зависимости от типа входящего потока и железе на котором запущена программа поэтому оптимальный размер буфера находиться путём экспериментов.
Лучше использовать степени двойки в качестве размера буфера, так как большинство аппаратных устройств имеют размер блока, равный степени двойки.

Наконец то дошли до более удобного метода создания BufferedReader используя помощник класса [[Java Files|Files из API java.nio ]]

```java
/*
Создание BufferedReader таким способом — хороший вариант для чтения файла с буфером, 
потому что нам не нужно вручную создавать FileReader и оборачивать его в BufferedReader. см [[Java Files]]

*/

Reader reader = Files.newBufferedReader(Paths.get(FILE_PATH));
```

#### 📙 Чтение построчно 
Чтение содержание файла используя метод `nextLine()`
```java
public String readAllLines(Reader reader) throws IOExсeption {
	StringBuilder content = new StringBuilder(); //накопление строк
	String line
	
	//чтение построчно
	while ((line = reader.readLine()) != null) { //пока есть строки читаем каждую
		content.append(line);
		content.append(System.lineSeparator()); //добавление символа новой строки, типо \n
	}
    
    return content.toString();
}
```

```java title:"lines method Java 8"
//более компактный вариант но не доступен на прямую через Reader, требуеться BufferedReader
public String readAllLinesWithStream(BufferedReader reader) {
    return reader.lines() //аналог readLine() в потоке, восвращает Stream строк
			    //собирает все в одну строку и добавляет перевод строки
                 .collect(Collectors.joining(System.lineSeparator())); 
}
```

#### 🌙 Завершение Потока
После использования BufferedReader мы должны закрывать поток. Лучше всего это делать автоматически с помощью **try-with-resources** 

```java
//try-with-reusources автоматически закрывает поток по окончанию выражения
try (BufferedReader reader = new BufferedReader(new FileReader(FILE_PATH))) {
	return readAllLines(reader);
}
```

#### Другие полезные методы
1. Чтение одиночного символа (char) через метод `read(...)`

```java
public String readAllCharactersOneByOne(BufferedReader reader) throws IOException {
	StringBuilder content = new StringBuilder();
	
	int value;
	while ((value = reader.read()) != -1) {
		content.append((char) vlaue);
	}
	
	reuturn content.toString();
}
/*
Считывает значения в виде ASCII-значений. Записывает в StringBuilder преабразовывая в char
Повторяется до конца потока, который опр. значением -1, возвращаемым методом read()
*/
```

2. Чтение множества символов, похоже на чтение через FileReader используя унаследованный метод `read(char cbuf[], int off, int len` 

```java
public String readMultipleChars(BufferedReader reader) throws IOException {
	int lenght;
	char[] chars = new char[lenght];
	int charsRead = reader.read(chars, 0, charsRead);
	
	if (charsRead != -1) {
		String result = new String(chars, 0, charsRead);
	} else {
		return "";
	}
	return result
}
```

3. Пропуск символов с использованием метода `skip(long n)` 

```java
@Test
public void givenBufferedReader_whensSkipChars_thenOk() throws IOException {
    StringBuilder result = new StringBuilder();

    try (BufferedReader reader = 
           new BufferedReader(new StringReader("1__2__3__4__5"))) {
        int value;
        while ((value = reader.read()) != -1) {
            result.append((char) value);
            reader.skip(2L);
        }
    }

    assertEquals("12345", result);
}
```

4.  mark и reset - методы`mark(int readAheadLimit)` и `reset()` которые мы можем использовать в определённой позиции потока что бы вернуться к ней позже. В прмере используем mark и reset для игнорирования пустого пространства в начале и конце потока.
```java
@Test
public void givenBufferedReader_whenSkipsWhitesspacesAtBeginning_thenOk() throws IOException {
	String result;
	f
	try (BufferedReader reader = new BufferedReader(new StringReader("     Lorem ipsum dolor sit amet."))) {
		do {
			reader.mark(1);
		} while (Character.isWhitespace(reader.read())) {
		
		reader.reset();
		result = reader.readLine();
	}
	assertEquals("Lorem ipsum dolor sit amet.", result);
}
```

