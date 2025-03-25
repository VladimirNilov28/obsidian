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
 
### 📌 Зачем использовать `BufferedWriter`?

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
```

### 👾Методы BufferedWriter
Метод `write(...)`:
- `write(...)` - пишет записывает один символ в буфер
- `write(char[] array)` - записывает символы из массива в буфер
- `write(String data)` - записывает строку в буфер

```java title:"Пример записи в буфер"
import java.io.FileWriter;
import java.io.BufferedWriter;

public class Main {
	public static void main(String[] args) {
		String data = "Some data for testing BifferedWritter class and his metods";
		
		try{
			FileWriter file = new FileWritter("output.txt");
			
			//без аргумента true файл будет перезаписан. Пример с аргументом true `new BufferedWriter(file, true)`
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

