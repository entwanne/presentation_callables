# Bases
- Appeler une fonction : `abs(5)`
- Valeur de retour (expression)
- Ou pas de retour (`print`)

# Il n'y a pas que les fonctions qui sont appelables

- types (`int`)
- méthodes (`"abc".replace("c", "d")`)
- lambdas (fonction-expressions)
- autres (`functools.partial`)

# Arguments des appelables

- Arguments et paramètres
- Arguments positionnels & nommés
- Valeurs par défaut des paramètres (+ cas de la mutabilité)
- Paramètres _positional-only_ (`/`), _keyword-only_ (`*`), variadiques (`*args`, `**kwargs`), limitations (ordre de définition et d'appel)
- Opérateur _splat_ et _unpacking_

# Signatures

- Annotations de types, _docstrings_ (`inspect.getdoc`)
- Signature de fonction (`inspect.signature`)
- _Binding_ de signature (`sig.bind`, `apply_defaults`, `replace`)
- `__signature__`, `__annotations__`, `__wrapped__`

# Décorateurs

- Utilisation de décorateurs simples ou paramétrés
- Application de plusieurs décorateurs
- Définition d'un décorateur simple
- Décorateurs paramétrés

# Outils

- Module `functools` (`functools.update_wrapper`, `functools.wraps`)
- Module `operator`

# Callables

- _builtin_ `callable`
- méthode `__call__`
- Créer des _callables_ à partir d'expressions (objet implémentant différents opérateurs dont `__call__`, solveur d'expressions mathématiques — simpy)
- Attribut `__code__` des fonctions
- Exécuter manuellement une fonction : `exec` (pour fonction sans paramètres)
- Construire une fonction : `types.FunctionType(code_obj, globals, ...)`
- Construire un _code object_: `compile` + `ast.FunctionDef(name, args, body, ...)`

```python
fdef = ast.FunctionDef(name='f', args=ast.arguments(posonlyargs=[], args=[ast.arg(arg='a', lineno=0, col_offset=0), ast.arg(arg='b', lineno=0, col_offset=0)], kwonlyargs=[], kw_defaults=[], defaults=[]), body=ast.parse("print(a+b)").body, lineno=0, col_offset=0, decorator_list=[])
code = compile(ast.Module(body=[fdef], type_ignores=[]), 'x', 'exec')
fcode = code.co_consts[0]
f = types.FunctionType(fcode, {})
```

* <https://github.com/entwanne/fun_python/blob/master/3/dynamic_function.py>
