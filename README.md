# goit-algo-hw-06
ДЗ Модуль 6
 ```
import re
from collections import UserDict

class Field:
    def __init__(self,value):
        self.value = value

    def __str__(self):
        return self.value

    def __repr__(self):
        return self.value

class Name (Field):
    def __init__(self, value):
        super().__init__(value)

class Phone(Field):
    def __init__(self, value):
        self.validate(value)
        super().__init__(value)
    
    def validate(self, value):
        if not re.fullmatch(r'\d{10}', value):
            raise ValueError (f'Phone numberb {value} is invalid.')
        
class Record:
    def __init__(self, name, phones = None):
        self.name = Name(name)
        self.phones = [Phone(phone) for phone in phones] if phones else []
        
    def add_phone(self, phone):
        self.phones.append(Phone(phone))

    def remove_phone(self,phone):
        phone_to_remove = self.find_phone(phone)
        if phone_to_remove:
            self.phones.remove(phone_to_remove)
        else:
            raise ValueError(f'Phone number {phone} is not found.')
    
    def edit_phone(self, old_phone, new_phone):
        self.remove_phone(old_phone)
        self.add_phone(new_phone)

    def find_phone(self,phone):
        for p in self.phones:
            if p.value == phone:
                return p
        return None
    
    def __str__(self):
        phones_str = ', '.join(phone.value for phone in self.phones)
        return f"Name: {self.name.value}, Phones: [{phones_str}]"

class AddressBook(UserDict):
    def add_record(self,record):
        self.data[record.name.value] = record
    
    def find(self, name):
        return self.data.get(name, None)
    
    def delete(self, name):
        if name in self.data:
            del self.data[name]
        else:
            raise ValueError(f"Record with name {name} not found.")
    
    def __str__(self):
         records = [str(record) for record in self.data.values()]
         return '\n'.join(records)


if __name__ == "__main__":
    address_book = AddressBook()

    # Створення нової адресної книги
    book = AddressBook()

    # Створення запису для John
    john_record = Record("John")
    john_record.add_phone("1234567890")
    john_record.add_phone("5555555555")

    # Додавання запису John до адресної книги
    book.add_record(john_record)

    # Створення та додавання нового запису для Jane
    jane_record = Record("Jane")
    jane_record.add_phone("9876543210")
    book.add_record(jane_record)

    # Виведення всіх записів у книзі
     
    print(book)

    # Знаходження та редагування телефону для John
    john = book.find("John")
    john.edit_phone("1234567890", "1112223333")

    print(john)  # Виведення: Contact name: John, phones: 1112223333; 5555555555

    # Пошук конкретного телефону у записі John
    found_phone = john.find_phone("5555555555")
    print(f"{john.name}: {found_phone}")  # Виведення: John: 5555555555

    # Видалення запису Jane
    book.delete("Jane")
```
