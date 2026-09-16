# Criterios de aceptación en formato Gherkin

## Épica 1 — Gestión de pedidos

```gherkin
Feature: HU-1.1 Agregar productos al pedido
  Como cliente
  quiero agregar productos al pedido
  para realizar una compra

  Scenario: Agregar un producto desde el menú
    Given que estoy viendo el menú
    When selecciono un producto y confirmo
    Then se agrega al pedido con su cantidad y precio
    And el sistema muestra el subtotal actualizado
```

```gherkin
Feature: HU-1.2 Modificar cantidad de productos
  Como cliente
  quiero modificar la cantidad de productos
  para ajustar mi pedido

  Scenario: Cambiar la cantidad de un producto en el pedido
    Given un producto ya agregado a mi pedido
    When cambio su cantidad
    Then el subtotal se recalcula automáticamente

  Scenario: Intentar dejar una cantidad menor a 1
    Given un producto en mi pedido
    When intento poner una cantidad menor a 1
    Then el sistema no lo permite
    And me indica que debo usar la opción de eliminar
```

```gherkin
Feature: HU-1.3 Cancelar pedido
  Como cliente
  quiero cancelar mi pedido
  para evitar que sea procesado

  Scenario: Cancelar un pedido pendiente
    Given un pedido en estado "pendiente"
    When cancelo el pedido
    Then el pedido cambia a estado "cancelado"
    And ya no puede ser procesado

  Scenario: Intentar cancelar un pedido en camino o entregado
    Given un pedido en estado "en camino" o "entregado"
    When intento cancelarlo
    Then el sistema no permite la cancelación
```

```gherkin
Feature: HU-1.4 Consultar estado del pedido
  Como cliente
  quiero consultar el estado de mi pedido
  para saber en qué proceso se encuentra

  Scenario: Ver el estado actual del pedido
    Given que tengo un pedido activo
    When consulto su estado
    Then veo uno de los siguientes valores: pendiente, en preparación, en camino, entregado o cancelado

  Scenario: El estado se actualiza
    Given que el estado de mi pedido cambia en el sistema
    When refresco la vista o el sistema actualiza en tiempo real
    Then veo el estado más reciente
```

```gherkin
Feature: HU-1.5 Ver resumen y total del pedido
  Como cliente
  quiero ver el resumen y total de mi pedido antes de confirmarlo
  para revisar mi compra

  Scenario: Consultar el resumen antes de confirmar
    Given que tengo productos agregados a mi pedido
    When accedo al resumen del pedido
    Then veo cada producto, cantidad, precio unitario y el total general

  Scenario: Editar el pedido desde el resumen
    Given que estoy viendo el resumen de mi pedido
    When decido hacer un cambio
    Then puedo volver a editar el pedido antes de confirmar
```

## Épica 2 — Gestión de clientes

```gherkin
Feature: HU-2.1 Registro
  Como cliente
  quiero registrarme
  para poder realizar pedidos

  Scenario: Registro exitoso
    Given que no tengo una cuenta
    When me registro ingresando nombre, correo y contraseña
    Then el sistema valida que el correo no esté previamente registrado
    And crea mi cuenta
```

```gherkin
Feature: HU-2.2 Inicio de sesión
  Como cliente
  quiero iniciar sesión
  para acceder a mi cuenta

  Scenario: Inicio de sesión con credenciales correctas
    Given que tengo una cuenta registrada
    When ingreso mi correo y contraseña correctos
    Then accedo a mi cuenta

  Scenario: Inicio de sesión con credenciales incorrectas
    Given que tengo una cuenta registrada
    When ingreso credenciales incorrectas
    Then el sistema muestra un mensaje de error
    And no indica cuál dato falló específicamente
```

```gherkin
Feature: HU-2.3 Actualizar datos personales
  Como cliente
  quiero actualizar mis datos personales
  para mantener mi información al día

  Scenario: Editar información del perfil
    Given que estoy en mi perfil
    When edito mi nombre, teléfono o dirección
    Then los cambios se guardan
    And se reflejan de inmediato en mi perfil
```

```gherkin
Feature: HU-2.4 Historial de pedidos
  Como cliente
  quiero ver mi historial de pedidos
  para revisar mis compras anteriores

  Scenario: Consultar el historial
    Given que tengo pedidos realizados anteriormente
    When accedo al historial
    Then veo la fecha, los productos y el estado final de cada pedido

  Scenario: Ordenar o filtrar el historial
    Given que estoy viendo mi historial de pedidos
    When aplico un filtro o un orden por fecha
    Then la lista se ajusta según lo solicitado
```

```gherkin
Feature: HU-2.5 Recuperar contraseña
  Como cliente
  quiero recuperar mi contraseña
  para volver a acceder a mi cuenta si la olvido

  Scenario: Solicitar recuperación de contraseña
    Given que olvidé mi contraseña
    When ingreso mi correo registrado y solicito la recuperación
    Then el sistema envía un enlace o código de restablecimiento
    And ese enlace o código tiene una duración limitada
```

## Épica 3 — Gestión de entregas

```gherkin
Feature: HU-3.1 Registrar dirección de entrega
  Como cliente
  quiero registrar mi dirección de entrega
  para que mi pedido llegue al lugar correcto

  Scenario: Registrar una nueva dirección
    Given que estoy gestionando mis direcciones
    When agrego una nueva dirección
    Then puedo marcarla como principal

  Scenario: Usar la dirección en un pedido
    Given que tengo al menos una dirección registrada
    When confirmo un pedido
    Then la dirección seleccionada se asocia al pedido
```

