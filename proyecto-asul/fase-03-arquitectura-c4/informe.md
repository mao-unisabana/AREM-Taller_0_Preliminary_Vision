# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
Taller 3 - Arquitectura Actual del Sistema (Modelo C4)

## 👥 Integrantes del equipo
- Mao Suárez
- Nicolas Clavijo

## 🧠 Descripción general del trabajo

El objetivo del taller era representar la arquitectura actual del sistema del cliente real con el Modelo C4, en sus dos primeras vistas: Contexto (C1) y Contenedores (C2). A diferencia del Taller 2 (que modeló el *dato* que fluye entre Asul y sus clientes), este taller modela el *ecosistema de herramientas* con el que Asul gestiona sus proyectos de desarrollo y soporte — el mismo ecosistema mencionado en la Ficha de Caracterización (Azure DevOps, GitHub, SharePoin) — pero ahora detallando quién accede a cada pieza y cómo se conectan entre sí, información levantada directamente con Alejandro (líder técnico) en una segunda reunión de entrevista centrada en talleres 3 a 6.

## 🔧 Proceso de desarrollo

Se aplicó la metodología de 4 pasos de la guía tanto para C1 como para C2:

**C1 — Contexto:** se identificaron las personas que acceden directamente al ecosistema (Alejandro y el Analista de Sistemas por el lado de soporte; el resto del equipo de desarrollo por el lado de proyectos) y el actor externo Cliente (Rentec), se ubicó el sistema en alcance (el ecosistema de gestión de Asul: Azure DevOps + GitHub + SharePoint) frente a sus tres sistemas externos (Jira del cliente, correo corporativo y el ambiente de producción), y se trazaron y etiquetaron las relaciones entre ellos.

**C2 — Contenedores:** se descompuso el sistema en alcance en sus contenedores reales — Azure Boards (épicas, historias, sprints, test plans y tickets), el repositorio de código en GitHub (separado de Azure DevOps, contrario a lo asumido inicialmente en el Taller 2), el repositorio documental en SharePoint, y el add-on Time Tracker para el registro de horas — se ubicó Microsoft Entra ID / Directorio Activo como infraestructura de soporte (autenticación), y se marcó explícitamente Azure Pipelines como un contenedor **instalado pero no utilizado**, ya que la entrevista confirmó que la compilación y el despliegue a producción son 100% manuales.

Ambos diagramas se construyeron en draw.io reutilizando la notación y paleta de color oficial del modelo C4 de la guía de clase (`c1-contexto-borrador.drawio` y `c2-contenedores-borrador.drawio`): óvalo azul para persona, rectángulo azul oscuro para el sistema en alcance, rectángulo gris de borde grueso para sistema externo, rectángulo azul claro para contenedor y cilindro gris para infraestructura de soporte. Se mantuvo además la convención ya establecida en la Fase 02 de este mismo proyecto de resaltar en rojo punteado los flujos manuales — en este caso, la sincronización Jira ↔ Azure Boards — para que la arquitectura completa del cliente se lea de forma consistente entre fases.

## 🧩 Análisis del modelo propuesto

El hallazgo más relevante de C1 es que la cadena real de escalamiento de un ticket tiene tres eslabones, no dos: el cliente final del asegurado radica una solicitud, un primer nivel de soporte del lado del cliente la analiza y decide si corresponde a Rentec, y solo entonces se crea o menciona el ticket que activa la notificación a Alejandro. Esto matiza el Problema #2 de la Ficha de Caracterización ("duplicidad de trabajo en el soporte"): la duplicidad no ocurre solo en el borde Jira–Azure DevOps, sino que ya viene precedida por un paso de triage fuera del control de Asul.

En C2, el hallazgo más relevante es que el repositorio de código vive en **GitHub**, separado de Azure DevOps — Azure DevOps se usa para boards, queries y (parcialmente) pipelines, pero no como Azure Repos. Esto es una corrección respecto al supuesto tomado en la Fase 02, donde se había modelado "Repositorio de Código (GitHub / Git)" como una posibilidad entre paréntesis; aquí se confirma que es GitHub y no Azure Repos. También se identificó que Azure Pipelines está instalado pero no se usa: la compilación y el despliegue a producción los hace manualmente Alejandro (con apoyo ocasional de Leonardo o Ronald), lo que representa tanto una oportunidad de automatización como un riesgo de punto único de falla (ver Fase 05 — Seguridad).

**Supuestos tomados:**
- Se asumió que "el ecosistema de gestión de Asul" es el sistema en alcance correcto para este taller (y no un sistema de software-producto individual de un proyecto de cliente), porque las preguntas de la entrevista y las respuestas obtenidas se centraron en las herramientas internas de gestión, no en la arquitectura de ninguna aplicación específica entregada a un cliente.
- El nombre del cliente al que llegan los tickets de Jira se tomó como "Rentec", confirmado en la Ficha de Caracterización del Taller 0 como uno de los clientes de Asul; el nombre del proveedor de soporte de primer nivel se transcribió en la entrevista como "SBS" — se mantiene entre comillas porque no pudo confirmarse contra ninguna fuente escrita y debe validarse con el equipo.
- Azure Monitor se incluyó en C2 como sistema externo asociado al ambiente de producción (no como contenedor del ecosistema de gestión), porque la entrevista lo describió monitoreando infraestructura, no proyectos de Azure DevOps; el origen exacto de su instalación quedó como fragmento poco claro en la transcripción y se señala como pendiente de confirmar.

