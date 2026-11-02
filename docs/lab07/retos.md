# Laboratorio 07: Extensión de TinyC++

Hasta el momento, contamos con las siguientes construcciones para el lenguaje **TinyC**:

```text
<program> ::= <command>

<command> ::= <simple_command>
            | <simple_command> <command>

<simple_command> ::= int <id> = <aexp> ;
                   | <id> = <aexp> ;
                   | <command> <command>
                   | while <aexp> { <command> }

<aexp> ::= <num>
         | <id>
         | ( <aexp> + <aexp> )
         | ( <aexp> - <aexp> )
         | ( <aexp> * <aexp> )
         | ( <aexp> == <aexp> )
         | ( <aexp> <= <aexp> )
         | ( ! <aexp> )

<id> ::= x
<num> ::= Z
```

En este laboratorio extenderemos el lenguaje con nuevas construcciones de control de flujo: `if-else`, `do-while`, `for` y `switch-case`. Para ello, completaremos el analizador léxico, el analizador sintáctica y el intérprete de **TinyC++**.

---

## Reto 1 — Completar el analizador léxico

### `Lexer.x`

Extiende el lenguaje para reconocer las nuevas palabras reservadas y símbolos correspondientes a las construcciones `if-else`, `do-while`, `for` y `switch-case`.

No olvides incluir las palabras reservadas `default` y el símbolo `:` (dos puntos), necesarios para declarar las distintas alternativas de una estructura `switch-case`.

**Recuerda:** en este reto únicamente debes preocuparte por extender el analizador léxico para reconocer los nuevos elementos del lenguaje **TinyC++**.

---

## Reto 2 — Completar el analizador sintáctico

### `Grammars.y`

Incorpora las nuevas construcciones a la gramática del lenguaje. Las producciones que debes completar tienen la siguiente estructura:

```text
<simple_command> ::= ...
                   | if <?> { <?> } else { <?> }
                   | do { <?> } while <?> ;
                   | for ( HeaderCommand ; <?> ; HeaderCommand ) { <?> }
                   | switch <?> { Case Cases }
```

En los lugares indicados con `<?>`, identifica el no terminal más adecuado para cumplir con las especificaciones de cada construcción, descritas en el documento de la práctica.

La definición de `HeaderCommand` ya está proporcionada. Sin embargo, deberás definir los no terminales `Case` y `Cases` para representar las alternativas de la estructura `switch-case`.

Ten en cuenta que, de acuerdo con las especificaciones, las etiquetas de los casos deben ser valores enteros y la estructura debe incluir obligatoriamente una alternativa `default`.

Recuerda que en TinyC no existen valores booleanos explícitos. Las condiciones de las estructuras de control se evalúan mediante expresiones enteras. Por ejemplo, para `if-else`, si la condición evalúa a `0`, se ejecuta la alternativa (`else`); si su valor es distinto de `0`, se ejecuta el consecuente (`if`).

**Tip:** ¿Realmente necesitas utilizar `<command>` en la posición de la condición de `if`? Identifica si existen otras construcciones que manejen una situación similar y reflexiona sobre cuál es el no terminal más adecuado.

---

## Reto 3 — Eliminar el azúcar sintáctica

### `Interp.hs`

Implementa la eliminación del azúcar sintáctica de las nuevas construcciones del lenguaje mediante la función:

```haskell
desugar :: SCommand -> Command
```

Además, define una función recursiva que permita eliminar el azúcar sintáctica de las distintas alternativas de `switch-case`:

```haskell
desugarSwitch :: SCommand -> Command
```

Para realizar este reto, considera las siguientes equivalencias:

**1. Ciclo `do-while`:**

```text
do { c } while a  ≡  c ; while a { c }
```

**2. Ciclo `for`:**

```text
for (i; a; u) { c }  ≡  i; while a { c; u }
```

**3. Estructura `switch-case`:**

Para un caso y una alternativa `default`, la estructura puede expresarse mediante un condicional:

```text
switch a {
    case n: { c }
    default: { d }
}
≡
if a == n then c else d
```

Para el caso general, deberás extender esta idea de manera recursiva, transformando cada alternativa en una serie de condicionales anidados.

**Tip:** Si se sabe que una expresión es un entero (`AExp`) no hace falta aplicar `desugar`. 

---

>>**Participación secreta**
>>
>>Revisa el siguiente [repositorio](https://github.com/paolasanv/equivalencia-semantica/blob/main/docs/equivalencia-dependiente-de-contexto.pdf) y explica con tus propias palabras:
>>¿Qué significa que dos programas sean equivalentes bajo semántica de paso grande?
>>Envía un correo con tu respuesta a la ayudante de laboratorio.

>>**Participación secreta (adicional)**
>>
>>Si utilizaste el asistente de pruebas **Rocq** en tu curso de Lógica Computacional, revisa la demostración de la [equivalencia entre los programas `while b do S end` e `if b then (S; while b do S end) else skip end`](https://github.com/paolasanv/equivalencia-semantica/blob/main/comm/SemanticaOperacional_comm.v).
>>¿La demostración de esta equivalencia procede por inducción? Explica cómo te diste cuenta. No es necesario que comprendas la demostración por completo.
>>Envía un correo con tu respuesta a la ayudante de laboratorio.

---

## Reto 4 — Completar la evaluación

### `Interp.hs`

Incorpora la evaluación de `If` dentro de la función:

```haskell
interp :: Command -> Environment -> Store -> State
```

Recuerda que la evaluación debe respetar la semántica de las construcciones del lenguaje **TinyC++**.


**Tip:** Revisa la firma de la función `interpA`.