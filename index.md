
PROYECTO FINAL
Mentalidad de Crecimiento y Comunicación en Entornos Digitales
Alan Contreras

Tema	Blog técnico y post-mortem constructivo
Caso analizado	Error en producción por validación incompleta de datos
Plataforma seleccionada	GitHub Pages

Documento preparado para completar con enlaces y capturas reales del repositorio.
 1. ENLACES DEL PROYECTO
URL pública del blog: [PEGAR AQUÍ EL ENLACE]
Enlace al repositorio: [PEGAR AQUÍ EL ENLACE]
Historial de commits: [PEGAR AQUÍ EL ENLACE]
Pull Request: [PEGAR AQUÍ EL ENLACE]
Importante: los enlaces deben corresponder al repositorio público y a la publicación real en GitHub Pages.
2. EVIDENCIAS FOTOGRÁFICAS
CAPTURA 1: Vista general del repositorio público en GitHub.
REEMPLAZAR ESTE RECUADRO POR LA CAPTURA DE PANTALLA
La evidencia debe mostrar la pantalla completa y ser legible.

CAPTURA 2: Blog técnico publicado y accesible desde la URL pública.
REEMPLAZAR ESTE RECUADRO POR LA CAPTURA DE PANTALLA
La evidencia debe mostrar la pantalla completa y ser legible.

 CAPTURA 3: Historial de commits con mensajes claros y ordenados.
REEMPLAZAR ESTE RECUADRO POR LA CAPTURA DE PANTALLA
La evidencia debe mostrar la pantalla completa y ser legible.

CAPTURA 4: Pull Request utilizado para revisar e integrar los cambios.
REEMPLAZAR ESTE RECUADRO POR LA CAPTURA DE PANTALLA
La evidencia debe mostrar la pantalla completa y ser legible.

 CAPTURA 5: Configuración de GitHub Pages o confirmación de la publicación.
REEMPLAZAR ESTE RECUADRO POR LA CAPTURA DE PANTALLA
La evidencia debe mostrar la pantalla completa y ser legible.

