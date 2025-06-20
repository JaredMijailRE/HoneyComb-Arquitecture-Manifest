# Palabras Claves

Acontinuacion se explicaran todas las palabras claves de go.

## 🧠 **Control de flujo**

**if**
Ejecuta un bloque si se cumple una condición.

```go
if x > 10 {
    fmt.Println("Mayor que 10")
}
```

**else**
Define una alternativa cuando `if` es falso.

```go
if x > 10 {
    fmt.Println("Mayor")
} else {
    fmt.Println("Menor o igual")
}
```

**switch**
Evalúa múltiples condiciones de forma más clara que varios `if`.

```go
switch x {
case 1:
    fmt.Println("Uno")
case 2:
    fmt.Println("Dos")
default:
    fmt.Println("Otro")
}
```

**case**
Define cada posible valor dentro de un `switch`.

**default**
Bloque que se ejecuta si ningún `case` coincide.

**fallthrough**
Fuerza la ejecución del siguiente `case`, incluso si no coincide.

```go
switch x {
case 1:
    fmt.Println("Uno")
    fallthrough
case 2:
    fmt.Println("Dos") // también se ejecuta
}
```

**for**
Único tipo de bucle en Go. Soporta forma `while`, `foreach` y tradicional.

```go
for i := 0; i < 3; i++ {
    fmt.Println(i)
}
```

**range**
Itera sobre arrays, slices, maps, strings o canales.

```go
nums := []int{1, 2, 3}
for i, v := range nums {
    fmt.Println(i, v)
}
```

**break**
Rompe un bucle o un `switch`.

```go
for {
    break // sale del bucle
}
```

**continue**
Salta a la siguiente iteración del bucle.

```go
for i := 0; i < 5; i++ {
    if i%2 == 0 {
        continue
    }
    fmt.Println(i)
}
```

**goto**
Salta a una etiqueta definida. Poco usado.

```go
goto Salto
fmt.Println("Esto se salta")
Salto:
fmt.Println("Llegaste aquí")
```

---

## 📦 **Declaración de datos y tipos**

**var**
Declara una variable.

```go
var edad int = 25
```

**const**
Declara una constante (valor inmutable).

```go
const Pi = 3.14
```

**type**
Declara un nuevo tipo o alias.

```go
type ID int
```

**struct**
Crea una estructura con campos personalizados.

```go
type Persona struct {
    Nombre string
    Edad   int
}
```

**interface**
Declara una interfaz con métodos que deben implementarse.

```go
type Lector interface {
    Leer() error
}
```

**map**
Estructura clave-valor (como un diccionario).

```go
m := map[string]int{"a": 1, "b": 2}
```

---

## ⚙️ **Funciones**

**func**
Declara una función.

```go
func saludar(nombre string) {
    fmt.Println("Hola", nombre)
}
```

**return**
Devuelve uno o más valores desde una función.

```go
func suma(a, b int) int {
    return a + b
}
```

**defer**
Retrasa la ejecución de una función hasta que la actual termine.

```go
func demo() {
    defer fmt.Println("Finaliza")
    fmt.Println("Inicio")
}
```

---

## 🚀 **Concurrencia**

**go**
Lanza una función en una goroutine (en paralelo).

```go
go saludar("Jared")
```

**chan**
Declara un canal para comunicar goroutines.

```go
ch := make(chan int)
ch <- 5
fmt.Println(<-ch)
```

**select**
Espera múltiples operaciones en canales.

```go
select {
case val := <-ch:
    fmt.Println(val)
default:
    fmt.Println("Nada disponible")
}
```

---

## 📚 **Organización del código**

**package**
Declara el nombre del paquete (modulo actual).

```go
package main
```

**import**
Incluye otros paquetes.

```go
import "fmt"
```