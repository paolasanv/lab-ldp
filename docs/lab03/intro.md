# Introducción

Durante la práctica anterior se implementaron las etapas de análisis léxico y análisis sintáctico de MiniLisp, utilizando **Alex** y **Happy**, respectivamente. Como resultado de estas etapas se obtiene un **Árbol de Sintaxis Abstracta** que representa de manera estructurada las expresiones válidas del lenguaje.

En esta práctica se partirá del ASA obtenido anteriormente para incorporar la etapa de evaluación de las expresiones. Para ello, se extenderá la gramática de MiniLisp con nuevas construcciones, particularmente identificadores, expresiones `let` y `let*`, que permitirán trabajar con variables y establecer asociaciones dentro de las expresiones.

Una vez incorporadas estas extensiones, se implementará la evaluación del lenguaje mediante una semántica de paso grande por sustitución. Este enfoque permitirá definir directamente la relación entre una expresión y el valor al que evalúa, utilizando la sustitución de las apariciones libres de los identificadores por sus valores correspondientes.

Como referencia para el desarrollo del intérprete puedes utilizar la implementación de [MINILISP02](https://github.com/lambdasspace/MiniLisp/blob/main/MINILISP02/Interp.hs), la cual aplica una semántica de paso pequeño y permite observar el tratamiento de identificadores, `let` y las operaciones básicas del lenguaje.
