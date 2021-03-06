---
id: tags
title: Tags
---

A tag is a high level operation, usually representing similar constructions that regular programming languages provide. They perform conditional execution, loops, printing, commenting, etc. Let's check out each of them one-by-one.

<AUTOGENERATED_TABLE_OF_CONTENTS>

---

### `Print`

Print tag is the most basic statement of all, all it does is it forumlates its body as a string expression.
```swift
templateEngine.evaluate("{{ 'This is a string' }}") // This is a string
```

Print statement is smart, it evaluates its body while using functions, variables, and data type instances.
```
templateEngine.evaluate("{{ 2 * round(value) }}", variables: ["value": 2.8]) // 6
```

### `If` and `If-Else`

It's easy to run conditional evaluation:
```swift
Tuxedo().evaluate("{% if true %}Hello{% endif %} {{ name }}!", variables: ["name": "John"]) // Hello John!
```

You can also use one with an `else` clause:
```swift
Tuxedo().evaluate("{% if greeting %}Hello{% else %}Bye{% endif %} {{ name }}!", variables: ["greeting": false, "name": "John"]) // Bye John!
```

### `For`

For loop iterates over an array and creates a local variable with every value in the collection:
```swift
Tuxedo().evaluate("{% for i in [1,2,3] %}{{i * 2}}{% if i is not last %}, {% endif %}{% endfor %}") // 2, 4, 6
```

### `Comment`

Comment statement removes its body and replaces it with an empty string, therefore makes its content disappear, as a commented block in regaular programming languages.
```swift
templateEngine.evaluate("Hello {# commented block #}World!") // Hello World!
```

### `Set`

Set creates named variables and assignes values to them.

It can be expressed in two distinct ways:
```swift
template.evaluate("{% set x = 4.0 %}")
template.evaluate("{% set x %}this{% endset %}")
```

### `Macro`

There's also an option to create custom macros (define basic functions):
```swift
template.evaluate("{% macro duplicate(value) %}value * 2{% endmacro %}{{ duplicate(4) }}") // 8
```

### `Block`

You can define blocks which print the body of them.
```swift
Tuxedo().evaluate("Title: {% block title %}Original{% endblock %}") // Title: Original
```

Blocks allows them to override their contents later
```swift
{% block title %}Other{% endblock %} // Title: Other
```

If you need the previous value, you can access it throug the `parent` method:
```swift
{% block title %}{{ parent() }} + New{% endblock %} // Title: Original + New
```

Or override the parent if it contains any variables:
```swift
Tuxedo().evaluate("""

{% block title %}Hello {{name}}{% endblock %}
{% block title %}{{ parent(name='John') }}!{% endblock %}

""", variables: ["name": "Geroge"]) // Hello John
```

### `Import`

If using template files, it's easy to import one using the import statement:
```swift
{% import 'template.txt' %}
```

It uses a file url path pattern, so `subfolder/template.txt` works when accessing files in a different folder.

### `Spaceless`

You can wrap your code around `spaceless` tags to remove every whitespace in the body.

```swift
Tuxedo().evaluate("{% spaceless %}   {% if true %}    Hello    {% endif %}    {% endspaceless %}") // Hello
```

Or, if you only need to eliminate whitespaces on one side, you can use the special `{-}` notation. If placed anywhere, it removes itself and the whitespaces around it.
```swift
Tuxedo().evaluate("asd   {% if true %}   Hello {-}  {% endif %}   ")
```