---
создал заметку: 2025-03-21
tags:
  - java
  - file
  - handling
  - io
aliases:
  - File Handling
---
### Описание
File class является частью пакета java.io, используется для создания объектов класса и последующего указания имя файла
 - **File Handling** является неотъемлемой частью любого языка программирования, позволяет выполнят хранение выходных данных в файле и дальнейшие операции над ними
 - **File Handling** это чтение и запись данных в файл
```Java title:"Example"
// Importing File Class
import java.io.File;

class Geeks 
{
    public static void main(String[] args)
    {
        // File name specified
        File obj = new File("myfile.txt"); //объяв/созд файла
        System.out.println("File Created!");
    }
}
```

В Java концепция потока (Stream) используется для операций [[IO]] над файлом.

**Stream в Java** - последовательность данных или же поток. Этот концепт используется для [[IO]] операций над файлом. Ниже приведены виды потоков данных:
#### 1. Input Stream
**Input** **Stream** - супер класс в java для всего входящего потока. Входящий поток (InputStream) используется для чтения данных с входных устройств, например клавиатура, интернет и тп. InputStream это абстрактный класс и по этому его использование по сути бесполезно однако его подклассы используются для чтения данных.
Пример подклассов класса InputStream:
- AudioInputStream
- ByteArrayInputStream
- FileInputStream
- FilterInputStream
- StringBufferInputStream
- ObjectInputStream

```Java title:"Объявление InputStream"
InputStream obj = new FileInputStream();
```

PS: Можно создать входящий поток из другого подкласса а так же из InputStream

Стандартные Методы InputStream: 
![[Pasted image 20250321210745.png]]

#### 3. OutputStream 
выходной поток используется для написания данных в различные устройства такие как монитор, файл и тп. OutputStream это абстрактный супер класс который представляет выходной поток. OutputStream это абстрактный класс и по этому его использование по сути бесполезно. Однако его подклассы используются для записи данных.

Пример подклассов:
- ByteArrayOutputStream
- FileOutputStream
- StringBufferOutputStream
- ObjectOutputStream
- DataOutputStream
- PrintStream

```java title:"Объявление OutputStream"
OutputStream obj = new FileOutputStream();
```

PS: PS: Можно создать входящий поток из другого подкласса а так же из OutputStream

Стандартные методы OutputStream
![[Pasted image 20250321213043.png]]

В зависимости от типа данных, есть два типа потока:
1. Byte Stream - этот поток используется для чтения или записи байтовых данных. Байтовой поток также подразделён на два типа:
	- Byte Input Stream: Для чтения байтовых данных с различных устройств
	- Byte Output Stream: Для записи байтовых данных в различные устройства
2. Character Stream - этот поток используется для чтения или записи символьных данных. Поток символьных данных так же разделён на два типа: 
	 - Character Input Stream: Нужен для чтения символьных данных с различных устройств
	 - Character Output Stream: Нужен для записи символьных данных в различные устройства

