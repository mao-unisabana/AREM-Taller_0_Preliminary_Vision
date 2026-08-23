# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
Taller 1 - Modelado de Proceso del Cliente con BPMN

## 👥 Integrantes del equipo
- Mao Suárez
- Nicolas Clavijo

## 🧠 Descripción general del trabajo

El objetivo del taller era modelar en BPMN un proceso real del cliente asignado, distinto del caso base de clase (Agendamiento de Citas — Clínica Salud Viva). Para Asul Tecnologías de la Información SAS se modeló el proceso de **Desarrollo y Soporte de Software**, que es el que la gerente comercial identificó como el más crítico y el que concentra los tres problemas registrados en la Ficha de Caracterización (estimaciones poco confiables, falta de automatización/duplicidad en soporte, y bugs por supuestos no validados).

## 🔧 Proceso de desarrollo

Durante la reunión de levantamiento, la cliente describió dos procesos que en la práctica ocurren de forma continua y no separada: el ciclo de requerimientos-desarrollo-pruebas-entrega, y la interpretación que hacen los desarrolladores del documento de requisitos firmado. En lugar de modelarlos como dos BPMN independientes, el equipo decidió **fusionarlos en un solo diagrama end-to-end**, porque separarlos habría ocultado justamente el punto donde ocurre el problema más mencionado por la cliente: los bugs que se originan cuando el desarrollador "se imagina" cómo interpretar un caso del documento de requisitos, en lugar de preguntar.

Se aplicaron los 5 pasos de la guía metodológica:

1. **Actores** — se identificaron cinco carriles a partir de lo descrito por la cliente: Cliente, Requerimientos, Desarrollo, Pruebas Internas y Pruebas Cliente.
2. **Inicio y fin** — el proceso inicia cuando el cliente detecta una necesidad de software o cambio, y termina cuando recibe el software desplegado en producción.
3. **Actividades** — se listaron las tareas de cada carril tal como las describió la cliente: documentar requisitos de forma manual (documento de ~35-45 páginas con historias de usuario y mockups), interpretar funcionalidades, estimar y planear el sprint, codificar, ejecutar pruebas internas, ejecutar pruebas de aceptación del cliente y desplegar en producción.
4. **Gateways** — se insertaron dos: "¿Pasa pruebas internas?" y "¿Cliente aprueba?", ambos exclusivos (XOR), reflejando el ciclo real de aprobación/rechazo que describió la cliente ("si pasa se va a pruebas del cliente, y si no se devuelve a desarrollo").
5. **Conectar y validar** — se etiquetaron ambas salidas de cada gateway y se verificó el modelo contra la checklist de la guía (un solo inicio, todo camino termina en el evento de fin, actividades nombradas con verbo de acción, sin elementos flotantes).

El modelo se construyó en draw.io (conector de Cowork), reutilizando la misma paleta y notación BPMN (`mxgraph.bpmn.event`, `mxgraph.bpmn.gateway2`) del caso base de clase, para mantener consistencia visual entre el modelo de clase y el del cliente real.

## 🧩 Análisis del modelo propuesto

El modelo se estructura como un pool único con cinco lanes horizontales, leído de izquierda a derecha. Representa fielmente el proceso real porque:

- Incluye el paso manual de documentación de requisitos como una actividad explícita (no implícita), ya que es una de las causas raíz que identificó la cliente.
- Marca con una anotación el punto exacto donde el equipo de Desarrollo interpreta el documento sin validar dudas, que es la causa principal de los bugs "de entendimiento" (a diferencia de bugs de código).
- Modela dos loops de retorno a Desarrollo — uno desde Pruebas Internas y otro desde Pruebas Cliente — en lugar de uno solo, porque la cliente fue explícita en que el reproceso ocurre en ambas etapas de prueba, no solo al final.

**Supuestos tomados:** se asumió que el mismo equipo de Desarrollo ejecuta el despliegue a producción (no hay un rol de DevOps/Operaciones separado, consistente con un equipo de menos de 10 personas); y que "Requerimientos" es un carril propio de Asul (no del cliente final), ya que la cliente describió un equipo interno dedicado a levantar y documentar requisitos antes de pasarlos a Desarrollo.

## 📈 Diagrama final entregado

Ver [`entrega/modelo-final.drawio`](modelo-final.drawio) — página única "Desarrollo y Soporte".

## 📋 Tabla de actores, entidades o componentes

| Nombre del elemento | Tipo | Descripción | Responsable |
|---------------------|------|-------------|-------------|
| Cliente | Actor / Lane | Solicita el cambio o desarrollo y ejecuta las pruebas de aceptación | Empresa cliente de Asul |
| Requerimientos | Lane (rol interno) | Levanta y documenta manualmente los requisitos como historias de usuario con mockups | Equipo de Requerimientos de Asul |
| Desarrollo | Lane (rol interno) | Interpreta el documento firmado, estima, codifica, corrige bugs y despliega a producción | Equipo de Desarrollo de Asul |
| Pruebas Internas | Lane (rol interno) | Ejecuta las pruebas funcionales antes de enviar la entrega al cliente | Equipo de Pruebas de Asul |
| Pruebas Cliente | Lane (actor del cliente) | Ejecuta la aceptación final del sprint entregado | Empresa cliente de Asul |

## 🔍 Investigación complementaria

### Tema investigado:
Buenas prácticas BPMN para modelar procesos de desarrollo de software con ciclos de retrabajo (loops de corrección).

### Resumen:
La especificación oficial de BPMN (OMG) recomienda usar gateways exclusivos (XOR) cuando solo un camino puede tomarse a la vez — como es el caso de "¿pasa la prueba?" — y reservar los gateways paralelos para actividades que realmente ocurren de forma simultánea, error común que se evitó en este modelo al no forzar paralelismo donde el proceso es estrictamente secuencial. La guía de Camunda sobre patrones de reproceso ("rework loops") sugiere además que el destino de un loop de corrección sea explícito y único (en este caso, siempre la actividad "Codificar / desarrollar"), en lugar de crear una tarea de corrección separada por cada punto de falla — esto evita duplicar lógica y mantiene el diagrama legible, algo que se aplicó dirigiendo ambos loops (pruebas internas y pruebas cliente) al mismo punto de retorno.

Esto se relaciona directamente con el taller porque el proceso de Asul es, en esencia, un proceso con alto índice de retrabajo (la cliente mencionó fases con más de 100 bugs internos), y modelarlo sin señalizar claramente ese patrón habría ocultado el problema principal que motivó este levantamiento.

## 📚 Referencias
- [1] Object Management Group (OMG). *Business Process Model and Notation (BPMN), Version 2.0*. https://www.omg.org/spec/BPMN/
- [2] Camunda. *BPMN 2.0 Modeling Reference*. https://camunda.com/bpmn/reference/
- [3] Bizagi. *BPMN Training Guide — buenas prácticas de modelado*. https://www.bizagi.com/
- [4] Fuente asistida por IA: Claude (Anthropic), agosto 2026 — apoyo en la construcción del diagrama BPMN en draw.io y en la redacción de este informe a partir de la transcripción de la reunión con el cliente.

---

_Este documento hace parte de la entrega del Taller 1 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
