# Criterios de aceptación en formato Gherkin

Cada historia de usuario tiene entre 3 y 6 escenarios. Los criterios son los mismos de la plantilla de refinamiento, escritos como `Feature` y `Scenario`.

## E1 — Gestión de pedidos

```gherkin
Feature: HU-1.1 Agregar productos al pedido
  Como cliente
  quiero agregar productos al pedido
  para realizar una compra

  Scenario: Agregar un producto disponible desde el menú
    Given que el cliente autenticado está viendo el menú
    When selecciona un producto disponible, indica la cantidad y confirma
    Then el sistema lo agrega al pedido con su cantidad y precio

  Scenario: Actualizar el subtotal al agregar otro producto
    Given que el pedido ya contiene productos
    When el cliente agrega otro producto
    Then el sistema muestra el subtotal actualizado

  Scenario: Intentar agregar un producto no disponible
    Given que un producto está marcado como "no disponible"
    When el cliente intenta agregarlo al pedido
    Then el sistema no lo permite e informa que el producto no está disponible

  Scenario: Intentar agregar un producto sin sesión iniciada
    Given que el cliente no ha iniciado sesión
    When intenta agregar un producto al pedido
    Then el sistema le solicita iniciar sesión o registrarse
```

```gherkin
Feature: HU-1.2 Modificar cantidad de productos
  Como cliente
  quiero modificar la cantidad de productos
  para ajustar mi pedido

  Scenario: Cambiar la cantidad de un producto
    Given un producto en el pedido
    When el cliente cambia su cantidad a un número entero mayor o igual a 1
    Then el subtotal del producto y el total del pedido se recalculan automáticamente

  Scenario: Intentar dejar una cantidad menor a 1
    Given un producto con cantidad 1
    When el cliente intenta ponerla en 0 o en un valor menor a 1
    Then el sistema no lo permite y sugiere usar la opción de eliminar

  Scenario: Eliminar un producto del pedido
    Given un producto en el pedido
    When el cliente usa la opción de eliminar
    Then el producto sale del pedido y el total se recalcula

  Scenario: Ingresar una cantidad no numérica
    Given que el cliente escribe un valor no numérico como cantidad
    When intenta guardar el cambio
    Then el sistema rechaza el valor y conserva la cantidad anterior
```

```gherkin
Feature: HU-1.3 Cancelar pedido
  Como cliente
  quiero cancelar mi pedido
  para evitar que sea procesado

  Scenario: Cancelar un pedido pendiente
    Given un pedido del cliente en estado "pendiente"
    When el cliente lo cancela
    Then el pedido cambia a estado "cancelado" y no puede ser procesado

  Scenario: Intentar cancelar un pedido que ya no está pendiente
    Given un pedido en estado "en preparación", "en camino" o "entregado"
    When el cliente intenta cancelarlo
    Then el sistema no lo permite e informa el motivo

  Scenario: Ver el pedido cancelado en el seguimiento y el historial
    Given un pedido que fue cancelado
    When el cliente consulta su estado o su historial
    Then ve el pedido con estado "cancelado"

  Scenario: El pedido cancelado deja de estar disponible para asignación
    Given un pedido cancelado
    When el administrador revisa los pedidos recibidos
    Then el pedido no aparece como activo ni disponible para asignar a un domiciliario
```

```gherkin
Feature: HU-1.4 Consultar estado del pedido
  Como cliente
  quiero consultar el estado de mi pedido
  para saber en qué proceso se encuentra

  Scenario: Ver el estado actual del pedido
    Given que el cliente tiene un pedido activo
    When consulta su estado
    Then el sistema muestra uno de los valores: pendiente, en preparación, en camino, entregado o cancelado

  Scenario: Ver el cambio de estado en tiempo real
    Given que el estado del pedido cambia en el sistema
    When el cliente tiene abierta la vista de seguimiento
    Then ve el nuevo estado en máximo 5 segundos o al refrescar la vista

  Scenario: Ver un pedido cancelado
    Given un pedido que fue cancelado
    When el cliente consulta su estado
    Then el sistema muestra "cancelado"

  Scenario: Intentar ver el pedido de otro cliente
    Given un pedido que pertenece a otro cliente
    When el cliente intenta consultarlo
    Then el sistema no le muestra la información
```