```gherkin
Feature: HU-3.2 Asignar domiciliario a un pedido
  Como administrador
  quiero asignar un domiciliario a un pedido
  para coordinar la entrega

  Scenario: Asignar un domiciliario disponible
    Given un pedido confirmado y domiciliarios disponibles
    When selecciono un domiciliario y lo asigno al pedido
    Then el pedido cambia de estado
    And el domiciliario recibe la notificación del pedido
```

```gherkin
Feature: HU-3.3 Consultar estado de la entrega
  Como cliente
  quiero consultar el estado de mi entrega
  para saber cuándo llegará mi pedido

  Scenario: Ver el estado de la entrega
    Given que mi pedido tiene una entrega en curso
    When consulto el estado de la entrega
    Then veo si está asignada, en camino o entregada

  Scenario: Ver el domiciliario asignado
    Given que mi entrega ya fue asignada a un domiciliario
    When consulto el estado de la entrega
    Then veo el nombre del domiciliario asignado
```

```gherkin
Feature: HU-3.4 Actualizar estado de la entrega
  Como domiciliario
  quiero actualizar el estado de la entrega
  para informar su progreso

  Scenario: Cambiar el estado de una entrega
    Given una entrega asignada a mí
    When actualizo su estado a "en camino" o "entregado"
    Then el cambio se refleja de inmediato para el cliente y el administrador
```

```gherkin
Feature: HU-3.5 Ver información del pedido asignado
  Como domiciliario
  quiero ver la información del pedido asignado
  para saber qué debo entregar

  Scenario: Consultar el detalle de un pedido asignado
    Given un pedido que me fue asignado
    When consulto su detalle
    Then veo los productos, las cantidades y la dirección de entrega

  Scenario: Intentar ver un pedido no asignado
    Given un pedido que no me fue asignado
    When intento consultarlo
    Then el sistema no me permite verlo
```

```gherkin
Feature: HU-3.6 Ver información del cliente asignado
  Como domiciliario
  quiero ver la información del cliente asignado
  para poder contactarlo si es necesario

  Scenario: Consultar datos básicos del cliente
    Given un pedido que me fue asignado
    When consulto la información del cliente
    Then veo su nombre y teléfono
    And no veo datos sensibles adicionales como contraseña o correo
```

```gherkin
Feature: HU-3.7 Ver pedidos asignados
  Como domiciliario
  quiero ver los pedidos que se me han asignado
  para organizar mis entregas

  Scenario: Consultar la lista de pedidos asignados
    Given que tengo pedidos asignados
    When consulto mi lista de pedidos
    Then veo mis pedidos activos ordenados por prioridad o fecha

  Scenario: Recibir un nuevo pedido asignado
    Given que estoy viendo mi lista de pedidos
    When se me asigna un nuevo pedido
    Then la lista se actualiza automáticamente
```

## Épica 4 — Gestión del menú

```gherkin
Feature: HU-4.1 Consultar menú
  Como cliente
  quiero consultar el menú
  para conocer los productos disponibles

  Scenario: Ver el menú disponible
    Given que accedo al menú del restaurante
    When lo consulto
    Then veo nombre, descripción, precio e imagen de cada producto
    And solo se listan los productos marcados como disponibles
```

```gherkin
Feature: HU-4.2 Agregar productos al menú
  Como administrador
  quiero agregar productos al menú
  para ofrecer nuevos platos

  Scenario: Agregar un nuevo producto
    Given que estoy en la administración del menú
    When agrego un producto indicando nombre, precio y categoría
    Then el producto aparece de inmediato en el menú si está marcado como disponible
```

```gherkin
Feature: HU-4.3 Modificar precios de productos
  Como administrador
  quiero modificar los precios de los productos
  para mantener la información actualizada

  Scenario: Modificar el precio de un producto
    Given un producto existente en el menú
    When cambio su precio a un valor válido
    Then el cambio se refleja en el menú al instante

  Scenario: Intentar poner un precio inválido
    Given un producto existente en el menú
    When intento poner un precio negativo o igual a cero
    Then el sistema no lo permite
```

```gherkin
Feature: HU-4.4 Cambiar disponibilidad de productos
  Como administrador
  quiero marcar productos como disponibles o no disponibles
  para reflejar su disponibilidad real

  Scenario: Marcar un producto como no disponible
    Given un producto disponible en el menú
    When lo marco como no disponible
    Then deja de mostrarse en el menú visible para el cliente

  Scenario: Revertir la disponibilidad
    Given un producto marcado como no disponible
    When lo marco nuevamente como disponible
    Then vuelve a mostrarse en el menú
```

```gherkin
Feature: HU-4.5 Eliminar productos del menú
  Como administrador
  quiero eliminar productos del menú
  para retirar los que ya no se ofrecen

  Scenario: Eliminar un producto del menú
    Given un producto existente en el menú
    When lo elimino
    Then deja de aparecer en el menú

  Scenario: Eliminar un producto con pedidos históricos
    Given un producto que ya aparece en pedidos históricos
    When lo elimino del menú
    Then los registros históricos que lo contienen no se ven afectados
```
