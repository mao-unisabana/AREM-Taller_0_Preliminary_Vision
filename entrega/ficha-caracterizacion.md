# 📄 Ficha de Caracterización del Cliente

_Misma estructura que la plantilla oficial en Word (`Material/Proyecto/Ficha de Caracterización del Cliente.docx`) — versión de equipo trabajada desde el repositorio de GitHub._

**Nombre del Equipo:** Nexio SAS
**Fecha:** 17 de agosto de 2026
**Nombre del Cliente:** Luz Miryan Barrera Diaz
**Rol/Organización:** Gerente Comercial — Asul Tecnologías de la Información SAS

## I. Información General del Negocio
- Nombre de la empresa o entidad: Asul Tecnologías de la Información SAS
- Sector económico: Desarrollo de software a la medida y servicios de soporte de TI
- Número de empleados / usuarios / clientes: Menos de 10 empleados. Clientes destacados: Rentec, Compucom, Aqua, Avidanti, Tecnalia Colombia, Keypport, SoftManagement y Fisla
- Ubicación principal (física o digital): Chía, Cundinamarca, Colombia
- Tecnologías principales actuales:
  - Desarrollo en .NET / .NET Core (Visual Studio) con arquitectura orientada a servicios (SOA); control de versiones con Git/GitHub
  - Gestión de proyectos, épicas, historias de usuario y test plans en Azure DevOps, con transcripción manual de casos de soporte hacia/desde el Jira del cliente
  - Repositorio documental de procesos en SharePoint; partners de Microsoft (365, Azure, Copilot); marcos de calidad de referencia CMMI, MPS.Br, ITMark e ISO 29110

## II. Objetivos Estratégicos
1. Mantener la viabilidad económica de la empresa.
2. Incrementar la satisfacción y fidelización de los clientes mediante el cumplimiento de altos estándares de eficiencia y calidad.
3. Fortalecer la competitividad del producto con atributos de seguridad, estabilidad y confiabilidad.
4. Brindar condiciones de trabajo que favorezcan el desarrollo profesional y el buen ambiente laboral del equipo.

## III. Problemas o necesidades identificadas
> Descritos como los vive el cliente (el síntoma), no como una solución técnica ya decidida.

- Problema #1: Los procesos de estimación no son efectivos; aunque existe una plantilla de estimación, en la práctica es muy difícil de cumplir, lo que genera desviaciones frente a lo aprobado durante los sprints y dificulta el seguimiento interno con el cliente, así como la medición y análisis de cada proyecto.
- Problema #2: Falta de automatización — por ejemplo en la gestión de requisitos y casos de prueba — y duplicidad de trabajo en el soporte: una vez aprobados los requisitos no existen herramientas que agilicen su gestión, y los casos registrados en Azure DevOps deben transcribirse manualmente campo a campo hacia y desde Jira, generando reprocesos y riesgo de error.
- Problema #3: La calidad del código se ve afectada por errores de los desarrolladores originados en supuestos no validados ("yo me imaginé que era así") en lugar de resolver dudas durante el proyecto; esto desgasta al equipo, es evaluado negativamente por el cliente, y evidencia la necesidad de mejorar la inspección del trabajo entregado y de ajustar la plantilla de bonificaciones, que actualmente no incentiva de forma adecuada la resolución de bugs.

## IV. Procesos clave del negocio
> Esta lista alimenta directamente el Taller 1 (BPMN): de aquí sale el proceso que el equipo modeló.

- Gestión de proyectos (proceso operacional): desarrollo de software a la medida bajo metodología Scrum, desde el levantamiento y firma de requisitos (historias de usuario refinadas con mockups) hasta la estimación, planeación, diseño, codificación, pruebas, entrega por sprint y control de cambios.
- Soporte y mantenimiento (proceso operacional): atención de tickets del cliente con bolsas de horas aprobadas; los casos se gestionan en Azure DevOps pero llegan desde el Jira del cliente, obligando a transcribir campo a campo entre ambas herramientas.
- Gestión estratégica y de apoyo (comercial, talento humano, financiera, seguridad de la información, administrativa y de calidad): planeación estratégica con objetivos e indicadores trimestrales, gestión comercial por referidos ("voz a voz"), consultorías e interventorías puntuales, todo documentado en un repositorio SharePoint alineado a la certificación ITMark.

## V. Expectativas frente a la solución
- Que la solución propuesta mantenga tiempos de respuesta rápidos y una relación costo-beneficio favorable, en línea con su visión estratégica.
- Que se preserven sus estándares de cumplimiento y calidad (ITMark, Microsoft Partner, CMMI, MPS.Br, ISO 29110) y sus valores corporativos (Honestidad, Respeto, Credibilidad y Autoexigencia).

**Restricciones:** No se puede reemplazar Azure DevOps ni las herramientas del ecosistema Microsoft (partner Microsoft, certificaciones ITMark/CMMI construidas sobre ese stack); la integración Jira–Azure DevOps no puede resolverse con el conector pago porque la licencia correspondiente no está disponible actualmente.

## VI. Persona de contacto
- Nombre del contacto: Luz Miryan Barrera Diaz
- Correo electrónico / teléfono: luz.barrera@asul-ti.com / 3006160200
- Rol o vínculo con la solución: Gerente Administrativa

---

_Este documento hace parte de la entrega del Taller 0 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
