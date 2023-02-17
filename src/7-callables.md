# Callables

## Callables

- La builtin `callable` permet de tester si un objet est callable

---

- Celle-ci vérifie qu'un objet possède une méthode `__call__`

---

- Un callable est ainsi un objet avec une telle méthode

## Callables

```python
class Adder:
    def __init__(self, add):
        self.add = add

    def __call__(self, x):
        return self.add + x
```

---

```pycon
>>> add_5 = Adder(5)
>>> callable(add_5)
```

---

```pycon
>>> add_5(3)
```

## Construction de callable

- On peut alors imaginer construire une fonction à partir d'objets callables

```python
class Expr:
    def __call__(self, **env):
        raise NotImplementedError
```

## Construction de callable

```python
class Scalar(Expr):
    def __init__(self, value):
        self.value = value

    def __repr__(self):
        return repr(self.value)

    def __call__(self, **env):
        return self.value
```

## Construction de callable

```python
class Operation(Expr):
    def __init__(self, op_func, op_repr, *args):
        self.func = op_func
        self.repr = op_repr
        self.args = args

    def __repr__(self):
        return self.repr(*self.args)

    def __call__(self, **env):
        values = (arg(**env) for arg in self.args)
        return self.func(*values)
```

## Construction de callable

```python
class Symbol(Expr):
    def __init__(self, name):
        self.name = name

    def __repr__(self):
        return self.name

    def __call__(self, **env):
        if self.name in env:
            return env[self.name]
        return self
```

## Construction de callable

```python
def make_op(op_func, op_fmt):
    return functools.partial(Operation, op_func, op_fmt)
```

```python
add = make_op(operator.add, '{} + {}'.format)
sub = make_op(operator.sub, '{} - {}'.format)
mul = make_op(operator.mul, '{} * {}'.format)
div = make_op(operator.truediv, '{} / {}'.format)
fdiv = make_op(operator.floordiv, '{} // {}'.format)
mod = make_op(operator.mod, '{} % {}'.format)
pow = make_op(operator.pow, '{} ** {}'.format)
```

## Construction de callable

```pycon
>>> expr = add(pow(Symbol('x'), Scalar(2)), Scalar(5))
>>> expr
```

```pycon
>>> expr(x=3)
```

## Construction de callable

```python
def function(func):
    def op_repr(*args):
        return f"{func.__name__}({', '.join(repr(arg) for arg in args)})"
    return functools.partial(Operation, func, op_repr)
```

```pycon
>>> import math
>>> expr = function(math.cos)(mul(Symbol('x'), Scalar(math.pi)))
>>> expr
```

```pycon
>>> expr(x=1)
```

## Construction de callable

```python
def ensure_expr(x):
    if isinstance(x, Expr):
        return x
    return Scalar(x)
```

```python
def binop(op_func):
    return lambda lhs, rhs: op_func(ensure_expr(lhs), ensure_expr(rhs))

def rev_binop(op_func):
    return lambda rhs, lhs: op_func(ensure_expr(lhs), ensure_expr(rhs))

Expr.__add__ = binop(add)
Expr.__radd__ = rev_binop(add)
Expr.__sub__ = binop(sub)
Expr.__rsub__ = rev_binop(sub)
Expr.__mul__ = binop(mul)
Expr.__rmul__ = rev_binop(mul)
Expr.__truediv__ = binop(div)
Expr.__rtruediv__ = rev_binop(div)
Expr.__floordiv__ = binop(fdiv)
Expr.__rfloordiv__ = rev_binop(fdiv)
Expr.__mod__ = binop(mod)
Expr.__rmod__ = rev_binop(mod)
Expr.__pow__ = binop(pow)
Expr.__rpow__ = rev_binop(pow)
```

## Construction de callable

```pycon
>>> expr = 3 * Symbol('x') ** 2 + 2
>>> expr
```

```pycon
>>> expr(x=10)
```
