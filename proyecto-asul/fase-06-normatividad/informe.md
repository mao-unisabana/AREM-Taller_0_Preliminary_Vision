# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
Taller 6 - Checklist de Cumplimiento Normativo

## 👥 Integrantes del equipo
- Mao Suárez
- Nicolas Clavijo

## 🧠 Descripción general del trabajo

El objetivo del taller era construir un checklist de cumplimiento normativo para Asul, cubriendo Habeas Data (Ley 1581 de 2012), buenas prácticas de seguridad de la información (ISO 27000/27001) y normatividad sectorial aplicable a sus clientes del sector asegurador. **Esta entrega es parcial**: la entrevista con la cliente (Luz Miryan, gerente administrativa) se agotó en el tiempo disponible justo antes de que se respondiera la última pregunta, sobre si el cliente exige a Asul cumplir alguna normativa o circular específica del sector financiero/asegurador como condición del contrato. Esa pregunta queda registrada como pendiente en este informe y en el checklist, en vez de completarse con una suposición.

## 🔧 Proceso de desarrollo

Se aplicaron los 5 pasos de la guía metodológica:

1. **Identificar datos y procesos sensibles** — datos de pólizas y asegurados de clientes del sector asegurador (cuando el cliente final es de ese sector); datos personales de empleados y candidatos de la propia Asul; código fuente y documentación contractual de todos los clientes.
2. **Construir el checklist por categoría** — se organizó en 4 categorías: Habeas Data, ISO 27001/27000, Confidencialidad y seguridad contractual, y Retención/gobierno de datos, más una categoría abierta de Normatividad sectorial (incompleta).
3. **Evaluar el cumplimiento** — cada criterio se calificó como ✅ Cumple, ⚠️ Cumple parcialmente / sin confirmar, o ❌ No cumple, con la evidencia citada directamente de la entrevista.
4. **Documentar el riesgo de cada brecha** — cada ❌ y cada ⚠️ relevante se llevó a la hoja "Brechas Identificadas" con su riesgo asociado.
5. **Priorizar y recomendar** — se asignó prioridad Alta/Media/Baja a cada brecha, considerando tanto el impacto regulatorio (Ley 1581) como el impacto reputacional/comercial (falta de certificación, falta de respuesta sobre exigencias sectoriales).

## 🧩 Análisis del modelo propuesto

Asul cumple de forma sólida el marco de Habeas Data (Ley 1581): tiene política pública, documentación interna y mecanismo de solicitud de eliminación/rectificación. También aplica controles de confidencialidad y anonimización de datos productivos en ambientes de prueba, algo especialmente relevante dado que maneja datos de pólizas y asegurados de clientes del sector financiero.

Las brechas más relevantes no están en la protección de datos personales de terceros, sino en **la gestión del ciclo de vida de la información propia del negocio**: no hay certificación externa de seguridad de la información (solo buenas prácticas internas bajo ITMark), y sobre todo, **no existe ningún proceso de eliminación o anonimización del código fuente y los documentos de un cliente una vez finaliza el contrato** — se conservan indefinidamente "porque a veces los clientes vuelven después de años". Esto es un hallazgo importante porque, si ese código o esos documentos contienen datos personales de los empleados o clientes finales de ese cliente, la retención indefinida entra en tensión directa con los principios de finalidad y conservación de la propia Ley 1581 que Asul sí cumple en su rol de responsable de los datos de sus propios empleados.

**Supuestos tomados:**
- El criterio 11 (normatividad sectorial) se marcó como "⚠️ (sin información)" en lugar de forzar una calificación de cumple/no cumple, porque no fue respondido en la entrevista — es preferible declarar explícitamente el vacío de información a inventar una respuesta.
- Se asumió que "ITMark" (mencionado como "itemarca" en la transcripción de la entrevista) corresponde a la certificación ITMark ya registrada en la Ficha de Caracterización del Taller 0 como uno de los marcos de calidad de Asul, y no a otro término.
- Dado que la pregunta sobre normativa sectorial no fue respondida, la brecha #4 de la tabla de brechas se prioriza como Alta no porque se haya confirmado un incumplimiento, sino porque es una pregunta de cumplimiento contractual sin cerrar — el riesgo de dejarla abierta es en sí mismo alto.

## 📈 Diagrama final entregado

