# Объяснение 2 проекта по Дата сайнсу!!!
Этот проект связан с синтаксисом и семантикой в Python (Семантика - это понимание смысла программы, синтаксис - это правила написания кода).

---

### Exercise 00. Типы данных
> - Turn-in directory: `ex00/`.
> - Files to turn in: `data_types.py`.
> - Allowed functions: any import is restricted.
> 
> Like any other language, Python has several built-in data types. In this exercise, you will become familiar with the most popular and useful ones.
> 
> 1. Create a script called `data_types.py` and define a `data_types()` function. In this function, declare 8 variables with different types and print their types to the standard output.
>2. You must reproduce the following output exactly:
>   
>        > python3 data_types.py
>        [int, str, float, bool, list, dict, tuple, set]
>
>3. Do not write the data types explicitly in your print. Remember to call your function at the end of your script, as explained in today's instructions:
>    
>        if __name__ == '__main__':
>            data_types()
>    
>4. Put your file in the `ex00` folder in the `src` directory of your repository.

Итак, для выполнения этого задания нам нужно вывести список `[int, str, float, bool, list, dict, tuple, set]` с условием, не писать типы данных полностью в своем принте. Для этого у нас есть возможность узнавать тип переменных через метод `type(a).__name__`.

Во первых, создадим переменные для нашего задания `int`, `str`, `float`, `bool`, `list`, `dict`, `tuple`, `set`. Потом создадим список из методов извлечения назавния типа для каждой переменной. А потом можно просто вывести этот список.

Вот код из файла `data_tupes.py`:
```python
def data_types():
    
    var_int = 42
    var_str = "Hello"
    var_float = 3.14
    var_bool = True
    var_list = [1, 2, 3]
    var_dict = {"key": "value"}
    var_tuple = (1, 2, 3)
    var_set = {1, 2, 3}
    
    types = [
        type(var_int).__name__,
        type(var_str).__name__,
        type(var_float).__name__,
        type(var_bool).__name__,
        type(var_list).__name__,
        type(var_dict).__name__,
        type(var_tuple).__name__,
        type(var_set).__name__
    ]
    
    print(types)


if __name__ == '__main__':
    data_types()

```

---

