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

