# Documento de Análisis y Selección de Patrones GoF

## Sistema de Pedidos para Restaurante — Sabor Express

---

## 1. Datos generales del proyecto

| Campo | Detalle |
|---|---|
| **Nombre del proyecto** | Sistema de Pedidos para Restaurante "Sabor Express" |
| **Equipo de trabajo** | Verónica Osorio Durango, Juan David Gonzalez Vasco, Jhonatan Moreno Palacios, Andrea Vargas Loaiza |
| **Asignatura** | Diseño de Sistemas de Información - Grupo 7 |
| **Docente** | Alexandra Guerrero Bocanegra |
| **Fecha** | Octubre de 2026 |

---

## 2. Descripción general del proyecto

**Problema que resuelve el proyecto:** Muchos restaurantes gestionan sus pedidos manualmente o mediante canales dispersos (llamadas, WhatsApp, notas en papel). Esto causa errores en cantidades y direcciones, falta de visibilidad en tiempo real del estado del pedido, asignación informal de domiciliarios y ausencia de un histórico digital.

**Objetivo general del sistema:** Centralizar la gestión de pedidos, menú y entregas a través de una plataforma digital que reduzca errores operativos, mejore la coordinación entre administrador, domiciliario y cliente, y brinde seguimiento en tiempo real.

**Descripción del producto propuesto:** Sistema compuesto por una aplicación móvil (frontend) para tres actores (Cliente, Administrador, Domiciliario), una API Backend con las reglas de negocio y una base de datos relacional para la persistencia del catálogo y los pedidos (modelo C4, niveles 1 a 3).

> **Nota sobre tecnologías:** el entregable deja las tecnologías de implementación por confirmar (C2). Por ello, los pseudocódigos de este documento son independientes del framework. Las referencias a mecanismos concretos (por ejemplo, WebSockets u Observables) son supuestos de implementación, no decisiones cerradas.

---

## 3. Alcance del MVP

### 3.1 Funcionalidades incluidas en el MVP

| ID | Funcionalidad del MVP | Prioridad | Justificación |
|---|---|---|---|
| F1 | Autenticación y gestión de sesiones (Cliente, Admin, Domiciliario) | Alta | Garantiza el acceso seguro según el rol (HU-2.1, HU-2.2). Las HU están redactadas para el cliente; el acceso del administrador y del domiciliario se asume con el mismo mecanismo de sesión (método `iniciarSesion` de `Usuario`). |
| F2 | Consulta de menú, alta de productos y gestión de disponibilidad | Alta | Permite al cliente ver la oferta vigente y al admin agregar productos y marcar agotados (HU-4.1, HU-4.2, HU-4.4). |
| F3 | Creación, ajuste y confirmación del pedido, con dirección de entrega | Alta | Recorrido central del cliente para armar su pedido, asociar la dirección y calcular el total (HU-1.1, HU-1.2, HU-1.5, HU-3.1). |
| F4 | Asignación y gestión de entregas | Alta | Coordina la asignación por el admin y la actualización de estados por el domiciliario (HU-3.2, HU-3.4, HU-3.5). |
| F5 | Seguimiento de estado del pedido en tiempo real | Media | Proporciona visibilidad continua al cliente sobre su compra (HU-1.4). |

### 3.2 Restricciones del MVP

- El proyecto debe implementar **5 patrones GoF no repetidos**.
- Cada patrón responde a una necesidad real del sistema basada en las historias de usuario del Release 1 (13 historias Must Have).
- La selección evita la sobreingeniería y se ajusta a la arquitectura planteada.
- Los patrones deben ser viables dentro del tiempo y alcance del MVP.

### 3.3 Funcionalidades fuera del MVP

- Cancelación de pedidos (HU-1.3, Release 2) e historial de pedidos (HU-2.4, Release 3).
- Consulta de estado de la entrega con nombre del domiciliario (HU-3.3, Release 2).
- Pasarela de pagos en línea, promociones, descuentos y cálculo de envío por zona o distancia.
- Seguimiento por mapa GPS en tiempo real e itinerarios optimizados de entrega.
- Carga masiva de productos e inventarios automatizados.

---

## 4. Matriz global de análisis de patrones del proyecto

| Necesidad de diseño detectada | Módulo o funcionalidad afectada | Tipo de problema | Patrón candidato | ¿Aporta valor al MVP? | ¿Se solapa con otro patrón? |
|---|---|---|---|---|---|
| Instanciación heterogénea de usuarios según su rol tras el login | Gestión de Clientes / Autenticación | Creacional | Factory Method | Sí | No |
| Construcción paso a paso de un pedido con múltiples ítems y dirección | Gestión de Pedidos | Creacional | Builder | Sí | No |
| Cambio de comportamiento del pedido según su fase (Pendiente, En preparación, En camino, Entregado) | Gestión de Pedidos y Entregas | Comportamiento | State | Sí | No |
| Notificación de cambios de estado del pedido a la interfaz del cliente | Gestión de Entregas / Seguimiento | Comportamiento | Observer | Sí | No |
| Cálculo del costo de envío del pedido, con tarifa fija en el MVP y otras tarifas posibles después | Gestión de Pedidos | Comportamiento | Strategy | Sí | No |