```gherkin
Feature: HU-1.5 Ver resumen y total del pedido
  Como cliente
  quiero ver el resumen y total de mi pedido antes de confirmarlo
  para revisar mi compra

  Scenario: Consultar el resumen antes de confirmar
    Given que el cliente tiene productos en su pedido
    When accede al resumen
    Then el sistema muestra cada producto con su cantidad, precio unitario y el total general

  Scenario: Editar el pedido desde el resumen
    Given que el cliente está en el resumen
    When decide hacer un cambio
    Then puede volver a editar el pedido antes de confirmar

  Scenario: Intentar confirmar un pedido sin productos
    Given un pedido sin productos
    When el cliente intenta confirmarlo
    Then el sistema no lo permite e indica que debe agregar al menos un producto

  Scenario: Confirmar el pedido desde el resumen
    Given que el cliente revisó el resumen
    When confirma el pedido
    Then el sistema lo registra y muestra la confirmación con el identificador del pedido
```

## E2 — Gestión de clientes

```gherkin
Feature: HU-2.1 Registro de cliente
  Como cliente
  quiero registrarme
  para poder realizar pedidos

  Scenario: Registro exitoso
    Given un usuario sin cuenta
    When ingresa nombre, correo y contraseña válidos y confirma
    Then el sistema crea la cuenta del cliente

  Scenario: Registro con un correo ya registrado
    Given que el correo ya está registrado
    When el usuario intenta registrarse con ese correo
    Then el sistema rechaza el registro e informa que el correo ya existe

  Scenario: Registro con campos obligatorios vacíos
    Given que falta el nombre, el correo o la contraseña
    When el usuario intenta registrarse
    Then el sistema indica los campos obligatorios sin crear la cuenta

  Scenario: Registro con correo de formato inválido
    Given que el correo tiene un formato inválido
    When el usuario intenta registrarse
    Then el sistema rechaza el dato e indica el error
```

```gherkin
Feature: HU-2.2 Inicio de sesión
  Como cliente
  quiero iniciar sesión
  para acceder a mi cuenta

  Scenario: Inicio de sesión con credenciales correctas
    Given un cliente con cuenta registrada
    When ingresa su correo y contraseña correctos
    Then accede a su cuenta

  Scenario: Inicio de sesión con credenciales incorrectas
    Given un cliente con cuenta registrada
    When ingresa credenciales incorrectas
    Then el sistema muestra un mensaje de error sin indicar cuál dato falló

  Scenario: Inicio de sesión con campos vacíos
    Given que el correo o la contraseña están vacíos
    When el cliente intenta iniciar sesión
    Then el sistema le pide completar ambos campos

  Scenario: Realizar un pedido sin sesión iniciada
    Given un usuario sin sesión iniciada
    When intenta realizar un pedido
    Then el sistema lo dirige a iniciar sesión
```

```gherkin
Feature: HU-2.3 Actualizar datos personales
  Como cliente
  quiero actualizar mis datos personales
  para mantener mi información al día

  Scenario: Editar la información del perfil
    Given un cliente en su perfil
    When edita su nombre, teléfono o dirección y guarda
    Then los cambios se guardan y se reflejan de inmediato en el perfil

  Scenario: Intentar guardar sin nombre
    Given que el cliente deja vacío el nombre
    When intenta guardar
    Then el sistema no guarda los cambios e indica que el campo es obligatorio

  Scenario: Intentar guardar un teléfono inválido
    Given que el cliente ingresa un teléfono con formato inválido
    When intenta guardar
    Then el sistema rechaza el dato e indica el error

  Scenario: Salir sin guardar los cambios
    Given que el cliente edita sus datos
    When sale de la pantalla sin guardar
    Then el sistema conserva los datos anteriores
```

```gherkin
Feature: HU-2.4 Ver historial de pedidos
  Como cliente
  quiero ver mi información e historial de pedidos
  para revisar mis compras anteriores

  Scenario: Consultar el historial de pedidos
    Given un cliente con pedidos realizados
    When accede al historial
    Then el sistema lista cada pedido con su fecha, productos y estado final

  Scenario: Ordenar o filtrar el historial por fecha
    Given que el cliente está viendo su historial
    When ordena o filtra por fecha
    Then la lista se ajusta según lo solicitado

  Scenario: Consultar el historial sin pedidos
    Given un cliente sin pedidos anteriores
    When accede al historial
    Then el sistema muestra un mensaje indicando que aún no tiene pedidos

  Scenario: Ver pedidos con productos eliminados del menú
    Given un pedido que contiene un producto que luego fue eliminado del menú
    When el cliente consulta el historial
    Then el pedido se sigue mostrando completo

  Scenario: Ver solo los pedidos propios
    Given un cliente con sesión iniciada
    When consulta el historial
    Then solo ve sus propios pedidos
```

