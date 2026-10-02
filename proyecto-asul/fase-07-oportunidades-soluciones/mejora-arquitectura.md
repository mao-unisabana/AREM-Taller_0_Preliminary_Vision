# Mejora de Arquitectura (TO-BE) — Identificación y Priorización de Mejoras

## Cliente
Asul Tecnologías de la Información SAS

## Integrantes del equipo
- Mao Suárez
- Nicolas Clavijo

---

## 1. Diagnóstico inicial

Con base en lo ya diagnosticado en los Talleres 3 a 6 (sin inventar hallazgos nuevos):

**¿Cuáles son los procesos o tecnologías que generan mayor fricción en la operación?**
La sincronización manual de tickets entre el Jira del cliente y Azure Boards de Asul (Fase 03/05) — cada actualización se transcribe a mano, campo a campo — y la dependencia casi total de Alejandro para cualquier despliegue a producción (Fase 04/05).

**¿Qué problemas recurrentes señalaron los usuarios o el cliente?**
La Ficha de Caracterización (Taller 0) registró tres problemas recurrentes de la cliente: estimaciones poco confiables, falta de automatización y duplicidad de trabajo en soporte (por la transcripción manual Jira↔Azure DevOps), y bugs originados en supuestos no validados por el equipo de desarrollo.

**¿Qué vulnerabilidades o riesgos quedaron evidenciados en el análisis previo?**
Del análisis STRIDE (Fase 05): la cuenta compartida de Jira, el sistema del cliente (R1, Alto — la amenaza de mayor riesgo de todo el taller; en Azure DevOps cada persona ya tiene su propia cuenta individual), el despliegue dependiente de una sola persona (R4, Alto), el backup semanal de SharePoint con rezago (R5, Medio), la falta de un control técnico (DLP) para la descarga de código (R6, Medio), y la ausencia de revisión periódica de roles en Azure DevOps (R8, Medio). Del checklist de Normatividad (Fase 06): la retención indefinida de código y documentos de clientes finalizados, sin proceso de eliminación o anonimización (brecha de prioridad Alta), y la falta de certificación externa ISO 27001 (brecha de prioridad Media). El MFA (confirmado habilitado en todas las cuentas) y la normativa sectorial (confirmado que no aplica) ya quedaron cerrados en sus respectivas fases y no se tratan como brechas aquí.

**Resumen del problema actual (foto del AS-IS):**

Asul opera con un ecosistema de gestión (Azure DevOps + GitHub + SharePoint) bien definido en su alcance (Fase 03), pero con tres puntos de fragilidad concentrados: (1) una frontera manual con el sistema del cliente que genera reprocesos y riesgo de inconsistencia; (2) una concentración operativa y de identidad en una sola persona (Alejandro), tanto para desplegar como para acceder al Jira del cliente con la única cuenta que este entregó; y (3) un vacío de gobierno sobre el ciclo de vida de la información de clientes que ya terminaron su contrato con Asul. Ninguno de los tres es un problema de "ataque externo": son problemas de proceso, identidad y continuidad operativa — coherente con que no se han reportado incidentes de seguridad en los últimos años.

---

## 2. Propuesta de mejoras

### 2.1 Lluvia de ideas (sin censura inicial)

| # | Idea de mejora | Tipo |
|---|---|---|
| 1 | Activar Azure Pipelines para automatizar build y despliegue | Tecnología |
| 2 | Negociar con el cliente licencias/cuentas individuales de Jira (o acceso federado) para el equipo de soporte | Proceso / Comercial |
| 3 | Conector pago o script propio de validación periódica entre Jira y Azure Boards | Tecnología |
| 4 | Automatizar el backup de SharePoint con copia incremental diaria | Tecnología |
| 5 | Implementar una herramienta de DLP (ej. Microsoft Purview) para controlar la descarga de código y documentos | Seguridad |
| 6 | Definir una política formal de retención y eliminación/anonimización de código y documentos post-contrato | Proceso / Normatividad |
| 7 | Establecer una revisión semestral de roles en Azure DevOps, aprovechando la auditoría de seguridad ya existente | Proceso |
| 8 | Documentar un runbook de despliegue y formalizar la capacitación de respaldo (Leonardo/Ronald) | Proceso |
| 9 | Evaluar un mapeo formal ITMark↔ISO 27000 o la certificación ISO 27001 | Seguridad / Normatividad |
| 10 | Dashboard o reporte periódico de estado de tickets para reducir la dependencia exclusiva del correo de alerta | Comunicación con el cliente |

### 2.2 Priorización (3 ideas seleccionadas, con justificación)

| Solución priorizada | Esfuerzo | Impacto | Quick win / Largo plazo | Justificación |
|---|---|---|---|---|
| Activar Azure Pipelines (CI/CD automatizado) | Medio | Alto | Quick win | Ya está incluido en la licencia de Azure DevOps (Fase 03); cierra R4, el segundo riesgo más alto de STRIDE. Solución única y clara — no requirió matriz de decisión. |
| Resolver el acceso con cuenta compartida de Jira | Alto (de negociación) / Bajo (de la opción elegida) | Alto | Largo plazo en origen, pero la mitigación elegida es quick win | Cierra R1, la amenaza de mayor riesgo de todo el análisis STRIDE. Tenía 3 opciones reales con trade-offs distintos, por lo que se decidió con una **matriz de decisión ponderada**, validada directamente con la gerencia de Asul — ver [`matriz-decision-cuenta-compartida.md`](matriz-decision-cuenta-compartida.md). |
| Definir política de retención y eliminación/anonimización post-contrato | Bajo | Alto | Quick win | Cierra la brecha de mayor prioridad de Normatividad (Fase 06); es una definición de política, no desarrollo técnico. Solución única — no requirió matriz de decisión. |

