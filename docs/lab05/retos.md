# Laboratorio 05: Extensión de MiniLisp++

## Reto 1 — Completar el lexer

### Lexer.x

Modifica el analizador léxico para reconocer `if`, `cond`, `else` y `letrec` como **palabras reservadas** del lenguaje, en lugar de tratarlas como identificadores.

---

## Reto 2 — Completar el analizador sintáctico

### Grammars.y

Añade las reglas de producción necesarias para incorporar los nuevos constructores de MiniLisp++.

En particular, recuerda que `cond` debe contener **al menos una condición y una alternativa** (`else`). Para facilitar el manejo de múltiples condiciones, utiliza un no terminal `Clauses` que permita representar una secuencia de cláusulas, incluyendo la alternativa final.

Por ejemplo, una expresión que usa `cond` puede ser:

```lisp
(cond (#f 1) 
      ((not #f) 2) 
      (else 3))
```

También es válida una expresión como:

```lisp
(cond ((and #t #f) (+ 1 2)) 
      (else (- 1 2)))
```

La alternativa `else` representa el caso que debe evaluarse cuando ninguna de las condiciones anteriores resulta verdadera.

---

## Reto 3 — Currificar y desazucarar

### `Interp.hs`

Debes transformar la sintaxis superficial de MiniLisp++ en una representación correspondiente al **lenguaje núcleo**.

Para ello, primero recupera de la práctica anterior las siguientes funciones:

```haskell
curryFun :: [Nombre] -> ASA -> Maybe ASA
curryApp :: ASA -> [ASA] -> Maybe ASA
binaryOp :: (ASA -> ASA -> ASA) -> [ASA] -> Maybe ASA
```

### 3.1 Eliminación del azúcar sintáctico de `cond`

Define por separado:

```haskell
desugarCond :: [(SASA, SASA)] -> SASA -> Maybe ASA
```

La función debe transformar una expresión `cond` en una secuencia de expresiones `if` anidadas.

Por ejemplo:

```lisp
(cond ((and #t #f) (+ 1 2))
      (else (- 1 2)))
```

debe convertirse en:

```lisp
(if (and #t #f)
    (+ 1 2)
    (- 1 2))
```

Si existen varias condiciones, cada una debe convertirse en un `if` anidado. Por ejemplo:

```lisp
(cond (c1 e1)
      (c2 e2)
      (else e3))
```

debe convertirse en:

```lisp
(if c1 e1
    (if c2 e2
        e3))
```

> **Hint:** Presta atención a la firma de la función `desugarCond`.
> ¿Cuál debería ser el papel de `desugar` dentro de `desugarCond`?

### 3.2 Completar `desugar`

Completa:

```haskell
desugar :: SASA -> Maybe ASA
```

para eliminar toda la sintaxis superficial que no pertenece al lenguaje núcleo.

> **Hint:** Revisa tu solución a `desugar` del laboratorio anterior. 
> ¿Qué tienen en común?

### 3.3 Caso especial: `letrec`

`letrec` requiere un tratamiento especial porque permite definir funciones recursivas.

Si en el lenguaje fuente tenemos:

```lisp
(letrec f e c)
```

queremos expresar `letrec` internamente como:

```lisp
(let (f (Y (lambda (f) e)))
     c)
```

para que **el desugar se realice sobre la versión definida con `let`** y no directamente sobre `letrec`.

**Nota:** En este reto `Y` solamente debe tratarse como un identificador. 

---


## Reto 4 — Evaluación diferida

### `Interp.hs`

En este reto se debe implementar la **evaluación diferida con alcance estático**.

A diferencia de una evaluación ansiosa, una expresión no tiene que evaluarse inmediatamente cuando aparece como argumento de una función. En su lugar, puede conservarse junto con el ambiente en el que fue creada y evaluarse hasta que su valor sea necesario.

Para representar esta situación, `Value` incluye:

```haskell
ExprV ASA Env
```

el cuál representa una cerradura de expresión. 

### 4.1 Recuperar `lookupEnv`

Recupera de la práctica anterior:

```haskell
lookupEnv :: Nombre -> Env -> Maybe Value
```

Esta función únicamente busca el valor asociado a un identificador dentro del ambiente.

Por ejemplo, si el ambiente contiene:

```haskell
("x", ExprV e env)
```

`lookupEnv` debe devolver:

```haskell
Just (ExprV e env)
```

y **no debe evaluar `e`**.


### 4.2 Puntos estrictos

En una evaluación diferida, no todas las expresiones necesitan evaluarse inmediatamente.

Existen determinados lugares llamados **puntos estrictos**, en los que sí es necesario obtener el valor de una expresión para poder continuar la evaluación.

En MiniLisp++, los puntos estrictos aparecen en:

* los operandos de las operaciones aritméticas;
* los operandos de las operaciones booleanas;
* la condición de `if`;
* la posición de función de una aplicación.

### 4.3 Definir `strict`

Define:

```haskell
strict :: Value -> Maybe Value
```

Su objetivo es **forzar la evaluación de un valor cuando sea necesario**.

Recueda que existen dos situaciones principales.

### 1. Valor ya evaluado

Si `strict` recibe un valor como:

```haskell
NumV n
BooleanV b
ClosureV x e env
```

no hay nada más que evaluar y debe devolver ese mismo valor.


### 2. Expresión suspendida

Si `strict` recibe:

```haskell
ExprV e env
```

significa que `e` todavía no ha sido evaluada.

En este caso, `strict` debe:

1. evaluar `e` utilizando el ambiente `env`;
2. verificar el resultado;
3. si el resultado sigue siendo una `ExprV`, continuar forzándolo;
4. si se obtiene un valor, devolverlo;
5. si la evaluación queda bloqueada y produce `Nothing`, devolver `Nothing`.

### 4.4 Implementar `bigStep`

Una vez definida `strict`, completa:

```haskell
bigStep :: Env -> ASA -> Maybe Value
```

para implementar la **semántica de paso grande con alcance estático y evaluación diferida**.

Debes utilizar como referencia las reglas semánticas establecidas en la décimotercera nota de clase. 

---

## Reto 5 — Recursión

### `MiniLispPlusPlus.hs`

En el Reto 3, `letrec` se transformó utilizando el identificador `Y`. En este reto se debe completar esa parte del lenguaje definiendo el **combinador de punto fijo `Y`**.

### 5.1 Definir `combinadorY`

Define:

```haskell
combinadorY :: ASA
```

utilizando únicamente los constructores:

```haskell
Fun
App
Id
```

El combinador debe representar:

```text
Y = λf. (λx. f (x x)) (λx. f (x x))
```

### 5.2 Definir `prelude`

Define:

```haskell
prelude :: Env
```

El `prelude` debe contener el identificador `Y` asociado con el valor resultante de evaluar `combinadorY` una vez en el ambiente vacío.

### 5.3 Completar `evalua`

Finalmente, define:

```haskell
evalua :: String -> Maybe Value
```

para integrar todas las etapas del intérprete.

La evaluación debe comenzar utilizando `prelude` como ambiente inicial y **no el ambiente vacío**, ya que `Y` debe estar disponible para las expresiones que utilizan `letrec`.

Si la evaluación tiene éxito no olvides aplicar `strict` a este resultado.

```text
Código fuente 
     -> Análisis léxico  
          -> Análisis sintáctico  
               -> Desazucaramiento 
                    -> Evaluación con ambiente adecuado
                         -> Resultado con strict
```

# Restricción de implementación

Recuerda que la solución debe desarrollarse **sin utilizar las estructuras `do` ni `case ... of`**. El uso de cualquiera de ellas tendrá una **penalización**.