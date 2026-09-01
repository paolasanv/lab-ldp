# Tests

Para ejecutar las pruebas de la practica 5, primero genera el analizador léxico y el analizador sintáctico con `alex` y `happy`:

```bash
alex Lexer.x
happy Grammars.y
```

A continuación, ejecuta el archivo de pruebas:

```bash
runghc --ghc-arg='-package array' TestLaboratorio05.hs
```

## Alternativas 

También es posible utilizar cualquiera de las siguientes alternativas:

**Con `runhaskell`:**

```bash
runhaskell --ghc-arg='-package array' TestLaboratorio05.hs
```

**Compilando el programa con `ghc`:**

```bash
ghc -package array TestLaboratorio05.hs -o tests
./tests
```

**Utilizando `ghci`:**

```bash
ghci -package array TestLaboratorio05.hs
```

Una vez dentro de `ghci`, ejecuta la función principal:

```haskell
ghci> main
```
