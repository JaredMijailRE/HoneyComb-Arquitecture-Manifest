# Manejo de Errores en Go

En Go, el manejo de errores es una parte explícita y fundamental del flujo de control. No se usan excepciones (`try-catch`), sino que las funciones que pueden fallar devuelven un valor de tipo `error` junto con su resultado normal.

## El Tipo `error`

`error` es una interfaz predefinida en Go con un único método:

```go
type error interface {
    Error() string
}
```

Cualquier tipo que implemente este método satisface la interfaz `error`. Si una función devuelve un `error` con valor `nil`, significa que no hubo ningún problema.

## Patrón Común de Manejo de Errores

El patrón más común es que una función devuelva su resultado y un error. El código que llama a la función debe comprobar si el error es `nil` antes de usar el resultado.

```go
import (
    "fmt"
    "strconv"
)

func main() {
    resultado, err := strconv.Atoi("123")
    if err != nil {
        // Hubo un error, no se puede usar 'resultado'.
        fmt.Println("Error:", err)
        return
    }
    // No hubo error, 'resultado' es válido.
    fmt.Println("El número es:", resultado)
}
```

## Creación de Errores

### `errors.New`

La forma más simple de crear un error es usando el paquete `errors`.

```go
import "errors"

func dividir(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("no se puede dividir por cero")
    }
    return a / b, nil
}
```

### `fmt.Errorf`

Para crear un error con formato (similar a `fmt.Sprintf`), se usa `fmt.Errorf`. Es muy útil para añadir contexto dinámico al mensaje de error.

```go
import "fmt"

func abrirArchivo(nombre string) error {
    // Simulación de error
    return fmt.Errorf("no se pudo abrir el archivo '%s': no existe", nombre)
}
```

## Errores Personalizados

Para errores más complejos, puedes definir tu propio tipo `struct` que implemente la interfaz `error`. Esto permite añadir más información al error, como un código de error o detalles adicionales.

```go
type MiError struct {
    Codigo  int
    Mensaje string
}

func (e *MiError) Error() string {
    return fmt.Sprintf("código %d: %s", e.Codigo, e.Mensaje)
}

func operacionCritica() error {
    return &MiError{
        Codigo:  500,
        Mensaje: "fallo en el sistema",
    }
}
```

Este enfoque permite al código que maneja el error inspeccionar los detalles del `MiError` y tomar decisiones basadas en su contenido. 