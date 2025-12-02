# Объяснение 3 проекта по Дата сайинсу!!!
Этот проект связан связан с освоением новой главы Python - ООП навыками. ООП (Обьектно-Ориентированное Программирование) в отличии от структурного программирования, разделяет код не только на различные функции программы, но и на классы и объекты в них. Это очень важная тема, т. к. она позволяет работать вам программистам декомпозировать программу очень досконально, и попадать к разным частым кода очень легко, например, у нас есть класс, отвечающий за работу с файлами, а в нем функции, создание файла, удаление, редактирование, переименовывание и т. д. Таким образом ООП позволяет обьединять функции в один единый класс и легко их вызывать.

### Инструкции для этого проекта
> ПЕРЕД ВЫПОЛНЕНИЕМ ЗАДАНИЙ, НАСТОЯТЕЛЬНО РЕКОМЕНДУЮ ОЗНАКОМИТЬСЯ С ЭТОЙ ЧАСТЬЮ, ТАК КАК ИЗ-ЗА ЭТОГО, РАБОТА МОЖЕТ БЫТЬ НЕЗАВАЛИДИРОВАНА!!!
1. НИКАКОЙ КОД В ГЛОБАЛЬНОЙ ЧАСТИ, используй функции!
2. Каждый файл и каждая программа ДОЛЖНА содержать в себе подобие этого фрагмента в конце кода:

```python
if __name__ == '__main__':
    # Тут могут быть твои тесты и обработка ошибок
```

3. Каждое исключение (ошибка), которое не было предусмотрено приведет к полному провалу всего проекта, даже если этого на спрашивали в заданиях.
4. Не должно быть никакого импорта сторонних библиотек, за исключением тех которые указаны в самом задании и помечены как `разрешенные библиотеки`
5. Все встроенные функции без импорта разрешены.

---

### Exercise 00. Простой класс
>- Turn-in directory: `ex00/`.
>- Files to turn in: `first_class.py`.
>- Allowed functions: all imports are restricted.
>
>This will be an easy warm-up exercise to introduce you to object-oriented programming in Python.
>
>1. Create a Python script called `first_class.py` that contains a class called Must_Read. This class reads the `data.csv` file and prints it. You can hardcode the name of the CSV file inside the class. Place the `print()` function inside your class. You will learn about methods and constructors later; for now, forget about them.
>2. `data.csv` contains the following data. You can create the file however you want.
>
>    ```
>    head,tail
>    0,1
>    1,0
>    0,1
>    1,0
>    0,1
>    0,1
>    0,1
>    1,0
>    1,0
>    0,1
>    1,0
>    ```
> 
>Example of launching the script:
>
>    ```
>    $ python3 first_class.py
>    head,tail
>    0,1
>    1,0
>    0,1
>    1,0
>    0,1
>    0,1
>    0,1
>    1,0
>    1,0
>    0,1
>    1,0
>    ```

В этом задании нам нужно написать простой класс, который будет выводить данные из `data.csv` и выводить их в `stdout`

Вот содержимое файла `first_class.py`:

```python
class Must_Read:
    file = open('data.csv', 'r')
    content = file.read()
    print(content, end='')
    file.close()


def main():
    try:
        Must_Read()
    except FileNotFoundError as e:
        print(f"Error: {e}")
    except Exception as e:
        print(f"An error occurred: {e}")


if __name__ == '__main__':
    main()
```

---

### Exercise 01. Метод
>- Turn-in directory: `ex01/`.
>- Files to turn in: `first_method.py`.
>- Allowed functions: all imports are is restricted.
>
>In the previous exercise, you created a class. Honestly, nobody creates classes like that in real life. Classes typically group together functions with similar topics and parameters. This is a better way to organize them. In this case, the functions are called methods.
>
>1. For this exercise, move the code from the body of the class to the `file_reader()` method. Methods are like functions in that they can return something. Classes cannot do that. Therefore, replace `print()` with `return()` in the method. Change the name of the class to "Research."
>2. The script must still have the exact same behavior. It needs to display the contents of the `data.csv` file. Save the script as `first_method.py`.

