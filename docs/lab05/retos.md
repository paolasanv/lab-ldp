# Laboratorio 05: Extensión de MiniLisp++

## Reto 1 — Completar el lexer

### Lexer.x

Modifica el analizador léxico para reconocer `if0`, `if`, `cond`, `else` y `letrec` como **palabras reservadas** del lenguaje, en lugar de tratarlas como identificadores.

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

---

## Reto 3 — Currificar y desazucarar

### Interp.hs

Recupera las siguientes definiciones desarrolladas en la práctica anterior:

```haskell
curryFun :: [Nombre] -> ASA -> Maybe ASA
curryApp :: ASA -> [ASA] -> Maybe ASA
```

Posteriormente, completa la función:

```haskell
desugar :: SASA -> Maybe ASA
```

incorporando la eliminación del azúcar sintáctico correspondiente a las nuevas construcciones, cuando sea necesario.

Observa que una expresión `cond` puede transformarse en una expresión equivalente utilizando `if`. Por ejemplo:

```lisp
(cond ((and #t #f) (+ 1 2)) 
      (else (- 1 2)))
```

es equivalente a:

```lisp
(if (and #t #f)
    (+ 1 2)
    (- 1 2))
```

**Hint:** Decide en qué momento del proceso de desazucaramiento resulta más conveniente realizar la transformación de `cond`.

---

## Reto 4 — Estrategia de evaluación

### Interp.hs

Recupera la definición de `lookupEnv` desarrollada en la práctica anterior:

```haskell
lookupEnv :: Nombre -> [(Nombre, a)] -> Maybe a
```

y revisa si es necesario modificarla para trabajar con el nuevo tipo:

```haskell
lookupEnv :: Nombre -> Env -> Maybe Binding
```

También puedes recuperar la definición de la **semántica de paso grande con alcance estático** desarrollada en la práctica anterior. Sin embargo, deberás realizar los cambios necesarios para que sea compatible con la siguiente firma:

```haskell
bigStep :: Estrategia -> Env -> ASA -> Maybe Value
```

Observa que ahora la función recibe un nuevo argumento: `Estrategia`.

En esta práctica implementarás la semántica de paso grande utilizando dos estrategias de evaluación:

* **Evaluación ansiosa:** los argumentos de una función se evalúan antes de realizar la aplicación.
* **Evaluación glotona o diferida:** los argumentos se almacenan sin evaluar y únicamente se evalúan cuando su valor es necesario.

Para implementar la evaluación diferida, utiliza la función:

```haskell
force :: Estrategia -> Binding -> Maybe Value
```

Esta función deberá obtener el valor asociado a un `Binding` de acuerdo con la estrategia de evaluación utilizada.

### Evaluación ansiosa

Considera la siguiente aplicación:

```lisp
(lambda (x) (+ x x)) (+ 3 4)
```

Bajo evaluación ansiosa, el argumento `(+ 3 4)` se evalúa antes de realizar la aplicación. Por lo tanto, el ambiente almacena directamente el valor de `x`:

```text
(lambda (x) (+ x x)) (+ 3 4)
→ (lambda (x) (+ x x)) 7
→ (+ x x)
```

En el ambiente, `x` queda asociado con el valor `7`:

```text
[x ↦ 7]
```

### Evaluación glotona o diferida

Bajo evaluación glotona, el argumento no se evalúa inmediatamente. En su lugar, se almacena la expresión en el ambiente:

```lisp
(lambda (x) (+ x x)) (+ 3 4)
```

La aplicación produce un ambiente donde `x` queda asociado con la expresión original:

```text
[x ↦ (+ 3 4)]
```

Por lo tanto, la expresión puede continuar su evaluación hasta que sea necesario obtener el valor de `x`:

```text
(+ x x)
→ (+ 7 7)
→ 14
```

En ambos casos, cuando se recupera una variable del ambiente, `force` debe garantizar que se obtenga finalmente un `Value`.

> **Hint:** Ten cuidado al definir la evaluación para cada estrategia. Primero identifica cuáles son las expresiones que poseen **puntos estrictos**, es decir, aquellas cuyos subcomponentes deben evaluarse para poder obtener un resultado.

Después, analiza si existen otros constructores que puedan evaluarse de la misma manera.

Además, no deberás implementar directamente la evaluación del caso `LetRec` dentro de `bigStep`. Para este caso utiliza la función `evaluaLetRec`, aunque todavía no esté implementada. A partir de su firma, identifica qué información debe recibir y cómo debe utilizarla.

---

## Reto 5 — Recursión

### Interp.hs

A partir de los mecanismos de recursión estudiados en clase, determina cuál resulta más conveniente para implementar `letrec` considerando las estrategias de evaluación de esta práctica.

Puedes utilizar alguno de los siguientes mecanismos:

* **Ambiente recursivo**
* **Combinador de punto fijo Y**
* **Combinador de punto fijo Z**

Selecciona el mecanismo que consideres más adecuado y justifica tu decisión durante la defensa de la práctica.

Define:

```haskell
mecanismoRecursion :: MecanismoRecursion
mecanismoRecursion = <tu decisión>
```

Posteriormente, implementa:

```haskell
evaluaLetRec :: Estrategia -> Nombre -> ASA -> ASA -> Env -> Maybe Value
```

Esta función deberá utilizar el mecanismo de recursión seleccionado para realizar la evaluación de `LetRec` de acuerdo con la estrategia de evaluación utilizada.

---

## Reto 6 — Integrar MiniLisp++

### MiniLispPlusPlus.hs

Finalmente, integra todas las etapas desarrolladas durante la práctica en el intérprete de MiniLisp++.

Implementa el flujo completo, desde el análisis léxico hasta la evaluación de la expresión, mediante la función:

```haskell
evalua :: Estrategia -> String -> Maybe Value
```

La función deberá recibir una estrategia de evaluación y un programa escrito en MiniLisp++, y deberá ejecutar las etapas correspondientes:

```text
Código fuente 
     -> Análisis léxico  
          -> Análisis sintáctico  
               -> Desazucaramiento 
                    -> Evaluación 
                         -> Resultado
```
