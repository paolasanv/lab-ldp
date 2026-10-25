# Laboratorio 06: Control del flujo de evaluación 

Recuerda, en CPS la función **no devuelve directamente su resultado**. En su lugar, recibe una continuación que indica **qué debe hacerse con ese resultado**.

En cada reto, antes de escribir código, identifica:

1. **Cuál es el resultado de la función.**
2. **Qué trabajo queda pendiente después de una llamada recursiva.**
3. **Qué información necesita conservar la continuación para realizar ese trabajo.**
4. **En qué momento se aplica la continuación.**

---

## Reto 1 — Potencia

Observa la definición con llamadas recursivas para calcular $n^m$:

```haskell
potencia :: Int -> Int -> Int
potencia _ 0 = 1
potencia n m = n * potencia n (m - 1)
```

Y la solución _parcial_ con recursión de cola:

```haskell
potenciaCola :: Int -> Int -> Int
potenciaCola n m = potenciaCola' n m 1  
  where
    potenciaCola' :: Int -> Int -> Int -> Int 
    potenciaCola' _ 0 acc = ? 
    potenciaCola' n m acc = ? 
```

Da la versión con CPS para la potencia

```haskell
potenciaCPS :: Int -> Int -> (Int -> r) -> r
potenciaCPS n 0 k = k ? 
potenciaCPS n m k = ?
```

**Tip:** En el caso base, ¿qué resultado debe recibir la continuación?

--- 

## Reto 2 — Longitud

Observa la definición con llamadas recursivas para obtener el tamaño de una lista:

```haskell
longitud :: [a] -> Int
longitud [] = 0
longitud (_ : xs) = 1 + longitud xs
```

Y la firma para su versión con recursión de cola:

```haskell
longitudCola :: [a] -> Int
```

Da la versión con CPS para longitud:

```haskell
longitudCPS :: [a] -> (Int -> r) -> r
```

**Tip 1:** Observa si la **firma** de `longitudCola` cumple con **uno de los requisitos** necesarios para implementar recursión de cola.

**Tip 2:** Para este caso, pasar de la versión con recursión de cola a CPS sólo requiere modificar qué se devuelve como resultado, más no el cómo. 

---
## Reto 3 — Reversa

Observa la definición con llamadas recursivas para reversa:

```haskell
reversa :: [a] -> [a]
reversa [] = []
reversa (x : xs) = reversa xs ++ [x]
```

Es necesario que definas la versión con recursión de cola 

```haskell
reversaCola :: [a] -> [a]
```

antes de dar la versión con CPS.

```haskell
reversaCPS :: [a] -> ([a] -> r) -> r
```

**Tip:** Los operadores *:* y *++* **no necesariamente** van a poderse utilizar de la misma manera entre la versión de cola y la versión CPS. Reflexiona sobre la diferencia de objetivos que existen entre un acumulador y una continuación.

---

## Reto 4 — Concatena

Observa la definición con llamadas recursivas para concatenar los elementos de una lista de listas:

```haskell
concatena :: [[a]] -> [a]
concatena [] = []
concatena (xs : xss) = xs ++ concatena xss
```

La definición con recursión de cola 

```haskell
concatenaCola :: [[a]] -> [a]
```
y la versión CPS

```haskell
concatenaCPS :: [[a]] -> ([a] -> r) -> r
concatenaCPS [] k = ?
concatenaCPS (x:xs) k = ?
```

requieren el uso del operador **++**, ten cuidado cómo lo usas cuando tengas que pasar *cierto* argumento a una continuación. 

Recuerda que, en general, no es lo mismo *x ++ v* que *v ++ x*. 

---

## Reto 5 — Intercala

Observa la definición con llamadas recursivas para intercalar los elementos entre dos listas:

```haskell
intercala :: [a] -> [a] -> [a]
intercala [] ys = ys
intercala xs [] = xs
intercala (x : xs) (y : ys) = x : y : intercala xs ys
```

Para la versión con recursión de cola 

```haskell
intercalaCola :: [a] -> [a] -> [a]
```

analiza si de verdad es suficiente devolver únicamente el acumulador. 

Y para la versión CPS 
```haskell
intercalaCPS :: [a] -> [a] -> ([a] -> r) -> r
intercalaCPS [] ys k = ?
intercalaCPS xs [] k = ?
intercalaCPS (x:xs) (y:ys) k = ?
```

piensa cómo utilizar la cabeza de ambas listas para resolver el intercalamiento a través de la continuación. 