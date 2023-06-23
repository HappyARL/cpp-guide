# Вопрос 3: 

---

- ## [Расскажите о разновидностях приведений типов в С++.](#title1) 
- ## [В чем разница между C-style cast, static_cast, const_cast и reinterpret_cast, когда они применимы, а когда нет?](#title2)
- ## [Для чего нужен каждый из этих кастов?](#title3) 

---

### <a id="title1">Расскажите о разновидностях приведений типов в С++.</a>

В C++ существует несколько разновидностей приведения типов:

1. _C-style cast_ - это старый способ приведения типов, который использует синтаксис C.

```c++
(int)x
int(x)
```

2. _static_cast_ - для приведения, которые проверяются только во время компиляции.
   static_cast возвращает ошибку, если компилятор обнаруживает, что вы пытаетесь выполнить приведение между полностью несовместимыми типами.
   Его также можно использовать для приведения между указателем на базу и указателем на производный, но компилятор не всегда может определить,
   будут ли такие преобразования безопасными во время выполнения.

```c++
double d = 1.58947;
int i = d;  // warning C4244 possible loss of data
int j = static_cast<int>(d);       // No warning.
string s = static_cast<string>(d); // Error C2440:cannot convert from
                                   // double to std:string

// No error but not necessarily safe.
Base* b = new Base();
Derived* d2 = static_cast<Derived*>(b);
```

3. _dynamic_cast_ — это приведение, для безопасных приведений указателя к базовой точке к указателю на производный от среды выполнения.
   Объект dynamic_cast более безопасный, чем для static_cast ниспроверений, но проверка среды выполнения влечет за собой некоторые издержки.

```c++
Base* b = new Base();

// Run-time check to determine whether b is actually a Derived*
Derived* d3 = dynamic_cast<Derived*>(b);

// If b was originally a Derived*, then d3 is a valid pointer.
if(d3)
{
   // Safe to call Derived method.
   cout << d3->DoSomethingMore() << endl;
}
else
{
   // Run-time check failed.
   cout << "d3 is null" << endl;
}

//Output: d3 is null;
```
   
4. _const_cast_ - это способ удаления или добавления константности к объекту. Он может быть использован для приведения константного объекта к неконстантному или наоборот.

```c++
const double pi = 3.14;
const_cast<double&>(pi); //No error.
```

5. _reinterpret_cast_ - это способ приведения типов, который позволяет интерпретировать битовое представление объекта как другой тип.
   Он может быть использован для приведения указателей между различными типами, такими как указатель на int и указатель на float.

```c++
const char* str = "hello";
int i = static_cast<int>(str);//error C2440: 'static_cast' : cannot
                              // convert from 'const char *' to 'int'
int j = (int)str; // C-style cast. Did the programmer really intend
                  // to do this?
int k = reinterpret_cast<int>(str);// Programming intent is clear.
                                   // However, it is not 64-bit safe.
```

---

### <a id="title2">В чем разница между C-style cast, static_cast, const_cast и reinterpret_cast, когда они применимы, а когда нет?</a>

Разница между этими кастами заключается в том, что:
- __C-style cast__ может быть использован для любого типа, но не является безопасным и может привести к неожиданным результатам.
- __static_cast__ используется для приведения объектов между связанными типами и является более безопасным, чем C-style cast.
- __const_cast__ используется для удаления или добавления константности к объекту.
- __reinterpret_cast__ используется для приведения указателей между различными типами,
  но может привести к непредсказуемым результатам и должен быть использован только в особых случаях.


---

### <a id="title3">Для чего нужен каждый из этих кастов?</a>

вся информация выше отвечает на этот вопрос

---

___ССЫЛКИ:___

приведение типов от Microsoft:

https://learn.microsoft.com/ru-ru/cpp/cpp/type-conversions-and-type-safety-modern-cpp?view=msvc-170
