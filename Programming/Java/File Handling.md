---
создал заметку: 2025-03-21
tags:
---
## Java File Class Methods
Ниже приведена таблица методов класса File:
![[Pasted image 20250321214326.png]]

## File Operations
Ниже приведены операции которые могут быть проведены над файлом в java:
- Create a File
- Read from a File
- Write to a File
- Delete a File

1. Create a File
	- Для создания файла в Java, ты можешь использовать метод createNewFile()
	- Если файл успешно создан будет возвращено Boolean значение true или false если файл уже существует
```java title:"Example"
import java.io.File;
import java.io.IOException;

public class CreateFile {
	public static void main(String[] args) {
		try {
			File obj = new File("myfile.txt"); //путь к файлу, не создание!!!
			//проверка есть ли файл или нет, если нет то создаёт новый
			if (obj.createNewFile()) {
				System.out.println("File created: " + Obj.getName());
			} else {
				System.out.println("File already exists.");
			}
		}
		catch (IOException e) {
			System.out.println("An error has occurred.");
            e.printStackTrace();
		}
	}
}
```
2. Write to a File
	- **FileWriter** используем вместе с его методом write() для написания текста в файл
```java title:"Example"
// Writing Files using Java Program

// Import the FileWriter class
import java.io.FileWriter;
import java.io.IOException; 

public class WriteFile 
{
    public static void main(String[] args)
    {
        // Writing Text File also
        // Exception Handling
        try {

            FileWriter Writer = new FileWriter("myfile.txt");

            // Writing File
            Writer.write("Files in Java are seriously good!!");
            Writer.close();
            
            System.out.println("Successfully written.");
        }

        // Exception Thrown
        catch (IOException e) {
            System.out.println("An error has occurred.");
            e.printStackTrace();
        }
    }
}

```

3. Read from a File
	- будем использовать Scanner для чтения содержимого файла

```java title:"Example"
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class ReadFile {
	public static void main(String[] args) {
		try {
			File obj = new File("myfile.txt");
			Scanner scanner = new Scanner(obj); //передаём File obj в Scanner для чтения из файла
			
			while (scanner.hasNextLine()) {
				String data = Reader.nextLine(); 
				System.out.println("data");
			}
			Reader.close();
		}
		catch (FileNotFoundException e){
			System.out.println("An error has occurred.");
            e.printStackTrace();
		}
	}
}
```

4. Delete a file 
	- delete() для удаления файла

```java title:"Example"
	// Deleting File using Java Program
import java.io.File; 

public class DeleteFile 
{
    public static void main(String[] args)
    {
        File Obj = new File("myfile.txt");
        
        // Deleting File
        if (Obj.delete()) {
            System.out.println("The deleted file is : " + Obj.getName());
        }
        else {
            System.out.println(
                "Failed in deleting the file.");
        }
    }
}

```



