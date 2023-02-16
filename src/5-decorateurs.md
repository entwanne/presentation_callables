# Décorateurs

## Décorateurs

- Les décorateurs sont un mécanisme permettant de transformer des callables
- Ils s'appliquent à des fonctions pour en modifier le comportement

---

```python
import functools

@functools.cache
def addition(a, b):
    print(f'Computing {a}+{b}')
    return a + b
```

```pycon
>>> addition(3, 5)
```

```pycon
>>> addition(1, 2)
```

## Décorateurs

- `functools.cache` a remplacé `addition` par une nouvelle fonction avec un mécanisme de cache

---

- La syntaxe précédente est équivalente à :

```pycon
def addition(a, b):
    print(f'Computing {a}+{b}')
    return a + b

addition = functools.cache(addition)
```

## Décorateurs

- On peut aussi appliquer plusieurs décorateurs à la suite

    ```python
    @deco1
    @deco2
    def function():
        ...
    ```

---

- Qui est équivalent à :

    ```python
    def function():
        ...

    function = deco1(deco2(function))
    ```

## Écriture de décorateurs

- Un décorateur est donc un callable qui reçoit un callable et renvoie un callable

---

- 🤯

---

```python
def decorator(func):
    return func
```

---

```python
@decorator
def addition(a, b):
    return a + b
```

```pycon
>>> addition(3, 5)
```

## Écriture de décorateurs

- Pour changer le comportement de la fonction décorée, il faut créer une nouvelle fonction reprenant son interface

```python
def decorator(func):
    def wrapper(*args, **kwargs):
        print('Calling decorated function')
        return func(*args, **kwargs)
    return wrapper
```

```python
@decorator
def addition(a, b):
    return a + b
```

```pycon
>>> addition(3, 5)
```

## Écriture de décorateurs

- `functools.cache` pourrait être réécrit ainsi

```python
def cache(func):
    func_cache = {}

    def wrapper(*args, **kwargs):
        # make an hashable key
        key = args, tuple(kwargs.items())
        if key not in func_cache:
            func_cache[key] = func(*args, **kwargs)
        return func_cache[key]

    return wrapper
```

---

```python
@cache
def addition(a, b):
    print(f'Computing {a}+{b}')
    return a + b
```

```pycon
>>> addition(3, 5)
```

## Décorateurs paramétrés

- Un décorateur ne peut pas être paramétré à proprement parler

---

- Mais on peut appeler une fonction renvoyant un décorateur

```python
@functools.lru_cache(maxsize=1)
def addition(a, b):
    print(f'Computing {a}+{b}')
    return a + b
```

```pycon
>>> addition(3, 5)
```

```pycon
>>> addition(1, 2)
```

## Décorateurs paramétrés

- Un décorateur paramétré est ainsi un callable renvoyant un callable recevant un callable et renvoyant à nouveau un callable

---

- 🤯 🤯 🤯

---

```python
def param_decorator(n):
    def decorator(func):
        def wrapper(*args, **kwargs):
            print(f'Function decorated with {n}')
            return func(*args, **kwargs)
        return wrapper
    return decorator
```

```python
@param_decorator(42)
def function():
    ...
```

```pycon
>>> function()
```