---

## 5. Fichas de análisis de los patrones seleccionados

---

### 5.1 Ficha de Análisis — Patrón 1: Factory Method

#### 5.1.1 Identificación general

- **Nombre del patrón:** Factory Method
- **Categoría:** [X] Creacional  [ ] Estructural  [ ] De comportamiento
- **Módulo, componente o funcionalidad donde se aplicará:** Módulo de Autenticación y Usuarios (Gestión de Clientes en el C3).

#### 5.1.2 Problema de diseño que se quiere resolver

Al autenticar un usuario en la plataforma, el sistema debe instanciar un tipo de usuario específico (Cliente, Administrador o Domiciliario). Si el servicio de autenticación crea estas clases directamente usando sus constructores concretos, se genera un alto acoplamiento y se vulnera la extensibilidad si en el futuro se agregan nuevos roles (ej. Cajero).

#### 5.1.3 Evidencias del problema en el proyecto

- [X] Alto acoplamiento
- [ ] Multiplicidad de algoritmos
- [ ] Cambio frecuente de comportamiento
- [X] Complejidad en la creación de objetos
- [ ] Necesidad de desacoplar emisor y receptor
- [ ] Necesidad de ampliar funcionalidad sin modificar la clase base
- [ ] Interfaz incompatible
- [ ] Otro: ______

**Explicación contextualizada:** La clase abstracta `Usuario` se extiende en tres roles con atributos y métodos diferentes (Cliente maneja teléfono y direcciones; Domiciliario maneja su disponibilidad). La API Backend debe retornar el tipo correcto de usuario sin acoplar el login a las clases concretas.

#### 5.1.4 Relación con el MVP

- **¿Por qué este problema sí debe resolverse en el MVP y no en una versión futura?:** La autenticación es el punto de entrada de los tres actores definidos en el MVP (HU-2.1, HU-2.2) y el modelo de dominio ya define la jerarquía `Usuario`.
- **¿Qué riesgo habría si no se aplica?:** Código condicional extenso (`switch/case` por tipo de rol) disperso en el backend, dificultando el mantenimiento.
- **¿Es razonable para el MVP?:** [X] Sí  [ ] No
- **Justificación:** Se implementa con una interfaz de fábrica y tres clases pequeñas, alineadas con el modelo de dominio del C4 (nivel 4).

#### 5.1.5 Análisis de adecuación del patrón

- **¿Qué parte del sistema cambia o podría cambiar?:** La creación de instancias de usuario según las credenciales y el rol.
- **¿Qué se desea desacoplar?:** El servicio de autenticación respecto a los constructores concretos de Cliente, Administrador y Domiciliario.
- **¿Qué ventaja concreta ofrece este patrón en este proyecto?:** Permite agregar nuevos tipos de usuario sin modificar la lógica de inicio de sesión. La fábrica a usar se resuelve con un registro rol → fábrica (un mapa), no con un `switch` en el login.
- **Principio de diseño que ayuda a cumplir:** [X] Abierto/Cerrado  [ ] Responsabilidad Única  [ ] Inversión de Dependencias  [ ] Segregación de Interfaces  [ ] Otro: ______
- **Justificación técnica:** Abre el sistema a la extensión de nuevos roles y cierra el código de login a modificaciones.

#### 5.1.6 Alternativas consideradas

| Patrón alternativo | ¿Por qué podría confundirse? | ¿Por qué no es el mejor en este caso? |
|---|---|---|
| Abstract Factory | Ambos crean objetos de una jerarquía. | Agrega complejidad innecesaria (familias de objetos) cuando solo se instancia una jerarquía simple de `Usuario`. |
| Prototype | Se usa para clonar objetos existentes. | No se requiere clonar usuarios, sino instanciarlos a partir de los datos de la cuenta. |

#### 5.1.7 No repetición ni solapamiento

- **¿Este patrón ya fue usado en otra parte del proyecto?:** [ ] Sí  [X] No
- **¿Su propósito funcional se solapa con otro patrón seleccionado?:** [ ] Sí  [X] No
- **Si la respuesta fue sí, explique por qué ambos siguen siendo necesarios:** No aplica.

#### 5.1.8 Aplicación concreta al diseño

| Rol del patrón | Clase/componente del proyecto |
|---|---|
| Product (clase abstracta) | `Usuario` |
| ConcreteProducts | `Cliente`, `Administrador`, `Domiciliario` |
| Creator | `UsuarioFactory` |
| ConcreteCreators | `ClienteFactory`, `AdminFactory`, `DomiciliarioFactory` |

