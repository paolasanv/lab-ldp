# Introducción

Esta semana estudiaste el concepto de **máquinas abstractas**, herramientas formales que permiten describir cómo se desarrolla un cómputo sin depender de la máquina física que lo ejecuta. En particular, revisaste las máquinas **CK** y **CEK**, las cuales permiten representar explícitamente distintos elementos del contexto en el que se evalúa una expresión.

Un elemento fundamental de estas máquinas son las **continuaciones**. Una continuación representa el **contexto de cómputo pendiente**: describe qué debe hacerse con el resultado de la expresión que se está evaluando. Por ejemplo, al evaluar una expresión dentro de una suma, la continuación puede representar que, una vez obtenido su resultado, este debe combinarse con el otro operando.

Esta idea puede llevarse del modelo formal a la programación mediante el **Continuation Passing Style (CPS)**. En este estilo, en lugar de que una función retorne directamente su resultado, recibe una **continuación** que determina qué hacer con dicho resultado. De esta manera, el flujo del cómputo se hace explícito y puede representarse mediante funciones.

Por otro lado, la **recursión de cola** es una forma de estructurar las llamadas recursivas en la que la llamada recursiva constituye la última operación de la función. Esto permite reutilizar un registro de activación en lugar de acumular uno nuevo por cada llamada.

La recursión de cola es un antecedente práctico importante para comprender CPS: mientras que en una función recursiva tradicional el trabajo que debe realizarse después de una llamada recursiva permanece implícito, en una función con recursión de cola dicho trabajo puede representarse explícitamente mediante un parámetro acumulador. En CPS, esta misma idea se generaliza al representar el **trabajo pendiente del cómputo mediante una continuación**.

En esta práctica transformarás funciones recursivas tradicionales a su versión **CPS**, haciendo explícito el contexto que normalmente queda implícito durante la ejecución de un programa.
