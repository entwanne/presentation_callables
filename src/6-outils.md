# Outils

## Outils

- Python propose différents outils autour des callables
- Afin de les identifier et les manipuler

## Builtins

- La fonction `callable` permet de savoir si un objet est _callable_

---

```pycon
>>> callable(len)
```

---

```pycon
>>> callable(str)
```

---

```pycon
>>> callable(str.replace)
```

---

```pycon
>>> callable(lambda: True)
```

---

```pycon
>>> callable(5)
```

## Module functools

- On a vu `functools.partial` pour l'application partielle
- Elle gère les arguments positionnels et nommés

```python
import functools

debug = functools.partial(print, '[DEBUG]', sep=' - ')
```

```pycon
debug(1, 2, 3)
```

## Module functools

- On a écrit plus tôt des décorateurs qui enrobaient nos fonctions

```python
@decorator
def addition(a: int, b: int) -> int:
    "Return the sum of two integers"
    return a + b
```

---

- Le problème est qu'on perd la signature et la documentation de la fonction initiale

```pycon
>>> inspect.signature(addition)
```

```pycon
>>> inspect.getdoc(addition)
```

## Module functools

- On pourrait copier manuellement `__doc__`, `__signature__` et autres

---

- Mais `functools` fournit une fonction `update_wrapper` pour faire cela plus simplement

```python
def decorator(func):
    def wrapper(*args, **kwargs):
        print('Calling decorated function')
        return func(*args, **kwargs)
    functools.update_wrapper(wrapper, func)
    return wrapper
```

## Module functools

```python
@decorator
def addition(a: int, b: int) -> int:
    "Return the sum of two integers"
    return a + b
```

```pycon
>>> inspect.signature(addition)
```

```pycon
>>> inspect.getdoc(addition)
```

## Module functools

- Cela fonctionne entre-autres en assignant un attribut `__wrapped__` à notre fonction

```pycon
>>> addition.__wrapped__
```

## Module functools

- `functools` possède aussi un décorateur `wraps` pour faciliter cela

```python
def decorator(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print('Calling decorated function')
        return func(*args, **kwargs)
    return wrapper
```

## Module operator

- Le module `operator` expose les différents opérateurs du langage
- `operator.call` permet notamment d'appeler un callable (Python 3.11)

```pycon
>>> import operator
>>> operator.call(addition, 1, 2)
```

## Module operator

- La fonction `itemgetter` renvoie un callable pour récupérer un élément d'un conteneur donné

```pycon
>>> get_name = operator.itemgetter('name')
>>> get_name({'name': 'John'})
```

---

```pycon
>>> get_fullname = operator.itemgetter('firstname', 'lastname')
>>> get_fullname({'firstname': 'Jude', 'lastname': 'Doe'})
```

## Module operator

- Ainsi que la fonction `attrgetter` pour récupérer un attribut

```pycon
>>> get_module = operator.attrgetter('__module__')
>>> get_module(int)
```

## Module operator

- Et `methodcaller` pour appeler une méthode sur un objet donné

```pycon
>>> replace = operator.methodcaller('replace', 'o', 'a')
>>> replace('toto')
```

## Module inspect

- Et pour rappel le module `inspect` dédié à l'introspection
- `inspect.isfunction`
- `inspect.getsource`