**Descripción breve de cómo funcionará en el sistema:** Tras validar las credenciales, el servicio de autenticación obtiene el rol del usuario, selecciona la fábrica correspondiente y le pide crear la instancia. El resto del backend trabaja con el tipo abstracto `Usuario`, sin conocer las clases concretas.

```typescript
// Pseudocódigo
abstract class UsuarioFactory {
  abstract crearUsuario(datos: any): Usuario;
}

class ClienteFactory extends UsuarioFactory {
  crearUsuario(datos: any): Usuario {
    return new Cliente(datos.id, datos.nombre, datos.correo, datos.telefono);
  }
}

// Registro rol -> fábrica (evita un switch en el login)
const fabricas: Record<string, UsuarioFactory> = {
  CLIENTE: new ClienteFactory(),
  ADMIN: new AdminFactory(),
  DOMICILIARIO: new DomiciliarioFactory(),
};
```

#### 5.1.9 Conclusión de decisión

- **Decisión final sobre este patrón:** [X] Se aprueba  [ ] Se descarta
- **Justificación final:** Resuelve la instanciación de los tres roles definidos en el modelo de dominio y deja el login cerrado a modificaciones.

---

### 5.2 Ficha de Análisis — Patrón 2: Builder

#### 5.2.1 Identificación general

- **Nombre del patrón:** Builder
- **Categoría:** [X] Creacional  [ ] Estructural  [ ] De comportamiento
- **Módulo, componente o funcionalidad donde se aplicará:** Módulo de Gestión de Pedidos (armado del pedido en la aplicación cliente).

#### 5.2.2 Problema de diseño que se quiere resolver

Un `Pedido` es un objeto complejo que se construye paso a paso (agregar productos, modificar cantidades, asociar la dirección de entrega y mostrar subtotales) antes de ser confirmado (HU-1.1, HU-1.2, HU-1.5, HU-3.1). Usar un constructor con muchos parámetros dificulta la legibilidad y la creación progresiva.

#### 5.2.3 Evidencias del problema en el proyecto

- [ ] Alto acoplamiento
- [ ] Multiplicidad de algoritmos
- [ ] Cambio frecuente de comportamiento
- [X] Complejidad en la creación de objetos
- [ ] Necesidad de desacoplar emisor y receptor
- [ ] Necesidad de ampliar funcionalidad sin modificar la clase base
- [ ] Interfaz incompatible
- [ ] Otro: ______

**Explicación contextualizada:** Un pedido incluye una lista dinámica de `DetallePedido`, una `Dirección` asociada y los datos del cliente. Su armado ocurre por etapas a lo largo de la navegación del usuario (menú → Mi Pedido → dirección → confirmación).

#### 5.2.4 Relación con el MVP

- **¿Por qué este problema sí debe resolverse en el MVP y no en una versión futura?:** El flujo de armado y resumen del pedido antes de confirmar es el corazón operativo del sistema (HU-1.1, HU-1.2, HU-1.5).
- **¿Qué riesgo habría si no se aplica?:** Constructores inconsistentes o mutación desordenada de la entidad `Pedido` en memoria, con pedidos incompletos que lleguen al backend.
- **¿Es razonable para el MVP?:** [X] Sí  [ ] No
- **Justificación:** Es una sola clase de apoyo (`PedidoBuilder`) que ordena la lógica del carrito sin añadir complejidad excesiva.

#### 5.2.5 Análisis de adecuación del patrón

- **¿Qué parte del sistema cambia o podría cambiar?:** La estructura interna del pedido mientras el usuario agrega o retira ítems.
- **¿Qué se desea desacoplar?:** El proceso de construcción del `Pedido` de su representación final.
- **¿Qué ventaja concreta ofrece este patrón en este proyecto?:** Permite armar el pedido paso a paso de forma fluida y validar que esté completo (al menos un producto y una dirección) antes de enviarlo al backend.
- **Principio de diseño que ayuda a cumplir:** [ ] Abierto/Cerrado  [X] Responsabilidad Única  [ ] Inversión de Dependencias  [ ] Segregación de Interfaces  [ ] Otro: ______
- **Justificación técnica:** Separa la lógica de ensamblado del pedido de la lógica de negocio del propio objeto `Pedido`.

#### 5.2.6 Alternativas consideradas

| Patrón alternativo | ¿Por qué podría confundirse? | ¿Por qué no es el mejor en este caso? |
|---|---|---|
| Factory Method | Ambos son patrones creacionales. | Factory crea el objeto de una sola vez; Builder lo construye por pasos interactivos. |
| Prototype | Podría usarse para copiar un pedido anterior. | En el MVP no se clonan pedidos históricos (el historial es Release 3); se arma un pedido nuevo. |

#### 5.2.7 No repetición ni solapamiento

