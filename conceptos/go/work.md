## `go work`: Gestión de Workspaces en Go

El comando `go work` permite crear **workspaces** en Go, facilitando el desarrollo simultáneo de múltiples módulos que están relacionados entre sí. Es especialmente útil en repositorios monolíticos (monorepos) o cuando se trabaja con dependencias locales.

> Los workspaces fueron introducidos en **Go 1.18+**.


### ¿Qué es un workspace?

Un workspace (`go.work`) especifica una lista de rutas locales a módulos (`go.mod`) que se tratarán como parte de un mismo entorno de desarrollo. Esto permite:

* Importar módulos locales sin publicarlos ni usar reemplazos (`replace`).
* Probar cambios en múltiples módulos de forma sincronizada.
* Evitar manipulación temporal de `go.mod` en cada módulo.


## Comandos principales

### `go work init [paths...]`

Crea un archivo `go.work` en el directorio actual, e incluye las rutas dadas como parte del workspace.

```bash
go work init ./modulo1 ./modulo2
```

### `go work use [paths...]`

Agrega uno o más directorios con módulos Go (`go.mod`) al workspace ya creado.

```bash
go work use ../otra-lib ../core-utils
```

### `go work sync`

Sincroniza los archivos `go.mod` de los módulos en el workspace con el archivo `go.work`, asegurando consistencia. Es útil después de cambiar dependencias entre módulos locales.


## Ejemplo básico

Supón que tienes esta estructura:

```
/proyecto
  /backend
    go.mod
  /frontend
    go.mod
```

Puedes crear un workspace así:

```bash
cd proyecto
go work init ./backend ./frontend
```

Esto crea un archivo `go.work` que permite importar código entre `backend` y `frontend` sin necesidad de reemplazos en `go.mod`.


## Archivo `go.work` generado (ejemplo)

```go
go 1.20

use (
    ./backend
    ./frontend
)
```


## Conclusión

El uso de `go work` simplifica el trabajo con múltiples módulos sin modificar sus `go.mod`. Es ideal para equipos grandes, desarrollo modular o monorepos.

