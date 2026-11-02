# Introducción

Hasta ahora hemos trabajado en el desarrollo de lenguajes de programación funcionales. En esta práctica cambiaremos el enfoque para introducir la mutabilidad del estado mediante el uso de comandos.

La programación imperativa se distingue principalmente de la programación funcional por la forma en que se modifica el *estado* de la memoria, la *secuencialización* de las instrucciones y el uso de *comandos* para describir las operaciones que debe ejecutar un programa.

En el paradigma funcional estamos acostumbrados a trabajar con **expresiones**, es decir, **construcciones que pueden evaluarse y reducirse a un valor**. En cambio, los **comandos representan instrucciones cuya ejecución puede producir modificaciones en el estado del programa**, una característica fundamental del estilo imperativo.

En las próximas prácticas estudiaremos este paradigma mediante la extensión del lenguaje [**TinyC**](https://github.com/manu-msr/tinyc), implementado en Haskell, al que llamaremos **TinyC++**. A lo largo de este proceso, exploraremos sus características y construiremos su implementación siguiendo un enfoque similar al que utilizamos anteriormente con **MiniLisp++**.