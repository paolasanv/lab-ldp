# Laboratorio 04: Desazucarado y cerraduras

## Reto 1 — Azúcar sintáctica

### Interp.hs

Implementa el **desazucarado** de funciones, aplicaciones de función, ligaduras y operaciones multiparamétricas, de manera que el ASA resultante utilice únicamente los constructores del lenguaje **sin azúcar sintáctica**.

### `curryFun :: [Nombre] -> ASA -> Maybe ASA`

Implementa la **currificación de funciones**. Una función con múltiples parámetros debe transformarse en una secuencia de funciones anidadas, donde cada función recibe un único parámetro.

Por ejemplo, la expresión:

```text
(lambda (x y z) (+ x y z))
```

debe convertirse en:

```text
(lambda (x)
  (lambda (y)
    (lambda (z)
      (+ x y z))))
```

### `curryApp :: ASA -> [ASA] -> Maybe ASA`

Implementa la **currificación de aplicaciones de función**. Una aplicación con múltiples argumentos debe transformarse en una secuencia de aplicaciones, de manera que cada aplicación suministre un único argumento.

Por ejemplo:

```text
((lambda (x) (and x y)) (not #t) #f)
```

debe convertirse en:

```text
(((lambda (x) (and x y)) (not #t)) #f)
```

Observa que la función se aplica primero al argumento `(not #t)` y, posteriormente, al argumento `#f`.

### `binaryOp :: (ASA -> ASA -> ASA) -> [ASA] -> Maybe ASA`

Implementa la transformación de operaciones con múltiples operandos en una secuencia de **operaciones binarias asociadas por la izquierda**.

Las operaciones deben considerarse estrictamente binarias después del desazucarado. Si una operación recibe menos de dos argumentos debe producir `Nothing`.

Por ejemplo:

```text
(+ 1 2 3 4)
```

debe convertirse en:

```text
(+(+(+ 1 2) 3) 4)
```


### `desugar :: SASA -> Maybe ASA`

Implementa el proceso de **desazucarado** para cada constructor del lenguaje.

La función debe:

1. Currificar las funciones con múltiples parámetros.
2. Currificar las aplicaciones de función con múltiples argumentos.
3. Eliminar el azúcar sintáctica de `let` y `let*`, transformando cada ligadura en una aplicación de función equivalente.
4. Transformar las operaciones aritméticas multiparamétricas en operaciones binarias anidadas.
5. Propagar el fallo (`Nothing`) cuando alguna de las transformaciones no sea válida.
6. Producir como resultado un `ASA` que utilice únicamente los constructores correspondientes al lenguaje **sin azúcar**.

Por ejemplo, la expresión:

```text
(let ((x 4))
  (+ x 5))
```

debe convertirse en una aplicación de función equivalente:

```text
((lambda (x) (+ x 5)) 4)
```

Para `let*`, las ligaduras deben conservar su evaluación **secuencial**. Por ejemplo:

```text
(let* ((x 4)
       (y (+ x 5)))
  (- y 2))
```

puede transformarse mediante aplicaciones de funciones anidadas:

```text
((lambda (x)
   ((lambda (y)
      (- y 2))
    (+ x 5)))
 4)
```

Observa que la expresión que calcula `y` puede utilizar `x`.

> **Hint:** ¿Es necesario transformar directamente `let*` en aplicaciones de función? 


---

## Reto 2 — Alcance estático y cerraduras

### Interp.hs & MiniLispPlusPlus.hs

Define la **semántica operacional de MiniLisp++ mediante cerraduras**.

### `lookupEnv :: Nombre -> Env -> Maybe Value`

Implementa la búsqueda de un identificador dentro del ambiente.

La función debe recuperar el **valor asociado a la ocurrencia más reciente** del identificador en el ambiente.

Por ejemplo, dado el ambiente:

```haskell
[("x", 5), ("y", 10), ("x", 20)]
```

la búsqueda de `x` debe devolver el valor correspondiente a su asignación más reciente que es 5.

### `bigStep :: Env -> ASA -> Maybe Value`

Define la **semántica operacional de paso grande** para `MiniLisp++`, siguiendo una estrategia de **evaluación ansiosa**.

La evaluación debe realizarse de acuerdo con las reglas semánticas definidas para el lenguaje. En particular, las expresiones que aparecen como argumentos de una aplicación deben evaluarse antes de realizar la aplicación de la función.

Para la evaluación de funciones y aplicaciones de función, utiliza **cerraduras**.

Una cerradura debe conservar:

* la función que se está evaluando; y
* el ambiente correspondiente al lugar donde dicha función fue definida.

De esta manera, las cerraduras permiten implementar el **alcance estático** del lenguaje.


### `evalua :: String -> Maybe Value`

Dentro de `MiniLispPlusPlus.hs`, implementa una función que realice el **proceso completo de evaluación de un programa**, desde el análisis léxico hasta la obtención del resultado final.

El proceso debe seguir las siguientes etapas:

1. Realizar el análisis léxico y sintáctico.
2. Obtener el `SASA` correspondiente.
3. Desazucarar la expresión mediante `desugar`.
4. Si el desazucarado es válido, evaluar el `ASA` resultante mediante `bigStep`.
5. Si alguna de las etapas falla, devolver `Nothing`.

La función `evalua` debe integrar todas las etapas anteriores y producir el resultado final de la evaluación.
