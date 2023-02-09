# Arguments des appelables

## Arguments

- Les arguments sont les valeurs envoyées à un _callable_

```pycon
>>> addition(3, 5)
```

---

- Ils peuvent être positionnels ou nommés

```pycon
>>> addition(a=3, b=5)
```

## Ordre de placement des arguments

- Les arguments positionnels se placent toujours avant les arguments nommés

```pycon
>>> addition(3, b=5)
```

---

```pycon
>>> addition(a=3, 5)
```

## Arguments et paramètres

- Ne pas confondre les arguments avec les paramètres qui sont les variables dans lesquelles sont reçus les arguments
- Un argument ne peut correspondre qu'à un seul paramètre

## Arguments et paramètres

- `a` et `b` sont les paramètres de la fonction `addition`
    ```python
    def addition(a, b):
        return a + b
    ```
- `3` et `5` sont les arguments de l'appel
    ```python
    addition(3, b=5)
    ```
- `5` est un argument nommé associé au nom `b`

## Arguments et paramètres

- Les arguments positionnels remplissent les paramètres de la gauche vers la droite
- Tandis que les arguments nommés remplissent les paramètres correspondant à leurs noms

## Valeur par défaut

- Un paramètre peut définir une valeur par défaut
- Elle sera utilisée si aucun argument n'est fourni pour ce paramètre

```python
def multiplication(a, b=1):
    return a * b
```

---

```pycon
>>> multiplication(3, 4)
```

---

```pycon
>>> multiplication(5)
```

## Valeur par défaut

- Attention : cette valeur par défaut est définie une seule fois pour la fonction

```pycon
>>> def append(item, dest=[]):
...     dest.append(item)
...     return dest
```

---

```pycon
>>> append(4, [1, 2, 3])
```

---

```pycon
>>> append('hello')
```

## Différentes sortes de paramètres

- Les paramètres peuvent être de plusieurs sortes :
    - _posititonal-only_, qui ne peuvent recevoir que des arguments positionnels
    - _keyword-only_, qui ne peuvent recevoir que des arguments nommés
    - _positional-or-keyword_, qui peuvent recevoir à la fois des arguments positionnels ou nommés

- Jusqu'ici nos paramètres étaient tous de type _positional-or-keyword_

## Différentes sortes de paramètres

- Il est possible à l'aide des délimiteurs `/` et `*` de spécifier la sorte de nos paramètres :
    - Les paramètres placés avant `/` sont _positional-only_
    - Les paramètres placés après `*` sont _keyword-only_
    - Les autres paramètres sont _positional-or-keyword_
- Ces délimiteurs sont bien sûr optionnels

## Différentes sortes de paramètres

```python
def function(first, /, second, third, *, fourth):
    ...
```

- `first` est _positional-only_
- `second` et `third` sont _positional-or-keyword_
- `fourth` est _positional-only_

## Différentes sortes de paramètres

- À l'appel cela signifie que `first` ne peut pas recevoir d'argument nommé et `fourth` ne peut pas recevoir d'argument positionnel

```pycon
>>> function(1, 2, 3, fourth=4)
```

```pycon
>>> function(1, second=2, third=3, fourth=4)
```

```pycon
>>> function(1, 2, 3, 4)
```

```pycon
>>> function(first=1, second=2, third=3, fourth=4)
```

## Ordre de définition des paramètres

- Les délimiteurs font qu'il y a un ordre à respecter pour définir les différents paramètres
    - D'abord _positional-only_, puis _positional-or-keyword_ et enfin _keyword-only_
- Les valeurs par défaut des paramètres sont aussi à prendre en compte dans l'ordre de définition

## Ordre de définition des paramètres

- Chez les arguments qui peuvent être positionnels (_positional-only_ ou _positional-or-keyword_) un paramètre sans valeur par défaut ne peut pas suivre un paramètre qui en a une

```python
def function(first, /, second, third=3):
    ...
```

```python
def function(first, /, second=2, third):
    ...
```

```python
def function(first=1, /, second, third):
    ...
```

## Ordre de définition des paramètres

- Le problème ne se pose pas pour les paramètres _keyword-only_ puisque l'ordre des arguments n'y a pas d'importance

```python
def function(foo=None, *, bar=True, baz):
    return (foo, bar, baz)
```

