# Вопрос 11: 

---
  
- ## [Что такое аллокаторы и зачем они нужны?](#title1) 
- ## [Предложите реализацию std::allocator.](#title2)
- ## [Что такое allocator_traits?](#title3) 
- ## [Что такое rebind аллокаторов и зачем это нужно?](#title4) 

---

### <a id="title1">Что такое аллокаторы и зачем они нужны?</a>

__ОПР:__ _Класс allocator_ - это шаблон класса описывает объект, который управляет выделением хранилища и освобождением для массивов объектов типа Type. 
          Объект класса allocator — это объект распределителя по умолчанию, указанный в конструкторах для нескольких шаблонов классов контейнеров в стандартной библиотеке C++.

---

### <a id="title2">Предложите реализацию std::allocator.</a>

Давайте посмотрим на реализацию стандартного аллокатора, который используется по умолчанию во всех контейнерах. Он очевидно должен просто обращаться к new/delete

```c++
template <typename T>
struct Allocator {
  T* allocate(size_t n) {
    return ::operator new(n * sizeof(T));
  }

  void deallocate(T* ptr, size_t) {
    ::operator delete(ptr);
  }

  template <typename... Args>
  void construct(T* ptr, const Args&...) { // здесь на самом деле должно быть Args&&
    new(ptr) T(args...); // а здесь std::forward(args)
  }

  void destroy(T* ptr) noexcept {
    ptr->~T();
  }
}
```

---

### <a id="title3">Что такое allocator_traits?</a>

Большинство методов у всех аллокаторов будут одинаковые (например construct и destroy) а еще внутри есть куча using, поэтому в c++ была добавлена специальная обертка allocator_traits.

allocator_traits это структура, которая шаблонным параметром принимает класс вашего аллокатора.

Почти все методы и внутренние юзинги работают по прицнипу "взять у аллокатора если есть, если нет сгенерировать автоматически".

Лучше всего посмотреть на описание и список методов [тут](https://en.cppreference.com/w/cpp/named_req/Allocator)

---

### <a id="title4">Что такое rebind аллокаторов и зачем это нужно?</a>

Rebind аллокаторов - это процесс создания нового аллокатора, который имеет другой тип выделенной памяти. Это полезно, когда требуется использовать один аллокатор для выделения памяти под один тип данных, а затем использовать тот же аллокатор для выделения памяти под другой тип данных.

В C++ аллокаторы предоставляются с помощью шаблонного класса std::allocator. Этот класс имеет вложенный тип rebind, который используется для создания нового аллокатора с другим типом данных.

```c++
template <typename T, typename Alloc = std::allocator<T>>
class List {
 public:
 private:
  using AllocTraits = std::allocator_traits<Alloc>;

  struct Node {
    Node* pref;
    Node* next;
    T value;
  };

  using NodeAlloc = typename AllocTraits::template rebind_alloc<Node>;
  using NodeAllocTraits = typename AllocTraits::template rebind_traits<Node>; // same as std::allocator_trais<NodeAlloc>

  Node* head_ = nullptr;
  Node* tail_ = nullptr;

  NodeAlloc alloc_;
};
```

Rebind аллокаторов полезен, когда требуется использовать один аллокатор для разных типов данных, но при этом сохранить совместимость с интерфейсом std::allocator. Это может быть полезно, например, при реализации контейнеров, которые могут хранить элементы разных типов.

---