- **¿Este patrón ya fue usado en otra parte del proyecto?:** [ ] Sí  [X] No
- **¿Su propósito funcional se solapa con otro patrón seleccionado?:** [ ] Sí  [X] No
- **Si la respuesta fue sí, explique por qué ambos siguen siendo necesarios:** No aplica.

#### 5.2.8 Aplicación concreta al diseño

| Rol del patrón | Clase/componente del proyecto |
|---|---|
| Builder | `PedidoBuilder` |
| Product | `Pedido` |
| Director | `CarritoService` (capa de la aplicación cliente que maneja "Mi Pedido") |

**Descripción breve de cómo funcionará en el sistema:** Mientras el cliente navega, `CarritoService` usa el `PedidoBuilder` para agregar productos, ajustar cantidades y asociar la dirección. Al confirmar, `build()` valida que el pedido esté completo y lo entrega. El total oficial lo calcula el backend (`CalculadoraPedidoService`, ver patrón 5), tal como establece el C2, donde la API centraliza el cálculo de totales. En pantalla solo se muestra un subtotal provisional.

```typescript
// Pseudocódigo
class PedidoBuilder {
  private pedido: Pedido = new Pedido();

  agregarProducto(producto: Producto, cantidad: number): PedidoBuilder {
    this.pedido.detalles.push(new DetallePedido(producto, cantidad));
    return this;
  }
  setDireccion(direccion: Direccion): PedidoBuilder {
    this.pedido.direccion = direccion;
    return this;
  }
  build(): Pedido {
    if (this.pedido.detalles.length === 0) throw new Error("Agregue al menos un producto");
    if (!this.pedido.direccion) throw new Error("Seleccione una dirección");
    return this.pedido; // el total oficial se calcula en el backend
  }
}
```

#### 5.2.9 Conclusión de decisión

- **Decisión final sobre este patrón:** [X] Se aprueba  [ ] Se descarta
- **Justificación final:** Encaja con el flujo diseñado en los prototipos de Figma (Mi Pedido → Confirmación) y con los criterios de aceptación de HU-1.5.

---

### 5.3 Ficha de Análisis — Patrón 3: State (Estado)

#### 5.3.1 Identificación general

- **Nombre del patrón:** State
- **Categoría:** [ ] Creacional  [ ] Estructural  [X] De comportamiento
- **Módulo, componente o funcionalidad donde se aplicará:** Módulo de Gestión de Pedidos y Entregas.

#### 5.3.2 Problema de diseño que se quiere resolver

El pedido avanza por varios estados (Pendiente, En preparación, En camino, Entregado) y el comportamiento permitido depende del estado actual. Por ejemplo, el domiciliario solo puede marcar "Entregado" si la entrega está "En camino" (HU-3.4), y solo se asigna domiciliario a un pedido confirmado (HU-3.2). Manejar estas restricciones con condicionales `if/else` dispersos vuelve el código frágil.

#### 5.3.3 Evidencias del problema en el proyecto

- [ ] Alto acoplamiento
- [ ] Multiplicidad de algoritmos
- [X] Cambio frecuente de comportamiento
- [ ] Complejidad en la creación de objetos
- [ ] Necesidad de desacoplar emisor y receptor
- [ ] Necesidad de ampliar funcionalidad sin modificar la clase base
- [ ] Interfaz incompatible
- [X] Otro: reglas de transición dependientes del estado

**Explicación contextualizada:** Las transiciones de estado alteran las acciones disponibles para el administrador y el domiciliario, y lo que ve el cliente en la línea de tiempo de seguimiento (Pendiente → En preparación → En camino → Entregado).

#### 5.3.4 Relación con el MVP

- **¿Por qué este problema sí debe resolverse en el MVP y no en una versión futura?:** El ciclo de vida del pedido es la columna vertebral de la solución (HU-1.4, HU-3.2, HU-3.4).
- **¿Qué riesgo habría si no se aplica?:** Permitir transiciones inválidas (por ejemplo, marcar "Entregado" un pedido que nunca salió) o llenar los controladores con validaciones repetidas.
- **¿Es razonable para el MVP?:** [X] Sí  [ ] No
- **Justificación:** Son cuatro clases de estado pequeñas. La cancelación (HU-1.3, RN-01) queda para el Release 2, pero el patrón la acomoda sin modificar los estados existentes.

#### 5.3.5 Análisis de adecuación del patrón

- **¿Qué parte del sistema cambia o podría cambiar?:** El comportamiento y las acciones permitidas del pedido a medida que avanza su procesamiento.
- **¿Qué se desea desacoplar?:** La lógica de las reglas de transición respecto a la clase `Pedido` y a los controladores.
- **¿Qué ventaja concreta ofrece este patrón en este proyecto?:** Cada estado encapsula sus transiciones válidas, de modo que una transición no permitida no puede ejecutarse. En el Release 2 bastará agregar `CanceladoState` y el método `cancelar` (RN-01) sin reescribir lo existente.
- **Principio de diseño que ayuda a cumplir:** [X] Abierto/Cerrado  [X] Responsabilidad Única  [ ] Inversión de Dependencias  [ ] Segregación de Interfaces  [ ] Otro: ______
- **Justificación técnica:** Cada estado maneja su propia lógica y se pueden agregar estados nuevos sin modificar los existentes.

