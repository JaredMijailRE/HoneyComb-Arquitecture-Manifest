## Go Data Types

Acontinuacion se mostraran todos los tipos de datos de Go.

---

**bool**: Tipo booleano. Solo puede tener los valores `true` o `false`.

**string**: Cadena de texto (UTF-8). Inmutable.

**int**: Entero con tamaño dependiente de la arquitectura (32 o 64 bits).

**int8**: Entero de 8 bits con signo (rango: -128 a 127).

**int16**: Entero de 16 bits con signo.

**int32**: Entero de 32 bits con signo.

**int64**: Entero de 64 bits con signo.

**uint**: Entero sin signo (tamaño depende de la arquitectura).

**uint8**: Entero sin signo de 8 bits (alias de `byte`).

**uint16**: Entero sin signo de 16 bits.


**uint32**: Entero sin signo de 32 bits.

**uint64**: Entero sin signo de 64 bits.

**uintptr**: Entero sin signo suficientemente grande para almacenar punteros.

**byte**: Alias de `uint8`, representa un carácter o byte de datos.

**rune**: Alias de `int32`, representa un carácter Unicode.

**float32**: Número decimal de 32 bits de precisión.

**float64**: Número decimal de 64 bits de precisión (por defecto para `float`).

**complex64**: Número complejo con partes `float32`.

**complex128**: Número complejo con partes `float64`.


---

**array**: Conjunto fijo de elementos del mismo tipo.

**slice**: Vista dinámica (rebanada) de un array. Su tamaño puede cambiar.

**map**: Colección de pares clave-valor (`map[clave]valor`).

**struct**: Conjunto de campos con tipos distintos, como un objeto o registro.

**pointer**: Dirección de memoria de una variable (`*T` apunta a tipo `T`).

**function**: Tipo que representa funciones, incluso anónimas o como parámetros.

**interface**: Conjunto de métodos que un tipo debe implementar.

**channel (chan)**: Estructura para comunicación entre goroutines.

**nil**: Valor cero para tipos como interfaces, slices, maps, punteros, funciones o canales.