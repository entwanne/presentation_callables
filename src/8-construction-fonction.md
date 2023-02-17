# Construction de fonction

## Fonctions VS callables

- Si une fonction est un callable, alors elle possède une méthode `__call__`

---

- Mais vers quoi pointe cette méthode ?

---

```pycon
>>> addition.__call__
```

```pycon
>>> addition.__call__(3, 5)
```

---

```pycon
>>> addition.__call__.__call__.__call__(3, 5)
```

---

- Un autre mécanisme est donc nécessaire

## Fonctions VS callables

- Les fonctions possèdent un attribut `__code__`
- Celui-ci contient le code (compilé) de la fonction

---

```python
def function():
    print('hello')
```

```pycon
>>> function.__code__
```

---

- Ce code est un objet exécutable

```pycon
>>> exec(function.__code__)
```

---

- Du moins pour les fonctions sans paramètres

## Construction de fonctions

- Une fonction est donc un enrobage autour d'un objet code

---

- On peut construire une fonction en créant une instance `FunctionType` du module `types`

```pycon
>>> import types
>>> newfunc = types.FunctionType(function.__code__, globals())
```

```pycon
>>> newfunc()
```

---

- Mais comment construire un objet code ?

## Construction de fonctions

- La fonction `compile` permet cela
- À partir d'un AST Python ou de code brut

---

- Un nœud `ast.FunctionDef` est alors nécessaire pour construire une fonction
- Celui-ci définit le nom, les paramètres et le corps de la fonction

## Construction de fonctions

```pycon
>>> import ast
>>> func_body = ast.parse("print('hello')").body
>>> func_body
```

---

```pycon
>>> fdef = ast.FunctionDef(
...     name='f',
...     args=ast.arguments(posonlyargs=[], args=[], kwonlyargs=[], kw_defaults=[], defaults=[]),
...     body=func_body,
...     lineno=0,
...     col_offset=0,
...     decorator_list=[],
... )
```

---

```pycon
>>> code = compile(ast.Module(body=[fdef], type_ignores=[]), 'x', 'exec')
>>> func_code = code.co_consts[0]
>>> func_code
```

---

```pycon
>>> f = types.FunctionType(func_code, {})
>>> f()
```

## Construction de fonctions

```python
def create_function(name, body, arg_names):
    function_body = ast.parse(body).body
    args = [ast.arg(arg=arg_name, lineno=0, col_offset=0) for arg_name in arg_names]

    function_def = ast.FunctionDef(
        name=name,
        args=ast.arguments(
            posonlyargs=[],
            args=args,
            kwonlyargs=[],
            defaults=[],
            kw_defaults=[]),
        body = function_body,
        decorator_list=[],
        lineno=0,
        col_offset=0,
    )
    module = compile(ast.Module(body=[function_def], type_ignores=[]), "<string>", "exec")
    function_code = next(c for c in module.co_consts if isinstance(c, types.CodeType))

    return types.FunctionType(function_code, globals())
```

## Construction de fonctions

```pycon
>>> addition = create_function('addition', 'return a + b', ('a', 'b'))
>>> addition
```

```pycon
>>> addition(3, 5)
```
