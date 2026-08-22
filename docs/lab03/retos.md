# Laboratorio 03: Semántica de paso grande por sustitución

La solución a los primeros dos ejercicios es análoga a la implementada en la práctica 2.

## Reto 1 — Completar el lexer
### Lexer.x

Modifica el analizador léxico para reconocer `let` y `let*` como **palabras reservadas** y no como identificadores.

---

## Reto 2 — Completar el analizador sintáctico
### Grammars.y

Incorpora las reglas de producción para las expresiones `let` y `let*`, junto con la regla correspondiente a los identificadores y los símbolos no terminales  `Binding` y `Bindings`.

---

## Reto 3 — Implementar la sustitución 
### Interp.hs

Completa las funciones auxiliares necesarias para implementar la **sustitución nominal sin captura**.

### `freeVars :: ASA -> [String]`

Esta función debe devolver la lista de variables libres que aparecen en una expresión. Para su implementación, utiliza la definición establecida en la **octava nota de clase**.

Para `let*`, las variables libres se definen mediante la siguiente regla:

$$
\begin{aligned}
\mathrm{FV}\left(
\mathrm{let}^*
\left((x_1,e_1),\ldots,(x_n,e_n)\right)
\mathrm{in}\ e
\right)
={}&\mathrm{FV}(e_1) \\
&{}\cup
\bigcup_{i=2}^{n}
\left(
\mathrm{FV}(e_i)
\setminus
\bigcup_{j=1}^{i-1}\{x_j\}
\right) \\
&{}\cup
\left(
\mathrm{FV}(e)
\setminus
\bigcup_{j=1}^{n}\{x_j\}
\right).
\end{aligned}
$$

Esto se debe a la forma en que se determina el alcance de las variables en `let*`. El ligador de una variable (x_i) tiene alcance sobre los *bindings* que se encuentran a su derecha y sobre el cuerpo de la expresión.

Sin embargo, si la misma variable aparece nuevamente como ligador dentro de la lista de *bindings*, el alcance del ligador anterior termina al encontrar dicha ocurrencia.

Por ejemplo:

```haskell
(let* ((x 2) (x 1) (y 4) (z (+ y x))) z)
```

se reduce mediante la siguiente secuencia:

```haskell
(let* ((x 2) (x 1) (y 4) (z (+ y x))) z)

=> (let* ((x 1) (y 4) (z (+ y x))) z)

=> (let* ((y 4) (z (+ y 1))) z)

=> (let* ((z (+ 4 1))) z)

=> 5
```

Observa que el segundo ligador de `x` oculta al primero, por lo que el valor `2` no se utiliza en los *bindings* posteriores.

### `names :: ASA -> [String]`

Obtén la lista de variables que aparecen como ligadores dentro de las expresiones `let` y `let*`.

### `freshName :: [String] -> String`

A partir de una lista de variables, que corresponderá a las variables ligadas de una expresión `let` o `let*`, define una función que permita obtener un nombre de variable **distinto de todos los nombres proporcionados como argumento**.

Esta función será utilizada para evitar la captura de variables durante la sustitución.

### `sust :: ASA -> String -> ASA -> ASA`

Define la operación de sustitución, denotada por

$$
e[x := s]
$$

donde `sust e x s` reemplaza las apariciones **libres** de (x) por (s) dentro de (e).

La sustitución debe respetar el alcance de las variables y realizar un **renombramiento de ligadores** cuando sea necesario para evitar la captura de variables. Utiliza las reglas establecidas en la **octava nota de clase**.

Esta función representa una secuencia dependiente de sustituciones y será utilizada como auxiliar para definir la semántica de paso grande de `let*`.

> **⚠️ OBSERVACIÓN IMPORTANTE**
>
>Las reglas de sustitución para `let` y `let*` son similares; la diferencia principal se encuentra en el alcance que debe renombrarse cuando **existe riesgo de captura**.
>
>Considera lo siguiente:
>
>| Constructor | Condición |
>| :---: | :--- |
>| `let` | se renombra el cuerpo |
>| `let*` | se renombrarán los bindings posteriores y el cuerpo |

---

### `sustMany :: ASA -> [Binding] -> ASA`

Define la sustitución simultánea. Esta función será necesaria para implementar la evaluación de `let`.

La sustitución simultánea se puede representar como:

$$
e[x_1 := e_1,\ldots,x_n := e_n].
$$

Por ejemplo, cuando tenemos `let` anidados, un *binding* del `let` exterior puede afectar las expresiones de los *bindings* del `let` interior:

```text
(let ((x 6))
  (let ((y (+ x 5)))
    y))
```

En este caso, la sustitución

$$
[x := 6]
$$

debe aplicarse también al *binding* de `y`, por lo que la sustitución aplicada resulta en:

```haskell
(let ((x 6))
  (let ((y (+ 6 5)))
    y))
```

En cambio, considera:

```haskell
(let ((x 6)
      (y (+ x 5)))
  y)
```

En este caso, la expresión debe producir un error, ya que los *bindings* de `let` se evalúan de manera simultánea. Por lo tanto, `x` no está disponible para evaluar la expresión asociada a `y`.

Observa que esta distinción aplica a `let` y **no a `let*`**, ya que en `let*` cada *binding* puede utilizar las variables ligadas por los *bindings* anteriores.


> **Extra:** Si tienes dudas sobre el resultado de evaluar una expresión `let` o `let*`, puedes utilizar como referencia el [compilador en línea de Racket](https://onecompiler.com/racket) para ejecutar las expresiones y verificar su resultado.

---

## Reto 4 — Completar la evaluación con semántica de paso grande 
### Interp.hs

Implementa la semántica operacional de **paso grande** para todos los constructores de `MiniLisp++`, utilizando las reglas de evaluación definidas en el documento de la práctica.

Lee con atención las **precondiciones** que deben cumplirse en cada regla de evaluación.

> **⚠️ OBSERVACIÓN IMPORTANTE**
>
>NO es válido traducir `let*` a `let` para implementar su evaluación. La semántica de `let*` debe implementarse de acuerdo con las reglas de evaluación establecidas para este constructor.
>
>Para la implementación puedes utilizar:
>
>* funciones de orden superior;
>* listas de comprensión;
>* funciones auxiliares propias;
>* las funciones de sustitución definidas en el Reto 3 (este punto en realidad es obligatorio).
