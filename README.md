# s21_containers

Групповой проект на языке C++. Реализация основных стандартных контейнерных классов языка C++. Моей задачей было реализовать list, map и написать модульные тесты (GTest)

## List

<details>
  <summary>Общая информация</summary>
<br />

List (список) - это последовательный контейнер, хранящий набор элементов произвольного размера в виде узлов, последовательно связанных указателями. Каждый узел хранит значение, соответствующее элементу списка, и указатель на следующий элемент. Такое устройство контейнера позволяет уйти от жестко фиксированного рамера, как, например, в статическом массиве, и делает более интуитивно понятным процесс добавления нового элемента в контейнер. 

Выше представлен пример списка из четырех элементов. Каждый из элементов списка представлен в виде структуры с двумя полями: значение узла и указатель на следующий элемент списка. Последний элемент списка ни на что не указывает. 

Подобное устройство списка позволяет простым образом (без каскадного сдвига) добавлять как в конец, так и в середину списка. При добавлении элемента в конкретную позицию списка создается новый узел, указывающий на следующий после данной позиции элемент, после чего указатель предыдущего элемента перемещается на новый.

При удалении элемента из списка, соответствующий узел освобождается, а указатели соседних элементов меняют значение: предыдущий элемент перемещает указатель на следующий после удаленного элемент.

Списки бывают односвязные или двусвязные. Односвязный список - это список, каждый узел которого хранит только один указатель: на следующий элемент списка (пример, приведенный выше). В двусвязном списке каждый узел хранит дополнительный указатель и на предыдущий элемент. Стандартная реализация контейнера list в С++ использует двусвязный список. 

В объекте класса контейнера хранятся указатели на "голову" и "хвост" списка, указывающие на первый и последний элементы списка. Контейнер List предоставляет прямой доступ только к "голове" и "хвосту", но позволяет добавлять и удалять элементы в любой части списка.
</details>

<details>
  <summary>Спецификация</summary>
<br />

*List Member type*

В этой таблице перечислены внутриклассовые переопределения типов (типичные для стандартной библиотеки STL), принятые для удобства восприятия кода класса:

| Member type            | definition                                                                             |
|------------------------|----------------------------------------------------------------------------------------|
| `value_type`             | `T` defines the type of an element (T is template parameter)                                  |
| `reference`              | `T &` defines the type of the reference to an element                                                             |
| `const_reference`        | `const T &` defines the type of the constant reference                                         |
| `iterator`               | internal class `ListIterator<T>` defines the type for iterating through the container                                                 |
| `const_iterator`         | internal class `ListConstIterator<T>` defines the constant type for iterating through the container                                           |
| `size_type`              | `size_t` defines the type of the container size (standard type is size_t) |

*List Functions*

В этой таблице перечислены основные публичные методы для взаимодействия с классом:

| Functions      | Definition                                      |
|----------------|-------------------------------------------------|
| `list()`  | default constructor, creates empty list                                  |
| `list(size_type n)`  | parameterized constructor, creates the list of size n                                 |
| `list(std::initializer_list<value_type> const &items)`  | initializer list constructor, creates list initizialized using std::initializer_list<T>    |
| `list(const list &l)`  | copy constructor  |
| `list(list &&l)`  | move constructor  |
| `~list()`  | destructor  |
| `operator=(list &&l)`      | assignment operator overload for moving object                                |

*List Element access*

В этой таблице перечислены публичные методы для доступа к элементам класса:

| Element access | Definition                                      |
|----------------|-------------------------------------------------|
| `const_reference front()`          | access the first element                        |
| `const_reference back()`           | access the last element                         |

*List Iterators*

В этой таблице перечислены публичные методы для итерирования по элементам класса (доступ к итераторам):

| Iterators      | Definition                                      |
|----------------|-------------------------------------------------|
| `iterator begin()`    | returns an iterator to the beginning            |
| `iterator end()`        | returns an iterator to the end                  |

*List Capacity*

В этой таблице перечислены публичные методы для доступа к информации о наполнении контейнера:

| Capacity       | Definition                                      |
|----------------|-------------------------------------------------|
| `bool empty()`          | checks whether the container is empty           |
| `size_type size()`           | returns the number of elements                  |
| `size_type max_size()`       | returns the maximum possible number of elements |

*List Modifiers*

В этой таблице перечислены публичные методы для изменения контейнера:

| Modifiers      | Definition                                      |
|----------------|-------------------------------------------------|
| `void clear()`          | clears the contents                             |
| `iterator insert(iterator pos, const_reference value)`         | inserts elements into concrete pos and returns the iterator that points to the new element     |
| `void erase(iterator pos)`          | erases element at pos                                 |
| `void push_back(const_reference value)`      | adds an element to the end                      |
| `void pop_back()`   | removes the last element        |
| `void push_front(const_reference value)`      | adds an element to the head                      |
| `void pop_front()`   | removes the first element        |
| `void swap(list& other)`                   | swaps the contents                                                                     |
| `void merge(list& other)`                   | merges two sorted lists                                                                      |
| `void splice(const_iterator pos, list& other)`                   | transfers elements from list other starting from pos             |
| `void reverse()`                   | reverses the order of the elements              |
| `void unique()`                   | removes consecutive duplicate elements               |
| `void sort()`                   | sorts the elements                |
| `iterator emplace(const_iterator pos, Args&&... args)`          | inserts new elements into the container directly before `pos`  |
| `void emplace_back(Args&&... args)`          | appends new elements to the end of the container  |
| `void emplace_front(Args&&... args)`          |

</details>


## Map

<details>
  <summary>Общая информация</summary>
<br />