В этом задании надо написать наш первый метод в нашем уже созданном классе и переместить код в тебе класса в код. 

Вот содержимое файла `first_method.py`:

```python
class Research:
    def file_reader(self):
        file = open('data.csv', 'r')
        content = file.read()
        file.close()
        return content


def main():
    try:
        research = Research()
        result = research.file_reader()
        print(result, end='')
    except FileNotFoundError as e:
        print(f"Error: {e}")
    except Exception as e:
        print(f"An error occurred: {e}")


if __name__ == '__main__':
    main()
```

---

### Exercise 02. Котнструктор

>- Turn-in directory: `ex02/`.
>- Files to turn in: `first_constructor.py`.
>- Allowed functions: `import sys`, `import os`.
>
>Hardcoding the file name in the method was not a good idea. Ideally, we would be able to provide the file path as a script parameter. It would also be great if we did not have to include the path in every method we bring in later. Fortunately, there is a solution. Python classes can have a constructor: `__init__()`. This method runs first when an instance of a class is created.
>
>Modify your code as follows:
>1. Inside the Research class, create an `__init__()` method that takes the path to the file to be read as an argument.
>2. Modify the `file_reader()` method. This method does almost the same thing as in the previous exercise — it just reads the file and returns its data. The difference is that the path to the file should be used from the `__init__()` method.
>3. If a file with a different structure is given and your program cannot read it, raise an exception. The correct file contains a header with two strings separated by a comma. One or more lines follow that contain either 0 or 1, but never both, and are delimited by a comma.
>4. Modify the main program. The script must still behave exactly the same. The path to the file should be provided as an argument to the script. The script should display the contents of the `data.csv` file. Save the script as `first_constructor.py`.
>
>Example of launching the script:
>```
>$ python3 first_constructor.py data.csv
>head,tail
>0,1
>1,0
>0,1
>1,0
>0,1
>0,1
>0,1
>1,0
>1,0
>0,1
>1,0

В этом задании нам нужно построить наш первый конструктор в классе.

Вот содержимое файла `first_constructor.py`

```python
import sys


class Research:
    def __init__(self, path):
        self.path = path
        self._validate_file()
    
    def _validate_file(self):
        try:
            with open(self.path, 'r') as file:
                lines = file.readlines()
                
                if len(lines) < 2:
                    raise ValueError("File must have a header and at least one data line")
                
                header = lines[0].strip()
                header_parts = header.split(',')
                if len(header_parts) != 2:
                    raise ValueError("Header must contain exactly two strings separated by a comma")
                
                for i, line in enumerate(lines[1:], start=2):
                    line = line.strip()
                    if not line:
                        continue
                    
                    parts = line.split(',')
                    if len(parts) != 2:
                        raise ValueError(f"Line {i} must contain exactly two values separated by a comma")
                    
                    val1, val2 = parts[0].strip(), parts[1].strip()
                    
                    if val1 not in ['0', '1'] or val2 not in ['0', '1']:
                        raise ValueError(f"Line {i} must contain only 0 or 1 values")
                    
                    if val1 == val2:
                        raise ValueError(f"Line {i} cannot have both values be the same (both 0 or both 1)")
        
        except FileNotFoundError:
            raise FileNotFoundError(f"File '{self.path}' not found")
        except Exception as e:
            if isinstance(e, (ValueError, FileNotFoundError)):
                raise
            raise ValueError(f"Error reading file: {e}")
    
    def file_reader(self):
        with open(self.path, 'r') as file:
            content = file.read()
        return content


def main():
    try:
        if len(sys.argv) != 2:
            print("Usage: python3 first_constructor.py <file_path>")
            return
        
        file_path = sys.argv[1]
        research = Research(file_path)
        result = research.file_reader()
        print(result, end='')
    
    except FileNotFoundError as e:
        print(f"Error: {e}")
    except ValueError as e:
        print(f"Error: {e}")
    except Exception as e:
        print(f"An error occurred: {e}")


if __name__ == '__main__':
    main()
```

---

### Exercise 03. Вложенный класс