```gherkin
Feature: HU-2.5 Recuperar contraseña
  Como cliente
  quiero recuperar mi contraseña
  para volver a acceder a mi cuenta si la olvido

  Scenario: Solicitar la recuperación de contraseña
    Given un cliente que olvidó su contraseña
    When ingresa su correo registrado y solicita la recuperación
    Then el sistema envía a ese correo un enlace o código para restablecerla

  Scenario: Restablecer la contraseña con un enlace vigente
    Given un enlace o código vigente
    When el cliente define una nueva contraseña válida
    Then el sistema la actualiza y puede iniciar sesión con ella

  Scenario: Usar un enlace o código vencido
    Given un enlace o código vencido
    When el cliente intenta usarlo
    Then el sistema lo rechaza y permite solicitar uno nuevo

  Scenario: Solicitar recuperación con un correo no registrado
    Given un correo que no está registrado
    When se solicita la recuperación
    Then el sistema muestra un mensaje genérico sin revelar si el correo existe
```

## E3 — Gestión de entregas

```gherkin
Feature: HU-3.1 Registrar dirección de entrega
  Como cliente
  quiero registrar mi dirección de entrega
  para que mi pedido llegue al lugar correcto

  Scenario: Registrar una nueva dirección
    Given un cliente en Mis Direcciones
    When agrega una nueva dirección completa
    Then el sistema la guarda junto a sus direcciones registradas

  Scenario: Marcar una dirección como principal
    Given un cliente con varias direcciones
    When marca una como principal
    Then esa queda como principal y las demás dejan de serlo

  Scenario: Asociar la dirección al pedido
    Given que el cliente va a confirmar un pedido
    When selecciona una de sus direcciones
    Then la dirección queda asociada al pedido antes de confirmarlo

  Scenario: Intentar guardar una dirección incompleta
    Given que la dirección está incompleta
    When el cliente intenta guardarla
    Then el sistema no la guarda e indica los datos faltantes

  Scenario: Confirmar un pedido sin direcciones registradas
    Given un cliente sin direcciones registradas
    When intenta confirmar un pedido
    Then el sistema le solicita registrar una dirección
```

```gherkin
Feature: HU-3.2 Asignar domiciliario a un pedido
  Como administrador
  quiero asignar un domiciliario a un pedido
  para coordinar la entrega

  Scenario: Ver los domiciliarios disponibles
    Given un pedido confirmado
    When el administrador abre la asignación
    Then el sistema muestra la lista de domiciliarios disponibles

  Scenario: Asignar un domiciliario a un pedido confirmado
    Given un pedido confirmado y un domiciliario disponible
    When el administrador lo asigna
    Then el estado del pedido cambia y el pedido aparece en la lista del domiciliario como notificación del pedido asignado

  Scenario: Asignar sin domiciliarios disponibles
    Given que no hay domiciliarios disponibles
    When el administrador intenta asignar un pedido
    Then el sistema informa la situación y el pedido queda sin asignar

  Scenario: Intentar asignar un pedido cancelado o no confirmado
    Given un pedido cancelado o no confirmado
    When el administrador intenta asignarlo
    Then el sistema no lo permite
```

```gherkin
Feature: HU-3.3 Consultar estado de la entrega
  Como cliente
  quiero consultar el estado de mi entrega
  para saber cuándo llegará mi pedido

  Scenario: Ver el estado de la entrega
    Given un pedido con entrega en curso
    When el cliente consulta la entrega
    Then el sistema muestra su estado: asignada, en camino o entregada

  Scenario: Ver el domiciliario asignado
    Given una entrega ya asignada
    When el cliente consulta la entrega
    Then el sistema muestra el nombre del domiciliario asignado

  Scenario: Consultar una entrega aún sin asignar
    Given un pedido que aún no tiene domiciliario asignado
    When el cliente consulta la entrega
    Then el sistema indica que la entrega está pendiente de asignación

  Scenario: Ver el cambio de estado de la entrega en tiempo real
    Given que el domiciliario cambia el estado de la entrega
    When el cliente tiene abierta la vista de seguimiento
    Then ve el nuevo estado en máximo 5 segundos
```