Map (словарь) - это ассоциативный контейнер, содержащий отсортированные по возрастанию ключа пары ключ-значение. То есть каждый элемент ассоциирован с некоторым уникальным ключом, и его положение в словаре определяется его ключом. Словари удобно применять, когда необходимо ассоциировать элементы с некоторым другим значением (не индексом). Например, предприятие осуществляет закупку оборудования, причем каждую позицию приходится закупать неоднократно. В таком случае удобно использовать словарь с парой идентификатор позиции - объем закупки. Здесь идентификатором может выступать не только число, но и строка. Таким образом, поиск в словаре осуществляется не по индексу, как в массиве, а по идентификатору - слову. 

Но каким образом словарь позволяет обращаться к парам по ключу и при этом оказывается всегда отсортированным? На самом деле, словарь имеет структуру бинарного дерева поиска (в реализации C++ это дерево - красно-черное), что позволяет сразу добавлять элементы в словарь согласно прямому порядку и находить элементы более эффективно, чем прямой просмотр всех элементов словаря. 

Бинарное дерево поиска - это структура состоящая также и узлов, но каждый узел обладает двумя указателями на двух других узлов - "потомков". В этом случае, текущей узел называется "родительским". В общем виде, бинарное дерево поиска гарантирует, что если у текущего узла есть потомки, то левый "потомок" содержит в себе элемент с меньшим значением, а правый - с большим. Таким образом, для поиска в дереве некоторого элемента, достаточно сравнивать специальной функцией компоратором (в случае со словарем, эта функция зависит от типа ключа) искомое значение со значением текущего узла. Если оно оказалось больше - следует переходить к "правому" потомку, меньше - к левому, а если значение оказалось равным, тогда искомый элемент найден.

</details>

<details>
  <summary>Спецификация</summary>
  <br />

*Map Member type*

В этой таблице перечислены внутриклассовые переопределения типов (типичные для стандартной библиотеки STL), принятые для удобства восприятия кода класса:

| Member type            | Definition                                                                             |
|------------------------|----------------------------------------------------------------------------------------|
| `key_type`               | `Key` the first template parameter (Key)                                                     |
| `mapped_type`           | `T` the second template parameter (T)                                                      |
| `value_type`             | `std::pair<const key_type,mapped_type>` Key-value pair                                                      |
| `reference`              | `value_type &` defines the type of the reference to an element                                                             |
| `const_reference`        | `const value_type &` defines the type of the constant reference                                         |
| `iterator`               | internal class `MapIterator<K, T>` or `BinaryTree::iterator` as internal iterator of tree subclass; defines the type for iterating through the container                                                 |
| `const_iterator`         | internal class `MapConstIterator<K, T>` or `BinaryTree::const_iterator` as internal const iterator of tree subclass; defines the constant type for iterating through the container                                           |
| `size_type`              | `size_t` defines the type of the container size (standard type is size_t) |

*Map Member functions*

В этой таблице перечислены основные публичные методы для взаимодействия с классом:

| Member functions      | Definition                                      |
|----------------|-------------------------------------------------|
| `map()`  | default constructor, creates empty map                                 |
| `map(std::initializer_list<value_type> const &items)`  | initializer list constructor, creates the map initizialized using std::initializer_list<T>    |
| `map(const map &m)`  | copy constructor  |
| `map(map &&m)`  | move constructor  |
| `~map()`  | destructor  |
| `operator=(map &&m)`      | assignment operator overload for moving object                                |

*Map Element access*

В этой таблице перечислены публичные методы для доступа к элементам класса:

| Element access         | Definition                                                                             |
|------------------------|----------------------------------------------------------------------------------------|
| `T& at(const Key& key)`                     | access specified element with bounds checking                                          |
| `T& operator[](const Key& key)`             | access or insert specified element                                                     |

*Map Iterators*

В этой таблице перечислены публичные методы для итерирования по элементам класса (доступ к итераторам):

| Iterators              | Definition                                                                             |
|------------------------|----------------------------------------------------------------------------------------|
| `iterator begin()`            | returns an iterator to the beginning                                                   |
| `iterator end()`                | returns an iterator to the end                                                         |

*Map Capacity*

В этой таблице перечислены публичные методы для доступа к информации о наполнении контейнера:

| Capacity               | Definition                                                                             |
|------------------------|----------------------------------------------------------------------------------------|
| `bool empty()`                  | checks whether the container is empty                                                  |
| `size_type size()`                   | returns the number of elements                                                         |
| `size_type max_size()`               | returns the maximum possible number of elements                                        |

*Map Modifiers*

В этой таблице перечислены публичные методы для изменения контейнера:

| Modifiers              | Definition                                                                             |
|------------------------|----------------------------------------------------------------------------------------|
| `void clear()`                  | clears the contents                                                                    |
| `std::pair<iterator, bool> insert(const value_type& value)`                 | inserts node and returns iterator to where the element is in the container and bool denoting whether the insertion took place                                        |
| `std::pair<iterator, bool> insert(const Key& key, const T& obj)`                 | inserts value by key and returns iterator to where the element is in the container and bool denoting whether the insertion took place    |
| `std::pair<iterator, bool> insert_or_assign(const Key& key, const T& obj);`       | inserts an element or assigns to the current element if the key already exists         |
| `void erase(iterator pos)`                  | erases element at pos                                                                        |
| `void swap(map& other)`                   | swaps the contents                                                                     |
| `void merge(map& other);`                  | splices nodes from another container                                                   |
| `std::pair<iterator,bool> emplace(Args&&... args)`          |

*Map Lookup*

В этой таблице перечислены публичные методы, осуществляющие просмотр контейнера:

| Lookup                 | Definition                                                                             |
|------------------------|----------------------------------------------------------------------------------------|
| `bool contains(const Key& key)`                  | checks if there is an element with key equivalent to key in the container                                   |

</details>

