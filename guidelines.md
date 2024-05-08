---
source-git-commit: 2d90738d01fef6e37a2c25784ed4d1338c037c23
workflow-type: ht
source-wordcount: '715'
ht-degree: 100%

---
# Directrices para contribuir a la documentación de Adobe Experience Manager

## Filosofía de la documentación

Los usuarios de Adobe Experience Manager están trabajando en entornos altamente competitivos, esforzándose por crear experiencias digitales que los diferencien de su competencia. Por lo tanto, cuando Adobe ofrece nuevas herramientas avanzadas en AEM, estas herramientas se complementan con documentación precisa y clara para permitir a los clientes utilizar inmediatamente su inversión en AEM y maximizar su retorno de la inversión.

El objetivo de la documentación de AEM es poner la información en manos de los usuarios de AEM lo antes posible. Por lo tanto, Adobe concede prioridad a una documentación precisa y útil y nos esforzamos por actualizarla y mejorarla continuamente.

## Contribuciones a la documentación

Para mejorar continuamente la documentación de AEM, contamos con la ayuda de toda la comunidad de usuarios de AEM. Ya sea a través de solicitudes de extracción o incidencias, las mejoras en la documentación pueden ser correcciones, aclaraciones, expansiones y ejemplos adicionales.

## Normas de documentación

Aunque Adobe agradece las contribuciones a su documentación, toda contribución que se haga a la documentación de AEM, ya sea en forma de solicitud de extracción o de incidencia, debe ajustarse a las normas de contribución y documentación de Adobe.

Las contribuciones que no cumplan estas normas se rechazarán.

### Adobe registra los casos de uso estándar.

La documentación de AEM abarca casos de uso estándar. Los casos de uso que exceden el ámbito de la instalación estándar y el uso del producto no forman parte de la documentación de AEM.

### Generalmente, Adobe no registra los errores ni sus soluciones alternativas.

La documentación de AEM abarca casos de uso estándar. Por este motivo, los errores, los efectos causados por errores y las soluciones alternativas para los errores no suelen registrarse.

Las excepciones a esta regla se aplican a las notas de la versión, donde los problemas conocidos pueden enumerarse con posibles soluciones aprobadas por el equipo de administración del producto de AEM.

### Las contribuciones a la documentación no sirven para responder preguntas técnicas.

Cualquier idea que tenga para mejorar la documentación de AEM es bienvenida como contribución. Sin embargo, los comentarios, las incidencias y las solicitudes de extracción están destinadas únicamente a las *contribuciones*. No están pensados para responder a sus preguntas sobre cómo utilizar AEM, implementar su proyecto de AEM o resolver problemas técnicos.

Puede informar cualquier pregunta sobre el uso de AEM o sobre errores técnicos mediante el proceso de soporte normal a través del [Portal de soporte empresarial de Experience Cloud](https://experienceleague.adobe.com/?support-solution=General#support) o lo discutido en la [Comunidad de administradores de experiencia](https://experienceleaguecommunities.adobe.com/t5/adobe-experience-manager/ct-p/adobe-experience-manager-community).

***Las contribuciones a la documentación de AEM no sustituyen al equipo de asistencia de Adobe*** y se rechaza cualquier contribución de este tipo que busque respuestas a preguntas relacionadas con la asistencia.

### Las contribuciones deben hacer referencia claramente a las páginas de documentación afectadas.

Si crea un problema para sugerir mejoras en la documentación, debe incluir vínculos a las páginas afectadas. Si crea un problema utilizando el vínculo **Editar esta página** en una página de documentación, el problema se crea automáticamente con un vínculo a la página.

Esto no se aplica a las solicitudes de extracción, ya que por su naturaleza hacen referencia a las páginas afectadas.

## Directrices de documentación

Adobe pide que cualquier contribución a su documentación siga ciertas pautas de estilo.

Seguir estas directrices facilita la revisión de su contribución y, por lo tanto, la integración en la documentación de Adobe es más rápida.

### Idioma y estilo

#### Idioma

* La documentación de AEM se redacta y se actualiza en inglés estadounidense (originalmente).
* Escriba frases lo más simples posibles.
* Utilice un lenguaje claro y conciso.

Recuerde que los lectores de la documentación de AEM están establecidos en todo el mundo, no se puede esperar que sean hablantes nativos o fluidos del inglés. Evite los coloquialismos, utilice un lenguaje claro y simple como sea posible.

#### Siga el Manual de estilo de Microsoft®

©El [Manual de estilo de Microsoft®](https://learn.microsoft.com/es_es/style-guide/welcome/) es una guía de estilo de documentación disponible libremente que se centra en documentación de software. La documentación AEM debe seguir esta guía siempre que sea posible.

### Formato

| Elemento | Estilo |
|---|---|
| Elemento u opción de la interfaz de usuario | **negrita** |
| Nombre de archivo, ruta, entrada de usuario, valores de parámetro | `monospaced` |
| Código, línea de comandos | ```Code Block``` |

### Capturas de pantalla

Las capturas de pantalla deben utilizarse con prudencia y solo cuando la descripción textual no sea suficiente.

No se deben utilizar marcadores u otras anotaciones en las capturas de pantalla (como marcos rojos, flechas o texto). De este modo, las capturas de pantalla son más fáciles de reutilizar o replicar en versiones localizadas de la documentación.

### Referencias específicas de la versión

Intente evitar cualquier referencia directa a una versión específica en todo el contenido de la documentación, siempre que sea posible. Esto hace que la documentación sea más flexible y extensible para futuras versiones.

### Uso de Día, AEM, CQ, CRX

El producto siempre se debe mencionar por su nombre completo, **Adobe Experience Manager**, por primera vez en un artículo y, después, se pueden usar sus siglas: **AEM**.

No se deben utilizar las palabras Día, Software de día, CQ y CRX, excepto cuando sea inevitable, como en nombres de clase o en referencia al historial de AEM.