### Exercise 01. Работа с файлами
>- Turn-in directory: `ex01/`.
>- Files to turn in: `read_and_write.py`.
>- Allowed functions: any import is restricted.
>
>For this exercise, feel free to define as many functions as you need and name them whatever you like. In the attached file `ds.csv`, which you may recognize from the previous day, there are several columns separated by commas containing different data about vacancies.
>
>1. Design a Python script called `read_and_write.py` that opens the file [ds.csv](https://drive.google.com/file/d/1tDEDTytYaUrfJsXD5z5QvJSb5VNlL-eZ/view), reads the data it contains, replaces all the comma delimiters with "\t", and saves the data to a new file `ds.tsv`. Be careful, your data may contain commas. If you replace them, you will corrupt the data.
>2. Put your script in the `ex01` folder in the `src` directory of your repository.

Тут надо просто сконвертировать файл `.csv` в `.tsv` заменяя запятые на табуляции, однако надо не забывать про то, что в самих данных тоже есть запятые, поэтому не забываем их экранировать.

Вот код из файла `read_and_write.py`:
```python
def parse_csv_line(line):
    fields = []
    current_field = []
    in_quotes = False
    i = 0
    
    while i < len(line):
        char = line[i]
        
        if char == '"':
            if in_quotes and i + 1 < len(line) and line[i + 1] == '"':
                # Escaped quote (double quote)
                current_field.append('"')
                i += 2
                continue
            else:
                # Toggle quote state
                in_quotes = not in_quotes
                i += 1
                continue
        
        if char == ',' and not in_quotes:
            # Field delimiter found
            fields.append(''.join(current_field))
            current_field = []
            i += 1
            continue
        
        current_field.append(char)
        i += 1
    
    # Add the last field
    fields.append(''.join(current_field))
    
    return fields


def read_csv_file(filename):
    rows = []
    
    with open(filename, 'r', encoding='utf-8') as file:
        for line in file:
            line = line.rstrip('\n\r')
            if line:
                fields = parse_csv_line(line)
                rows.append(fields)
    
    return rows


def write_tsv_file(filename, rows):
    with open(filename, 'w', encoding='utf-8') as file:
        for row in rows:
            file.write('\t'.join(row) + '\n')


def main():
    input_file = 'ds.csv'
    output_file = 'ds.tsv'
    
    data = read_csv_file(input_file)
    
    write_tsv_file(output_file, data)
    
    print(f"Successfully converted {input_file} to {output_file}")


if __name__ == "__main__":
    main()
```

---
### Exercise 02. Поиск по ключу
>- Turn-in directory: `ex02/`.
>- Files to turn in: `stock_prices.py`.
>- Allowed functions: `import sys`.
>
>1. You have the following dictionaries to copy to one of your functions:
>
>        COMPANIES = {
>        'Apple': 'AAPL',
>        'Microsoft': 'MSFT',
>        'Netflix': 'NFLX',
>        'Tesla': 'TSLA',
>        'Nokia': 'NOK'
>        }
>
>        STOCKS = {
>        'AAPL': 287.73,
>        'MSFT': 173.79,
>        'NFLX': 416.90,
>        'TSLA': 724.88,
>        'NOK': 3.37
>        }
>
>2. Write a program that takes the name of a company (e.g., Apple) as an argument and displays its stock price (e.g., 287.73) on the standard output. If the program is given a company name that is not in the dictionary, it should display "Unknown company." If no arguments are provided, or if too many arguments are provided, your program should do nothing and quit.
>
>        $> python3 stock_prices.py tesla
>        724.88
>        $> python3 stock_prices.py Facebook
>        Unknown company
>        $> python3 stock_prices.py Tesla Apple

> Для выполнения этого задания нужно знать что такое словари в Python. 

В задании надо искать цены на акции, ввод будет название (надо игнорировать регистр), а вывод будет цена нашей акции, но, как мы видим, цена нашей акции в словаре STOCKS, а название компании в COMPANIES, поэтому нам надо будет переводить название в аббревиатуру, а только потом в цену.

Вот содержимое файла `stock_prices.py`:

```python
import sys


def get_stock_price():
    
    COMPANIES = {
        'Apple': 'AAPL',
        'Microsoft': 'MSFT',
        'Netflix': 'NFLX',
        'Tesla': 'TSLA',
        'Nokia': 'NOK'
    }

    STOCKS = {
        'AAPL': 287.73,
        'MSFT': 173.79,
        'NFLX': 416.90,
        'TSLA': 724.88,
        'NOK': 3.37
    }

    if len(sys.argv) != 2:
        return

    company_name = sys.argv[1]

    ticker = None
    for company, tick in COMPANIES.items():
        if company.lower() == company_name.lower():
            ticker = tick
            break

    if ticker and ticker in STOCKS:
        print(STOCKS[ticker])
    else:
        print("Unknown company")


if __name__ == "__main__":
    get_stock_price()
```

---

### Exercise 03. Поиск по значению и по ключу
> - Turn-in directory: `ex03/`.
> - Files to turn in: `ticker_symbols.py`.
> - Allowed functions: `import sys`.
>
>1. You will use the same two dictionaries as in the previous exercise. Copy them again using one of the functions in your script.
>2. Create a program that takes a ticker symbol (e.g., AAPL) and displays the company name and stock price, using a space as the delimiter. The program's behavior should be identical to that of the previous exercise.
>
>        $> python3 ticker_symbols.py tsla
>        Tesla 724.88
>        $> python3 ticker_symbols.py FB
>        Unknown ticker
>        $> python3 ticker_symbols.py TSLA AAPL

В этом задании нам нужно искать не только по ключу, но и по значению.

Вот сожержимое файла `ticker_symbols.py`:

```python
import sys

def get_ticker_info():

    COMPANIES = {
        'Apple': 'AAPL',
        'Microsoft': 'MSFT',
        'Netflix': 'NFLX',
        'Tesla': 'TSLA',
        'Nokia': 'NOK'
    }

    STOCKS = {
        'AAPL': 287.73,
        'MSFT': 173.79,
        'NFLX': 416.90,
        'TSLA': 724.88,
        'NOK': 3.37
    }

    if len(sys.argv) < 2:
        return

    for ticker in sys.argv[1:]:
        ticker_upper = ticker.upper()
        
        if ticker_upper in STOCKS:
            company_name = None
            for name, symbol in COMPANIES.items():
                if symbol == ticker_upper:
                    company_name = name
                    break
            
            if company_name:
                print(f"{company_name} {STOCKS[ticker_upper]}")
        else:
            print("Unknown ticker")

if __name__ == "__main__":
    get_ticker_info()
```

---

### Exercise 04. Словари
>- Turn-in directory: `ex04/`.
>- Files to turn in: `to_dictionary.py`.
>- Allowed functions: any import is restricted.
>
>1. Create a script named `to_dictionary.py` in one of the functions and for this function copy the following list of tuples as is:
>
>        list_of_tuples = [
>        ('Russia', '25'),
>        ('France', '132'),
>        ('Germany', '132'),
>        ('Spain', '178'),
>        ('Italy', '162'),
>        ('Portugal', '17'),
>        ('Finland', '3'),
>        ('Hungary', '2'),
>        ('The Netherlands', '28'),
>        ('The USA', '610'),
>        ('The United Kingdom', '95'),
>        ('China', '83'),
>        ('Iran', '76'),
>        ('Turkey', '65'),
>        ('Belgium', '34'),
>        ('Canada', '28'),
>        ('Switzerland', '26'),
>        ('Brazil', '25'),
>        ('Austria', '14'),
>        ('Israel', '12')
>        ]
>
>2. Your script should transform this variable into a dictionary, with the number as the key and the country as the value. As you can see, the same key may have several values. The script must display the dictionary's contents on standard output according to this precise formatting:
>
>        '132' : 'France'
>        '132' : 'Germany'
>        '178' : 'Spain'
>        ...
>
>3. Think about why the order is not necessarily identical to that of the example.

В этом задании нужно сделать конвертировать картеж в словарь (советую изучить и то и другое) и вывести их в формате отображенном в примере.

Вот содержимое файла `to_dictionary.py`:

```python
def to_dictionary():

    list_of_tuples = [
        ('Russia', '25'),
        ('France', '132'),
        ('Germany', '132'),
        ('Spain', '178'),
        ('Italy', '162'),
        ('Portugal', '17'),
        ('Finland', '3'),
        ('Hungary', '2'),
        ('The Netherlands', '28'),
        ('The USA', '610'),
        ('The United Kingdom', '95'),
        ('China', '83'),
        ('Iran', '76'),
        ('Turkey', '65'),
        ('Belgium', '34'),
        ('Canada', '28'),
        ('Switzerland', '26'),
        ('Brazil', '25'),
        ('Austria', '14'),
        ('Israel', '12')
    ]
    
    result_dict = {}
    
    for country, number in list_of_tuples:
        if number in result_dict:
            result_dict[number].append(country)
        else:
            result_dict[number] = [country]
    
    for key in result_dict:
        for country in result_dict[key]:
            print(f"'{key}' : '{country}'")


if __name__ == "__main__":
    to_dictionary()
```
Почему порядок может быть разным:

- В Python 3.7+ словари сохраняют порядок добавления, поэтому он зависит от времени первого добавления каждого ключа.
- Однако порядок стран в пределах одного ключа зависит от порядка их расположения в исходном списке.
- Разные версии или реализации Python могут обрабатывать это по-разному.
- Порядок отображения соответствует внутреннему порядку словаря, который основан на времени первого добавления ключей.

---

### Exercise 05. Поиск по значению или по ключу

>- Turn-in directory: `ex05/`.
>- Files to turn in: `all_stocks.py`.
>- Allowed functions: `import sys`.
>
>1. You still have those two dictionaries from `ex02`. And you should still copy them into one of your functions in the script.
>2. Write a program that exhibits the following behavior:
>    - The program should take a string containing many expressions separated by commas as an argument.
>    - For each expression in the string, the program should detect whether it is a company name, a ticker symbol, or neither.
>    - The program should not be case-sensitive, but it should be able to work with whitespaces.
>    - If there are no arguments or too many arguments, the program should display nothing.
>    - When there are two commas in a row in the string, the program should not display anything.
>    - The program must display the results separated by line breaks and use the following formatting:
>
>          $ python3 all_stocks.py 'TSLA , aPPle, Facebook'
>          TSLA is a ticker symbol for Tesla
>          Apple stock price is 287.73
>          Facebook is an unknown company or an unknown ticker symbol
>          $ python3 all_stocks.py 'TSLA,, apple'
>          $ python3 all_stocks.py 'TSLA, , apple'
>          $ python3 all_stocks.py TSLA AAPL

В этом задании нам нужно написать программу, которая может вывести цену, когда было введено название компании, название компании, когда будет введена аббревиатура компании. Еще надо предусмотреть ввод сразу нескольких компаний как в примере.

Вот содержимое `all_stocks.py`:

```python
import sys

def all_stocks():
    
    COMPANIES = {
        'Apple': 'AAPL',
        'Microsoft': 'MSFT',
        'Netflix': 'NFLX',
        'Tesla': 'TSLA',
        'Nokia': 'NOK'
    }
    
    STOCKS = {
        'AAPL': 287.73,
        'MSFT': 173.26,
        'NFLX': 416.90,
        'TSLA': 724.88,
        'NOK': 3.37
    }
    
    if len(sys.argv) != 2:
        return
    
    arg = sys.argv[1]
    expressions = arg.split(',')

    for expr in expressions:
        if expr.strip() == '':
            return
    
    for expr in expressions:
        expr_stripped = expr.strip()
        expr_lower = expr_stripped.lower()
        
        is_ticker = False
        for ticker in STOCKS.keys():
            if expr_stripped.upper() == ticker:
                company_name = None
                for company, tick in COMPANIES.items():
                    if tick == ticker:
                        company_name = company
                        break
                if company_name:
                    print(f"{ticker} is a ticker symbol for {company_name}")
                is_ticker = True
                break
        
        if is_ticker:
            continue
        
        is_company = False
        for company, ticker in COMPANIES.items():
            if expr_lower == company.lower():
                stock_price = STOCKS[ticker]
                print(f"{company} stock price is {stock_price}")
                is_company = True
                break
        
        if is_company:
            continue
        
        print(f"{expr_stripped} is an unknown company or an unknown ticker symbol")

if __name__ == "__main__":
    all_stocks()
```

---

### Exercise 06. Сортировка словаря
>- Turn-in directory: `ex06/`.
>- Files to turn in: `dict_sorter.py`.
>- Allowed functions: any import is restricted.
>
>1. For this exercise, take the list of tuples from `ex04` containing countries and numbers, and create a dictionary where the countries are the keys and the numbers are the values. Copy it into one of your functions in the script.
>2. Write a program that displays the country names first in descending order by number and then in alphabetical order by name if the numbers are equal. Display one per line without the numbers:
>
>         The USA
>         Spain
>         Italy
>         France
>         Germany
>         ...

В это задании нам надо написать программу, которая сортирует словарь из задания 04 по ключу по убыванию, а если ключи одинаковы, то сортировать значение по алфавиту и вывести значения через отступ.

Вот содержимое файла `dict_sorter.py`:

```python
def sort_countries():

    list_of_tuples = [
        ('Russia', '25'),
        ('France', '132'),
        ('Germany', '132'),
        ('Spain', '178'),
        ('Italy', '162'),
        ('Portugal', '17'),
        ('Finland', '3'),
        ('Hungary', '2'),
        ('The Netherlands', '28'),
        ('The USA', '610'),
        ('The United Kingdom', '95'),
        ('China', '83'),
        ('Iran', '76'),
        ('Turkey', '65'),
        ('Belgium', '34'),
        ('Canada', '28'),
        ('Switzerland', '26'),
        ('Brazil', '25'),
        ('Austria', '14'),
        ('Israel', '12')
    ]
    
    countries = dict(list_of_tuples)
    
    countries = {k: int(v) for k, v in countries.items()}
    
    sorted_countries = sorted(countries.items(), key=lambda x: (-x[1], x[0]))
    
    for country, _ in sorted_countries:
        print(country)


if __name__ == "__main__":
    sort_countries()
```

---

### Exercise 07. Множества
>- Turn-in directory: `ex07/`.
>- Files to turn in: `marketing.py`.
>- Allowed functions: `import sys`.
>
>For this exercise, imagine that you work in a marketing department. You will work with two different lists of email accounts. The first list contains the email accounts of your clients. The second list contains the email accounts of participants in your most recent event, some of whom were your clients. The third list contains the email accounts of clients who viewed your most recent promotional email.
>
>In business terms, you need to:
>1. Create a list of clients who have not seen your promotional email yet. This list will be sent to the call center to contact these individuals.
>2. Create a list of participants who are not your clients. Send them an introductory email about your products.
>3. Create a list of clients who did not participate in the event. Send them a link to the event video and slides.
>
>Technical details:
>- Create different functions that convert your lists to sets. Use the set operators necessary to perform the aforementioned business tasks and return the required lists of email accounts.
>- Arrange your code in a script. The script takes the name of the task to be performed as an argument: `call_center`, `potential_clients`, or `loyalty_program`. If the wrong name is given, raise an exception.
>- For this exercise, use the following three lists:
>
>        clients = ['andrew@gmail.com', 'jessica@gmail.com', 'ted@mosby.com',
>        'john@snow.is', 'bill_gates@live.com', 'mark@facebook.com',
>        'elon@paypal.com', 'jessica@gmail.com']
>        participants = ['walter@heisenberg.com', 'vasily@mail.ru',
>        'pinkman@yo.org', 'jessica@gmail.com', 'elon@paypal.com',
>        'pinkman@yo.org', 'mr@robot.gov', 'eleven@yahoo.com']
>        recipients = ['andrew@gmail.com', 'jessica@gmail.com', 'john@snow.is']

В этом задании нам нужно написать программу, которая будет выводить електронные почты, которые будут удовлетворять множествам по условиям задачи.

Вот содержимое файла `marketing.py`:
```python
import sys


def get_clients_set():
    clients = ['andrew@gmail.com', 'jessica@gmail.com', 'ted@mosby.com',
               'john@snow.is', 'bill_gates@live.com', 'mark@facebook.com',
               'elon@paypal.com', 'jessica@gmail.com']
    return set(clients)


def get_participants_set():
    participants = ['walter@heisenberg.com', 'vasily@mail.ru',
                    'pinkman@yo.org', 'jessica@gmail.com', 'elon@paypal.com',
                    'pinkman@yo.org', 'mr@robot.gov', 'eleven@yahoo.com']
    return set(participants)


def get_recipients_set():
    recipients = ['andrew@gmail.com', 'jessica@gmail.com', 'ted@mosby.com',
                  'john@snow.is', 'bill_gates@live.com', 'elon@paypal.com']
    return set(recipients)


def call_center():
    clients = get_clients_set()
    recipients = get_recipients_set()
    
    not_contacted = clients - recipients
    
    return sorted(list(not_contacted))


def potential_clients():
    clients = get_clients_set()
    participants = get_participants_set()
    
    non_clients = participants - clients
    
    return sorted(list(non_clients))


def loyalty_program():
    clients = get_clients_set()
    participants = get_participants_set()
    
    non_participants = clients - participants
    
    return sorted(list(non_participants))


def main():
    if len(sys.argv) != 2:
        raise ValueError("Usage: python marketing.py <task_name>")
    
    task = sys.argv[1]
    
    tasks = {
        'call_center': call_center,
        'potential_clients': potential_clients,
        'loyalty_program': loyalty_program
    }
    
    if task not in tasks:
        raise ValueError(
            f"Invalid task name: '{task}'. "
            f"Valid options are: call_center, potential_clients, loyalty_program"
        )
    
    result = tasks[task]()
    
    print(f"\n{task.upper().replace('_', ' ')}:")
    print("-" * 50)
    for email in result:
        print(email)
    print(f"\nTotal: {len(result)} email(s)")


if __name__ == "__main__":
    main()
```

---

### Exercise 08. Работа со строками как со списками
> 