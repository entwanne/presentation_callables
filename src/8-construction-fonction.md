# Construction de fonction

## Construction de fonction

```pycon
>>> func.__call__.__call__.__call__.__call__.__call__.__call__(1)
```

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
