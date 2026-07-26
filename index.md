---
layout: default
title: "Validación incompleta en una integración"
---

# Validación incompleta en una integración: lecciones de un incidente en producción

## Contexto

Como parte de un equipo encargado del mantenimiento de una plataforma de trámites digitales, participé en la implementación de una nueva validación para uno de los formularios del sistema.

El cambio tenía como objetivo impedir que los usuarios enviaran trámites sin completar un dato obligatorio. Para realizarlo fue necesario coordinar el trabajo entre análisis funcional, desarrollo y control de calidad.

La modificación fue probada correctamente en los casos principales y posteriormente se publicó en producción.

## Problema

Luego de la publicación comenzaron a registrarse errores en algunos trámites enviados mediante una integración externa.

La validación se había implementado solamente en la interfaz utilizada por los usuarios, pero no en el servicio encargado de recibir y guardar la información.

Esto permitía que otros sistemas enviaran registros sin completar el dato obligatorio. Como consecuencia, algunos trámites se almacenaban de manera incompleta y fallaban cuando avanzaban hacia la siguiente etapa del proceso.

Durante el análisis se identificaron los siguientes problemas:

- El requerimiento no aclaraba que la validación debía aplicarse tanto en la interfaz como en el backend.
- El equipo asumió que todos los datos ingresarían desde el formulario web.
- Los casos de prueba solamente contemplaban el recorrido principal.
- No se evaluó el comportamiento de las integraciones externas.
- La comunicación entre análisis, desarrollo y QA fue insuficiente.

## Acciones realizadas

### 1. Análisis del error

Se revisaron los registros del sistema y se realizaron consultas sobre la base de datos para identificar qué trámites estaban fallando.

A partir de esta revisión se comprobó que los registros problemáticos tenían vacío el mismo campo obligatorio y que todos provenían de una integración externa.

### 2. Reproducción del problema

Se reprodujo el inconveniente en un ambiente de prueba enviando una solicitud directamente al servicio sin completar el campo obligatorio.

La prueba confirmó que el backend aceptaba la información aunque el dato no estuviera presente.

### 3. Contención del incidente

Se identificaron los trámites afectados y se evitó que continuaran avanzando hasta corregir la información faltante.

También se notificó a los equipos involucrados para evitar nuevos envíos incorrectos durante la resolución.

### 4. Corrección técnica

El equipo de desarrollo incorporó una validación en el backend para impedir que el servicio aceptara registros incompletos.

Además, se mejoró el mensaje de error para indicar claramente qué dato debía corregirse.

La corrección se realizó en una rama específica y quedó registrada mediante commits y un Pull Request.

### 5. Ampliación de las pruebas

Se agregaron nuevos casos de prueba para verificar:

- Envíos realizados desde la interfaz.
- Solicitudes enviadas mediante integraciones externas.
- Campos obligatorios vacíos.
- Valores inválidos.
- Mensajes de error.
- Comportamiento correcto del sistema después de la corrección.

### 6. Post-mortem constructivo

Después de resolver el incidente se realizó una revisión sin buscar culpables.

La causa raíz fue la interpretación incompleta del requerimiento y la falta de análisis de todos los canales por los cuales podía ingresar la información.

Como acciones preventivas se decidió:

- Documentar explícitamente las validaciones de frontend y backend.
- Incluir las integraciones externas dentro de los criterios de aceptación.
- Revisar los casos de prueba entre análisis, desarrollo y QA.
- Actualizar la documentación técnica del servicio.
- Utilizar una lista de verificación antes de publicar cambios en producción.

## Aprendizajes

Este incidente permitió comprender que una validación aplicada solamente en la interfaz no garantiza la integridad de la información.

También dejó los siguientes aprendizajes:

- Las validaciones importantes deben realizarse en el backend.
- Los requerimientos deben contemplar todos los canales de ingreso de datos.
- Los criterios de aceptación deben ser claros y verificables.
- Las pruebas no deben limitarse al recorrido principal.
- Las integraciones externas deben incluirse en el análisis de impacto.
- La comunicación entre los roles es tan importante como la solución técnica.
- Un post-mortem debe enfocarse en mejorar el proceso y no en encontrar culpables.

## Reflexión sobre feedback radicalmente sincero

Durante el análisis del incidente fue necesario comunicar de manera directa que el requerimiento original era ambiguo, que las pruebas habían sido insuficientes y que el desarrollo había asumido un único canal de ingreso de información.

Este feedback podría haberse interpretado como una crítica personal hacia quienes participaron de la implementación. Sin embargo, se presentó utilizando evidencias obtenidas de los registros, las pruebas realizadas y la documentación disponible.

También fue necesario aceptar que el problema no pertenecía exclusivamente a desarrollo. El análisis funcional debía haber especificado mejor el alcance y QA debía haber incorporado escenarios relacionados con las integraciones.

La sinceridad permitió reconocer las fallas del proceso y generar mejoras concretas. La principal enseñanza fue que señalar un problema con claridad, respeto y evidencia no busca responsabilizar a una persona, sino evitar que el mismo incidente vuelva a ocurrir.
