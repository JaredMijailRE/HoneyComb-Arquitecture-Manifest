# Interfaces en Go

Las interfaces en Go son un concepto poderoso para lograr **abstracción** y **polimorfismo**. Definen un conjunto de métodos que un tipo concreto debe implementar. A diferencia de otros lenguajes, la implementación de interfaces en Go es **implícita**.

## ¿Qué es una Interfaz?

Una interfaz es un tipo que especifica un comportamiento a través de una colección de firmas de métodos.

```go
// 'Impresora' es una interfaz para cualquier tipo
// que pueda imprimir su contenido.
type Impresora interface {
    Imprimir() string
}
```

Cualquier tipo que tenga un método `Imprimir() string` satisface automáticamente la interfaz `Impresora`.

## Implementación Implícita

No existe la palabra clave `implements`. Si un tipo define todos los métodos de una interfaz, implementa esa interfaz. Esto fomenta la creación de interfaces pequeñas y desacopladas.

**Ejemplo:**

```go
import "fmt"

// Nuestro tipo concreto.
type Documento struct {
    Texto string
}

// 'Documento' implementa 'Impresora' porque tiene el método Imprimir().
func (d Documento) Imprimir() string {
    return fmt.Sprintf("Documento: %s", d.Texto)
}
```

## Polimorfismo y Abstracción

Las interfaces permiten escribir funciones que operan sobre comportamientos, no sobre tipos concretos. Esto hace que el código sea más flexible y reutilizable.

```go
// Esta función acepta cualquier tipo que implemente 'Impresora'.
func ProcesarImpresion(p Impresora) {
    fmt.Println("Imprimiendo ->", p.Imprimir())
}

func main() {
    doc := Documento{Texto: "Hola Mundo"}
    ProcesarImpresion(doc) // Funciona porque Documento es una Impresora.
}
```

## Inyección de Dependencias

Las interfaces son la clave para la inyección de dependencias en Go. En lugar de que un componente cree sus propias dependencias, estas se le "inyectan" como interfaces. Esto facilita las pruebas y el desacoplamiento.

```go
// Un servicio que depende de un notificador.
type Notificador interface {
    Enviar(mensaje string) error
}

type ServicioDePedidos struct {
    notificador Notificador // Dependencia como interfaz
}

func (s *ServicioDePedidos) CrearPedido() {
    // ... lógica del pedido ...
    s.notificador.Enviar("¡Nuevo pedido creado!")
}
```

Al probar `ServicioDePedidos`, podemos pasarle un `Notificador` falso (un *mock*) en lugar de uno real que envíe correos o SMS.

## La Interfaz Vacía: `interface{}`

Una interfaz sin métodos, `interface{}`, se conoce como la "interfaz vacía". Puede contener un valor de **cualquier tipo**, ya que todos los tipos tienen cero o más métodos.

Para usar el valor subyacente, se requiere una **aserción de tipo**.

```go
var i interface{}
i = "hola"

// Aserción de tipo para obtener el string.
s, ok := i.(string)
if ok {
    fmt.Println(s) // "hola"
}
```

La interfaz vacía es útil, pero se debe usar con cuidado, ya que se pierde la seguridad de tipos que ofrece Go. En Go 1.18+, se prefiere `any` como alias de `interface{}`. 