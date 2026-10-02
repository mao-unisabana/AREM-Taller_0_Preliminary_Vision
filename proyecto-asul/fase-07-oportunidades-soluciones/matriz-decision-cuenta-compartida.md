# Matriz de Decisión Ponderada

## Brecha que se decide

R1 — Cuenta compartida de Jira, el sistema del cliente (Fase 05 - Seguridad, STRIDE: Spoofing/Repudiation, Nivel de Riesgo Alto, la amenaza de mayor riesgo de todo el análisis). Nota de alcance: esto es sobre Jira, no sobre Azure DevOps — en Azure DevOps cada persona del equipo de Asul ya tiene su propia cuenta individual con permisos designados; el cliente nunca interactúa con Azure DevOps.

---

## Paso 1 — Problema en términos de impacto

Todo el lado de soporte de Asul (Alejandro y el Analista de Sistemas) accede al **Jira del cliente** con una única cuenta (la de Alejandro), entregada por el cliente junto con una sola licencia. Cuando algo se consulta o se actualiza mal en un ticket de Jira, no hay forma de distinguir quién de los dos lo hizo — el historial de actividad de la plataforma queda atribuido siempre a la misma identidad técnica. Si esa contraseña se filtra, cualquiera que la tenga consulta y actualiza tickets del cliente con el nivel de acceso del líder técnico, sin que haya una segunda cuenta que aislar o revocar. Esto no afecta a Azure DevOps: ahí cada persona del equipo de Asul ya tiene su propia cuenta individual con permisos designados, y el cliente no tiene ni necesita acceso a esa herramienta.

---

## Paso 2 — Último momento responsable para decidir

| Dato | Valor |
|---|---|
| Fecha en que el problema empieza a doler | No hay una fecha concreta: es un riesgo latente desde que se entregó la licencia, no un incidente en curso |
| Tiempo que necesita la opción más probable (implementación + pruebas) | Bitácora interna: 1-2 semanas (definir plantilla de registro y socializarla con el equipo) |
| **Último momento responsable** (fecha − tiempo necesario) | No aplica un plazo duro — el cliente confirmó que no hay urgencia ni evento (auditoría, renovación de contrato) que lo dispare |
| Fecha de hoy y días que quedan | Octubre de 2026 — se puede evaluar con calma, pero no hay razón para posponerlo indefinidamente dado que es la amenaza de mayor riesgo del taller de Seguridad |

---

## Paso 3 — Criterios, pesos y escala

Pesos validados directamente con la gerencia de Asul (Luz Miryan) en octubre de 2026, dadas dos señales explícitas del negocio: (1) no hay presupuesto para licencias adicionales de Jira ni se ve ganancia comercial en insistir con el cliente por eso ahora, y (2) la gerencia está buscando activamente **dividir el riesgo y el poder de decisión que hoy se concentra en Alejandro**, no solo en esta cuenta sino en general.

| Criterio | Peso (%) | Qué significa 5 | Qué significa 1 |
|---|---|---|---|
| Costo / viabilidad financiera inmediata | 35 | No requiere presupuesto adicional ni aprobación comercial | Requiere presupuesto adicional que hoy no está disponible |
| Reducción del riesgo de seguridad (trazabilidad individual) | 25 | Elimina por completo el riesgo de suplantación/repudio | No reduce el riesgo en absoluto |
| Contribución a dividir el riesgo y la dependencia de una persona | 25 | Reduce claramente que todo dependa de Alejandro | No cambia la dependencia de una sola persona |
| Relación comercial con el cliente (Rentec) | 15 | No genera fricción ni pide algo ya descartado | Genera fricción alta (insistir en algo que el cliente ya no ve con buenos ojos) |
| **Total** | **100** | | |

**Por qué esos pesos:** el costo pesa más (35%) porque la gerencia ya fue explícita en que no hay presupuesto disponible — cualquier opción que dependa de inversión nueva parte en desventaja real, no hipotética. Reducir el riesgo de seguridad y dividir la dependencia de una persona pesan igual (25% cada uno) porque son los dos objetivos de fondo que motivan esta decisión: uno es el hallazgo técnico de Seguridad (Fase 05) y el otro es un objetivo estratégico explícito de la gerencia. La relación comercial pesa menos (15%) porque, aunque importa, no es el factor que más debería mover esta decisión puntual.

**Criterio eliminatorio:** ninguno — la gerencia indicó explícitamente que no hay un umbral duro, aunque en la práctica el criterio de costo funciona como un filtro fuerte dado que no hay presupuesto liberado.

---

## Paso 4 — Opciones

| Opción | Descripción |
|---|---|
| A | Negociar con el cliente (Rentec) licencias o cuentas individuales de Jira para el equipo de soporte de Asul (o acceso federado/invitado), en vez de una única licencia compartida |
| B | Bitácora interna de acceso compartido: registro de quién usa la cuenta y cuándo, con buenas prácticas internas de manejo de la contraseña |
| C | Aceptar el riesgo residual documentado (no ha habido incidentes de seguridad en los últimos años, según la entrevista de Fase 05/06) |

---

## Paso 5 — Consejo consultado

| A quién (quien sabe / a quien le afecta) | Qué aportó | Qué opción afecta |
|---|---|---|
| Luz Miryan (gerencia de Asul) | Confirmó que no hay presupuesto para licencias adicionales y que no se percibe ganancia en insistirle al cliente por ahora; además, confirmó que la gerencia busca activamente dividir el riesgo y el poder de decisión que hoy se concentra en Alejandro, "casi fuente de verdad" del equipo | Descarta en la práctica la Opción A; valida que la Opción B conecta directamente con un objetivo estratégico de la gerencia, no solo con un hallazgo técnico |

