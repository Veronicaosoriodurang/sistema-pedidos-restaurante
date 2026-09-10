# 7. Diagrama de clases

El siguiente diagrama representa las principales clases identificadas en el entregable y sus relaciones. Se presenta directamente en formato Mermaid para que pueda visualizarse correctamente dentro de GitHub sin depender de una imagen externa.

```mermaid
classDiagram
    class Usuario {
        +int idUsuario
        +string nombre
        +string correo
        +string contrasena
        +string telefono
        +iniciarSesion()
        +actualizarDatos()
    }

    class Cliente {
        +realizarPedido()
        +consultarHistorial()
        +registrarDireccion()
    }

    class Administrador {
        +agregarProducto()
        +modificarProducto()
        +eliminarProducto()
        +asignarDomiciliario()
    }

    class Domiciliario {
        +string disponibilidad
        +verPedidosAsignados()
        +actualizarEstadoEntrega()
    }

    class Pedido {
        +int idPedido
        +date fecha
        +double total
        +string estadoPedido
        +calcularTotal()
        +cancelarPedido()
        +confirmarPedido()
    }

    class Entrega {
        +int idEntrega
        +string estado
        +actualizarEstado()
    }

    class Direccion {
        +int idDireccion
        +string direccion
        +string municipio
        +actualizarDireccion()
    }

    class Producto {
        +int idProducto
        +string nombre
        +string descripcion
        +double precio
        +bool disponible
        +string imagen
        +modificarPrecio()
        +cambiarDisponibilidad()
    }

    class Categoria {
        +int idCategoria
        +string nombre
        +agregarProducto()
        +eliminarProducto()
    }

    Usuario <|-- Cliente
    Usuario <|-- Administrador
    Usuario <|-- Domiciliario

    Cliente "1" --> "0..*" Pedido : realiza
    Pedido "1" --> "0..1" Entrega : genera
    Domiciliario "1" --> "0..*" Entrega : atiende
    Entrega "0..*" --> "1" Direccion : se entrega en
    Pedido "1" --> "1..*" Producto : contiene
    Categoria "1" --> "0..*" Producto : clasifica
```

## Interpretación general

- `Usuario` funciona como clase general de la cual se especializan `Cliente`, `Administrador` y `Domiciliario`.
- El `Cliente` realiza pedidos.
- Un `Pedido` contiene uno o varios productos y puede generar una entrega.
- El `Domiciliario` gestiona las entregas que le son asignadas.
- La `Entrega` se relaciona con una dirección de destino.
- Los `Productos` se organizan por categorías.

> Este diagrama corresponde a la estructura conceptual presentada en el Entregable 1. La versión en Mermaid permite visualizarlo directamente en GitHub de forma más estable.