#### 5.3.6 Alternativas consideradas

| Patrón alternativo | ¿Por qué podría confundirse? | ¿Por qué no es el mejor en este caso? |
|---|---|---|
| Strategy | Ambos usan clases intercambiables. | Strategy cambia algoritmos de cálculo; State cambia el comportamiento según el estado interno del objeto. |
| Command | Se usa para encapsular peticiones. | No gestiona el ciclo de vida ni las transiciones de estado de una entidad. |

#### 5.3.7 No repetición ni solapamiento

- **¿Este patrón ya fue usado en otra parte del proyecto?:** [ ] Sí  [X] No
- **¿Su propósito funcional se solapa con otro patrón seleccionado?:** [ ] Sí  [X] No
- **Si la respuesta fue sí, explique por qué ambos siguen siendo necesarios:** No aplica.

#### 5.3.8 Aplicación concreta al diseño

| Rol del patrón | Clase/componente del proyecto |
|---|---|
| Context | `Pedido` / `Entrega` |
| State (interfaz) | `EstadoPedidoState` |
| ConcreteStates (MVP) | `PendienteState`, `EnPreparacionState`, `EnCaminoState`, `EntregadoState` |
| ConcreteState (Release 2) | `CanceladoState` (junto con `cancelar`, RN-01) |

**Descripción breve de cómo funcionará en el sistema:** El `Pedido` guarda su estado actual y delega en él cada acción. Cuando el administrador o el domiciliario ejecuta una acción, el estado actual decide si es válida y, si lo es, deja el pedido en el siguiente estado. Si la acción no es válida, el estado la rechaza con un error.

```typescript
// Pseudocódigo
interface EstadoPedidoState {
  avanzar(pedido: Pedido): void;
  // Release 2: cancelar(pedido: Pedido): void;  (RN-01, solo desde Pendiente)
}

class PendienteState implements EstadoPedidoState {
  avanzar(pedido: Pedido) { pedido.setEstado(new EnPreparacionState()); }
}
class EnPreparacionState implements EstadoPedidoState {
  avanzar(pedido: Pedido) { pedido.setEstado(new EnCaminoState()); }
}
class EnCaminoState implements EstadoPedidoState {
  avanzar(pedido: Pedido) { pedido.setEstado(new EntregadoState()); }
}
class EntregadoState implements EstadoPedidoState {
  avanzar(pedido: Pedido) { throw new Error("El pedido ya fue entregado"); }
}
```

#### 5.3.9 Conclusión de decisión

- **Decisión final sobre este patrón:** [X] Se aprueba  [ ] Se descarta
- **Justificación final:** Garantiza la integridad del flujo de entregas (HU-3.4) y deja preparada la regla RN-01 para el Release 2.

---

### 5.4 Ficha de Análisis — Patrón 4: Observer (Observador)

#### 5.4.1 Identificación general

- **Nombre del patrón:** Observer
- **Categoría:** [ ] Creacional  [ ] Estructural  [X] De comportamiento
- **Módulo, componente o funcionalidad donde se aplicará:** Módulo de Gestión de Entregas y Seguimiento del Pedido.

#### 5.4.2 Problema de diseño que se quiere resolver

El requisito no funcional RNF-05 exige que los cambios de estado del pedido y de la entrega se reflejen en la interfaz del cliente en un máximo de 5 segundos (HU-3.4 extiende esta expectativa al administrador). La interfaz debe enterarse de los cambios ocurridos en el backend sin consultas constantes y sin que la lógica de entrega dependa de las pantallas.

#### 5.4.3 Evidencias del problema en el proyecto

- [ ] Alto acoplamiento
- [ ] Multiplicidad de algoritmos
- [ ] Cambio frecuente de comportamiento
- [ ] Complejidad en la creación de objetos
- [X] Necesidad de desacoplar emisor y receptor
- [ ] Necesidad de ampliar funcionalidad sin modificar la clase base
- [ ] Interfaz incompatible
- [ ] Otro: ______

**Explicación contextualizada:** Cuando el domiciliario marca la entrega "En camino" (HU-3.4), la vista de seguimiento del cliente (HU-1.4) y el panel del administrador deben enterarse automáticamente.

#### 5.4.4 Relación con el MVP

