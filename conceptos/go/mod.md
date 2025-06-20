## `go mod`

Los siguientes comandos permiten el manejo de paquetes y modulos en go.

### `download`

* **Función:** Descarga todos los módulos requeridos por `go.mod` a la caché local (`$GOPATH/pkg/mod`).
* **Uso típico:** Pre-cargar dependencias antes de compilaciones o en entornos sin acceso a Internet.

### `init`

* Inicializa un nuevo módulo en el directorio actual, creando `go.mod` con tu ruta de módulo.

### `edit`

* Permite modificar `go.mod` desde línea de comandos (añadir/reemplazar dependencias, cambiar módulo, etc.).
* Ideal para scripts o herramientas automatizadas.

### `graph`

* Muestra el grafo de dependencias completas con versiones. Útil para visualizar dependencias transitivas.

### `tidy`

* Ajusta `go.mod` y `go.sum`:

  * Añade dependencias faltantes.
  * Elimina dependencias no usadas.
* Garantiza un estado limpio y reproducible del proyecto.

### `vendor`

* Crea un directorio `vendor/` con todos los paquetes necesarios para construir el módulo.
* Facilita builds aislados, CI, despliegues sin Internet o sin depender de proxies externos.

### `verify`

* Comprueba que los módulos en caché no hayan sido modificados desde su descarga.
* Detecta cambios inesperados en dependencias, comparando con `go.sum`.

### `why`

* Muestra la ruta más corta en el grafo de imports desde el módulo principal hasta un paquete o módulo especificado.
* Ayuda a entender por qué se incluye una dependencia.