3. ENTRADA DEL BLOG TÉCNICO
Validación incompleta en una integración: lecciones de un incidente en producción
Contexto
En mi trabajo como analista funcional dentro de un equipo encargado del mantenimiento de una plataforma de trámites digitales, participé en la implementación de una nueva validación para uno de los formularios del sistema. El cambio tenía como objetivo evitar que los usuarios enviaran trámites sin completar un dato obligatorio que luego era utilizado por otros procesos internos.
La modificación involucró a diferentes roles: análisis funcional definió el requerimiento, desarrollo implementó el cambio y QA verificó su funcionamiento antes de la publicación. Las pruebas realizadas sobre el formulario principal fueron satisfactorias y la versión fue desplegada en producción.
Problema
Poco tiempo después de la publicación comenzaron a registrarse fallas en algunos trámites. Los casos afectados quedaban detenidos cuando intentaban avanzar hacia la siguiente etapa del proceso porque el dato obligatorio se encontraba vacío.
La investigación mostró que la validación había sido aplicada únicamente en la interfaz web. Sin embargo, la plataforma también recibía información desde una integración externa que consumía el servicio directamente. Como el backend no validaba el campo, el servicio aceptaba registros incompletos aunque el formulario web impidiera ese comportamiento.
El incidente no se originó por una única acción individual, sino por varias debilidades del proceso:
•	El requerimiento no especificaba que la validación debía implementarse tanto en la interfaz como en el backend.
•	El equipo asumió que todos los datos ingresarían desde el formulario web.
•	Los casos de prueba se concentraron en el recorrido principal y no contemplaron la integración externa.
•	No se realizó una revisión conjunta del impacto entre análisis, desarrollo y QA.
•	La documentación del servicio no detallaba suficientemente los campos obligatorios.
Acciones realizadas
Para resolver el incidente y evitar que se repitiera, se aplicó un plan de trabajo dividido en acciones de diagnóstico, contención, corrección y prevención.
1. Análisis del incidente
Se revisaron los registros de errores del sistema y se realizaron consultas sobre la base de datos para identificar un patrón común. Todos los trámites afectados presentaban vacío el mismo campo y habían ingresado mediante la integración externa.
2. Reproducción del problema
El equipo reprodujo el inconveniente en un ambiente de prueba enviando una solicitud directamente al servicio sin completar el dato obligatorio. La prueba confirmó que el backend aceptaba la información y permitía guardar un registro inválido.
3. Contención
Se identificaron los trámites afectados y se evitó que continuaran avanzando hasta completar o corregir la información faltante. También se informó temporalmente a los equipos consumidores del servicio para reducir la generación de nuevos casos.
4. Corrección técnica
Desarrollo incorporó la validación en el backend para rechazar cualquier solicitud que no incluyera el campo obligatorio. Además, se agregó un mensaje de error claro para indicar qué dato debía ser corregido. La solución se implementó en una rama específica y se revisó mediante un Pull Request.
5. Ampliación de pruebas
Se incorporaron nuevos escenarios para comprobar:
•	Envíos realizados desde el formulario web.
•	Solicitudes enviadas directamente mediante la integración externa.
•	Campos obligatorios vacíos o ausentes.
•	Valores con formatos inválidos.
•	Mensajes de error comprensibles.
•	Funcionamiento correcto del proceso después de la corrección.
6. Post-mortem constructivo
Luego de resolver el incidente se realizó una revisión enfocada en comprender las causas y mejorar el proceso, sin buscar culpables. La causa raíz fue una definición incompleta del alcance de la validación, acompañada por supuestos no confirmados y una cobertura de pruebas limitada.
Como medidas preventivas se acordó:
•	Especificar en los requerimientos si una validación debe aplicarse en frontend, backend o ambos.
•	Incluir todas las fuentes de ingreso de datos dentro del análisis de impacto.
•	Revisar los criterios de aceptación entre análisis, desarrollo y QA antes de comenzar el desarrollo.
•	Agregar casos de integración y escenarios alternativos a los planes de prueba.
•	Actualizar la documentación técnica del servicio.
•	Utilizar una lista de verificación antes de cada publicación.
Control de versiones
El proyecto fue documentado en un repositorio público para mantener trazabilidad sobre la creación del blog y las mejoras aplicadas. Los mensajes de commit propuestos son:
docs: creación inicial de la entrada técnica
docs: descripción del incidente y análisis de causa raíz
docs: incorporación de acciones correctivas y preventivas
docs: agregado de evidencia y reflexión sobre feedback
docs: revisión final y publicación en GitHub Pages
También se creó una rama de trabajo y un Pull Request para revisar la claridad del contenido antes de integrarlo a la rama principal. Esta práctica permitió evidenciar la evolución de la documentación y aplicar el feedback de manera ordenada.
Aprendizajes
•	Una validación en la interfaz mejora la experiencia del usuario, pero no garantiza la integridad de los datos.
•	Las reglas críticas deben validarse en el backend, porque los servicios pueden ser consumidos por múltiples canales.
•	Los requerimientos deben indicar claramente el alcance técnico y los puntos de integración afectados.
•	Las pruebas deben contemplar recorridos principales, escenarios alternativos e integraciones externas.
•	La comunicación entre análisis, desarrollo y QA es parte de la calidad del producto.
•	Un post-mortem efectivo debe transformar un error en acciones preventivas concretas.
•	Registrar cambios mediante commits y Pull Requests facilita la trazabilidad y el aprendizaje colectivo.
Reflexión sobre feedback radicalmente sincero
Durante el análisis fue necesario comunicar de manera directa que el requerimiento era ambiguo, que las pruebas no habían contemplado todos los canales de ingreso y que el equipo había trabajado sobre un supuesto que nunca fue validado.
Este feedback podía interpretarse como una crítica personal hacia quienes participaron de la implementación. Para evitarlo, la conversación se apoyó en evidencias: registros del sistema, consultas a la base de datos, reproducción del error y documentación disponible. El foco se mantuvo en el proceso y en las decisiones, no en las personas.
También fue necesario aceptar responsabilidades compartidas. Desde análisis funcional se debía haber especificado mejor el alcance; desarrollo debía haber confirmado dónde aplicar la regla; y QA debía haber incluido escenarios de integración. Reconocer estas oportunidades de mejora permitió construir una solución más completa.
La experiencia demostró que el feedback radicalmente sincero no consiste en expresarse de manera agresiva, sino en señalar los problemas con claridad, respeto y evidencia. La sinceridad fue útil porque terminó convirtiéndose en cambios concretos que reducen la posibilidad de repetir el incidente.
4. REFLEXIÓN FINAL
La resolución de este incidente confirmó que los problemas técnicos rara vez dependen de una sola persona. En este caso, la falla apareció por una combinación de requerimientos incompletos, supuestos no validados, pruebas limitadas y comunicación insuficiente entre los roles.
Aplicar una mentalidad de crecimiento permitió dejar de buscar responsables y concentrarse en comprender qué debía modificarse. El feedback directo ayudó a reconocer las debilidades del proceso y a convertirlas en acciones verificables: validaciones en backend, criterios de aceptación más precisos, pruebas de integración y una revisión conjunta antes de publicar.
El principal aprendizaje fue que la calidad no depende únicamente del código. También depende de cómo se comunica el alcance, cómo se revisan los supuestos y cómo se registra el conocimiento generado después de un incidente. La documentación, el control de versiones y el feedback sincero permiten que el error no quede como una experiencia aislada, sino que se transforme en una mejora para todo el equipo.
5. CHECKLIST DE ENTREGA
☒ Entrada de blog creada y estructurada según la plantilla.
☒ Problema real del rubro informático documentado.
☒ Contexto, problema, acciones y aprendizajes incluidos.
☒ Post-mortem constructivo desarrollado.
☒ Reflexión sobre feedback radicalmente sincero incluida.
☐ Repositorio público creado.
☐ Commits realizados y visibles.
☐ Pull Request creado.
☐ GitHub Pages configurado.
☐ URL pública verificada.
☐ Capturas reales insertadas en este documento.

Antes de entregar: reemplazá los cinco recuadros por capturas reales, completá los enlaces y marcá como realizados los puntos pendientes del checklist. No se deben utilizar imágenes simuladas como evidencia.
