# Introducción a Go

Go (o Golang) es un lenguaje de programación de código abierto desarrollado por Google. Está diseñado para ser **simple, eficiente y confiable**. Sus características principales son:

- **Compilado y estáticamente tipado**: El código se convierte a binario nativo, lo que garantiza un alto rendimiento y detección de errores en tiempo de compilación.
- **Concurrencia nativa**: Facilita la creación de programas que realizan múltiples tareas simultáneamente a través de **goroutines** y **canales**.
- **Gestión de memoria automática**: Incluye un recolector de basura que simplifica el manejo de la memoria.
- **Sintaxis limpia y minimalista**: Inspirada en C, pero más simple y fácil de leer.
- **Herramientas potentes**: Viene con un ecosistema de herramientas integradas para formateo, testing, gestión de dependencias y más.

Este directorio contiene una serie de documentos para entender los conceptos fundamentales de Go.

## Documentación Actual

A continuación se describe el contenido de cada archivo en este directorio:

- **`palabras clave.md`**: Explica las palabras reservadas del lenguaje, como `if`, `for`, `func`, `go`, `defer`, entre otras. Es el punto de partida para entender la sintaxis de Go.

- **`types.md`**: Describe los tipos de datos básicos y compuestos de Go, desde enteros (`int`), cadenas (`string`) y booleanos (`bool`), hasta estructuras más complejas como `structs`, `slices`, `maps` e `interfaces`.

- **`mod.md`**: Detalla el uso de `go mod`, la herramienta oficial para la gestión de dependencias. Explica comandos esenciales como `init`, `tidy`, `download` y `vendor`.

- **`work.md`**: Introduce el concepto de *workspaces* de Go (`go work`), una característica útil para trabajar en múltiples módulos a la vez, especialmente en monorepos.

- **`errores.md`**: Explicar el manejo de errores en Go, que se basa en devolver un valor de tipo `error`. Cubrir patrones comunes y cómo crear errores personalizados.

- **`punteros.md`**: Detallar el uso de punteros en Go. Aunque su uso es más seguro y simple que en C/C++, es un concepto fundamental para entender cómo se comparte y modifica la memoria.

- **`interfaces.md`**: Profundizar en las interfaces. Son clave para la abstracción y el diseño de software flexible en Go. Explicar la implementación implícita y su uso en la inyección de dependencias.

- **`testing.md`**: Describir cómo escribir y ejecutar pruebas unitarias y de integración con el paquete `testing`. Incluir ejemplos de *table-driven tests*.
