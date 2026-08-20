
# Ejecución del intérprete de MiniLisp++

Para ejecutar el intérprete de **MiniLisp++**, asegúrate de haber generado el analizador léxico y el analizador sintáctico. Después ejecuta el intérprete con:

```bash
runghc --ghc-arg='-package array' --ghc-arg='-package haskeline' MiniLispPlusPlus.hs
```

## Alternativas para ejecutar el intérprete

**Con `runhaskell`:**

```bash
runhaskell --ghc-arg='-package array' --ghc-arg='-package haskeline' MiniLispPlusPlus.hs
```

**Utilizando `ghci`:**

```bash
ghci -package array -package haskeline MiniLispPlusPlus.hs
```

Una vez dentro de `ghci`, ejecuta la función principal:

```haskell
ghci> main
```

## Uso del intérprete

Una vez que el intérprete se encuentre en ejecución, se mostrará el prompt de **MiniLisp++**, donde es posible introducir expresiones para evaluarlas. Por ejemplo:

```text
MiniLisp++> (+ 4 (- 0 1))
Num 4
```

El intérprete también permite utilizar las **flechas del teclado** para facilitar la interacción. La flecha **↑** permite recuperar expresiones introducidas anteriormente, mientras que las flechas **←** y **→** permiten desplazarse entre los caracteres de la expresión actual.
