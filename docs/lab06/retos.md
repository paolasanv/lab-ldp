# Laboratorio 06: Control del flujo de evaluación 

Recuerda, en CPS la función **no devuelve directamente su resultado**. En su lugar, recibe una continuación que indica **qué debe hacerse con ese resultado**.

En cada reto, antes de escribir código, identifica:

1. **Cuál es el resultado de la función.**
2. **Qué trabajo queda pendiente después de una llamada recursiva.**
3. **Qué información necesita conservar la continuación para realizar ese trabajo.**
4. **En qué momento se aplica la continuación.**

---

## Reto 1 — Factorial

Observa la definición con llamadas recursivas para factorial:

```haskell
factorial :: Integer -> Integer
factorial 0 = 1
factorial n = n * factorial (n - 1)
```

Y la versión con recursión de cola:

```haskell
factorialRC :: Integer -> Integer -> Integer
factorialRC 0 acc = acc
factorialRC n acc = factorialRC (n - 1) (n * acc)
```

Da la versión con CPS para factorial

```haskell
factorialCPS :: Integer -> (Integer -> r) -> r
```

**Tip:** En el caso base, ¿qué resultado debe recibir la continuación?

--- 

## Reto 2 — Suma

Observa la definición con llamadas recursivas para suma:

```haskell
suma :: [Integer] -> Integer
suma [] = 0
suma (x:xs) = x + suma xs
```

Y la versión con recursión de cola:

```haskell
sumaRC :: [Integer] -> Integer -> Integer
sumaRC [] acc = acc
sumaRC (x:xs) acc = sumaRC xs (x + acc)
```

Da la versión con CPS para suma:

```haskell
sumaCPS :: [Integer] -> (Integer -> r) -> r
```

**Tip:** ¿Qué representa `(Integer -> r)`? 

---
## Reto 3 — Longitud

Observa la definición con llamadas recursivas para longitud:

```haskell
longitud :: [a] -> Int
longitud [] = 0
longitud (_:xs) = 1 + longitud xs
```

Da la versión con CPS para longitud:

```haskell
longitudCPS :: [a] -> (Int -> r) -> r
```

**Tip:** Define primero una versión con recursión de cola. ¿Qué tiene en común con los retos anteriores?

---

## Reto 4 — *map*

Observa la definición con llamadas recursivas para `mapDirecto`:

```haskell
mapDirecto :: (a -> b) -> [a] -> [b]
mapDirecto _ [] = []
mapDirecto f (x:xs) = f x : mapDirecto f xs
```

Da la versión con CPS para `mapDirecto`:

```haskell
mapCPS :: (a -> b) -> [a] -> ([b] -> r) -> r
```

**Tip:** ¿Cómo puedes hacer que la continuación agregue `f x` al resultado obtenido?

---

## Reto 5 — *filter*

Observa la definición con llamadas recursivas para `filtra`:

```haskell
filtra :: (a -> Bool) -> [a] -> [a]
filtra _ [] = []
filtra p (x:xs)
  | p x       = x : filtra p xs
  | otherwise = filtra p xs
```

Da la versión con CPS para `filtra`:

```haskell
filtraCPS :: (a -> Bool) -> [a] -> ([a] -> r) -> r
```

**Tip:** Usa la solución a `mapCPS` como referencia. Determina qué debe hacer la continuación cuando `p x` es verdadero y cuando es falso.

---

## Reto 6 — *sumaArbol*

Observa la definición con llamadas recursivas para `sumaArbol`:

```haskell
sumaArbol :: Arbol Integer -> Integer
sumaArbol Vacio = 0
sumaArbol (Nodo x izq der) =
  x + sumaArbol izq + sumaArbol der
```

Da la versión con CPS para `sumaArbol`:

```haskell
sumaArbolCPS :: Arbol Integer -> (Integer -> r) -> r
```

**Tip:** Después de obtener la suma del subárbol izquierdo, todavía necesitas procesar el derecho y combinar ambos resultados. ¿Cómo puede una continuación representar ese trabajo pendiente?