- **¿Por qué este problema sí debe resolverse en el MVP y no en una versión futura?:** Responde directamente a RNF-05 y al criterio de HU-1.4 que pide ver el nuevo estado en máximo 5 segundos.
- **¿Qué riesgo habría si no se aplica?:** Depender del refresco manual o del sondeo periódico dificultaría cumplir el límite de 5 segundos de RNF-05 y cargaría innecesariamente el servidor.
- **¿Es razonable para el MVP?:** [X] Sí  [ ] No
- **Justificación:** Basta un sujeto que publique el cambio de estado y suscriptores en las pantallas de seguimiento. Como supuesto de implementación, se puede apoyar en un canal de eventos (por ejemplo, WebSockets) y en Observables en el cliente; esto queda por confirmar en el C2.

#### 5.4.5 Análisis de adecuación del patrón

- **¿Qué parte del sistema cambia o podría cambiar?:** El estado del pedido y de la entrega en el servidor.
- **¿Qué se desea desacoplar?:** El componente que modifica el estado (domiciliario/administrador) de los componentes que lo muestran (pantalla del cliente y panel del administrador).
- **¿Qué ventaja concreta ofrece este patrón en este proyecto?:** Notificación reactiva inmediata a los suscritos, y posibilidad de agregar nuevos observadores sin tocar la lógica de entrega.
- **Principio de diseño que ayuda a cumplir:** [X] Abierto/Cerrado  [ ] Responsabilidad Única  [X] Inversión de Dependencias  [ ] Segregación de Interfaces  [ ] Otro: ______
- **Justificación técnica:** El sujeto notifica a una abstracción (observador) sin conocer las pantallas concretas que escuchan.

#### 5.4.6 Alternativas consideradas

| Patrón alternativo | ¿Por qué podría confundirse? | ¿Por qué no es el mejor en este caso? |
|---|---|---|
| Mediator | Centraliza la comunicación entre objetos. | Mediator coordina interacciones entre muchos colegas; Observer propaga eventos de un sujeto a sus suscriptores. |
| Polling (sondeo) | También permite enterarse de cambios. | No es un patrón GoF sino una técnica de consulta; es ineficiente y no garantiza reflejar el cambio dentro de los 5 segundos de RNF-05. |

#### 5.4.7 No repetición ni solapamiento

- **¿Este patrón ya fue usado en otra parte del proyecto?:** [ ] Sí  [X] No
- **¿Su propósito funcional se solapa con otro patrón seleccionado?:** [ ] Sí  [X] No
- **Si la respuesta fue sí, explique por qué ambos siguen siendo necesarios:** No aplica (State define las transiciones; Observer solo difunde el cambio ya ocurrido).

#### 5.4.8 Aplicación concreta al diseño

| Rol del patrón | Clase/componente del proyecto |
|---|---|
| Subject (Sujeto) | `PedidoEstadoObservable` (servicio de eventos de estado) |
| Observer (interfaz) | `PedidoObserver` |
| ConcreteObservers | `SeguimientoPedidoView` (cliente), `PanelPedidosView` (administrador) |

**Descripción breve de cómo funcionará en el sistema:** Cuando un cambio de estado se confirma en el backend, el sujeto publica el evento. Las vistas de seguimiento del cliente y del administrador, suscritas al pedido, reciben el nuevo estado y actualizan su línea de tiempo sin necesidad de refrescar.

```typescript
// Pseudocódigo (independiente del framework)
interface PedidoObserver {
  actualizar(pedidoId: string, nuevoEstado: string): void;
}

class PedidoEstadoObservable {
  private observadores: PedidoObserver[] = [];
  suscribir(o: PedidoObserver) { this.observadores.push(o); }
  notificar(pedidoId: string, nuevoEstado: string) {
    this.observadores.forEach(o => o.actualizar(pedidoId, nuevoEstado));
  }
}

class SeguimientoPedidoView implements PedidoObserver {
  actualizar(pedidoId: string, nuevoEstado: string) {
    this.actualizarLineaDeTiempo(nuevoEstado);
  }
}
```

#### 5.4.9 Conclusión de decisión

- **Decisión final sobre este patrón:** [X] Se aprueba  [ ] Se descarta
- **Justificación final:** Es la forma más directa de cumplir RNF-05 (actualización en menos de 5 s) con bajo acoplamiento (HU-1.4, HU-3.4).

---

### 5.5 Ficha de Análisis — Patrón 5: Strategy (Estrategia)

#### 5.5.1 Identificación general

- **Nombre del patrón:** Strategy
- **Categoría:** [ ] Creacional  [ ] Estructural  [X] De comportamiento
- **Módulo, componente o funcionalidad donde se aplicará:** Módulo de Gestión de Pedidos (cálculo del total y del costo de envío, en la API Backend).

#### 5.5.2 Problema de diseño que se quiere resolver

