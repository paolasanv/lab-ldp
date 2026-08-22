# Introducción

Para continuar con la implementación del intérprete de MiniLisp, en esta práctica se incorporará el soporte para **funciones y aplicaciones de función**, con el objetivo de modelar las principales características de la *evaluación funcional* del lenguaje.

En esta versión se trabajará con un subconjunto más reducido de MiniLisp, en el que se incorpora la **eliminación del azúcar sintáctico** de las expresiones `let`. Este proceso de eliminación permitirá transformar dichas construcciones a una forma más básica del lenguaje antes de llevar a cabo su evaluación.

Además, la incorporación de funciones y, particularmente, de la currificación requiere realizar una transformación adicional sobre el Árbol de Sintaxis Abstracta. El árbol producido por el análisis sintáctico deberá convertirse en una estructura binaria que facilite la representación de las aplicaciones de función y su posterior evaluación.

Finalmente, se extenderá el intérprete para evaluar estas nuevas construcciones mediante una semántica de paso grande. Para ello, será necesario trabajar con **ambientes y cerraduras** que permitan conservar el contexto en el que se definen las funciones y utilizarlo posteriormente durante su aplicación.

Como referencia para el desarrollo de esta práctica se utilizará la implementación de [MINILISP03](https://github.com/lambdasspace/MiniLisp/blob/main/MINILISP03/VERSION03/InterpEnvEst.hs), que presenta una implementación de funciones, aplicaciones, ambientes y cierres para la evaluación de MiniLisp.
