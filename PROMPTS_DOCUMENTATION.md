# CodeGeeX AI Prompts Documentation

Это полная документация всех промтов для AI, используемых в приложении CodeGeeX. Промты сгруппированы по тематикам с подробным описанием назначения каждого.

## Оглавление

1. [Промты идентификации языков программирования (Language Tags)](#1-промты-идентификации-языков-программирования)
2. [Промты для перевода кода (Code Translation)](#2-промты-для-перевода-кода)
3. [Примеры промтов для генерации кода](#3-примеры-промтов-для-генерации-кода)
4. [Тестовые промты и API примеры](#4-тестовые-промты-и-api-примеры)
5. [Служебные константы (Import Helpers)](#5-служебные-константы)

---

## 1. Промты идентификации языков программирования

### Описание
Теги языков программирования используются для идентификации целевого языка при генерации кода. Эти теги автоматически добавляются в начало кода, чтобы модель понимала, на каком языке нужно генерировать код. Это критически важная часть архитектуры CodeGeeX, так как модель обучена распознавать эти специфичные маркеры.

### Расположение
- **Файл:** `codegeex/data/data_utils.py`
- **Строки:** 7-64

### Назначение
- Указывает модели целевой язык программирования для генерации
- Автоматически добавляется перед кодом в процессе генерации
- Используется в бенчмарках для тестирования кроссязыковых возможностей
- Применяется в веб-интерфейсе Gradio для обработки пользовательского ввода

### Использование в коде
- `deployment/server_gradio.py:118-119` - добавление тега языка в веб-интерфейсе
- `codegeex/benchmark/utils.py:107-108` - использование в бенчмарках
- `codegeex/data/process_pretrain_dataset.py:60-61` - предобработка данных

### Промт (Language Tags Dictionary)

```python
LANGUAGE_TAG = {
    # C-подобные языки
    "c"            : "// language: C",
    "c++"          : "// language: C++",
    "cpp"          : "// language: C++",
    "c#"           : "// language: C#",
    "csharp"       : "// language: C#",
    "cuda"         : "// language: Cuda",
    "java"         : "// language: Java",
    "scala"        : "// language: Scala",
    "php"          : "// language: PHP",
    "js"           : "// language: JavaScript",
    "javascript"   : "// language: JavaScript",
    "typescript"   : "// language: TypeScript",
    "go"           : "// language: Go",
    "rust"         : "// language: Rust",
    "kotlin"       : "// language: Kotlin",
    "pascal"       : "// language: Pascal",
    "groovy"       : "// language: Groovy",
    "actionscript" : "// language: ActionScript",
    "solidity"     : "// language: Solidity",
    "swift"        : "// language: swift",

    # Python-подобные языки (# для комментариев)
    "python"       : "# language: Python",
    "perl"         : "# language: Perl",
    "lua"          : "# language: Lua",
    "shell"        : "# language: Shell",
    "ruby"         : "# language: Ruby",
    "r"            : "# language: R",
    "gdscript"     : "# language: GDScript",
    "julia"        : "# language: Julia",
    "elixir"       : "# language: Elixir",
    "powershell"   : "# language: PowerShell",

    # SQL и базы данных
    "sql"          : "-- language: SQL",

    # Функциональные языки
    "haskell"      : "-- language: Haskell",
    "lean"         : "-- language: Lean",
    "lisp"         : "; language: Lisp",
    "scheme"       : "; language: Scheme",
    "clojure"      : "; language: Clojure",

    # Научные и математические языки
    "matlab"       : "% language: Matlab",
    "fortran"      : "!language: Fortran",
    "tex"          : "% language: TeX",
    "erlang"       : "% language: Erlang",
    "prolog"       : "% language: Prolog",

    # Веб-технологии
    "css"          : "/* language: CSS */",
    "vue"          : "<!--language: Vue-->",
    "markdown"     : "<!--language: Markdown-->",
    "html"         : "<!--language: HTML-->",

    # Системные и специализированные языки
    "objectivec"   : "// language: Objective-C",
    "objective-c"  : "// language: Objective-C",
    "objective-c++": "// language: Objective-C++",
    "dart"         : "// language: Dart",
    "vb"           : "' language: Visual Basic",
    "delphi"       : "{language: Delphi}",
    "basic"        : "' language: Basic",
    "assembly"     : "; language: Assembly",
    "abap"         : "* language: Abap",
    "excel"        : "' language: Excel",
    "cobol"        : "// language: Cobol",
}
```

### Примеры использования

#### Пример 1: Python код с тегом языка
```python
# language: Python
def add(a, b):
    '''
    Find the sum of a and b.
    '''
    return a + b
```

#### Пример 2: C++ код с тегом языка
```cpp
// language: C++
int add(int a, int b) {
    /* Find the sum of a and b. */
    return a + b;
}
```

#### Пример 3: JavaScript код с тегом языка
```javascript
// language: JavaScript
function add(a, b) {
    // Find the sum of a and b.
    return a + b;
}
```

### Важные особенности
1. **Формат комментария зависит от языка** - тег использует стиль комментариев, принятый в целевом языке
2. **Поддержка синонимов** - например, "c++" и "cpp" оба указывают на C++
3. **58 поддерживаемых языков** - от популярных (Python, Java, C++) до специализированных (ABAP, Prolog, Lean)
4. **Стандартизированный формат** - всегда "language: LanguageName" в соответствующем стиле комментария

---

## 2. Промты для перевода кода

### Описание
Промт для кроссязыкового перевода кода - одна из ключевых возможностей CodeGeeX. Этот промт позволяет автоматически переводить код с одного языка программирования на другой, сохраняя логику и функциональность. Промт использует специальный формат "code translation", который модель распознает как задачу перевода, а не генерации нового кода.

### Расположение
- **Файл:** `codegeex/benchmark/utils.py`
- **Строки:** 79-95
- **Функция:** `read_translation_dataset()`

### Назначение
- Автоматический перевод кода между языками программирования
- Тестирование кроссязыковых возможностей модели на бенчмарках (например, HumanEval-X)
- Миграция кодовых баз между языками
- Обучение программистов различным языкам через примеры

### Структура промта

```
code translation
{SOURCE_LANGUAGE}:
{source_code_declaration}
{source_code_solution}
{TARGET_LANGUAGE}:
{target_code_declaration}
```

### Промт (Template Format)

```python
# Шаблон построения промта для перевода кода
prompt = f'''code translation
{lang_src}:
{dataset_src[k]["declaration"]}
{dataset_src[k]["canonical_solution"]}
{lang_tgt}:
{dataset_tgt[k]["declaration"]}
'''
```

### Компоненты промта

1. **Заголовок "code translation"** - специальный маркер, указывающий модели на задачу перевода
2. **Название исходного языка** (lang_src) - например "Python:", "Java:", "C++:"
3. **Объявление функции на исходном языке** (declaration) - сигнатура функции с docstring/комментариями
4. **Решение на исходном языке** (canonical_solution) - полная реализация функции
5. **Название целевого языка** (lang_tgt) - язык, на который нужно перевести
6. **Объявление функции на целевом языке** (declaration) - только сигнатура, без реализации

### Полный пример промта: Python → C++

```
code translation
Python:
from typing import List

def has_close_elements(numbers: List[float], threshold: float) -> bool:
    """ Check if in given list of numbers, are any two numbers closer to each other than
    given threshold.
    >>> has_close_elements([1.0, 2.0, 3.0], 0.5)
    False
    >>> has_close_elements([1.0, 2.8, 3.0, 4.0, 5.0, 2.0], 0.3)
    True
    """
    for idx, elem in enumerate(numbers):
        for idx2, elem2 in enumerate(numbers):
            if idx != idx2:
                distance = abs(elem - elem2)
                if distance < threshold:
                    return True
    return False

C++:
#include<stdio.h>
#include<vector>
#include<math.h>
using namespace std;
bool has_close_elements(vector<float> numbers, float threshold){
```

**Ожидаемая генерация модели:**
```cpp
    for (int i=0; i<numbers.size(); i++) {
        for (int j=i+1; j<numbers.size(); j++) {
            if (abs(numbers[i]-numbers[j]) < threshold) {
                return true;
            }
        }
    }
    return false;
}
```

### Полный пример промта: Java → Python

```
code translation
Java:
public class Solution {
    public static boolean hasCloseElements(int[] nums, int threshold) {
        for (int i = 0; i < nums.length - 1; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (Math.abs(nums[i] - nums[j]) < threshold) {
                    return true;
                }
            }
        }
        return false;
    }
}
Python:
```

**Ожидаемая генерация модели:**
```python
def has_close_elements(nums, threshold):
    for i in range(len(nums) - 1):
        for j in range(i + 1, len(nums)):
            if abs(nums[i] - nums[j]) < threshold:
                return True
    return False
```

### Использование в коде

#### В функции read_translation_dataset()
```python
def read_translation_dataset(src_path, tgt_path):
    dataset_src = stream_jsonl(src_path)
    dataset_tgt = stream_jsonl(tgt_path)
    lang_src = dataset_src[0]["language"]
    lang_tgt = dataset_tgt[0]["language"]
    prompts, solutions, references = [], [], []

    for k in tqdm(range(len(dataset_src))):
        prompt = f'''code translation
{lang_src}:
{dataset_src[k]["declaration"]}
{dataset_src[k]["canonical_solution"]}
{lang_tgt}:
{dataset_tgt[k]["declaration"]}
'''
        prompts.append(prompt)
        solutions.append(dataset_tgt[k]["canonical_solution"])
        references.append(dataset_tgt[k])

    return prompts, solutions, references
```

### Важные особенности

1. **Жесткая структура** - формат "code translation" должен быть точно соблюден
2. **Двоеточие после языка обязательно** - "Python:" а не "Python"
3. **Полный исходный код** - включает и declaration, и solution для контекста
4. **Только declaration целевого языка** - модель должна сгенерировать solution
5. **Сохранение семантики** - модель переводит логику, а не дословно код
6. **Адаптация к идиомам языка** - результат использует идиоматичные конструкции целевого языка

### Поддерживаемые направления перевода

CodeGeeX поддерживает перевод между любой парой из следующих языков:
- Python
- Java
- C++
- JavaScript
- Go
- Rust

Всего возможно **30 направлений перевода** (6 языков × 5 целевых языков для каждого).

### Тестовые данные

**Файл:** `tests/test_prompt.txt`

Этот файл содержит тестовый промт для проверки функциональности перевода:

```
code translation
Java:
public class Solution {
    public static boolean hasCloseElements(int[] nums, int threshold) {
        for (int i = 0; i < nums.length - 1; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (Math.abs(nums[i] - nums[j]) < threshold) {
                    return true;
                }
            }
        }
        return false;
    }
}
Python:
```

---

## 3. Примеры промтов для генерации кода

### Описание
Коллекция демонстрационных промтов, используемых в веб-интерфейсе Gradio для показа возможностей CodeGeeX. Эти примеры охватывают 15 различных языков программирования и демонстрируют паттерны написания эффективных промтов для генерации кода.

### Расположение
- **Файл:** `deployment/example_inputs.jsonl`
- **Использование:** `deployment/server_gradio.py` (загружает примеры для UI)

### Назначение
- Демонстрация возможностей модели для новых пользователей
- Примеры правильного форматирования промтов
- Быстрый старт работы с системой (кнопка "Select Example")
- Тестирование базовой функциональности на разных языках

### Общий паттерн промтов для генерации кода

```
[Комментарий с описанием задачи на естественном языке]
[Сигнатура функции/класса/структуры кода]
```

Ключевые принципы:
1. Описание пишется в комментариях, соответствующих стилю языка
2. Описание должно быть четким и конкретным
3. После описания следует начало кода (сигнатура, объявление)
4. Модель дополнит код после этой точки

---

### 3.1. Python - Суммирование чисел

**Язык:** Python
**Задача:** Базовая математическая функция

```python
# Write a function that returns the sum of the numbers from 1 to n.
# For example, if n is 5, then the function should return 1 + 2 + 3 + 4 + 5.

# You may assume that n is a positive integer.
def sum_of_numbers(n):
```

**Особенности:**
- Использование # для комментариев
- Четкое описание ожидаемого поведения с примером
- Указание предусловий (n - положительное целое)
- Имя функции описывает ее назначение

---

### 3.2. C++ - Суммирование чисел

**Язык:** C++
**Задача:** Та же задача на C++

```cpp
// Write a function that returns the sum of the numbers from 1 to n.
// For example, if n is 5, then the function should return 1 + 2 + 3 + 4 + 5.

#include <iostream>
using namespace std;
int sum_of_numbers(int n) {
```

**Особенности:**
- Использование // для комментариев (стиль C++)
- Включены необходимые заголовочные файлы
- Указан тип возвращаемого значения и параметра

---

### 3.3. C - Суммирование чисел

**Язык:** C
**Задача:** Та же задача на чистом C

```c
// Write a function that returns the sum of the numbers from 1 to n.
// For example, if n is 5, then the function should return 1 + 2 + 3 + 4 + 5.

#include <stdio.h>
#include <stdlib.h>
int sum(int n)
{
```

**Особенности:**
- Стандартные заголовочные файлы для C (stdio.h, stdlib.h)
- Стиль открывающей скобки на новой строке (K&R style)

---

### 3.4. C# - Суммирование чисел

**Язык:** C#
**Задача:** Та же задача на C#

```csharp
// Write a function that returns the sum of the numbers from 1 to n.
// For example, if n is 5, then the function should return 1 + 2 + 3 + 4 + 5.
private int sum(int n) {
```

**Особенности:**
- Модификатор доступа (private)
- Синтаксис методов C#

---

### 3.5. Java - Суммирование чисел

**Язык:** Java
**Задача:** Та же задача на Java с классом

```java
// Write a function that returns the sum of the numbers from 1 to n.
// For example, if n is 5, then the function should return 1 + 2 + 3 + 4 + 5.

public class SumOfNumbers {
```

**Особенности:**
- Начинается с объявления public class (требование Java)
- Модель должна сгенерировать метод внутри класса

---

### 3.6. HTML - Создание домашней страницы

**Язык:** HTML
**Задача:** Веб-верстка

```html
<!--Write a homepage of CodeGeeX.-->

<div class="container">
```

**Особенности:**
- Использование HTML комментариев <!-- -->
- Начало контейнера для содержимого
- Модель сгенерирует структуру HTML страницы

---

### 3.7. PHP - Суммирование с проверками

**Язык:** PHP
**Задача:** Функция с обработкой edge cases

```php
// Write a function that returns the sum of the numbers from 1 to n.
// For example, if n is 5, then the function should return 1 + 2 + 3 + 4 + 5.
// If n is 0, then the function should return 0.
// If n is less than 0, then the function should return -1.
/**
 * @param {number} n
 * @return {number}
 */
function sum ($n) {
```

**Особенности:**
- Более детальное описание с edge cases (n=0, n<0)
- PHPDoc комментарии для документации
- Показывает, как модель обрабатывает сложные требования

---

### 3.8. JavaScript - Базовая функция

**Язык:** JavaScript
**Задача:** Простая функция суммирования

```javascript
// Write a function that returns the sum of the numbers from 1 to n.
// For example, if n is 5, then the function should return 1 + 2 + 3 + 4 + 5.

function sum(n) {
```

**Особенности:**
- Простой синтаксис JS функций
- Отсутствие типов (динамическая типизация)

---

### 3.9. TypeScript - Цикл for

**Язык:** TypeScript
**Задача:** Вариант с указанием конкретного подхода (for loop)

```typescript
// Write a function that returns the sum of the numbers from 1 to n,
// but using a for loop instead of a while loop.

function sumForLoop(n) {
```

**Особенности:**
- Явное указание требуемого алгоритма (for loop)
- Имя функции отражает используемый подход
- Демонстрация детального контроля над генерацией

---

### 3.10. Go - Цикл for

**Язык:** Go
**Задача:** Та же задача с for loop на Go

```go
// Write a function that returns the sum of the numbers from 1 to n,
// but using a for loop instead of a while loop.

func sumN(n int) int {
```

**Особенности:**
- Синтаксис Go: типы после имен параметров
- func ключевое слово
- Типизированное возвращаемое значение

---

### 3.11. Rust - Цикл for

**Язык:** Rust
**Задача:** Та же задача на Rust

```rust
// Write a function that returns the sum of the numbers from 1 to n,
// but using a for loop instead of a while loop.

fn sum_numbers(n: usize) -> usize {
```

**Особенности:**
- fn для объявления функций
- Тип usize (беззнаковое целое размером с указатель)
- Синтаксис -> для типа возврата

---

### 3.12. SQL - Запросы к базе данных

**Язык:** SQL
**Задача:** Операции с таблицей

```sql
-- Search all the records from the table CodeGeeX
-- Delete iterms with odd indices
```

**Особенности:**
- Использование -- для комментариев
- Описание операций с данными
- Модель сгенерирует SELECT и DELETE запросы

---

### 3.13. Kotlin - Суммирование

**Язык:** Kotlin
**Задача:** Базовая функция на Kotlin

```kotlin
// Write a function that returns the sum of the numbers from 1 to n.
// For example, if n is 5, then the function should return 1 + 2 + 3 + 4 + 5.

fun sum(n: Int): Int {
```

**Особенности:**
- fun ключевое слово (как в Scala)
- Типы с заглавной буквы (Int)
- Двоеточие перед типом

---

### 3.14. Fortran - Модуль с функцией

**Язык:** Fortran
**Задача:** Научные вычисления

```fortran
! Write a function that returns the sum of the numbers from 1 to n.
! For example, if n is 5, then the function should return 1 + 2 + 3 + 4 + 5.

! Use the following header:
! module sum_numbers
! end
module sum_numbers
```

**Особенности:**
- Использование ! для комментариев
- Структура модуля Fortran
- Явное указание требуемой структуры кода

---

### 3.15. R - Статистическая функция

**Язык:** R
**Задача:** Базовая функция на R

```r
# Write a function that returns the sum of the numbers from 1 to n.
# For example, if n is 5, then the function should return 1 + 2 + 3 + 4 + 5.
sum_numbers <- function(n) {
```

**Особенности:**
- Синтаксис R: <- для присваивания
- function() для объявления функций
- Стиль именования snake_case

---

### Паттерны и best practices

Из этих примеров видны следующие паттерны успешных промтов:

1. **Ясность описания**
   - Четко сформулируйте, что должна делать функция
   - Приведите примеры входных и выходных данных

2. **Спецификация edge cases**
   - Укажите особые случаи (n=0, n<0, пустой массив)
   - Определите ожидаемое поведение

3. **Контекст кода**
   - Включайте необходимые импорты/заголовочные файлы
   - Предоставьте сигнатуру функции с типами

4. **Стиль комментариев**
   - Используйте стиль комментариев, принятый в языке
   - // для C-подобных, # для Python-подобных, -- для SQL

5. **Детализация алгоритма** (опционально)
   - Можно указать конкретный подход ("using for loop")
   - Можно указать структуру данных ("using hash map")

6. **Имена переменных и функций**
   - Используйте осмысленные имена
   - Следуйте конвенциям языка (camelCase, snake_case)