El total a pagar de un pedido es la suma de los productos más el costo de envío. En el MVP el envío es una tarifa fija (el prototipo de Figma muestra "Costo de envío" en el resumen de pago), pero el restaurante podría cambiar la política de cobro más adelante (por zona, por distancia o gratis sobre un monto mínimo). Si la fórmula se incrusta en `Pedido`, cada cambio comercial obliga a modificar esa clase.

#### 5.5.3 Evidencias del problema en el proyecto

- [ ] Alto acoplamiento
- [X] Multiplicidad de algoritmos
- [ ] Cambio frecuente de comportamiento
- [ ] Complejidad en la creación de objetos
- [ ] Necesidad de desacoplar emisor y receptor
- [ ] Necesidad de ampliar funcionalidad sin modificar la clase base
- [ ] Interfaz incompatible
- [ ] Otro: ______

**Explicación contextualizada:** El sistema debe mostrar el subtotal, el costo de envío y el total en el resumen del pedido (HU-1.5, RF-05). HU-3.1 deja explícitamente fuera el cálculo de envío por zona, lo que confirma que la política de envío cambiará en versiones posteriores.

> **Ajuste de trazabilidad propuesto:** el costo de envío aparece en el prototipo de Figma pero no en el texto de HU-1.5 ni de RF-05. Se propone agregar al criterio de aceptación de HU-1.5 (y a RF-05) que el resumen muestre subtotal, costo de envío y total.

#### 5.5.4 Relación con el MVP

- **¿Por qué este problema sí debe resolverse en el MVP y no en una versión futura?:** El resumen del pedido (HU-1.5) ya debe mostrar el desglose con costo de envío; definir desde el inicio el punto de extensión evita reescribir el cálculo cuando cambie la tarifa.
- **¿Qué riesgo habría si no se aplica?:** Fórmulas de cobro rígidas acopladas a la entidad `Pedido`.
- **¿Es razonable para el MVP?:** [X] Sí  [ ] No
- **Justificación:** En el MVP solo se implementa una estrategia concreta (tarifa fija) y una interfaz de una sola operación. No se implementan promociones, descuentos ni envío por distancia, que están fuera del alcance. El costo adicional es mínimo y deja preparado el cambio.

#### 5.5.5 Análisis de adecuación del patrón

- **¿Qué parte del sistema cambia o podría cambiar?:** El algoritmo para calcular el costo de envío.
- **¿Qué se desea desacoplar?:** La clase `Pedido` y el servicio de cálculo respecto a la fórmula específica del envío.
- **¿Qué ventaja concreta ofrece este patrón en este proyecto?:** Permite cambiar la política de envío sin modificar `Pedido` ni el flujo de confirmación.
- **Principio de diseño que ayuda a cumplir:** [X] Abierto/Cerrado  [X] Responsabilidad Única  [ ] Inversión de Dependencias  [ ] Segregación de Interfaces  [ ] Otro: ______
- **Justificación técnica:** Separa el cálculo tarifario en clases de estrategia independientes.

#### 5.5.6 Alternativas consideradas

| Patrón alternativo | ¿Por qué podría confundirse? | ¿Por qué no es el mejor en este caso? |
|---|---|---|
| State | Ambos encapsulan comportamiento en clases separadas. | State cambia según el ciclo de vida del pedido; Strategy intercambia la fórmula de cálculo. |
| Template Method | Define el esqueleto de un algoritmo mediante herencia. | Usa herencia rígida en lugar de composición, lo que hace menos flexible el intercambio de tarifas. |
| Constante o parámetro de configuración | Con una sola tarifa fija bastaría un valor configurable. | Es válida si la tarifa nunca cambiará su forma de cálculo; se descarta porque HU-3.1 ya anticipa envío por zona en una versión futura. |

#### 5.5.7 No repetición ni solapamiento

- **¿Este patrón ya fue usado en otra parte del proyecto?:** [ ] Sí  [X] No
- **¿Su propósito funcional se solapa con otro patrón seleccionado?:** [ ] Sí  [X] No
- **Si la respuesta fue sí, explique por qué ambos siguen siendo necesarios:** No aplica.

#### 5.5.8 Aplicación concreta al diseño

| Rol del patrón | Clase/componente del proyecto |
|---|---|
| Context | `CalculadoraPedidoService` (API Backend; usa `Pedido`) |
| Strategy (interfaz) | `CalculoEnvioStrategy` |
| ConcreteStrategy (MVP) | `EnvioTarifaFijaStrategy` |
| ConcreteStrategies (versiones futuras, no implementadas) | `EnvioPorZonaStrategy`, `EnvioGratisStrategy` |

**Descripción breve de cómo funcionará en el sistema:** Al confirmar el pedido, el backend suma los detalles para obtener el subtotal, le pide a la estrategia de envío configurada el costo de envío y devuelve el total. Cambiar la política de envío consiste en configurar otra estrategia.

