# 📁 Proyecto ASUL — Arquitectura Empresarial

Este es el repositorio que se mantiene durante todo el semestre para **Asul Tecnologías de la Información SAS**. Cada taller del curso sigue teniendo su propio repositorio (`AREM-Taller_1_BPMN`, `AREM-Taller_2_Modelo_Informacion`, ...) con la Parte 1 (trabajo en clase con el caso base de la Clínica Salud Viva) como registro histórico de esa entrega puntual — eso no se toca. Lo que cambia es dónde vive la Parte 2, "Aplicación al Cliente Real": de aquí en adelante, todos los entregables sobre Asul quedan consolidados en esta carpeta, organizados por fase, en lugar de repartidos entre repos.

## Fases

| Fase | Contenido | Ubicación | Taller de origen |
|---|---|---|---|
| 00 — Preliminary y Architecture Vision | Ficha de caracterización, documento de visión, referencias | [`../entrega/`](../entrega/) (se queda donde está — ya es parte de este mismo repo) | Taller 0 |
| 01 — Business Architecture (BPMN) | Modelo BPMN del proceso de Desarrollo y Soporte, informe técnico, referencias | [`fase-01-business-architecture-bpmn/`](fase-01-business-architecture-bpmn/) | Taller 1 |
| 02 — Datos AS-IS (Modelo de Información) | ERD y diagrama de contexto unificados, informe técnico, referencias | [`fase-02-datos-as-is/`](fase-02-datos-as-is/) | Taller 2 |
| 03 en adelante | Se agregan aquí a medida que avance el semestre (C4/Aplicaciones, Tecnología, Seguridad, Normatividad, TO-BE, ...) | `fase-03-...` | Talleres 3+ |

## Por qué está organizado así

Cada repo de taller sigue siendo la fuente de verdad para su propia calificación (ahí vive la metodología de clase, el caso base y el historial de commits de esa entrega puntual). Pero el proyecto real de Asul es uno solo y avanza de forma acumulativa — el ERD de la Fase 02 usa las mismas entidades que el BPMN de la Fase 01, y ambos parten de los objetivos estratégicos de la Fase 00. Tenerlo todo junto acá permite ver la arquitectura completa del cliente en un solo lugar, sin tener que saltar entre tres repositorios para entender cómo se conecta todo.

---

_Carpeta de consolidación del proyecto aplicado — curso Arquitectura Empresarial, Universidad de La Sabana._
