# Sistema de Pedidos para Restaurante

Proyecto académico de **Diseño de Sistemas de Información** desarrollado para centralizar y digitalizar el proceso de pedidos de un restaurante, desde la consulta del menú hasta la entrega del pedido.

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

El proyecto plantea el diseño de un sistema de pedidos para restaurante que permita centralizar la gestión de clientes, productos, pedidos y entregas.

Actualmente, muchos restaurantes gestionan sus pedidos mediante llamadas, mensajes de WhatsApp o notas físicas, lo que puede generar errores en el registro de productos, cantidades, direcciones y estados de entrega. La solución propuesta busca organizar este proceso mediante una plataforma con funciones específicas para clientes, administradores y domiciliarios.

## Objetivo

Diseñar una solución que permita a los clientes consultar el menú, gestionar pedidos, registrar direcciones y realizar seguimiento del estado de la entrega; al administrador gestionar el menú y asignar domiciliarios; y al domiciliario consultar y actualizar las entregas asignadas.

## Roles del sistema

### Cliente

El cliente puede:

- Registrarse e iniciar sesión.
- Consultar el menú.
- Agregar y modificar productos en un pedido.
- Consultar el resumen del pedido.
- Cancelar pedidos cuando corresponda.
- Registrar direcciones de entrega.
- Consultar el estado del pedido.
- Consultar el estado de la entrega.
- Consultar el historial de pedidos.
- Actualizar sus datos personales.
- Recuperar su contraseña.

### Administrador

El administrador puede:

- Agregar productos al menú.
- Modificar precios.
- Cambiar la disponibilidad de los productos.
- Eliminar productos sin afectar los pedidos históricos.
- Consultar pedidos recibidos.
- Asignar domiciliarios a los pedidos.

### Domiciliario

El domiciliario puede:

- Consultar los pedidos que tiene asignados.
- Consultar la información necesaria del pedido.
- Consultar los datos básicos del cliente.
- Actualizar el estado de la entrega.
- Consultar su información de perfil.

## Alcance funcional

El sistema contempla los siguientes módulos principales:

- Gestión de pedidos.
- Gestión de clientes.
- Gestión de entregas.
- Gestión del menú.
- Gestión de direcciones.
- Historial de pedidos.
- Administración de productos.
- Asignación de domiciliarios.
- Seguimiento del pedido y de la entrega.

## Documentación del proyecto

| Documento | Descripción |
|---|---|
| [01 - Modelo verbal](docs/01-modelo-verbal.md) | Problema, causas, impacto, flujo actual y propuesta de solución. |
| [02 - Requisitos](docs/02-requisitos.md) | Requisitos de usuario, funcionales y no funcionales. |
| [03 - Reglas de negocio](docs/03-reglas-negocio.md) | Restricciones y condiciones que gobiernan el sistema. |
| [04 - Matriz de trazabilidad](docs/04-trazabilidad.md) | Relación entre requisitos de usuario, requisitos funcionales, requisitos no funcionales y reglas de negocio. |
| [05 - Story Mapping](docs/05-story-mapping.md) | Organización funcional del producto por actividades y épicas. |
| [06 - Historias de usuario](docs/06-historias-de-usuario.md) | Historias de usuario y criterios de aceptación. |
| [07 - Diagrama de clases](docs/07-diagrama-de-clases.md) | Representación conceptual de las principales clases del sistema. |
| [08 - Prototipo Figma](docs/08-prototipo-figma.md) | Descripción de las pantallas de alta fidelidad para cliente, administrador y domiciliario. |
| [Documento original](docs/entregable-1-original.pdf) | Entregable académico completo utilizado como fuente del repositorio. |

## Evidencias visuales

Las principales figuras del entregable se encuentran en la carpeta [`docs/assets`](docs/assets).

Incluyen:

- Story Mapping.
- Diagrama de clases.
- Pantallas del prototipo Figma para cliente.
- Pantallas del prototipo Figma para administrador.
- Pantallas del prototipo Figma para domiciliario.

## Equipo de trabajo

| Integrante | Identificación académica |
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
    ├── entregable-1-original.pdf
    └── assets/
        ├── README.md
        ├── story-mapping.png
        ├── diagrama-clases.png
        ├── figma-cliente-1.png
        ├── figma-cliente-2.png
        ├── figma-cliente-3.png
        ├── figma-administrador.png
        └── figma-domiciliario.png
```

## Estado del proyecto

**Fase actual:** análisis y diseño.

El repositorio contiene la documentación correspondiente al primer entregable del proyecto: modelo verbal, requisitos, reglas de negocio, matriz de trazabilidad, Story Mapping, historias de usuario, diagrama de clases y prototipo de alta fidelidad.

En una fase posterior podrán incorporarse el código fuente, las pruebas, la documentación técnica de implementación y los manuales de usuario.
