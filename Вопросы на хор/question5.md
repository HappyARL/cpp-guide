# Вопрос 5: 
      
---
  
- ## [В чем особенности и проблемы исключений в конструкторах и деструкторах?](#title1) 
- ## [Расскажите про идиому RAII.](#title2)
- ## [Расскажите про спецификацию исключений и ключевое слово noexcept.](#title3) 
- ## [Расскажите про оператор noexcept.](#title4) 
- ## [Расскажите про стандартную иерархию исключений (весь список перечислять не надо, примерно).](#title5) 
- ## [Как правильно определить свой объект исключения?](#title6) 
- ## [Расскажите про приведение типов в catch.](#title7) 

---

### <a id="title1">В чем особенности и проблемы исключений в конструкторах и деструкторах?</a>


В конструкторах, если происходит исключение, объект не будет полностью создан, и его деструктор не будет вызван. Это может привести к утечкам ресурсов и некорректному состоянию объекта.

В деструкторе может произойти ситуация, когда при летящем исключении мы хотим бросить еще одно. В данном случае наш код просто упадет. Начиная с C++11 все деструкторы помечены как noexcept (можно явно написать noexcept(false)).

---

### <a id="title2">Расскажите про идиому RAII.</a>

Идиома RAII предлагает связать приобретение ресурсов с инициализацией объекта. Например, если объект использует динамическую память, то его конструктор должен выделить эту память, а деструктор должен освободить ее. Таким образом, если происходит исключение в конструкторе, деструктор будет автоматически вызван и ресурсы будут корректно освобождены.


---

### <a id="title3">Расскажите про спецификацию исключений и ключевое слово noexcept.</a>

Спецификация исключений позволяет указать, какие исключения может выбросить функция или метод. Она записывается после списка параметров функции с помощью ключевого слова throw, за которым следуют типы исключений, разделенные запятой. Например:

```c++
void myFunction() throw(ExceptionType1, ExceptionType2) {
    // ...
}
```

Ключевое слово noexcept указывает, что функция или метод не выбрасывает исключений. Например:

```c++
void myFunction() noexcept {
    // ...
}
```

Оператор noexcept позволяет проверить, выбрасывает ли выражение исключение. Он возвращает значение типа bool - true, если выражение не выбрасывает исключение, и false в противном случае. Например:

```c++
bool isNoexcept = noexcept(myFunction());
```

noexcept возвращает false, если в выражении есть хотя бы один из операторов:
- throw
- new
- dynamic_cast
- noexcept function call

---

### <a id="title4">Расскажите про оператор noexcept.</a>

выше

---

### <a id="title5">Расскажите про стандартную иерархию исключений (весь список перечислять не надо, примерно).</a>

1. logic_error
  - invalid_argument
  - domain_error
  - length_error
  - out_of_range
  - future_error (since C++11)
  - runtime_error
  - range_error
  - overflow_error
  - underflow_error
  - regex_error (since C++11)
  - system_error (since C++11)
  - ios_base::failure (since C++11)
  - filesystem::filesystem_error (since C++17)
  - tx_exception (TM TS)
  - nonexistent_local_time (since C++20)
  - ambiguous_local_time (since C++20)
  - format_error (since C++20)
2. bad_typeid
3. bad_cast
  - bad_any_cast (since C++17)
4. bad_optional_access (since C++17)
5. bad_expected_access (since C++23)
6. bad_weak_ptr (since C++11)
7. bad_function_call (since C++11)
8. bad_alloc
  - bad_array_new_length (since C++11)
9. bad_exception
10. ios_base::failure (until C++11)
11. bad_variant_access (since C++17)

---

### <a id="title6">Как правильно определить свой объект исключения?</a>

Для определения своего объекта исключения нужно создать класс, который наследуется от std::exception или его производного класса. Обычно в таком классе реализуются конструкторы, чтобы передать информацию об ошибке, и переопределяются методы what() (причем он виртуальный) для предоставления описания ошибки.


---

### <a id="title7">Расскажите про приведение типов в catch.</a>

Приведение типов в блоке catch позволяет перехватывать исключения разных типов. Можно использовать как ссылки, так и указатели для перехвата объектов исключений. Например:

```c++
try {
    // ...
} catch (ExceptionType1& e) {
    // обработка исключения типа ExceptionType1
} catch (ExceptionType2* e) {
    // обработка исключения типа ExceptionType2
}
```

В блоке catch можно также использовать множественное перехватывание, чтобы перехватить исключения разных типов с одинаковым обработчиком. Например:

```c++
try {
    // ...
} catch (ExceptionType1& e) {
    // обработка исключения типа ExceptionType1
} catch (ExceptionType2& e) {
    // обработка исключения типа ExceptionType2
} catch (...) {
    // обработка всех остальных исключений
}
```

Это позволяет более гибко обрабатывать исключения и предоставлять различные варианты обработки для разных типов ошибок.

---

