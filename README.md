# Sistema de Pedidos para Restaurante

Proyecto académico de la asignatura Diseño de Sistema de Información. La idea es digitalizar el proceso de pedidos de un restaurante: desde que el cliente ve el menú hasta que el pedido le llega a la puerta.

## Tabla de contenido

1. [Descripción general](#descripción-general)
2. [Objetivo](#objetivo)
3. [Roles del sistema](#roles-del-sistema)
4. [Alcance funcional](#alcance-funcional)
5. [Documentación del proyecto](#documentación-del-proyecto)
6. [Evidencias visuales](#evidencias-visuales)
7. [Equipo de trabajo](#equipo-de-trabajo)
8. [Estructura del repositorio](#estructura-del-repositorio)
9. [Estado del proyecto](#estado-del-proyecto)

## Descripción general

Hoy en día muchos restaurantes siguen manejando sus pedidos por teléfono, WhatsApp o notas de papel. Funciona, pero es fácil que se pierda un pedido, se anote mal una dirección o el cliente se quede sin saber en qué va su comida. Este proyecto busca resolver eso con una plataforma centralizada, pensada para tres tipos de usuario: cliente, administrador y domiciliario.

## Objetivo

Que el cliente pueda ver el menú, armar su pedido, guardar sus direcciones y hacer seguimiento a la entrega sin depender de una llamada. Que el administrador maneje el menú y reparta los pedidos entre los domiciliarios. Y que el domiciliario tenga claro qué le toca entregar y a quién.

## Roles del sistema

### Cliente

- Registrarse e iniciar sesión.
- Consultar el menú.
- Agregar y modificar productos en un pedido.
- Ver el resumen del pedido antes de confirmar.
- Cancelar pedidos cuando corresponda.
- Registrar direcciones de entrega.
- Consultar el estado del pedido y de la entrega.
- Consultar el historial de pedidos.
- Actualizar sus datos personales.
- Recuperar su contraseña.

### Administrador

- Agregar productos al menú.
- Modificar precios.
- Cambiar la disponibilidad de los productos.
- Eliminar productos sin afectar los pedidos históricos.
- Consultar los pedidos recibidos.
- Asignar domiciliarios a los pedidos.

### Domiciliario

- Consultar los pedidos que tiene asignados.
- Ver la información necesaria del pedido.
- Ver los datos básicos del cliente.
- Actualizar el estado de la entrega.
- Consultar su perfil.

## Alcance funcional

Los módulos principales del sistema son:

- Gestión de pedidos
- Gestión de clientes
- Gestión de entregas
- Gestión del menú
- Gestión de direcciones
- Historial de pedidos
- Administración de productos
- Asignación de domiciliarios
- Seguimiento del pedido y de la entrega

## Documentación del proyecto

| Documento | Descripción |
|---|---|
| [01 - Modelo verbal](docs/01-modelo-verbal.md) | Problema, causas, impacto, flujo actual y propuesta de solución. |
| [02 - Requisitos](docs/02-requisitos.md) | Requisitos de usuario, funcionales y no funcionales. |
| [03 - Reglas de negocio](docs/03-reglas-negocio.md) | Restricciones y condiciones que gobiernan el sistema. |
| [04 - Matriz de trazabilidad](docs/04-trazabilidad.md) | Relación entre requisitos de usuario, requisitos funcionales, requisitos no funcionales y reglas de negocio. |
| [05 - Story Mapping](docs/05-story-mapping.md) | Organización funcional del producto por actividades y épicas. |
| [06 - Historias de usuario](docs/06-historias-de-usuario.md) | Historias de usuario y criterios de aceptación. |
| [07 - Diagrama de clases](docs/07-diagrama-de-clases.md) | Diagrama de clases en Mermaid, se ve directo en GitHub. |
| [08 - Prototipo Figma](docs/08-prototipo-figma.md) | Pantallas de alta fidelidad para cliente, administrador y domiciliario. |

## Evidencias visuales

Las imágenes del entregable están en [`docs/assets`](docs/assets): el Story Mapping y las capturas del prototipo en Figma para cada rol (cliente, administrador y domiciliario). El diagrama de clases no está como imagen porque quedó embebido en Mermaid dentro de su propio documento.

## Equipo de trabajo

| Integrante | Identificación |
|---|---|
| Verónica Osorio Durango | 21258235 |
| Juan David Gonzalez Vasco | 24158446 |
| Jhonatan Moreno Palacios | 23158374 |
| Andrea Vargas Loaiza | 24158960 |

**Docente:** Alexandra Guerrero Bocanegra
**Asignatura:** Diseño de Sistema de Información
**Institución:** Instituto Tecnológico Metropolitano - ITM
**Ciudad:** Medellín, Colombia
**Año:** 2026

## Estructura del repositorio

```text
sistema-pedidos-restaurante/
├── README.md
├── CONTRIBUTING.md
├── .gitignore
└── docs/
    ├── 01-modelo-verbal.md
    ├── 02-requisitos.md
    ├── 03-reglas-negocio.md
    ├── 04-trazabilidad.md
    ├── 05-story-mapping.md
    ├── 06-historias-de-usuario.md
    ├── 07-diagrama-de-clases.md
    ├── 08-prototipo-figma.md
    └── assets/
        ├── README.md
        ├── Story_Mapping_Restaurante.png
        ├── figma-cliente-1.png
        ├── figma-cliente-2.png
        ├── figma-cliente-3.png
        ├── figma-administrador.png
        └── figma-domiciliario.png
```

## Estado del proyecto

**Fase actual:** análisis y diseño.

Por ahora el repo tiene la documentación del primer entregable: modelo verbal, requisitos, reglas de negocio, matriz de trazabilidad, story mapping, historias de usuario, diagrama de clases y prototipo de alta fidelidad. Más adelante entraría el código, las pruebas y los manuales de usuario.
