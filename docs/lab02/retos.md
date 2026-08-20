# Laboratorio 02: Describiendo un lenguaje

## Reto 1: Lexer.x (analizador léxico)
Añade las reglas léxicas correspondientes para reconocer los nuevos tokens solicitados (and, or, \*, /, expt, <, >, <=, >=, eq, add1, sub1, zero?).

Ejemplo: 

Supongamos que también queremos reconocer listas en MiniLisp.

```haskell
tokens :-
  $white+    
  -- Tokens ya presentes en MINILISP01
  \(                    { \_ -> TokenPA }
  \)                    { \_ -> TokenPC }
  ...
  -- Nuevos patrones léxicos
  nil                   {\_ -> TokenNil}
  list                  {\_ -> TokenList}

  .                     { \s -> error ("Lexical error: caracter no reconocido = "
                                      ++ show s
                                      ++ " | codepoints = "
                                      ++ show (map fromEnum s)) }
```
A la izquierda se especifica la expresión regular (el patrón a reconocer) y a la derecha una función en Haskell que mapea el texto reconocido hacia el tipo *Token*.

Los constructores de los tokens se definen en el tipo de dato algebraico *Token* dentro del mismo archivo:

```haskell
data Token
  = TokenNum Int
  | TokenBool Bool
  | ...
  | TokenPA
  | TokenPC
  | TokenNil  	-- Nuevo
  | TokenList 	-- Nuevo
```

## Reto 2: Grammars.y (definición de la gramática)
Integra las reglas de producción (unarias, binarias y n-arias) en el no terminal principal ASA, respetando la notación prefija y el agrupamiento por paréntesis para MiniLisp.

*Nota: Si una regla requiere  operaciones multiparamétricas, utiliza un símbolo no terminal auxiliar que definirás formalmente en el Reto 3.*

Ejemplo:

Al añadir tokens dentro del analizador léxico (Lexer.x) también hay que definidos en el analizador sintáctico o parser (Grammars.y). 

Nota: Este paso **no es necesario** para el reto #2.

```haskell
%token 
      nat           { TokenNum $$ }
      bool          { TokenBool $$ }
      ... 
      '('           { TokenPA }
      ')'           { TokenPC }
      nil           { TokenNil }  -- Nuevo
      "list"        { TokenList } -- Nuevo
%%      
```
Después definimos las reglas de producción para el Árbol de Sintaxis Abstracta. En este ejemplo definimos la regla para listas.

```haskell
ASA : nat                  { Num $1 }
    | bool                 { Boolean $1 }   
    | ...
    | nil 				         { Nil }
    | '(' "list" lista ')' { List $3 } 
```

A la izquierda se define la estructura sintáctica esperada en el código fuente (MiniLisp).

A la derecha se define el código de Haskell que permite construir el nodo correspondiente del ASA. Los identificadores posicionales ($n) extraen el valor semántico del n-ésimo símbolo de la regla de producción. Es decir, en la regla de *"list"*, el constructor *List* recibe como parámetro la subexpresión en la tercera posición ($3), que debe corresponder a un grupo de ASAs.

El ASA también debe definirse en el bloque Haskell de Grammars.y:

```haskell
data ASA
  = Num Int
  | Boolean Bool
  | ...
  | Nil 		    -- Nuevo 
  | List [ASA]  -- Nuevo
```

## Reto 3: Grammars.y (sintaxis abstracta)

Define formalmente los no terminales auxiliares utilizados en el Reto 2, empleando reglas recursivas para representar secuencias de dos o más argumentos.

Ejemplo:

El no terminal *lista* representa una secuencia no vacía de expresiones (1 o más elementos), es entonces que una lista en MiniLisp es vacía (*nil*) o contiene al menos un elemento (*list*). 

```haskell
ASA : nat                  { Num $1 }
    | bool                 { Boolean $1 }   
    | ...
    | nil 				         { Nil }
    | '(' "list" lista ')' { List $3}

lista :  ASA	             {[$1]}
	| ASA lista              {$1 : $2}
```

*Hint: Propón una nueva definición, similar a *lista*,  para representar una secuencia de ASAs con al menos dos elementos.* 
