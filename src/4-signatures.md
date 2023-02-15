# Signatures de fonctions

## Signatures de fonctions

- La signature d'une fonction représente son interface, décrivant comment on peut l'appeler
- La fonction `signature` du module `inspect` permet de récupérer la signature d'une fonction

```pycon
>>> import inspect
>>> inspect.signature(addition)
```

---

- On voit ainsi que notre fonction `addition` attend deux paramètres `a` et `b`

## Signatures

- L'object renvoyé par `inspect.signature` permet d'explorer la signature de la fonction

```pycon
>>> sig = inspect.signature(addition)
>>> sig.parameters
```

---

```pycon
>>> sig.parameters.keys()
```

---

```pycon
>>> sig.parameters['a']
```

---

```pycon
>>> sig.parameters['a'].kind
```

## Signatures

- On peut aussi connaître la valeur par défaut d'un paramètre

```pycon
>>> sigmul = inspect.signature(multiplication)
>>> sigmul.parameters['b']
```

---

```pycon
>>> sigmul.parameters['b'].default
```

## Binding

- On peut utiliser une signature pour réaliser un _binding_ sur des arguments
- C'est-à-dire faire la correspondance entre les arguments et les paramètres

```pycon
>>> binding = sig.bind(3, b=5)
>>> binding
```

---

```pycon
>>> binding.arguments
```

---

```pycon
>>> binding.args, binding.kwargs
```

---

- C'est une manière de normaliser les arguments passés lors d'un appel
    - Tous ceux qui peuvent être positionnels sont stockés dans `args` et les autres dans `kwargs`

## Binding

- Les mêmes vérifications ont lieu que lors d'un appel
- Il faut ainsi préciser tous les arguments nécessaires à l'appel

```pycon
>>> sig.bind(5)
```

## Binding

- Il est aussi possible d'appliquer les valeurs par défaut des paramètres sur un _binding_

```pycon
>>> binding = sigmul.bind(10)
>>> binding
```

```pycon
>>> binding.apply_defaults()
>>> binding
```

## Modification de signature

- L'objet signature en lui-même est inaltérable
- Mais il possède une méthode `replace` pour en créer une copie modifiée

---

```pycon
>>> sig.replace(parameters=[])
```

## Modification de signature

- Il en est de même pour les objets représentant les paramètres

```pycon
>>> from inspect import Parameter
>>> sig.parameters['a'].replace(kind=Parameter.POSITIONAL_ONLY)
```

## Modification de signature

- On peut alors dériver notre signature pour n'accepter que les arguments positionnels

```pycon
>>> newsig = sig.replace(parameters=[p.replace(kind=Parameter.POSITIONAL_ONLY) for p in sig.parameters.values()])
>>> newsig
```

---

```pycon
>>> newsig.bind(1, 2)
```

---

```pycon
>>> newsig.bind(a=1, b=2)
```

## Modification

- On peut ensuite assigner la nouvelle signature à l'attribut `__signature__` de la fonction

```pycon
>>> addition.__signature__ = newsig
```

---

- Cela change bien la signature renvoyée par `inspect.signature`

```pycon
>>> inspect.signature(addition)
```

---

- Mais n'affecte pas le comportement réel de la fonction

```pycon
>>> addition(a=1, b=2)
```

## Annotations

- La signature d'une fonction comprend aussi les annotations de paramètres et de retour
- Les annotations permettent de préciser les types attendus

```pycon
def addition(a: int, b: int) -> int:
    return a + b
```

---

```pycon
>>> sig = inspect.signature(addition)
>>> sig
```

## Annotations

- Les annotations sont exposées dans les attributs de la signature

```pycon
>>> sig.return_annotation
```

```pycon
>>> sig.parameters['a'].annotation
```

## Annotations

- Elles sont aussi exposées dans l'attribut spécial `__annotations__` de la fonction

```pycon
>>> addition.__annotations__
```

- Ainsi que via `inspect.get_annotations`

```pycon
>>> inspect.get_annotations(addition)
```

## Annotations

- `inspect.get_annotations` est préférable
    - Elle gère les cas d'absence de `__annotations__` sur l'objet et différents problèmes potentiels
    - Elle permet l'évaluation dynamique des annotations

## Annotations

```python
def addition(a: "int", b: "int") -> "int":
    return a + b
```

```pycon
>>> addition.__annotations__
```

```pycon
>>> inspect.get_annotations(addition)
```

---

```pycon
>>> inspect.get_annotations(addition, eval_str=True)
```

## Documentation

- La documentation permet d'expliciter le comportement d'une fonction
- Cela se fait en Python à l'aide de docstrings

```python
def addition(a: int, b: int) -> int:
    "Return the sum of two integers"
    return a + b
```

- Celle-ci n'est pas exposée dans la signature

## Documentation

- La docstring est exposée dans l'argument `__doc__` de la fonction

```pycon
>>> addition.__doc__
```

- Ou via `inspect.getdoc`

```pycon
>>> inspect.getdoc(addition)
```

## Documentation

- Cette dernière est préférable pour un meilleur formattage

```python
def function():
    """
    Docstring of the function
    on multiple lines
    """
```

```pycon
>>> function.__doc__
```

```pycon
>>> inspect.getdoc(function)
```