```gherkin
Feature: HU-3.4 Actualizar estado de la entrega
  Como domiciliario
  quiero actualizar el estado de la entrega
  para informar su progreso

  Scenario: Marcar la entrega como en camino
    Given una entrega asignada al domiciliario
    When pulsa "En camino"
    Then el estado de la entrega cambia a "en camino"

  Scenario: Marcar la entrega como entregada
    Given una entrega en camino
    When el domiciliario pulsa "Entregado"
    Then el estado de la entrega cambia a "entregado"

  Scenario: Reflejar el cambio para el cliente y el administrador
    Given que el domiciliario actualiza el estado
    When el cliente o el administrador consultan la entrega
    Then ven el nuevo estado de inmediato, en máximo 5 segundos

  Scenario: Intentar actualizar una entrega no asignada
    Given una entrega que no fue asignada al domiciliario
    When intenta actualizar su estado
    Then el sistema no se lo permite
```

```gherkin
Feature: HU-3.5 Ver información del pedido asignado
  Como domiciliario
  quiero ver la información del pedido asignado
  para saber qué debo entregar

  Scenario: Consultar el detalle de un pedido asignado
    Given un pedido asignado al domiciliario
    When abre su detalle
    Then ve los productos, las cantidades y la dirección de entrega

  Scenario: Intentar ver un pedido no asignado
    Given un pedido que no fue asignado al domiciliario
    When intenta consultarlo
    Then el sistema no le muestra la información

  Scenario: Verificar la dirección de entrega del pedido
    Given un pedido con dirección seleccionada por el cliente
    When el domiciliario abre el detalle
    Then la dirección mostrada coincide con la asociada al pedido

  Scenario: Ver un pedido ya entregado en la lista del día
    Given un pedido ya entregado
    When el domiciliario revisa su lista del día
    Then el pedido sigue visible como completado
```

```gherkin
Feature: HU-3.6 Ver información del cliente asignado
  Como domiciliario
  quiero ver la información del cliente asignado
  para poder contactarlo si es necesario

  Scenario: Consultar los datos básicos del cliente
    Given un pedido asignado al domiciliario
    When consulta la información del cliente
    Then ve su nombre y su teléfono

  Scenario: No mostrar datos sensibles
    Given un pedido asignado
    When el domiciliario consulta al cliente
    Then no se muestran datos sensibles adicionales como contraseña o correo

  Scenario: Intentar ver el cliente de un pedido no asignado
    Given un pedido que no fue asignado al domiciliario
    When intenta ver los datos del cliente
    Then el sistema no se los muestra

  Scenario: Cliente sin teléfono registrado
    Given un cliente sin teléfono registrado
    When el domiciliario consulta sus datos
    Then el sistema muestra el nombre e indica que no hay teléfono disponible
```

```gherkin
Feature: HU-3.7 Ver pedidos asignados
  Como domiciliario
  quiero ver los pedidos que se me han asignado
  para organizar mis entregas

  Scenario: Consultar la lista de pedidos asignados
    Given un domiciliario con pedidos asignados
    When consulta su lista
    Then ve sus pedidos activos ordenados por prioridad o fecha

  Scenario: Recibir un nuevo pedido asignado
    Given que el domiciliario está viendo su lista
    When se le asigna un nuevo pedido
    Then la lista se actualiza e incluye el nuevo pedido

  Scenario: Consultar la lista sin pedidos asignados
    Given un domiciliario sin pedidos asignados
    When consulta su lista
    Then el sistema muestra un mensaje indicando que no tiene entregas pendientes

  Scenario: Ver solo los pedidos propios
    Given un domiciliario con sesión iniciada
    When consulta su lista
    Then solo ve los pedidos que le fueron asignados a él
```

## E4 — Gestión del menú

