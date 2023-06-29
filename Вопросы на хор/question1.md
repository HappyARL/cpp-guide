# Вопрос 1: 

---

- ## [Расскажите о ключевых словах static, explicit, mutable, friend с примерами ситуаций, когда их следует применять.](#title1) 

---

### <a id="title1">Расскажите о ключевых словах static, explicit, mutable, friend с примерами ситуаций, когда их следует применять.</a>

1. Ключевое слово __static__ используется для создания статических переменных и функций в C++. Статическая переменная существует в течение всего времени выполнения программы и общая для всех экземпляров класса. Статическая функция может быть вызвана без создания экземпляра класса.

```c++
Пример использования static:
cpp
class MyClass {
 public:
  static int count; // статическая переменная
  static void printCount() { // статическая функция
    cout << "Count: " << count << endl;
  }
};

int MyClass::count = 0; // инициализация статической переменной

int main() {
  MyClass::count++; // доступ к статической переменной через класс
  MyClass::printCount(); // вызов статической функции через класс
  return 0;
}
```

2. Ключевое слово __explicit__ используется для явного запрета неявного преобразования типов при вызове конструктора. Это позволяет избежать неожиданных неявных преобразований типов и повысить безопасность кода.

```c++
class MyClass {
 public:
  explicit MyClass(int value) : m_value(value) {}
  int getValue() const { return m_value; }

 private:
  int m_value;
};

void printValue(const MyClass& obj) {
  cout << obj.getValue() << endl;
}

int main() {
  MyClass obj = 10; // ошибка компиляции без explicit
  printValue(20); // ошибка компиляции без explicit
  return 0;
}
```

3. Ключевое слово __mutable__ используется для указания, что член класса может быть изменен, даже если объект класса объявлен как константный. Это полезно, когда нужно изменить состояние объекта, но не изменять его логическое состояние.

```c++
class MyClass {
 public:
  int getValue() const {
    m_counter++; // изменение mutable переменной внутри константной функции
    return m_value;
  }

 private:
  int m_value;
  mutable int m_counter; // mutable переменная
};

int main() {
  const MyClass obj;
  obj.getValue(); // изменение mutable переменной в константном объекте
  return 0;
}
```

4. Ключевое слово __friend__ используется для предоставления доступа приватным членам класса другим классам или функциям. Дружественные классы или функции могут обращаться к приватным членам без необходимости использовать геттеры или сеттеры.

```c++
class MyClass {
 public:
  friend class FriendClass; // FriendClass является другом MyClass
 private:
  int m_privateValue;
};

class FriendClass {
 public:
  void accessPrivateValue(const MyClass& obj) {
    cout << obj.m_privateValue << endl; // доступ к приватному члену MyClass
  }
};

int main() {
  MyClass obj;
  FriendClass friendObj;
  friendObj.accessPrivateValue(obj); // вызов функции доступа к приватному члену
  return 0;
}
```

---
