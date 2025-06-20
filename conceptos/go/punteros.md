# Punteros en Go

Un puntero en Go es una variable que almacena la **dirección de memoria** de otra variable. Permiten compartir datos entre diferentes partes de un programa y modificar el valor original de una variable desde una función.

A diferencia de lenguajes como C/C++, los punteros en Go son más seguros y simples, ya que no permiten aritmética de punteros.

## Declaración y Operadores

### `*` (Asterisco)

Se usa de dos maneras:
1.  Para declarar un tipo puntero: `*T` es un puntero a un valor de tipo `T`.
2.  Para **desreferenciar** un puntero: `*p` accede al valor almacenado en la dirección de memoria a la que apunta `p`.

### `&` (Ampersand)

Es el operador de "dirección". `&x` genera un puntero a la variable `x`.

## Ejemplo de Uso

```go
package main

import "fmt"

func main() {
    // 1. Declaramos una variable 'numero'.
    numero := 42

    // 2. Declaramos un puntero 'p' y le asignamos la dirección de 'numero'.
    var p *int
    p = &numero

    fmt.Printf("Valor de 'numero': %d\n", numero)
    fmt.Printf("Dirección de 'numero' (&numero): %p\n", &numero)
    fmt.Printf("Valor de 'p' (la dirección que almacena): %p\n", p)
    
    // 3. Desreferenciamos 'p' para obtener el valor al que apunta.
    fmt.Printf("Valor al que apunta 'p' (*p): %d\n", *p)

    // 4. Modificamos el valor de 'numero' a través del puntero.
    *p = 100
    fmt.Printf("Nuevo valor de 'numero' (modificado vía puntero): %d\n", numero)
}
```

## Punteros y Funciones

El uso más común de los punteros es para permitir que una función modifique el valor de una variable que fue declarada fuera de ella.

```go
func duplicar(valor *int) {
    *valor = *valor * 2
}

func main() {
    x := 10
    duplicar(&x) // Pasamos la dirección de 'x'.
    fmt.Println(x) // Muestra 20
}
```

Si no usáramos un puntero, la función `duplicar` recibiría una **copia** de `x`, y el valor original no se vería afectado.

## Valor Cero (`nil`)

El valor cero de un puntero es `nil`. Un puntero `nil` no apunta a ninguna dirección de memoria. Intentar desreferenciar un puntero `nil` causará un pánico en tiempo de ejecución.

```go
var p *int
fmt.Println(p) // Muestra <nil>

if p != nil {
    fmt.Println(*p) // Esto no se ejecuta
}
```

## Punteros en `structs`

Es muy común usar punteros con `structs` para evitar copiar estructuras grandes de datos al pasarlas a funciones. Go ofrece un atajo sintáctico para acceder a los campos de un struct a través de un puntero:

```go
type Persona struct {
    Nombre string
}

p := &Persona{Nombre: "Jared"}

// En lugar de hacer esto:
// (*p).Nombre = "Ana"

// Go permite esto:
p.Nombre = "Ana"
``` 