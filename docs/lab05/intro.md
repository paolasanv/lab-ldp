# Introducción

En la práctica anterior extendimos nuestra versión de **MiniLisp** con funciones y aplicaciones de función. ¡Es momento de llevarlo a otro nivel!

Durante esta semana has aprendido sobre dos estrategias fundamentales de evaluación: **evaluación ansiosa** y **evaluación diferida**. Estas estrategias determinan **cuándo se evalúan las expresiones de un programa**. Mientras que la evaluación ansiosa busca reducir los argumentos a valores lo antes posible, la evaluación diferida busca posponer su evaluación hasta que su resultado sea estrictamente necesario. Para ello, esta última estrategia se apoya en la identificación de **puntos estrictos**, es decir, aquellos lugares de una expresión donde realmente necesitamos conocer el valor de un subcomponente para continuar con la evaluación.

En esta práctica implementaremos una de las estrategias de evaluación, manteniendo el **alcance estático** que utilizamos en las prácticas anteriores. Además, extenderemos nuevamente el lenguaje para incorporar algunas construcciones fundamentales de los lenguajes funcionales: `if`, `cond` y `letrec`.

Finalmente, aprovecharemos que MiniLisp ya cuenta con funciones, aplicaciones de función y ambientes con el objetivo de incorporar una construcción más interesante para definir **asignaciones locales recursivas**: `letrec`.