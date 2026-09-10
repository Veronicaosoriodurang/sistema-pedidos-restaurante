# 2. Requisitos del sistema

## 2.1 Requisitos de usuario

| ID | Requisito | Descripción |
|---|---|---|
| RU-01 | Registro de pedidos | Como cliente, necesito agregar, modificar y confirmar productos de forma sencilla. |
| RU-02 | Seguimiento del pedido | Como cliente, necesito conocer en todo momento el estado de mi pedido. |
| RU-03 | Gestión de cuenta | Como cliente, necesito registrarme, iniciar sesión, actualizar mis datos y recuperar mi contraseña. |
| RU-04 | Consulta de menú | Como cliente, necesito consultar productos y precios disponibles. |
| RU-05 | Seguimiento de la entrega | Como cliente, necesito conocer el estado de mi entrega y disponer de una dirección registrada. |
| RU-06 | Gestión del menú | Como administrador, necesito agregar, modificar, eliminar y cambiar disponibilidad de productos. |
| RU-07 | Asignación de domiciliarios | Como administrador, necesito asignar un domiciliario a cada pedido. |
| RU-08 | Gestión de entregas asignadas | Como domiciliario, necesito ver mis pedidos y clientes asignados y actualizar el estado de la entrega. |

## 2.2 Requisitos funcionales

| ID | Nombre | Descripción | Rol |
|---|---|---|---|
| RF-01 | Agregar productos al pedido | Agregar productos del menú indicando cantidad. | Cliente |
| RF-02 | Modificar cantidad | Modificar cantidades y recalcular subtotal. | Cliente |
| RF-03 | Cancelar pedido | Cancelar mientras esté en estado pendiente. | Cliente |
| RF-04 | Consultar estado del pedido | Mostrar pendiente, en preparación, en camino, entregado o cancelado. | Cliente |
| RF-05 | Ver resumen del pedido | Mostrar productos, cantidades, precios y total antes de confirmar. | Cliente |
| RF-06 | Registrar cuenta | Registrar nombre, correo y contraseña. | Cliente |
| RF-07 | Iniciar sesión | Acceder con correo y contraseña. | Cliente |
| RF-08 | Actualizar datos personales | Actualizar nombre, teléfono y dirección. | Cliente |
| RF-09 | Consultar historial | Consultar pedidos anteriores, fecha, productos y estado final. | Cliente |
| RF-10 | Recuperar contraseña | Recuperar mediante enlace o código enviado al correo. | Cliente |
| RF-11 | Registrar dirección de entrega | Registrar una o varias direcciones y seleccionar una principal. | Cliente |
| RF-12 | Asignar domiciliario | Asignar un domiciliario disponible a un pedido confirmado. | Administrador |
| RF-13 | Consultar estado de la entrega | Mostrar asignada, en camino o entregada. | Cliente |
| RF-14 | Actualizar estado de la entrega | Cambiar a “en camino” o “entregado”. | Domiciliario |
| RF-15 | Ver información del pedido asignado | Mostrar productos, cantidades y dirección. | Domiciliario |
| RF-16 | Ver información del cliente asignado | Mostrar nombre y teléfono. | Domiciliario |
| RF-17 | Ver pedidos asignados | Mostrar la lista de pedidos asignados. | Domiciliario |
| RF-18 | Consultar menú | Mostrar productos disponibles con nombre, descripción, precio e imagen. | Cliente |
| RF-19 | Agregar productos al menú | Crear productos con nombre, precio y categoría. | Administrador |
| RF-20 | Modificar precios | Cambiar el precio de productos existentes. | Administrador |
| RF-21 | Marcar disponibilidad | Activar o desactivar la disponibilidad de productos. | Administrador |
| RF-22 | Eliminar productos del menú | Eliminar sin afectar pedidos históricos. | Administrador |

## 2.3 Requisitos no funcionales

| ID | Tipo | Requisito |
|---|---|---|
| RNF-01 | Rendimiento | Consultas del menú en máximo 2 segundos bajo condiciones normales. |
| RNF-02 | Disponibilidad | Disponibilidad mínima del 99 %. |
| RNF-03 | Usabilidad | Un usuario sin experiencia debe completar un pedido en máximo 5 pasos. |
| RNF-04 | Escalabilidad | Soportar al menos 100 usuarios concurrentes sin degradación perceptible. |
| RNF-05 | Actualización en tiempo real | Cambios de estado visibles en máximo 5 segundos. |
