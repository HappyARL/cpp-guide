# Вопрос 4: 

---
  
- ## [Расскажите о шаблонах в C++: зачем они нужны и что это такое?](#title1) 
- ## [Приведите пример шаблонной функции и класса.](#title2)
- ## [Расскажите как определить шаблонный метод шаблонного класса вне класса.](#title3) 

---

### <a id="title1">Расскажите о шаблонах в C++: зачем они нужны и что это такое?</a>

__ОПР:__ _Шаблоны_ - это пример полиморфизма в С++. Рассмотрим пример: у нас есть функция ___GetMax___  который возвращает максимум от двух чисел.
Мы (люди) можем без особых проблем сранвить целые и дробные значения, пока как компьютер нет. Таким образом мы должны определить данную функцию
для разных типов чтобы все работало.

```c++
int GetMax(const int& a, const int& b) {
    return a > b ? a : b;
}

double GetMax(const double& a, const double& b) {
    return a > b ? a : b;
}

std::string GetMax(const std::string a, const std::string& b) {
    return a > b ? a : b;
}
```

Это запарно и неэффективно, но можно сделать умнее используя вышеупомянутые шаблоны.
Тогда код выше будет иметь следующий вид:

```c++
template <typename T>
T GetMax(const T& a, const T& b) {
    return a > b ? a : b;
}
```

Мы создали шаблон для функции ___GetMax___ который используем тип T для сравнения, который олицетворяет: int, float, double ...
Работает это все потому что компилятор создает копии данной функции подставляя в Т различный известные типы, тем самым вызывая только нужный нам метод с определенным типом.

__! Замечание 1__ Если вызвать метод от типа у которого не определен опертор сравнения, то будет СЕ.

Далее идет логичный вопрос: "что будет если сравнить два разных типа???"

```c++
GetMax(1, 2.0);
```

Ответ: будет СЕ

Чтобы этого избежать данную ошибку надо:
- либо написть явно какой тип т.е : ___GetMax<int>(1, 2.0)___
- либо написать шаблон который принимает два типа :

```c++
template <typename T, typename U>
T GetMax(const T& a, const U& b) {
    return a > b ? a : b;
}
```

Помимо функций, "шаблонными" могут быть: классы, методы, константы, метод внутри шаблонного класса.

```c++
// шаблонный класс
template <typename T>
class vector {
    ...
};

// шаблонная коснтанта
template <typename T>
const T pi = 3.14;

// шаблонный метож
struct Vector {
    template <typename U>
    void push_back(const U&);
}

// шаблонный метод в шаблонном классе
template <typename T>
struct Vector {
    template <typename U>
    void push_back(const U&);

    T value;
}
```

Теперь поговорим что меожно с шаблоном делать:
1. __Перегрузка шаблонных функций__

Рассмотрим следующий код:

```c++
template <typename T>
void f(T x) {
    std::cout << 1;
}

void f(int x) {
    std::cout << 2;
}

int main() {
    f(0);
}
```

Данный код на выводе напечает 2, так как _void f(int x)_ является частным случаем _void f(T x)_.
Компилятор предпочиатем выбирать более частные случаи.

Еще один пример:

```c++
#include <iostream>

template <typename T, U>
void f(T x, U y) {
    std::cout << 1;
}

template <typename T>
void f(T x, T y) {
    std::cout << 2;
}

void f(int x, double y) {
    std::cout << 3;
}

int main() {
    f(0, 0);
}
```

В данном примере 2я версия лучше 1й потому что более частная и лучше 3й потому что не надо делать приведение типов
Отсюда нам важно запомнить два основных правила перегрузки функций:

Частное лучше общего

Если есть более частная версия, но нужно будет сделать приведение типов, то лучше более общая версия

2. __Специализация шаблона__

При определении шаблона класса компилятор сам генерирует по этому шаблону классы, которые применяют определенные типы. 
Однако мы сами можем определить подобные классы для конкретного набора параметров шаблона. Подобные определения классов называются специализацией шаблона. 
Специализация шаблона может быть полной и частичной.

Пример _полной специализации_:

```c++
template <typename T>
class Vector {
    ...
}

template <>
class Vector<bool> {
    ...
}
```

Пример _частичной специализации_:

```c++
template <typename T>
class Vector<T*> {
    ...
}

```

У функций так такой особо специализации нет, так как есть перегурзка
более подробно тут: [Лекция 8](https://gitlab.com/yaishenka/cpp_course/-/blob/main/lectures/lecture_08.md#2-%D1%82%D0%B5%D0%BF%D0%B5%D1%80%D1%8C-%D0%BF%D1%80%D0%BE-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8)

Теперь поговорим какеи параметры могут быть у шаблона:
- _non-type parametr_
- _type parametr_
- _template parametr_

__non-type parametr__

__ОПР:__ Шаблонными параметры, не являющимися типами могут быть только целочисленные типы а также bool и char

Хороший пример класса, который использует такие шаблонные параметры это std::array (обертка над честным массивом).
Зачем это вообще нужно? Чтобы типы от разных констант не были совместимы друг с другом

__template parametr__

Допустим мы пишем свой Stack и хотим передавать в шаблонах какой контейнер использовать, то есть хотим пользоваться им примерно так:

```c++
template <...>
class Stack {
  public:
    ...
  private:
    Container<T> container_;
}

int main() {
    Stack<int, std::vector> s;
}
```

Чтобы это работало нам надо написать объявление нашего класса вот так:

```c++
template <typename T, template <typename U> typename Container>
class Stack {
  public:
    ...
  private:
    Container<T> container_;
}
```

На самом деле U можно не писать, так как этот тип нигде не используется. Тогда наш класс будет выглядеть вот так:

```c++
template <typename T, template <typename> typename Container>
class Stack {
  public:
    ...
  private:
    Container<T> container_;
}
```

---

### <a id="title2">Приведите пример шаблонной функции и класса.</a>

Пример функции это _GetMax_, пример класса это _Vector_, которые были описаны выше.

---

### <a id="title3">Расскажите как определить шаблонный метод шаблонного класса вне класса.</a>

Чтобы определить такой метод вне класса нужно написать

```c++
template <typename T>
struct Vector {
    template <typename U>
    void push_back(const U&);

    T value;
}

template <typename T>
template <typename U>
void Vector<T>::push_back(const U&) {
    ...
}
```
