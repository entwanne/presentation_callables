# Fonctions

## Fonctions

- Une fonction est une opération qui calcule un résultat qu'elle renvoie à partir d'arguments qu'on lui donne
- Elle est appelée à l'aide d'une paire de parenthèses

```pycon
>>> abs(5)
```

```pycon
>>> abs(-1.2)
```

## Fonctions

- Elle travaille sur tous types de valeurs

```pycon
>>> len('Hello')
```

```pycon
>>> all([True, 'foo', 4, {'key': 0}])
```

## Fonctions

- Une fonction ne prend pas nécessairement d'arguments

```pycon
>>> input()
```

## Fonctions

- Mais renvoie toujours une et une seule valeur

```pycon
>>> print("abc")
```

```pycon
>>> ret = print("abc")
>>> print(ret)
```

---

```pycon
>>> divmod(7, 2)
```

---

```pycon
>>> a, b = divmod(7, 2)
```

## Fonctions

- Tout appel de fonction peut ainsi être utilisé au sein d'une expression

```pycon
>>> round(3.5) * 2 + abs(-5)
```

## Fonctions

- Une fonction ne renvoie pas nécessairement toujours la même valeur pour les mêmes arguments

```pycon
>>> import random
>>> random.randrange(10)
```

---

```pycon
>>> it = iter(range(10))
```

```pycon
>>> next(it)
```

## Définition de fonction

- On définit une fonction à l'aide du mot-clé `def`
- La définition associe une fonction à un nom et lui spécifie une liste de paramètres
- Le bloc de code suivant la ligne de définition est le corps de la fonction
- La valeur de retour d'une fonction est renvoyée à l'aide du mot-clé `return`

```python
def addition(a, b):
    return a + b
```

```pycon
>>> addition(3, 5)
```
