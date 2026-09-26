# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
Taller 4 - Mapa de Infraestructura y Diagnóstico Técnico

## 👥 Integrantes del equipo
- Mao Suárez
- Nicolas Clavijo

## 🧠 Descripción general del trabajo

El objetivo del taller era construir el mapa de infraestructura tecnológica real de Asul y, a partir de él, priorizar los riesgos técnicos más relevantes. La entrevista con Alejandro (líder técnico) confirmó que Asul no tiene infraestructura propia: todo su cómputo, tanto el de gestión (Fase 03) como el de los ambientes productivos de sus clientes, está alojado en Azure, sobre máquinas virtuales, sin componentes on-premise.

## 🔧 Proceso de desarrollo

Se aplicaron los 5 pasos de la guía metodológica:

1. **Identificar componentes** — puestos de trabajo del equipo (computadores propios, sin ambiente de desarrollo centralizado), máquinas virtuales de Azure, Azure SQL Database, el equipo que actúa como servidor de sincronización de SharePoint, y Azure Monitor.
2. **Agrupar por zona/capa** — puestos de trabajo de Asul, la red del cliente (firewall de IPs fijas y VPN exclusiva del cliente), la región primaria de Azure (US East) y su réplica (US West), y la zona de backups.
3. **Conectar los componentes** — se trazaron los accesos de los desarrolladores hacia los ambientes de cliente (limitados por whitelist de IP fija, sin acceso de Asul a la VPN del cliente), la replicación de las máquinas virtuales y de la base de datos entre regiones, y el flujo de sincronización semanal hacia el backup de SharePoint.
4. **Marcar redundancia y capacidad** — se identificaron dos réplicas de Azure SQL Database (una para reportes, que descarga a la base productiva, y otra disponible para continuidad) y la replicación geográfica East/West con pruebas periódicas de recuperación ante desastres.
5. **Diagnosticar y priorizar** — ver tabla de diagnóstico más abajo.

El diagrama se construyó en draw.io reutilizando la notación del ejemplo de clase (`mapa-borrador.drawio`): cajas punteadas para zonas, rectángulo azul para componentes de infraestructura, cilindro gris para bases de datos, y el estilo de advertencia (fondo rojo, ⚠️) para los componentes en riesgo.

## 🧩 Análisis del modelo propuesto

El hallazgo estructural más importante es que **no existe infraestructura redundante para el propio ecosistema de gestión de Asul** (Azure DevOps, GitHub): toda la redundancia descrita en la entrevista (réplicas de base de datos, réplica geográfica US East/West, pruebas de disaster recovery) aplica a los **ambientes productivos de los clientes**, no a las herramientas internas con las que Asul opera. El respaldo de SharePoint depende de un único equipo que actúa como servidor de sincronización, con una copia adicional solo semanal — es decir que, en el peor caso, una falla del computador que sincroniza SharePoint podría significar hasta una semana de información no respaldada en un segundo disco.

El segundo hallazgo es la concentración de conocimiento operativo: Alejandro es quien despliega a producción en prácticamente todos los casos, con capacitación puntual (no sistemática) de dos personas de respaldo. Esto es consistente con lo encontrado en la Fase 03 sobre Azure Pipelines instalado pero no utilizado — la ausencia de automatización de despliegue y la dependencia de una persona son la misma causa raíz vista desde dos ángulos distintos (arquitectura de herramientas vs. infraestructura operativa).

### Tabla de diagnóstico priorizado

| Componente | Riesgo diagnosticado | Categoría | Impacto si ocurre | Prioridad |
|---|---|---|---|---|
| Despliegue a producción (Alejandro) | Punto único de falla operativo (bus factor = 1) | Disponibilidad / Continuidad | Si Alejandro no está disponible, los despliegues a producción se retrasan o se detienen | Alta |
| Backup de SharePoint (sincronización semanal a disco) | Rezago de respaldo — ventana de pérdida de hasta 7 días | Continuidad de negocio | Pérdida de documentos o versiones recientes si falla el equipo de sincronización antes de la copia semanal | Alta |
| Azure DevOps (Boards, historial de tickets) sin backup propio descrito | Falta de plan de respaldo/recuperación documentado | Continuidad de negocio | Pérdida de trazabilidad de requisitos y soporte ante un incidente de la plataforma | Media |
| Separación de infraestructura entre clientes | No se pudo confirmar en la entrevista si los ambientes de distintos clientes están completamente aislados | Seguridad / Aislamiento | Un incidente en el ambiente de un cliente podría, en el peor escenario no confirmado, afectar a otro | Media (pendiente de validar) |
| Acceso a ambientes de cliente por IP fija (whitelist) | Control rígido, sin VPN propia de Asul | Seguridad de red | Cambios de IP del equipo bloquean el acceso hasta actualizar la whitelist con el cliente | Baja |

