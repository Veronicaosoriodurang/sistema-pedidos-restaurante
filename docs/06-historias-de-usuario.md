# 6. Historias de usuario

Las historias de usuario se gestionan como **Issues** en este repositorio, no solo como documentación estática. Cada HU tiene su propio issue en la pestaña [Issues](../../issues), con el título `HU-X.X: nombre`, la historia y los criterios de aceptación como checklist marcable, etiquetado según la épica a la que pertenece:

- `épica-pedidos`
- `épica-clientes`
- `épica-entregas`
- `épica-menu`

Este documento resume el contenido de esas historias como referencia rápida.

## Épica 1 — Gestión de pedidos

### HU-1.1 — Agregar productos
**Como** cliente, **quiero** agregar productos al pedido **para** realizar una compra.

**Criterios de aceptación**
- Al seleccionar y confirmar un producto desde el menú, se agrega con cantidad y precio.
- El subtotal se actualiza después de cada producto.

### HU-1.2 — Modificar cantidades
**Como** cliente, **quiero** modificar la cantidad de productos **para** ajustar mi pedido.

**Criterios de aceptación**
- El subtotal se recalcula automáticamente.
- No se permiten cantidades menores a 1.

### HU-1.3 — Cancelar pedido
**Como** cliente, **quiero** cancelar mi pedido **para** evitar que sea procesado.

**Criterios de aceptación**
- Solo se cancela cuando está pendiente.
- No se permite cancelar pedidos en camino o entregados.

### HU-1.4 — Consultar estado
**Criterios de aceptación**
- Muestra: pendiente, en preparación, en camino, entregado o cancelado.
- El estado se actualiza en tiempo real o al refrescar.

### HU-1.5 — Ver resumen
**Criterios de aceptación**
- Muestra producto, cantidad, precio unitario y total.
- Permite volver a editar antes de confirmar.

## Épica 2 — Gestión de clientes

### HU-2.1 — Registro
**Criterios de aceptación**
- Solicita al menos nombre, correo y contraseña.
- Valida que el correo no esté registrado.

### HU-2.2 — Inicio de sesión
**Criterios de aceptación**
- Permite entrar con credenciales correctas.
- Con credenciales incorrectas muestra un error sin revelar qué dato falló.

### HU-2.3 — Actualizar datos
**Criterios de aceptación**
- Editar nombre, teléfono y dirección.
- Cambios visibles de inmediato.

### HU-2.4 — Historial
**Criterios de aceptación**
- Lista fecha, productos y estado final.
- Permite ordenar o filtrar por fecha.

### HU-2.5 — Recuperar contraseña
**Criterios de aceptación**
- Solicitud por correo registrado.
- Envío de enlace o código de duración limitada.

## Épica 3 — Gestión de entregas

### HU-3.1 — Registrar dirección
**Criterios de aceptación**
- Permite varias direcciones.
- Una puede marcarse como principal.

### HU-3.2 — Asignar domiciliario
**Criterios de aceptación**
- El administrador ve domiciliarios disponibles.
- Al asignar, el domiciliario recibe el pedido.

### HU-3.3 — Consultar entrega
**Criterios de aceptación**
- Muestra asignada, en camino o entregada.
- Muestra el domiciliario asignado.

### HU-3.4 — Actualizar entrega
**Criterios de aceptación**
- El domiciliario cambia a "en camino" o "entregado".
- El cambio se refleja para cliente y administrador.

### HU-3.5 — Ver pedido asignado
**Criterios de aceptación**
- Productos, cantidades y dirección.
- Solo pedidos asignados al domiciliario.

### HU-3.6 — Ver cliente asignado
**Criterios de aceptación**
- Nombre y teléfono.
- No muestra datos sensibles adicionales.

### HU-3.7 — Ver pedidos asignados
**Criterios de aceptación**
- Lista pedidos activos por prioridad o fecha.
- Se actualiza al recibir una nueva asignación.

## Épica 4 — Gestión del menú

### HU-4.1 — Consultar menú
**Criterios de aceptación**
- Nombre, descripción, precio e imagen.
- Solo productos disponibles.

### HU-4.2 — Agregar productos
**Criterios de aceptación**
- Requiere nombre, precio y categoría.
- Se refleja inmediatamente si está disponible.

### HU-4.3 — Modificar precios
**Criterios de aceptación**
- Cambios visibles inmediatamente.
- No admite valores menores o iguales a cero.

### HU-4.4 — Cambiar disponibilidad
**Criterios de aceptación**
- Los no disponibles dejan de mostrarse al cliente.
- El administrador puede revertir el estado.

### HU-4.5 — Eliminar productos
**Criterios de aceptación**
- Deja de aparecer en el menú.
- No afecta registros históricos.
