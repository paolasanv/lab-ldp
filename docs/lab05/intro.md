# Introducción

En la práctica anterior extendimos nuestra versión de **MiniLisp** con funciones y aplicaciones de función. ¡Es momento de llevarlo a otro nivel!

Durante esta semana has aprendido sobre dos estrategias fundamentales de evaluación: **evaluación ansiosa** y **evaluación diferida**. Estas estrategias determinan **cuándo se evalúan las expresiones de un programa**. Mientras que la evaluación ansiosa busca reducir los argumentos a valores lo antes posible, la evaluación diferida busca posponer su evaluación hasta que su resultado sea estrictamente necesario. Para ello, esta última estrategia se apoya en la identificación de **puntos estrictos**, es decir, aquellos lugares de una expresión donde realmente necesitamos conocer el valor de un subcomponente para continuar con la evaluación.

En esta práctica implementaremos ambas estrategias de evaluación, manteniendo el **alcance estático** que utilizamos en las prácticas anteriores. Además, extenderemos nuevamente el lenguaje para incorporar algunas construcciones fundamentales de los lenguajes funcionales: `if`, `if0`, `cond` y `letrec`.

Finalmente, aprovecharemos que MiniLisp ya cuenta con funciones, aplicaciones de función y ambientes con el objetivo de incorporar una construcción más interesante para definir **asignaciones locales recursivas**: `letrec`.

Para implementar la evaluación de `letrec`, podrás elegir entre diferentes mecanismos de recursión, como el uso de **ambientes recursivos** o **combinadores de punto fijo**. 

¡La elección queda en tus manos! 

Sin embargo, tendrás que justificar durante la defensa por qué consideras que el mecanismo seleccionado es adecuado para la estrategia de evaluación utilizada.