**Supuestos tomados:**
- Se asumió que "toda la operación está en Azure" implica que no hay servidores físicos ni otro proveedor cloud, tal como lo confirmó Alejandro explícitamente ("solamente en Azure... todos [son VMs]").
- La pregunta sobre si los ambientes de distintos clientes comparten o no infraestructura no obtuvo una respuesta clara en la entrevista (se registró como una confusión de idioma en la transcripción); se documenta como riesgo no confirmado en vez de asumir una respuesta.
- No se identificó en la entrevista un cuello de botella técnico específico de capacidad o rendimiento (a diferencia del ejemplo de RedExpress de la guía); los riesgos priorizados aquí son de continuidad y disponibilidad operativa, no de rendimiento.

## 📈 Diagrama final entregado

Ver [`mapa-final.drawio`](mapa-final.drawio).

## 📋 Tabla de actores, entidades o componentes

| Nombre del elemento | Tipo | Descripción | Responsable |
|---|---|---|---|
| Puestos de trabajo Asul | Componente | Computadores propios de cada desarrollador, sin ambiente centralizado de desarrollo | Asul |
| VMs — Ambiente de Producción | Componente | Máquinas virtuales en Azure, por proyecto de cliente | Asul o Cliente, según el proyecto |
| Azure SQL Database | Base de datos | Motor de base de datos usado en los proyectos, con 2 réplicas | Asul |
| Firewall del Cliente (IP whitelist) | Componente de red | Restringe el acceso a los ambientes de un cliente a IPs fijas autorizadas | Cliente |
| VPN del Cliente | Componente de red | Uso exclusivo del cliente; Asul no tiene acceso a ella | Cliente |
| Equipo-servidor de sincronización SharePoint | Componente | Sincroniza y respalda semanalmente el contenido documental | Asul |
| Azure Monitor | Componente de monitoreo | Genera métricas e informes de infraestructura | Asul |

## 🔍 Investigación complementaria

### Tema investigado:
Impacto del factor humano único ("bus factor") en la disponibilidad de despliegues, y buenas prácticas de respaldo documental frente a la sincronización periódica manual.

### Resumen:
El concepto de "bus factor" (o "lottery factor") describe el riesgo de que el conocimiento crítico de un sistema recaiga en una sola persona; la práctica recomendada por la industria (Forsgren et al., *Accelerate*, 2018; también documentada por Google en su libro *Site Reliability Engineering*, 2016) es reducirlo mediante automatización (para que el conocimiento quede en el pipeline, no en la persona) y mediante rotación documentada de responsables. El caso de Alejandro en Asul combina ambos vectores de riesgo: no hay automatización de despliegue (Fase 03) y la capacitación de respaldo (Leonardo, Ronald) fue puntual y no sistemática.

Sobre el respaldo de SharePoint, Microsoft documenta que SharePoint Online ya incluye retención y versionado nativos en la nube; el mecanismo adicional descrito por Alejandro (un equipo local que sincroniza y luego copia semanalmente a otro disco) es una capa manual añadida por Asul, probablemente por hábito o por necesidad de una copia fuera de la nube del cliente — pero introduce exactamente el rezago de una semana que un mecanismo de copia incremental diaria evitaría.

## 📚 Referencias
- [1] Forsgren, N., Humble, J., Kim, G. *Accelerate: The Science of Lean Software and DevOps*. IT Revolution Press, 2018.
- [2] Beyer, B., Jones, C., Petoff, J., Murphy, N. (eds). *Site Reliability Engineering*. O'Reilly / Google, 2016.
- [3] Microsoft. *Azure SQL Database — Business continuity and disaster recovery*. https://learn.microsoft.com/azure/azure-sql/database/business-continuity-high-availability-disaster-recover-hadr-overview
- [4] Microsoft. *SharePoint Online — versioning and retention*. https://learn.microsoft.com/sharepoint/
- [5] Barrera Díaz, Luz Miryan / Suárez, Alejandro. *Reunión de levantamiento de información — Talleres 3 a 6, Asul Tecnologías de la Información SAS*. Transcripción de reunión, septiembre de 2026. Fuente primaria del equipo.
- [6] Fuente asistida por IA: Claude (Anthropic), septiembre 2026 — apoyo en la construcción del mapa de infraestructura en draw.io y en la redacción de este informe.

---

_Este documento hace parte de la entrega del Taller 4 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
