# Autres callables

## Callables

- Les fonctions sont des callables / appelables (objets que l'on peut appeler à l'aide de parenthèses)
- Mais il n'y a pas que les fonctions qui sont appelables

## Types

- Les types sont appelables, pour en créer des instances

```pycon
>>> int('123')
```

---

```pycon
>>> str()
```

---

```pycon
>>> range(10)
```

## Méthodes

- Les méthodes des objets sont appelables

```pycon
>>> "Hello".replace("l", "_")
```

---

- De même que les méthodes de classes

```pycon
>>> dict.fromkeys([1, 2, 3])
```

## Lambdas

- Les lambdas sont des définitions de fonctions sous forme d'expressions

```pycon
>>> f = lambda x: x+1
>>> f(5)
```

---

```pycon
>>> (lambda i: i**2)(4)
```

---

- On parle aussi de fonctions anonymes

## Autres

- Les objects `functools.partial` sont des appels partiels de fonctions
- Ils stockent les arguments donnés à la création pour les réutiliser lors de l'appel final

```pycon
>>> import functools
>>> f = functools.partial(max, 0)
>>> f(1, 2)
```

---

```pycon
>>> f(-1, -2)
```
