# Restaurante App — Semana 15 (Conceptos fundamentales de manejo de eventos)

## Proposito de la Semana 15

Evolucion del aplicativo modular de la Semana 14 incorporando una operacion
sencilla de **venta** para estudiar los **fundamentos del manejo de eventos** en
Tkinter. Se conserva el inicio de sesion, la navegacion por menu lateral, la
consulta de usuarios y la gestion de productos ya desarrolladas, y se agrega una
seccion **Ventas** que relaciona un **usuario** con un **producto** y una
**fecha**.

La interfaz sigue separando responsabilidades: la UI coordina la interaccion del
usuario, y las validaciones, reglas y persistencia de la venta se delegan a
`RestauranteServicio`.

## Manejo de eventos aplicado

La venta permite observar el recorrido completo de una accion del usuario:

```text
USUARIO
   ↓
realiza una accion
   ↓
BOTON / COMPONENTE
   ↓
command=
   ↓
CALLBACK
   ↓
RestauranteServicio
   ↓
PERSISTENCIA (ventas.json)
   ↓
RESPUESTA EN LA INTERFAZ (tabla)
```

- Un boton **"Registrar venta"** se asocia con el metodo `registrar_venta()`
  mediante `command=` (**sin parentesis**), por lo que el callback se ejecuta
  cuando el usuario presiona el boton.
- El callback obtiene las selecciones de usuario y producto, solicita el
  registro a `RestauranteServicio`, limpia el formulario, actualiza la tabla
  de ventas y comunica el resultado al usuario.
- No se lee ni se escribe el JSON directamente desde la vista; toda la
  persistencia se maneja en la capa de servicios.

## Estructura del proyecto

```
restaurante_app/
├── datos/
│   ├── productos.json     # Productos del restaurante
│   ├── usuarios.json      # Usuarios para el acceso
│   └── ventas.json        # Ventas registradas
├── modelos/
│   ├── __init__.py
│   ├── producto.py        # Entidad Producto
│   ├── usuario.py         # Entidad Usuario
│   └── venta.py           # Entidad Venta (usuario + producto + fecha)
├── servicios/
│   ├── __init__.py
│   ├── archivo_servicio.py         # Lectura/escritura de archivos JSON
│   └── restaurante_servicio.py     # Acceso, productos y ventas
├── ui/
│   ├── __init__.py
│   ├── login_view.py      # Pantalla de acceso
│   └── main_view.py       # Menu lateral y secciones (incluye Ventas)
├── assets/
│   ├── icons/            # Iconos usados en la interfaz
│   └── logo/             # Logo e icono de la ventana
├── main.py
└── README.md
```

## Gestion de ventas

En la seccion **Ventas**:

- se selecciona un **usuario registrado** con un `ttk.Combobox`;
- se selecciona un **producto registrado** con un `ttk.Combobox`;
- se presiona el boton **"Registrar venta"**, vinculado mediante `command=`;
- el callback solicita la operacion a `RestauranteServicio`, que valida la
  existencia del usuario y del producto y genera el identificador (V001, ...);
- la venta se guarda en `datos/ventas.json` y se presenta en un `ttk.Treeview`.

Los recursos visuales (iconos y logo) se integran desde la carpeta `assets/`
como parte de la experiencia de usuario.

## Como ejecutar

1. Clonar o descargar el repositorio.
2. Tener instalado **Python 3** con la libreria estandar (Tkinter incluido).
3. Desde la carpeta `restaurante_app/` ejecutar:

   ```bash
   python main.py
   ```

4. Iniciar sesion con un usuario registrado en `datos/usuarios.json`,
   navegar por el menu lateral y probar el registro de una venta.

## Acceso de prueba

El archivo `datos/usuarios.json` incluye los siguientes usuarios para poder
iniciar sesion en la aplicacion:

| Usuario  | Clave       | Rol          |
|----------|-------------|--------------|
| `admin`  | `admin123`  | Administrador (U001) |
| `cajero` | `cajero123` | Cajera (U002)         |

Tambien puede agregarse o quitarse usuarios editando directamente ese archivo
JSON antes de ejecutar la aplicacion.