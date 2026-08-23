# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
Taller 2 - Modelo de Información y Diagrama de Contexto

## 👥 Integrantes del equipo
- Mao Suárez
- Nicolas Clavijo

## 🧠 Descripción general del trabajo

El objetivo del taller era modelar las entidades principales del dominio del cliente real (distinto del dominio de la Clínica Salud Viva usado en clase) mediante un modelo entidad-relación (ERD) y un diagrama de contexto de negocio. Para Asul Tecnologías de la Información SAS se construyó un modelo **unificado** que cubre a la vez el dominio de datos del proceso de Desarrollo y Soporte y el del proceso de escalamiento de tickets (ambos modelados en BPMN en el Taller 1), porque en la práctica ambos procesos comparten las mismas entidades de fondo: un ticket de soporte termina gestionándose como una tarea de desarrollo más dentro de Azure DevOps.

## 🔧 Proceso de desarrollo

Se aplicó la metodología de 4 pasos de la guía tanto para el ERD como para el diagrama de contexto:

**ERD:** se identificaron 10 entidades a partir de los procesos BPMN del Taller 1 (Cliente, Proyecto, Requerimiento, Sprint, TareaDesarrollo, Colaborador, TicketSoporte, Despliegue, CasoPrueba, Bug), se les asignaron atributos con su clave primaria, se trazaron las relaciones con verbo ("solicita", "incluye", "planifica", "se gestiona como", etc.) y se asignó cardinalidad en ambos extremos de cada relación.

**Diagrama de contexto:** se trazó el límite organizacional de Asul, ubicando dentro los tres sistemas propios (Azure DevOps, repositorio de código en GitHub, repositorio documental en SharePoint) y dejando fuera al Cliente (actor), a Jira del cliente y al ambiente de producción (ambos como sistemas externos, con borde punteado). Se etiquetó cada flujo con la información que transporta, marcando en rojo punteado los dos flujos que hoy son manuales (Jira ↔ Azure DevOps) para visualizar el problema de integración que reportó la cliente.

Ambos diagramas se construyeron en draw.io reutilizando la notación y paleta de color del caso base de clase (`modelo-er-borrador.drawio` y `contexto-borrador.drawio`): rectángulo para entidad, óvalo para atributo (óvalo oscuro para la PK), rombo para relación; óvalo para actor externo, rectángulo punteado para sistema externo y rectángulo sólido para sistema interno.

## 🧩 Análisis del modelo propuesto

El ERD se organiza en dos filas de cinco entidades para mantenerlo legible pese a cubrir dos procesos; la mayoría de relaciones conectan entidades adyacentes en la cuadrícula, y solo dos relaciones ("corrige" y "se gestiona como") cruzan en diagonal porque conectan procesos distintos del negocio (calidad y soporte) con el mismo punto de ejecución: la tarea de desarrollo. No se encontraron relaciones N:N sin resolver — todas quedaron como 1:N o N:1 sobre una entidad intermedia (por ejemplo, `TicketSoporte` se conecta a `TareaDesarrollo`, no directamente a `Colaborador`).

El diagrama de contexto representa fielmente el dolor principal identificado por la cliente: la frontera entre Jira (del cliente) y Azure DevOps (de Asul) no tiene integración automática por licencias no disponibles, así que cada actualización de estado se transcribe manualmente en ambos sentidos — de ahí que ambos flujos estén resaltados visualmente distintos del resto.

**Supuestos tomados:** se asumió que el ambiente de producción está alojado por el cliente (consistente con lo descrito por la cliente sobre "publicación en ambiente de cliente"), por lo que se modeló como sistema externo y no interno; y que un ticket de soporte, una vez transcrito a Azure DevOps, se gestiona con el mismo tipo de entidad que una tarea de desarrollo regular (de ahí la relación `TicketSoporte —se gestiona como— TareaDesarrollo`).

## 📈 Diagrama final entregado

Ver [`entrega/modelo-final-er.drawio`](modelo-final-er.drawio) y [`entrega/diagrama-contexto-final.drawio`](diagrama-contexto-final.drawio).

## 📋 Tabla de actores, entidades o componentes

| Nombre del elemento | Tipo | Descripción | Responsable |
|---------------------|------|-------------|-------------|
| Cliente | Entidad / Actor | Empresa que contrata desarrollo y soporte a Asul | Cliente de Asul |
| TicketSoporte | Entidad | Caso de soporte reportado por el cliente, con id de Jira e id de Azure DevOps | Equipo de Soporte de Asul |
| TareaDesarrollo | Entidad | Unidad de trabajo de desarrollo, con horas estimadas y reales | Equipo de Desarrollo de Asul |
| Azure DevOps | Sistema interno | Gestiona épicas, historias, sprints, test plans y tickets | Asul |
| Jira (Cliente) | Sistema externo | Sistema del cliente donde se reportan los tickets de soporte | Cliente de Asul |

## 🔍 Investigación complementaria

### Tema investigado:
Patrones de integración (o falta de integración) entre sistemas de tickets de distintas organizaciones — el caso Jira–Azure DevOps.

### Resumen:
La documentación oficial de Atlassian sobre integraciones de Jira describe el patrón que Asul no puede usar hoy: un conector bidireccional que sincroniza automáticamente el estado de un issue entre dos instancias de tracking de trabajo distintas, típicamente ofrecido como add-on de pago (Jira Cloud) o mediante Azure DevOps Marketplace ("Jira integration extension"). Sin ese conector, la alternativa documentada en la literatura de gestión de servicios de TI (ITSM) es exactamente la que aplica Asul de forma manual: un proceso humano de "traducción" campo a campo entre dos sistemas de registro, que la propia guía de ITIL 4 identifica como una fuente común de errores y demoras en el ciclo de vida de un incidente, al depender de que una persona no olvide replicar cada actualización de estado en ambos sistemas.

Esto valida por qué el diagrama de contexto resalta ese flujo de forma distinta al resto: no es una decisión estética, sino la representación de un riesgo operativo real y documentado en la industria, que además es coherente con el Problema #2 de la Ficha de Caracterización del Taller 0 ("duplicidad de trabajo en el soporte").

## 📚 Referencias
- [1] Chen, P. *The Entity-Relationship Model — Toward a Unified View of Data*. ACM Transactions on Database Systems, 1976.
- [2] Atlassian. *Jira and Azure DevOps integrations*. https://www.atlassian.com/software/jira/integrations
- [3] AXELOS. *ITIL 4 Foundation — Incident Management practice*. 2019.
- [4] Fuente asistida por IA: Claude (Anthropic), agosto 2026 — apoyo en la construcción del ERD y el diagrama de contexto en draw.io y en la redacción de este informe.

---

_Este documento hace parte de la entrega del Taller 2 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
