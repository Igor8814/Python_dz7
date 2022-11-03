view.py
```python
def user_command():
    num = int(input("Input \n 1. If You want to find a contact: \n 2. If You want to create a new contact: \n 3. If You want to see all contacts:  \n 4. If You want to close the programm: \n "))
    if 0 < num < 5:
        return num
    else:
        print("Incorrect number! The program will be closed.")
        return 4

def find_contact():
    find_me = input('Input contacts firstname or lastname or middlename: ')
    return find_me

def create_contact():
    new_contact = {"id": ""}
    new_contact['secondname'] = input('Input Secondname or press "enter" in case if you do not want to add it: ')
    new_contact['name'] = input('Input Firstname: ')
    while new_contact['name'] == '':
        new_contact['name'] = input('Input Firstname: ')
    new_contact['lastname'] = input('Input Lastname or press "enter" in case if you do not want to add it: ')
    new_contact['phone_number'] = input('Input phone number: ')
    new_contact['comment'] = input('Leave a comment here or press "enter" in case if you do not need it ') + "\n"
    print(new_contact)
    return new_contact


from tabulate import tabulate


def print_all_contacts(data):
    data_to_print = []
```
Record.py
```python
def save_data(data, src):
    with open(src, "w", encoding="utf-8") as file:
        file.write(convert_data(data, ";"))
    with open("dataBase.txt", "w", encoding="utf-8") as file:
        file.write(convert_data(data, "::"))


def convert_data(data, separator):
    result = " "
    for i in data:
        result += separator.join(i.values())
    return result
```
read.py
```python
def get_data(src, separator):
    dicts_from_file = []
    name_symbol = ["id", "secondname", "lasname", "Phone number", "comment"]
    with open(src, "r", encoding="utf-8") as file:
        for line in file:
            dicts_from_file.append(dict(zip(name_symbol, line.split(separator))))

    return dicts_from_file
```
processing.py
```python
import read
import Record
import view

data = read.get_data("dataBase.csv", ";")


def last_id():
    id_list = [int(i["id"]) for i in data if i["id"].isdigit()]
    return str(max(id_list) + 1)


def what_contact(what_find):
    data_to_print = [i for i in data if what_find in i.values()]
    return data_to_print


def main_logic():
    value = 0
    while value != 4:
        value = view.user_command()
        if value == 1:
            what_find = view.find_contact()
            view.print_all_contacts(what_contact(what_find))
        elif value == 2:
            contact = view.create_contact()
            contact["id"] = last_id()
            data.append(contact)
            Record.save_data(data, "dataBase.csv")


        elif value == 3:
            view.print_all_contacts(data)
        elif value == 4:
            print('Bye - bye! =)')
            break
        print("Вы выбрали {}-й пункт".format(value))
```
main.py
```python
import processing
processing.main_logic()
```
dataBase.csv
```python
1,Иванов,Иван Петрович,генеральный директор,111,1000.0
2,Иванова,Мария Ивановна,главный бухгалтер,222,999.99
3,Иванов,Иван Иванович,менеджер,333,888.88
4,Петров,Петр,работник,444,150.0
5,Сидоров,Сидор Петрович,сторож,555,250.0
6,Шишкин,Анатолий,преподаватель,666,1000000.0
```
dataBase.txt
~~~
Иванов
Иван
111
описание Иванова
Петров
Петр
222
описание Петрова
Васичкина
Василиса
333
описание Васичкиной
Питонов
Антон
777
умеет в Питон
~~~