## 📈 Diagrama final entregado

Ver [`c1-contexto-final.drawio`](c1-contexto-final.drawio) y [`c2-contenedores-final.drawio`](c2-contenedores-final.drawio).

## 📋 Tabla de actores, entidades o componentes

| Nombre del elemento | Tipo | Descripción | Responsable |
|---|---|---|---|
| Alejandro | Persona | Líder técnico; único administrador de despliegues; único titular de la cuenta de Jira entregada por el cliente (licencia única) | Asul |
| Analista de Sistemas | Persona | Colabora con Alejandro en la gestión de casos de soporte | Asul |
| Equipo de Desarrollo | Persona (grupo) | Historias, sprints, pruebas y tareas de los proyectos | Asul |
| Cliente (Rentec) | Persona externa | Equipo del cliente que gestiona tickets en Jira, tras el triage de un primer nivel de soporte | Cliente de Asul |
| Ecosistema de Gestión Asul | Sistema en alcance | Azure DevOps + GitHub + SharePoint, con Time Tracker como add-on | Asul |
| Azure Boards | Contenedor | Épicas, historias, sprints, test plans y tickets de soporte | Asul |
| Repositorio de Código (GitHub) | Contenedor | Control de versiones del código fuente de los proyectos | Asul |
| Repositorio Documental (SharePoint) | Contenedor | Documentos de requisitos, líneas base y versiones borrador | Asul |
| Time Tracker | Contenedor | Add-on de Azure DevOps para registrar horas de soporte y desarrollo | Asul |
| Azure Pipelines | Contenedor (no utilizado) | Instalado pero no usado; el build/deploy es 100% manual | Asul |
| Microsoft Entra ID / Directorio Activo | Infraestructura de soporte | Autenticación de usuarios con dominio propio de Asul | Asul |
| Jira | Sistema externo | Sistema de tickets del cliente; sin conector automático hacia Azure Boards | Cliente de Asul |
| Ambiente de Producción | Sistema externo | VMs en Azure con Azure SQL Database, replicadas en East/West US | Asul o Cliente, según el proyecto |

## 🔍 Investigación complementaria

### Tema investigado:
Riesgos arquitectónicos de tener herramientas de automatización de CI/CD instaladas pero no adoptadas ("shelfware") y su relación con la dependencia de una sola persona para los despliegues.

### Resumen:
La literatura de DevOps (Forsgren et al., *Accelerate*, 2018) identifica la automatización del despliegue como una de las cuatro prácticas técnicas con mayor correlación con el desempeño organizacional, precisamente porque reduce la dependencia de conocimiento tácito de una sola persona. El caso de Asul es un ejemplo textual de lo contrario: Azure Pipelines está disponible dentro de la licencia de Azure DevOps que ya se paga, pero no se usa, y el despliegue depende casi en su totalidad de Alejandro, con capacitación puntual y no sistemática de dos personas de respaldo (Leonardo y Ronald). Microsoft documenta oficialmente que Azure Pipelines puede automatizar builds y releases desde el mismo repositorio de Azure Boards o desde GitHub, sin requerir licenciamiento adicional al ya contratado — es decir que la barrera identificada no es económica sino de adopción y tiempo del equipo, coherente con el Objetivo Estratégico #3 de la Ficha de Caracterización ("fortalecer la competitividad del producto con atributos de seguridad, estabilidad y confiabilidad").

Este hallazgo se retoma en la Fase 05 (Seguridad) como un riesgo de tipo Denial of Service / disponibilidad por dependencia de una sola persona.

## 📚 Referencias
- [1] Brown, S. *The C4 Model for Visualising Software Architecture*. https://c4model.com/
- [2] Forsgren, N., Humble, J., Kim, G. *Accelerate: The Science of Lean Software and DevOps*. IT Revolution Press, 2018.
- [3] Microsoft. *Azure Pipelines documentation*. https://learn.microsoft.com/azure/devops/pipelines/
- [4] Barrera Díaz, Luz Miryan / Suárez, Alejandro. *Reunión de levantamiento de información — Talleres 3 a 6, Asul Tecnologías de la Información SAS*. Transcripción de reunión, septiembre de 2026. Fuente primaria del equipo.
- [5] Fuente asistida por IA: Claude (Anthropic), septiembre 2026 — apoyo en la construcción de los diagramas C1/C2 en draw.io y en la redacción de este informe.

---

_Este documento hace parte de la entrega del Taller 3 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
