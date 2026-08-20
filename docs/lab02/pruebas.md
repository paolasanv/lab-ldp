# Tests

Para ejecutar las pruebas de la pŕactica 2, primero genera el analizador léxico y el analizador sintáctico con `alex` y `happy`:

```bash
alex Lexer.x
happy Grammars.y
```

A continuación, ejecuta el archivo de pruebas:

```bash
runghc --ghc-arg='-package array' TestLaboratorio02.hs
```

## Alternativas 

También es posible utilizar cualquiera de las siguientes alternativas:

**Con `runhaskell`:**

```bash
runhaskell --ghc-arg='-package array' TestLaboratorio02.hs
```

**Compilando el programa con `ghc`:**

```bash
ghc -package array TestLaboratorio02.hs -o tests
./tests
```

**Utilizando `ghci`:**

```bash
ghci -package array TestLaboratorio02.hs
```

Una vez dentro de `ghci`, ejecuta la función principal:

```haskell
ghci> main
```

## Prueba del análisis léxico y sintáctico

Para comprobar directamente el resultado del análisis léxico y sintáctico, inicia `ghci` con el archivo de pruebas:

```bash
ghci -package array TestLaboratorio02.hs
```

Utiliza la función `parse` junto con `lexer` para analizar una expresión. Por ejemplo:

```haskell
ghci> parse $ lexer "(+ 1 2)"
Add [Num 1, Num 2]
```