### Introducción

Previamente has estudiado los conceptos de **sintaxis léxica** y **sintaxis libre de contexto**, y has analizado estas características para el lenguaje **MiniLisp**. En esta práctica retomaremos dicho análisis para llevarlo de la definición teórica a una implementación concreta mediante las herramientas **Alex** y **Happy**, utilizadas en conjunto con Haskell.

La **sintaxis léxica** permite determinar cuáles son los lexemas válidos de un lenguaje a partir de las expresiones regulares que los describen. Cada lexema reconocido se asociará con un **token o componente léxico**, representado mediante un tipo de dato definido en el lenguaje anfitrión, Haskell en nuestro caso.

Para implementar esta parte utilizaremos **Alex**, una herramienta que permite especificar y generar *analizadores léxicos*. A partir de las expresiones regulares y los tokens definidos previamente, construiremos el analizador léxico de una versión extendida de [MINILISP01](https://github.com/lambdasspace/MiniLisp/tree/main/MINILISP01).

Una vez identificados los tokens, es necesario establecer cómo pueden combinarse para formar expresiones válidas del lenguaje. La **sintaxis libre de contexto**, también denominada **sintaxis concreta**, define precisamente las secuencias de tokens que son válidas de acuerdo con la gramática del lenguaje. Estas reglas determinan la estructura de las instrucciones que eventualmente podrán utilizarse para expresar programas en MiniLisp.

Para implementar esta segunda parte utilizaremos **Happy**, una herramienta que permite definir una gramática libre de contexto y generar un *analizador sintáctico*. En nuestro caso, Happy utilizará los tokens producidos por Alex para determinar si una secuencia de entrada cumple con la gramática extendida de MiniLisp.

Finalmente, la definición de la gramática en Happy requiere establecer qué estructura tendrá el resultado del análisis sintáctico. Aquí aparece la **sintaxis abstracta**, una representación más sencilla de la estructura del programa que omite detalles propios de la sintaxis concreta que no son necesarios para su posterior procesamiento. Para construir esta representación utilizaremos un **tipo de dato algebraico** que nos permitirá modelar el **árbol de sintaxis abstracta (ASA)**. Happy será el encargado de construir dicho árbol a partir de las reglas de la gramática.