```typescript
// Pseudocódigo
interface CalculoEnvioStrategy {
  calcularCosto(subtotal: number): number;
}

class EnvioTarifaFijaStrategy implements CalculoEnvioStrategy {
  calcularCosto(subtotal: number): number {
    return 25.00; // Tarifa base fija del MVP (valor del prototipo)
  }
}

class CalculadoraPedidoService {
  constructor(private envio: CalculoEnvioStrategy) {}
  total(subtotal: number): number {
    return subtotal + this.envio.calcularCosto(subtotal);
  }
}
```

#### 5.5.9 Conclusión de decisión

- **Decisión final sobre este patrón:** [X] Se aprueba  [ ] Se descarta
- **Justificación final:** Mantiene limpio el cálculo del resumen (HU-1.5) y deja un punto de extensión mínimo para las tarifas de envío futuras, sin implementar funcionalidad fuera del MVP.

---

## 6. Consolidado final de los patrones seleccionados

| N° | Patrón GoF | Categoría | Módulo/Funcionalidad | Problema que resuelve | Justificación en el MVP | ¿Se repite? |
|---|---|---|---|---|---|---|
| 1 | Factory Method | Creacional | Autenticación / Usuarios | Instanciación acoplada de roles | Permite crear Cliente, Admin y Domiciliario tras el login sin `switch/case` rígidos (HU-2.1, HU-2.2). | No |
| 2 | Builder | Creacional | Gestión de Pedidos | Construcción compleja e interactiva del pedido | Facilita la adición progresiva de productos y dirección antes de confirmar (HU-1.1, HU-1.2, HU-1.5, HU-3.1). | No |
| 3 | State | Comportamiento | Ciclo de vida del Pedido y la Entrega | Transiciones de estado frágiles con condicionales | Controla el flujo pendiente → entregado y las acciones permitidas en cada estado (HU-1.4, HU-3.2, HU-3.4). | No |
| 4 | Observer | Comportamiento | Seguimiento de Entregas | Refresco manual o sondeo de datos | Cumple RNF-05 (actualización en menos de 5 s) para cliente y administrador (HU-1.4, HU-3.4). | No |
| 5 | Strategy | Comportamiento | Cálculo de total y costo de envío | Fórmula de envío acoplada a `Pedido` | Encapsula la tarifa fija del MVP y deja extensible la política de envío (HU-1.5). | No |

---

## 7. Validación global de consistencia

| Criterio | Sí | No | Observación |
|---|---|---|---|
| Cada patrón responde a un problema real del sistema | [X] | [ ] | Cada patrón está vinculado a historias de usuario, requisitos y reglas del entregable. |
| Los 5 patrones son distintos entre sí | [X] | [ ] | 2 creacionales y 3 de comportamiento, sin repetir ninguno. |
| Ningún patrón fue incluido solo para cumplir con el número | [X] | [ ] | Strategy es el de menor impacto en el MVP (una sola estrategia concreta) y se mantiene por el punto de extensión previsto en HU-3.1; si el equipo lo prefiere, es el candidato a reemplazar. |
| Todos los patrones caben en el alcance del MVP | [X] | [ ] | Todos se apoyan solo en historias del Release 1 (HU-1.1, 1.2, 1.4, 1.5, 2.1, 2.2, 3.1, 3.2, 3.4); la cancelación (Release 2) solo queda prevista. |
| La selección evita sobreingeniería | [X] | [ ] | Se descartaron Abstract Factory y Command, y no se implementan promociones, envío por zona ni cancelación. |
| Existe coherencia entre problema, solución y diseño | [X] | [ ] | Concuerda con el C4 (niveles 2 a 4), con el modelo de dominio y con los prototipos de Figma. |

---

## 8. Conclusión general del análisis

Los 5 patrones GoF seleccionados (Factory Method, Builder, State, Observer y Strategy) son adecuados y coherentes con el alcance del MVP de Sabor Express.

- **Factory Method** y **Builder** dan una capa creacional ordenada para los usuarios por rol y para el armado del pedido.
- **State** protege el flujo del pedido y la entrega, y deja lista la regla RN-01 para cuando se implemente la cancelación en el Release 2.
- **Observer** responde al requisito de actualización en tiempo real por debajo de 5 segundos (RNF-05).
- **Strategy** desacopla el cálculo del costo de envío en el resumen del pedido (HU-1.5), con solo una estrategia concreta en el MVP.

Esta combinación respeta los principios SOLID evaluados en el entregable (SRP, OCP, DIP), evita implementar funcionalidades fuera del MVP y mantiene bajo el riesgo de sobreingeniería. Los mecanismos tecnológicos concretos quedan como supuestos por confirmar en el C2.

---

## 9. Regla de decisión recomendada

1. Un patrón solo debe incluirse si resuelve un problema real del sistema.
2. Debe ser implementable dentro del alcance del MVP.
3. No debe repetir la función de otro patrón ya seleccionado.
4. Debe mejorar el diseño sin añadir complejidad innecesaria.