La matriz de decisión para la cuenta compartida de Jira concluyó en implementar una **bitácora interna de acceso compartido** (Opción B, puntaje ponderado 4.25 sobre 5, robusta ante dos escenarios distintos de pesos), descartando negociar licencias individuales de Jira porque la gerencia confirmó que no hay presupuesto disponible ni ganancia comercial clara en insistirle al cliente en este momento, y que el objetivo de fondo es **dividir el riesgo y el poder de decisión hoy concentrado en Alejandro** — el mismo objetivo estratégico que motiva priorizar Azure Pipelines.

---

## 3. Visualización TO-BE

### 3.1 Proceso mejorado

El proceso de despliegue deja de ser "Alejandro revisa, compila y publica manualmente" para convertirse en "el equipo hace push a GitHub → Azure Pipelines compila, prueba y despliega automáticamente → Alejandro queda como respaldo solo para casos excepcionales". El proceso de soporte no cambia en su frontera con el cliente (la sincronización Jira↔Azure Boards sigue siendo manual, ver sección 4), pero ahora cada consulta o cambio hecho con la cuenta de Jira compartida queda registrado en una bitácora interna, dando trazabilidad sin depender de que el cliente entregue más licencias.

### 3.2 Cambios en aplicaciones, infraestructura y flujos de información

Ver anexos [`to-be-aplicaciones-final.drawio`](to-be-aplicaciones-final.drawio) (extiende el C2 del Taller 3: Azure Pipelines pasa de "instalado, no utilizado" a activo, y se agrega el contenedor de Bitácora de Acceso Compartido sobre el uso de la cuenta de Jira) y [`to-be-tecnologia-final.drawio`](to-be-tecnologia-final.drawio) (extiende el mapa del Taller 4: el despliegue automatizado reduce la severidad del riesgo de Alejandro como punto único de falla, y se añade la política de retención sobre los discos de backup de código). Ambos diagramas mantienen sin cambios los elementos del AS-IS que no se tocaron en esta iteración (la sincronización manual Jira↔Azure Boards, el backup semanal de SharePoint, y la separación de infraestructura entre clientes, que sigue sin confirmarse).

### 3.3 Controles de seguridad integrados

Del Taller 5 se integran explícitamente: la mitigación de R1 (cuenta compartida de Jira) vía la bitácora de acceso, y la mitigación de R4 (despliegue de punto único de falla) vía Azure Pipelines. El MFA, ya confirmado habilitado en todas las cuentas (actualización de Fase 05), se mantiene como control vigente sin cambios. La separación de roles Administrator/Contributor de Azure DevOps (control ya existente) sigue aplicando sobre el nuevo flujo de Pipelines sin modificaciones adicionales.

---

## 4. Análisis de beneficios y riesgos

Ver la matriz completa en el anexo [`matriz-brechas.xlsx`](matriz-brechas.xlsx) (hojas "Brechas Cerradas", "Riesgos de Implementación", "Capacidades" y "Paquetes de Trabajo"). Resumen:

| Mejora / Solución | Beneficio de negocio | Beneficio tecnológico/seguridad | Riesgo, limitación o dependencia de implementación |
|---|---|---|---|
| Azure Pipelines (CI/CD) | Despliegues más rápidos y consistentes, sin depender de la disponibilidad de una persona | Cierra R4 (punto único de falla en el despliegue) | Requiere tiempo de configuración y validación por proyecto; Alejandro sigue como respaldo durante la transición |
| Bitácora de acceso compartido (cuenta de Jira) | No requiere presupuesto adicional ni renegociar con el cliente; avanza el objetivo de la gerencia de dividir el riesgo de una sola persona | Mitiga R1 (la amenaza de mayor riesgo de STRIDE) con trazabilidad interna | Es un control de proceso, no técnico: depende de la disciplina del equipo; no elimina el riesgo de raíz |
| Política de retención post-contrato | Reduce exposición legal y reputacional frente a la Ley 1581 | Cierra la brecha de mayor prioridad de Normatividad | Requiere definir plazos y coordinarlos con cláusulas contractuales ya firmadas |

**Capacidades mejoradas y paquetes de trabajo:** las tres soluciones priorizadas se agrupan en tres paquetes de trabajo (WP1 Automatización del despliegue, WP2 Control de acceso compartido, WP3 Gobierno del ciclo de vida de la información), cada uno quick win de 1 a 4 semanas, que elevan la madurez de sus capacidades de negocio asociadas de 2 a 3-4 sobre 5 (detalle en la hoja "Capacidades" del anexo). Quedan explícitamente en el backlog, sin priorizar en esta iteración: la sincronización Jira↔Azure Boards (idea #3), el backup diario de SharePoint (idea #4), el DLP (idea #5), la revisión de roles (idea #7) y la certificación ISO 27001 (idea #9) — no porque no importen, sino porque el ejercicio de priorización de 2-3 ideas de este taller exige dejar explícito qué no se aborda todavía.

---

## Anexos
- Diagrama TO-BE de Aplicaciones: [`to-be-aplicaciones-final.drawio`](to-be-aplicaciones-final.drawio)
- Diagrama TO-BE de Tecnología: [`to-be-tecnologia-final.drawio`](to-be-tecnologia-final.drawio)
- Matriz de brechas (Gap Analysis): [`matriz-brechas.xlsx`](matriz-brechas.xlsx)
- Matriz de decisión ponderada (cuenta compartida): [`matriz-decision-cuenta-compartida.md`](matriz-decision-cuenta-compartida.md)

---

_Este documento hace parte de la entrega del Taller 7 (Opportunities & Solutions) del curso AREM - Universidad de La Sabana._