*(Pendiente de un segundo consejo opcional: validar con Alejandro el diseño concreto de la bitácora — formato, dónde se registra, quién la audita — antes de implementarla; no cambia el resultado de esta matriz, pero sí el detalle de la Fase 3 de implementación.)*

---

## Paso 6 — Puntajes con justificación

| Opción | Costo/viabilidad (35%) | Reducción de riesgo (25%) | Dividir dependencia (25%) | Relación comercial (15%) |
|---|---|---|---|---|
| A — Licencias individuales de Jira | 1 — sin presupuesto disponible, confirmado por la gerencia | 5 — resuelve el problema de raíz con identidades individuales reales en Jira | 3 — divide el acceso a esta herramienta puntual, pero no la dependencia general de conocimiento de Alejandro | 1 — la gerencia ya evaluó que insistir no tiene ganancia clara para el cliente |
| B — Bitácora interna | 5 — costo cero, 100% bajo control de Asul | 3 — mitiga el repudio con registro, pero no evita que cualquiera con la contraseña actúe como Alejandro (no es una barrera criptográfica) | 4 — permite que más de una persona use la cuenta de forma controlada y documentada, repartiendo la operación sin que todo pase literalmente por Alejandro | 5 — no involucra al cliente en absoluto |
| C — Aceptar el riesgo | 5 — costo cero | 1 — no reduce nada | 1 — no cambia la dependencia; de hecho la perpetúa | 5 — no involucra al cliente |

**Totales ponderados** (puntaje × peso, sumado):

| Opción | Cálculo | Total |
|---|---|---|
| A | 1×0.35 + 5×0.25 + 3×0.25 + 1×0.15 | **2.50** |
| B | 5×0.35 + 3×0.25 + 4×0.25 + 5×0.15 | **4.25** |
| C | 5×0.35 + 1×0.25 + 1×0.25 + 5×0.15 | **3.00** |

**Sensibilidad:** escenario alternativo priorizando seguridad por encima de costo (Reducción de riesgo 40%, Dividir dependencia 30%, Costo 20%, Relación comercial 10%):

| Escenario de pesos | Total A | Total B | Total C | ¿Gana la misma opción? |
|---|---|---|---|---|
| Pesos del negocio (Costo 35 / Riesgo 25 / Dependencia 25 / Comercial 15) | 2.50 | **4.25** | 3.00 | — |
| Alternativo (Riesgo 40 / Dependencia 30 / Costo 20 / Comercial 10) | 3.20 | **3.90** | 2.20 | Sí — gana B en ambos escenarios |

La Opción B gana de forma robusta incluso cuando se le da mucho más peso a la seguridad que al costo — la diferencia con la Opción C (que también es gratis) está en que B sí aporta a dividir la dependencia de una persona, que es el objetivo estratégico explícito de la gerencia.

---

## Paso 7 — Decisión

- **Decisión:** implementar la Opción B — bitácora interna de acceso compartido (sobre el uso de la cuenta de Jira).
- **Trade-off aceptado:** se acepta no lograr una individualización criptográfica real de la identidad en Jira (la cuenta sigue siendo técnicamente una sola), a cambio de no requerir presupuesto adicional, no depender de una renegociación con el cliente que la gerencia ya descartó como poco viable, y poder implementarse de inmediato.
- **Alternativas descartadas y su razón:** la Opción A (licencias individuales) es, en teoría, la que mejor resuelve el riesgo de seguridad de raíz, pero se descarta en la práctica porque la gerencia ya evaluó que no hay presupuesto disponible ni ganancia comercial clara en insistirle al cliente en este momento — no se descarta por ser mala solución, sino por inviabilidad de negocio confirmada. La Opción C (aceptar el riesgo) se descarta porque, a diferencia de B, no aporta nada al objetivo estratégico de dividir la dependencia de una sola persona.

---

## Paso 8 — Reevaluación

Se vuelve a evaluar esta decisión si cambia alguna de las condiciones que la sustentan: una renovación o ampliación de contrato con Rentec que abra espacio a renegociar licencias de Jira, un incidente de seguridad real que involucre la cuenta de Jira compartida, o un cambio en la política de presupuesto de Asul.

---

## Registro de uso de IA

Claude (Anthropic) se usó como copiloto para estructurar los criterios, calcular los totales ponderados y la sensibilidad, y redactar esta matriz, a partir de la información que el equipo recogió directamente de la gerencia de Asul (sin compartir datos sensibles de clientes).

| Paso | Qué se le pidió a la IA | Qué propuso | Dato verificado o recalculado | Qué cambió el equipo |
|---|---|---|---|---|
| Paso 3 (pesos) | Proponer una distribución de pesos coherente con las dos señales del negocio (sin presupuesto, dividir el riesgo) | Costo 35% / Riesgo 25% / Dependencia 25% / Comercial 15% | El equipo validó que la distribución refleja lo que dijo la gerencia y no una preferencia técnica propia | Se mantuvo la propuesta tal cual, por ser consistente con las respuestas directas del cliente |
| Paso 6 (puntajes y totales) | Calcular los totales ponderados y un escenario de sensibilidad alternativo | Los totales de la tabla y la tabla de sensibilidad | El equipo recalculó manualmente los tres totales ponderados antes de aceptarlos | Ninguno — los cálculos coincidieron |

- [x] Puntué por mi cuenta antes de comparar con la IA (evita el anclaje).
- [x] Verifiqué o recalculé toda cifra y afirmación de la IA que entró a la matriz.
- [x] Los pesos los fijó o validó el negocio (gerencia de Asul), no la IA.

---

_Esta decisión es el borrador de una ADR: en el Taller 9 se formaliza su registro (Contexto, Problema, Decisión, Alternativas, Consecuencias)._