```pycon
>>> function(baz=False)
```

## Paramètres variadiques

- Il existe aussi des paramètres spéciaux pour récupérer tout un ensemble d'arguments, qu'on appelle paramètres variadiques
    - Ce nom vient du fait qu'ils récupèrent un nombre variable d'arguments…
    - Autrement dit des arguments variadiques

## Paramètres variadiques

- Un paramètre préfixé d'un `*` récupère tous les arguments positionnels restants
    - Il prend alors la place du délimiteur `*` dans la liste des paramètres
    - C'est-à-dire qu'il se place nécessairement après tous les paramètres qui peuvent recevoir des arguments positionnels
    - Il ne peut y avoir qu'un paramètre préfixé d'un `*`
    - Ce paramètre contiendra alors un tuple des arguments positionnels donnés à la fonction
    - Les arguments positionnels qui correspondent à un paramètre précis ne seront pas inclus dans ce tuple
    - Il est d'usage d'appeler ce paramètre `args`

## Paramètres variadiques

```python
def my_sum(*args):
    total = 0
    for item in args:
        total += item
    return total
```

```pycon
>>> my_sum(1, 2, 3)
```

```pycon
>>> my_sum()
```

## Paramètres variadiques

- Ils peuvent bien sûr être combinés avec d'autres sortes de paramètres

```python
def my_sum(first, /, *args):
    total = first
    for item in args:
        total += item
    return total
```

```pycon
>>> my_sum(1, 2, 3)
```

```pycon
>>> my_sum('a', 'b', 'c')
```

```pycon
>>> my_sum()
```

## Paramètres variadiques

- C'est aussi ce qui est utilisé par la fonction `print` par exemple pour accepter un nombre arbitraire d'arguments

```pycon
>>> print(1, 'foo', ['hello', 'world'])
```

## Paramètres variadiques

- De manière similaire, le préfixe `**` permet de définir un paramètre qui récupère tous les arguments nommés restants
    - Ce paramètre se place nécessairement après tous les autres
    - Il est unique lui aussi
    - Il contient le dictionnaire des arguments nommés passés à la fonction
    - Seuls les arguments nommés ne correspondant à aucun autre paramètre sont présents dans ce dictionnaire
    - Il est d'usage d'appeler ce paramètre `kwargs`

## Paramètres variadiques

```python
def make_dict(**kwargs):
    return kwargs
```

```pycon
>>> make_dict(foo=1, bar=2)
```

## Paramètres variadiques

- Combinables eux-aussi avec d'autres sortes de paramètres

```python
def make_obj(id, **kwargs):
    return {'id': id} | kwargs
```

```pycon
>>> make_obj('#1', foo='bar')
```

```pycon
>>> make_obj(id='#1', foo='baz')
```

## Opérateurs _splat_

- Ces délimiteurs `*` et `**` ne sont pas utilisables uniquement dans les listes de paramètres
- On les retrouve aussi dans les listes d'arguments, sous le nom d'opérateurs _splat_
- Ils ont l'effet inverse de celui en place pour les paramètres

## Opérateurs _splat_

- Ainsi `*` appliqué à une liste (ou tout autre itérable) transforme ses éléments en arguments positionnels

```pycon
>>> addition(*[3, 5])
```

```pycon
>>> print(*(i**2 for i in range(10)))
```

## Opérateurs _splat_

- Et contrairement aux paramètres, on peut appliquer le _splat_ à plusieurs arguments
- On peut aussi préciser d'autres arguments

```pycon
>>> my_sum(*range(5), 10, *range(3))
```

## Opérateurs _splat_

- Quant à `**` il s'applique à un dictionnaire (ou similaire) et transforme les éléments en arguments nommés
- Les clés du dictionnaires doivent alors être des chaînes de caractères

```pycon
>>> addition(**{'a': 3, 'b': 5})
```

## Unpacking

- On retrouve d'ailleurs aussi l'opérateur _splat_ dans les opérations d'_unpacking_
- L'_unpacking_ consiste à extraire des valeurs depuis un itérable à l'aide d'une assignation spéciale

```pycon
>>> first, *middle, last = *range(5), 8, *range(3)
```

```pycon
>>> first
```

```pycon
>>> middle
```

```pycon
>>> last
```
