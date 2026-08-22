# Laboratorio 04: Alcance estático mediante cerraduras

## Reto 1 — Reconocer lambda
### Lexer.x

Añade `lambda` como **palabra reservada** del lenguaje. Asegúrate de que el analizador léxico la reconozca como una palabra reservada y no como un identificador.

---

## Reto 2 — Construir el ASA fuente
### Grammars.y

Añade las reglas de producción correspondientes a las **abstracciones de función** y a las **aplicaciones de función**.

Considera las siguientes restricciones:

* Los parámetros de una función deben contener **al menos una variable**.
* Los argumentos de una aplicación de función deben contener **al menos una expresión**.

---

## Reto 3 — Currificar y desazucarar 
### Desugar.hs

Implementa la **currificación** de funciones y aplicaciones de función, de manera que el ASA resultante utilice únicamente funciones **uniparamétricas**.

### `curryFun :: [Nombre] -> ASA -> Maybe ASA`

Currifica una función con múltiples parámetros convirtiéndola en una secuencia de funciones con un único parámetro.

Por ejemplo:

```lisp
(lambda (x y z) (+ x y z))
```

debe convertirse en:

```lisp
(lambda (x)
  (lambda (y)
    (lambda (z)
      (+ x y z))))
```


### `curryApp :: ASA -> [ASA] -> Maybe ASA`

Currifica una aplicación de función con múltiples argumentos, convirtiéndola en una secuencia de aplicaciones uniparamétricas.

Por ejemplo:

```lisp
((lambda (x) (and x y)) (not #t #f) #f)
```

debe convertirse en:

```lisp
(((lambda (x) (and x y)) (not #t #f)) #f)
```


### `desugar :: SASA -> Maybe ASA`

Implementa el proceso de **desazucarado** del lenguaje.

Esta función debe:

1. Currificar las funciones y aplicaciones de función.
2. Eliminar el azúcar sintáctica de `let`, transformando cada expresión `let` en una aplicación de función equivalente.
3. Producir como resultado un `ASA` que utilice únicamente los constructores correspondientes al lenguaje sin azúcar.

Por ejemplo, una expresión como:

```lisp
(let ((x 5))
  (+ x 1))
```

debe transformarse en:

```lisp
((lambda (x) (+ x 1)) 5)
```

---

## Reto 4 — Evaluar con cerraduras
### Interp.hs

Define la **semántica operacional de MiniLisp++ mediante cerraduras**, utilizando las reglas de evaluación definidas en el documento de la práctica.

### `freeVars :: ASA -> [Nombre]`

Obtén el conjunto de variables libres que aparecen dentro de una expresión.

La implementación debe respetar el alcance de las variables establecida en la **décima nota de clase**.

### `lookupEnv :: Nombre -> [(Nombre, a)] -> Maybe a`

Implementa la búsqueda de un identificador dentro del ambiente.

La función debe recuperar el **valor asociado a la ocurrencia más reciente** del identificador en el ambiente.

Por ejemplo, dado el ambiente:

```haskell
[("x", 5), ("y", 10), ("x", 20)]
```

la búsqueda de `x` debe devolver el valor correspondiente a su asignación más reciente que es 5.

### `bigStep :: Env -> ASA -> Maybe Value`

Define la semántica operacional de **paso grande** para `MiniLisp++`, siguiendo la estrategia utilizada en las prácticas anteriores.

Para la evaluación de funciones y aplicaciones de función, utiliza las reglas de evaluación mediante **cerraduras** definidas en el documento de esta práctica.

En particular, una cerradura debe conservar la función junto con el ambiente correspondiente a su **definición**, de manera que se respete el **alcance estático** del lenguaje.

---

## Reto 5 — Integrar MiniLisp++ 
### MiniLispPlusPlus.hs

Completa la función:

```haskell
evalua :: String -> Maybe Value
```

Esta función debe realizar todo el proceso de evaluación de un programa, desde el **análisis léxico** hasta la **evaluación**.

El proceso debe seguir las siguientes etapas:

1. Realizar el análisis léxico y sintáctico.
2. Obtener el `SASA` correspondiente.
3. Desazucarar la expresión mediante `desugar`.
4. Si el desazucarado es válido, evaluar el `ASA` resultante mediante `bigStep`.
5. Si alguna de las etapas falla, devolver `Nothing`.

La función `evalua` debe integrar todas las etapas anteriores y producir el resultado final de la evaluación.
