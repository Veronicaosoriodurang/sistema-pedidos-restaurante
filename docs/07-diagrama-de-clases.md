# 7. Diagrama de clases

El siguiente diagrama representa las principales clases identificadas y sus relaciones. Se presenta directamente en formato Mermaid

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
        +bool disponibilidad
        +verPedidosAsignados()
        +actualizarEstadoEntrega()
    }

    class Pedido {
        +int idPedido
        +date fecha
        +double total
        +string estadoPedido
        +agregarProducto()
        +modificarCantidad()
        +cancelarPedido()
        +confirmarPedido()
    }

    class DetallePedido {
        +int idDetalle
        +int cantidad
        +double precioUnitario
        +double subtotal
        +calcularSubtotal()
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
        +bool principal
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
    Cliente "1" --> "0..*" Direccion : registra
    Pedido "1" --> "1..*" DetallePedido : contiene
    DetallePedido "0..*" --> "1" Producto : referencia
    Pedido "1" --> "0..1" Entrega : genera
    Domiciliario "1" --> "0..*" Entrega : atiende
    Entrega "0..*" --> "1" Direccion : se entrega en
    Categoria "1" --> "0..*" Producto : clasifica
```

## Interpretación general

- `Usuario` funciona como clase general de la cual se especializan `Cliente`, `Administrador` y `Domiciliario`.
- El `Cliente` realiza pedidos y registra sus propias direcciones de entrega.
- Un `Pedido` está compuesto por uno o varios `DetallePedido`, cada uno con la cantidad y el subtotal de un producto específico. Esta clase intermedia evita relacionar `Pedido` y `Producto` directamente, ya que un mismo producto puede aparecer en muchos pedidos distintos.
- Un `Pedido` puede generar una `Entrega`.
- El `Domiciliario` gestiona las entregas que le son asignadas.
- La `Entrega` se relaciona con una dirección de destino, y esa dirección puede marcarse como principal.
- Los `Productos` se organizan por categorías.
