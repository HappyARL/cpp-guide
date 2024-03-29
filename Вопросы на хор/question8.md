# Вопрос 8: 

---
  
- ## [Расскажите про инвалидацию ссылок и итераторов в контейнерах vector, deque, map, unordered_map, list.](#title1) 

---

### <a id="title1">Расскажите про инвалидацию ссылок и итераторов в контейнерах vector, deque, map, unordered_map, list.</a>

Инвалидация ссылок и итераторов в контейнерах может происходить при изменении структуры контейнера, таких как добавление или удаление элементов. Каждый контейнер имеет свои правила инвалидации ссылок и итераторов.

1. vector:
   - функции insert, emplace_back, emplace, push_back вызывают reallocation, если новый размер больше старой емкости.
     reallocation делает недействительными все ссылки, указатели и итераторы, ссылающиеся на элементы в последовательности.
     Если reallocation не происходит, все итераторы и ссылки перед точкой вставки остаются действительными.

2. deque:
   - Вставка в середине deque делает недействительными все итераторы и ссылки на элементы deque.
   - Вставка в любом конце deque делает недействительными все итераторы в deque, но не влияет на действительность ссылок на элементы deque.

3. list:
   - Не влияет на действительность итераторов и ссылок. Если генерируется исключение, никаких последствий не возникает.
   - Функции insert, emplace_front, emplace_back, emplace, push_front, push_back подпадают под действие этого правила.

4. map:
   - элементы insert и emplace не должны влиять на действительность итераторов и ссылок на контейнер.

5. unordered_map:
   - Повторное хэширование делает недействительными итераторы, изменяет порядок между элементами и изменяет, в каких корзинах отображаются элементы, но не делает недействительными указатели или ссылки на элементы.
   - Элементы insert и emplace не должны влиять на действительность ссылок на элементы контейнера, но могут сделать недействительными все итераторы для контейнера.
   - Элементы insert и emplace не должны влиять на действительность итераторов, если (N+n) <= z * B, где N - количество элементов в контейнере до операции insert, n - количество вставленных элементов, B - количество ячеек контейнера, а z - количество максимальный коэффициент загрузки контейнера.

Важно помнить, что использование недействительных ссылок или итераторов может привести к неопределенному поведению программы или ошибкам времени выполнения. Поэтому необходимо быть внимательным при изменении контейнеров и обновлять ссылки и итераторы при необходимости.

---