Ver [`checklist-cliente.xlsx`](checklist-cliente.xlsx) (hojas "Checklist General" con 11 criterios evaluados, y "Brechas Identificadas" con 5 brechas priorizadas).

## 📋 Tabla de actores, entidades o componentes

| Nombre del elemento | Tipo | Descripción | Responsable |
|---|---|---|---|
| Política de tratamiento de datos personales | Control / Documento | Publicada en la web de Asul, a la luz de la Ley 1581 de 2012 | Asul |
| Cláusula de confidencialidad | Control contractual | Firmada por empleados y clientes | Asul |
| Proceso de ofuscamiento de datos de prueba | Control técnico | Anonimiza datos productivos antes de usarlos en ambientes de prueba | Asul |
| Certificación ITMark | Marco de calidad | Marco de referencia de buenas prácticas de Asul (incluye controles alineados a ISO 27000) | Asul |
| Auditoría de seguridad semestral | Control organizacional | Revisión de contraseñas, escritorio limpio, correo limpio, cada 6 meses | Asul |
| Retención indefinida de código fuente | Brecha identificada | Sin proceso de eliminación o anonimización tras el cierre de un contrato | Asul |
| Normativa sectorial del cliente | Pendiente | No se confirmó si el cliente exige el cumplimiento de una normativa o circular sectorial específica | Cliente (Rentec) |

## 🔍 Investigación complementaria

### Tema investigado:
Tensión entre la retención indefinida de información de clientes finalizados y los principios de finalidad y conservación de la Ley 1581 de 2012 (Habeas Data) en Colombia.

### Resumen:
La Ley 1581 de 2012 y su decreto reglamentario 1377 de 2013 establecen que los datos personales solo pueden conservarse durante el tiempo necesario para cumplir la finalidad que justificó su recolección, después de lo cual deben ser suprimidos o anonimizados, salvo que exista una obligación legal de conservarlos por más tiempo (por ejemplo, obligaciones tributarias o contables). El caso de Asul —conservar indefinidamente el código fuente y los documentos de clientes ya finalizados, alegando que "a veces contactan después de años"— es una justificación de conveniencia comercial, no una obligación legal, por lo que representa un vacío de cumplimiento real si ese código o esos documentos incluyen datos personales de empleados o usuarios finales del cliente. La recomendación estándar de la industria (y de la Superintendencia de Industria y Comercio como autoridad de protección de datos en Colombia) es definir una política de retención con plazos explícitos, y anonimizar en vez de eliminar cuando se necesite conservar el valor técnico del código sin conservar los datos personales asociados.

## 📚 Referencias
- [1] Congreso de Colombia. *Ley 1581 de 2012 — Régimen General de Protección de Datos Personales*.
- [2] Presidencia de la República de Colombia. *Decreto 1377 de 2013*, reglamentario de la Ley 1581 de 2012.
- [3] Superintendencia de Industria y Comercio (SIC). *Guía para la implementación del principio de responsabilidad demostrada (Accountability)*.
- [4] ISO/IEC. *ISO/IEC 27000:2018 — Information technology — Security techniques — Information security management systems — Overview and vocabulary*.
- [5] Barrera Díaz, Luz Miryan / Suárez, Alejandro. *Reunión de levantamiento de información — Talleres 3 a 6, Asul Tecnologías de la Información SAS*. Transcripción de reunión, septiembre de 2026. Fuente primaria del equipo (entrevista incompleta — ver limitaciones arriba).
- [6] Fuente asistida por IA: Claude (Anthropic), septiembre 2026 — apoyo en la construcción del checklist en Excel y en la redacción de este informe.

## ⚠️ Pendiente antes de la sustentación

- Completar con el cliente la pregunta sobre normativa o circular sectorial específica exigida contractualmente (fila 11 del checklist).
- Confirmar el nombre exacto del primer nivel de soporte del cliente (transcrito como "SBS") y del segundo cliente mencionado con modelo de alojamiento distinto (transcrito como "SMPPI"), para citarlos con precisión si terminan siendo relevantes para este taller.
- Confirmar si existe habilitación de MFA en todas las cuentas de Microsoft 365/Azure (quedó sin responder explícitamente, ver Fase 05).

---

_Este documento hace parte de la entrega del Taller 6 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