```gherkin
Feature: HU-4.1 Consultar menú
  Como cliente
  quiero consultar el menú
  para conocer los productos disponibles

  Scenario: Ver el menú disponible
    Given que el cliente accede al menú
    When lo consulta
    Then ve el nombre, la descripción, el precio y la imagen de cada producto

  Scenario: No listar productos no disponibles
    Given que existen productos marcados como "no disponible"
    When el cliente consulta el menú
    Then esos productos no se listan

  Scenario: Ver los productos organizados por categoría
    Given que el menú tiene productos de distintas categorías
    When el cliente lo consulta
    Then los productos aparecen organizados por categoría (entradas, platos fuertes, bebidas, postres)

  Scenario: Consultar el menú en el tiempo esperado
    Given el sistema bajo condiciones normales de carga
    When el cliente consulta el menú
    Then la respuesta tarda como máximo 2 segundos
```

```gherkin
Feature: HU-4.2 Agregar productos al menú
  Como administrador
  quiero agregar productos al menú
  para ofrecer nuevos platos

  Scenario: Agregar un producto nuevo
    Given el administrador en el formulario de nuevo platillo
    When ingresa nombre, precio y categoría válidos y guarda
    Then el producto queda creado

  Scenario: Ver el producto nuevo en el menú
    Given un producto creado y marcado como disponible
    When el cliente consulta el menú
    Then el producto aparece de inmediato

  Scenario: Intentar guardar con campos obligatorios vacíos
    Given que falta el nombre, el precio o la categoría
    When el administrador intenta guardar
    Then el sistema no crea el producto e indica los campos obligatorios

  Scenario: Intentar guardar con precio inválido
    Given que el precio es menor o igual a cero
    When el administrador intenta guardar
    Then el sistema rechaza el dato por incumplir la regla de precios

  Scenario: Crear un producto como no disponible
    Given un producto creado como "no disponible"
    When el cliente consulta el menú
    Then el producto no aparece
```

```gherkin
Feature: HU-4.3 Modificar precios de productos
  Como administrador
  quiero modificar los precios de los productos
  para mantener la información actualizada

  Scenario: Modificar el precio de un producto
    Given un producto existente
    When el administrador cambia su precio a un valor mayor que cero y guarda
    Then el cambio se refleja en el menú al instante

  Scenario: Intentar poner un precio negativo o cero
    Given un producto existente
    When el administrador ingresa un precio negativo o igual a cero
    Then el sistema no lo permite

  Scenario: Intentar poner un precio no numérico
    Given que el administrador escribe un valor no numérico como precio
    When intenta guardar
    Then el sistema rechaza el dato y conserva el precio anterior

  Scenario: Conservar el precio en pedidos ya registrados
    Given un pedido ya registrado
    When el administrador cambia el precio del producto
    Then el pedido conserva el precio unitario con el que fue hecho
```

```gherkin
Feature: HU-4.4 Cambiar disponibilidad de productos
  Como administrador
  quiero marcar productos como disponibles o no disponibles
  para reflejar la disponibilidad real del menú

  Scenario: Marcar un producto como no disponible
    Given un producto disponible
    When el administrador lo marca como "no disponible"
    Then deja de mostrarse en el menú visible para el cliente

  Scenario: Revertir la disponibilidad
    Given un producto "no disponible"
    When el administrador lo marca nuevamente como disponible
    Then vuelve a mostrarse en el menú

  Scenario: Impedir agregar un producto no disponible
    Given un producto "no disponible"
    When un cliente intenta agregarlo a un nuevo pedido
    Then el sistema no lo permite

  Scenario: Confirmar un pedido con un producto que se agotó
    Given que un cliente tiene el producto en su pedido sin confirmar y el administrador lo marca como "no disponible"
    When el cliente intenta confirmar
    Then el sistema informa el producto afectado y solicita actualizar el pedido
```

```gherkin
Feature: HU-4.5 Eliminar productos del menú
  Como administrador
  quiero eliminar productos del menú
  para retirar los que ya no se ofrecen

  Scenario: Eliminar un producto del menú
    Given un producto existente
    When el administrador confirma su eliminación
    Then el producto deja de listarse en el menú

  Scenario: Conservar los pedidos históricos
    Given un producto que ya aparece en pedidos históricos
    When el administrador lo elimina
    Then esos registros no se ven afectados

  Scenario: Cancelar la eliminación
    Given que el administrador selecciona eliminar un producto
    When el sistema solicita confirmación y el administrador la cancela
    Then el producto se conserva en el menú

  Scenario: Impedir pedir un producto eliminado
    Given un producto eliminado
    When un cliente consulta el menú o intenta agregarlo a un nuevo pedido
    Then el producto no aparece ni puede agregarse
```
