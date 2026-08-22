# Laboratorio 01

## Reto 1 — Tipos básicos

Define una función que calcule la distancia euclidiana entre un punto `(x₁, y₁)` y el origen `(0, 0)`.

La distancia euclidiana entre dos puntos `(x₁, y₁)` y `(x₂, y₂)` se calcula mediante:

$$
d = \sqrt{(x_2-x_1)^2 + (y_2-y_1)^2}
$$

---

## Reto 2 — Funciones predfinidas

A partir de una lista de enteros, calcula la suma de los cuadrados de aquellos elementos que sean pares.

Por ejemplo:

```haskell
sumaCuadradosPares [1, 2, 3, 4]
```

debe producir:

```text
20
```

---

## Reto 3 — Definición de funciones

Utiliza funciones como argumentos para definir una función que permita aplicar otra función tres veces sobre un valor.

---

## Reto 4 — Expresiones **let** y **where**

Utiliza `let` o `where` para definir una función que calcule la varianza de un conjunto de datos:

$$
\sigma^2 = \frac{\sum_{i=1}^{N}(x_i-\mu)^2}{N}
$$

---

## Reto 5 — Condicional if y guardias 

Define una función que determine cómo se encuentra el clima a partir de la temperatura actual.

Considera las siguientes categorías:

* **frio extremo:** temperatura menor a `1 °C`.
* **frio:** temperatura menor o igual a `15 °C`.
* **templado:** temperatura a lo más de `25 °C`.
* **calido:** temperatura que no sobrepasa los `35 °C`.
* **calor extremo:** temperatura mayor a `36 °C`.

---

## Reto 6 — Recursión en listas

Define, utilizando recursión, una función que intercale un símbolo entre los elementos de una lista.

Por ejemplo:

```haskell
intercala " " ["hola", "mundo", ":)"]
```

debe producir:

```haskell
["hola", " ", "mundo", " ", ":)"]
```

---

## Reto 7 — Definición de tipos

Define una función que permita evaluar expresiones representadas mediante un tipo de dato algebraico.

Por ejemplo, si tenemos una expresión como:

```haskell
Suma (Lit 1) (Producto (Lit 1) (Lit 4))
```

la función:

```haskell
evalua (Suma (Lit 1) (Producto (Lit 1) (Lit 4)))
```

debe producir:

```text
5
```