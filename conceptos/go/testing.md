# Testing en Go

Go tiene un soporte nativo y ligero para escribir pruebas unitarias, de integración y benchmarks, todo a través del paquete `testing` y la herramienta `go test`.

## Conceptos Básicos

-   **Archivos de prueba**: El código de prueba para un paquete `mi_paquete` debe estar en uno o más archivos `*_test.go` dentro del mismo directorio.
-   **Funciones de prueba**: Deben comenzar con el prefijo `Test` y recibir un solo argumento: `t *testing.T`.
-   **Ejecución**: Se utiliza el comando `go test` en el directorio del paquete.

## Ejemplo de Prueba Unitaria

Supongamos que tenemos una función `Suma` en un archivo `calculadora.go`:

```go
// calculadora.go
package calculadora

func Suma(a, b int) int {
    return a + b
}
```

La prueba correspondiente estaría en `calculadora_test.go`:

```go
// calculadora_test.go
package calculadora

import "testing"

func TestSuma(t *testing.T) {
    resultado := Suma(2, 3)
    esperado := 5

    if resultado != esperado {
        t.Errorf("Suma(2, 3) = %d; se esperaba %d", resultado, esperado)
    }
}
```

-   `t.Errorf` formatea un mensaje de error y marca la prueba como fallida, pero continúa su ejecución.
-   `t.Fatalf` es similar, pero detiene la ejecución de la prueba inmediatamente.
-   `t.Logf` imprime un mensaje informativo (solo visible con `go test -v`).

## Ejecutar Pruebas

Para ejecutar todas las pruebas en el paquete actual:

```bash
go test
```

Para una salida más detallada, incluyendo los nombres de las pruebas que se ejecutan:

```bash
go test -v
```

## Pruebas Basadas en Tabla (Table-Driven Tests)

Este es un patrón muy común en Go para probar múltiples combinaciones de entradas y salidas de forma limpia. Se define un slice de structs donde cada elemento es un caso de prueba.

```go
func TestSumaConTabla(t *testing.T) {
    casos := []struct {
        a, b, esperado int
    }{
        {1, 2, 3},
        {0, 0, 0},
        {-1, 1, 0},
        {10, -5, 5},
    }

    for _, tc := range casos {
        resultado := Suma(tc.a, tc.b)
        if resultado != tc.esperado {
            t.Errorf("Suma(%d, %d) = %d; se esperaba %d", tc.a, tc.b, resultado, tc.esperado)
        }
    }
}
```

Este enfoque hace que sea muy fácil añadir nuevos casos de prueba.

## Sub-pruebas

Para tener un control más granular y mejores informes, se pueden usar sub-pruebas con `t.Run`.

```go
func TestSumaConSubPruebas(t *testing.T) {
    // ... (definición de 'casos' como antes) ...

    for _, tc := range casos {
        t.Run(fmt.Sprintf("%d+%d", tc.a, tc.b), func(t *testing.T) {
            resultado := Suma(tc.a, tc.b)
            if resultado != tc.esperado {
                t.Fatalf("se esperaba %d, pero se obtuvo %d", tc.esperado, resultado)
            }
        })
    }
}
```

Con `t.Run`, `go test -v` mostrará el resultado de cada caso de prueba individualmente.

## Otras Funcionalidades

El paquete `testing` también soporta:
-   **Benchmarks**: Para medir el rendimiento del código (funciones `BenchmarkXxx`).
-   **Ejemplos**: Como documentación viva que se verifica durante las pruebas (funciones `ExampleXxx`).
-   **Pruebas de cobertura**: Para ver qué porcentaje del código está cubierto por las pruebas (`go test -cover`). 