# Lista de tareas

Aplicación académica de escritorio en C#, Windows Forms, .NET Framework 4.8 y SQL Server para registrar tareas y consultar si están pendientes o completadas.

[Ver portafolio](https://jeison-rosario.github.io/proyectos/)

## Funcionalidades

- Agregar tareas con título y descripción, inicialmente pendientes.
- Cargar registros desde SQL Server en un `ListView`.
- Eliminar la tarea seleccionada con confirmación.
- Marcar la tarea seleccionada como completada con confirmación.
- Guardar los elementos del listado mediante inserción o actualización según ID.
- Cambiar fondo y fuente mediante los ajustes de interfaz.

El código actual concentra interfaz y ADO.NET en `Form1.cs`. La descripción anterior mencionaba capas, pero no se observan proyectos separados de datos, negocio y entidades en esta solución.

## Requisitos y preparación

Windows, Visual Studio con desarrollo de escritorio .NET, targeting pack de **.NET Framework 4.8** y SQL Server. No necesita Node.js ni servidor web; no es una demo ejecutable directamente en GitHub Pages.

```sh
git clone https://github.com/Jeison-Rosario/Lista-de-tareas.git
```

1. Revisar y ejecutar **`Lista de tareas.sql`** en una instancia de pruebas. Crea **`ListaTareas`** y **`Tareas`**. No repetir la creación sobre una base existente sin revisar su estado.
2. Abrir **`Lista de tareas/WindowsFormsApp2.sln`**.
3. En **`Lista de tareas/Form1.cs`**, cambiar el servidor de `SqlConnection conect`. La conexión está en ese archivo, no en `App.config`.

```csharp
new SqlConnection(
    @"Server=SERVIDOR\INSTANCIA;Database=ListaTareas;Trusted_Connection=True;"
);
```

4. Configurar permisos para el usuario de Windows, compilar y ejecutar.
5. Pulsar **Cargar**. Una base nueva comienza sin tareas.

Si se utiliza autenticación SQL, conservar las credenciales fuera del repositorio.

## Modelo

| Columna | Tipo | Uso |
| --- | --- | --- |
| `tareaID` | `INT IDENTITY`, PK | Identificador |
| `titulo` | `NVARCHAR(100)` | Título |
| `descripcion` | `NVARCHAR(250)` | Descripción |
| `estado` | `BIT` | 0 pendiente, 1 completada |

El SQL permite valores nulos en título, descripción y estado. La aplicación interpreta `estado` como booleano; al insertar datos manualmente, utilizar 0 o 1 para evitar errores de lectura.

## Uso

1. Escribir título y descripción y pulsar **Agregar**.
2. Seleccionar una fila para marcarla como realizada o eliminarla.
3. Aceptar o cancelar el diálogo correspondiente.
4. Usar **Cargar** para consultar otra vez la base; también vacía los campos de entrada.
5. Probar los ajustes de fondo y fuente durante la sesión.

**Guardar** recorre las filas del listado y las sincroniza por ID. No se observa una acción dedicada para editar el título y la descripción de una tarea existente; no debe confundirse con un editor completo ya implementado.

## Estructura

| Archivo | Responsabilidad |
| --- | --- |
| `Lista de tareas.sql` | Esquema inicial |
| `Lista de tareas/WindowsFormsApp2.sln` | Solución |
| `Lista de tareas/WindowsFormsApp2.csproj` | Proyecto 4.8 |
| `Lista de tareas/Program.cs` | Inicio |
| `Lista de tareas/Form1.cs` | Consultas y eventos |
| `Lista de tareas/Form1.Designer.cs` | Controles y asociaciones |
| `Lista de tareas/Properties/` | Recursos del proyecto |

## Comprobación manual y diagnóstico

Agregar y recargar una tarea; completarla y comprobar su persistencia; cancelar y confirmar una eliminación. Probar acciones sin fila seleccionada, entradas vacías y textos que excedan los tamaños del esquema. No hay suite automatizada incluida.

- **No conecta:** revisar servidor en `Form1.cs`, servicio SQL Server y permisos.
- **No aparecen registros al abrir:** pulsar Cargar; el constructor configura columnas, no carga datos.
- **Error al convertir estado:** buscar filas con `estado = NULL`.
- **Falta Framework 4.8:** instalar targeting pack.

## Límites y mejoras

No incluye usuarios, fechas límite, búsqueda, etiquetas ni retorno de completada a pendiente. La eliminación es física. Los ajustes visuales no tienen persistencia implementada.

Extraer el acceso a datos del formulario, mover la conexión a configuración, validar tamaños, añadir edición explícita y manejar errores con cierre de conexiones. Definir transacciones para guardados de varias filas y excluir `bin/` y `obj/` en futuras revisiones.

## Validación, licencia y autor

Revisión estática de SQL, formulario, diseñador y proyecto; no se compiló en Windows ni se ejecutó la base durante esta ampliación.

Consultar [LICENSE](LICENSE). Autor: [Jeison Rosario](https://github.com/Jeison-Rosario).
