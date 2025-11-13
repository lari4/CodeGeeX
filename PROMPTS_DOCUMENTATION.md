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

