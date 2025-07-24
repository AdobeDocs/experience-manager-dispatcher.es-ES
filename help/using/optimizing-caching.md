---
title: Optimizar un sitio web para el rendimiento de la caché
description: Aprenda a diseñar un sitio web para maximizar las ventajas del almacenamiento en caché.
contentOwner: User
products: SG_EXPERIENCEMANAGER/DISPATCHER
topic-tags: dispatcher
content-type: reference
redirecttarget: https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/configuring-performance.html
index: y
internal: n
snippet: y
source-git-commit: c41b4026a64f9c90318e12de5397eb4c116056d9
workflow-type: tm+mt
source-wordcount: '1128'
ht-degree: 96%

---


# Optimizar un sitio web para el rendimiento de la caché {#optimizing-a-website-for-cache-performance}

<!-- 

Comment Type: remark
Last Modified By: Silviu Raiman (raiman)
Last Modified Date: 2017-10-25T04:13:34.919-0400

<p>This is a redirect to /experience-manager/6-2/sites/deploying/using/configuring-performance.html</p>

 -->

>[!NOTE]
>
>Las versiones de Dispatcher son independientes de AEM. Es posible que se le haya redirigido a esta página si ha seguido un vínculo a la documentación Dispatcher. Ese vínculo estaba incrustado en la documentación de una versión anterior de AEM.

Dispatcher ofrece una serie de mecanismos integrados que puede utilizar para optimizar el rendimiento. Esta sección le explica cómo diseñar su sitio web para maximizar los beneficios del almacenamiento en caché.

>[!NOTE]
>
>Puede ayudarle a recordar que Dispatcher almacena la caché en un servidor web estándar. Esto significa que:
>
>* Se puede almacenar en caché todo lo que se quiera almacenar como página y solicitar mediante una dirección URL
>* No se pueden almacenar otros elementos, como encabezados HTTP, cookies, datos de sesión y datos de formularios.
>
>En general, muchas estrategias de almacenamiento en caché implican la selección de buenas direcciones URL y no depender de estos datos adicionales.

## Utilizar una codificación de página coherente {#using-consistent-page-encoding}

Los encabezados de petición HTTP no se almacenan en caché, por lo que pueden producirse problemas si almacena información de codificación de páginas en el encabezado. En este caso, cuando Dispatcher sirve una página desde la caché, se utiliza la codificación predeterminada del servidor web para la página. Existen dos formas de evitar este problema:

* Si solo utiliza una codificación, asegúrese de que la utilizada en el servidor web sea la misma que la predeterminada del sitio web de AEM.
* Utilice una etiqueta `<META>` en la sección HTML `head` para configurar la codificación, como en el siguiente ejemplo:

```xml
        <META http-equiv="Content-Type" content="text/html; charset=EUC-JP">
```

## Evitar parámetros de URL {#avoid-url-parameters}

Si es posible, evite los parámetros de URL para las páginas que desee almacenar en caché. Por ejemplo, si tiene una galería de imágenes, la siguiente URL nunca se almacena en la caché (a menos que Dispatcher esté [configurado como corresponde](dispatcher-configuration.md#main-pars_title_24)):

```xml
www.myCompany.com/pictures/gallery.html?event=christmas&amp;page=1
```

Sin embargo, puede colocar estos parámetros en la dirección URL de la página de la siguiente manera:

```xml
www.myCompany.com/pictures/gallery.christmas.1.html
```

>[!NOTE]
>
>Esta URL llama a la misma página y a la misma plantilla que gallery.html. En la definición de la plantilla, puede especificar qué secuencia de comandos procesa la página o puede utilizar la misma secuencia de comandos para todas las páginas.

## Personalizar por dirección URL {#customize-by-url}

Si permite a los usuarios cambiar el tamaño de la fuente (o cualquier otra personalización del diseño), asegúrese de que las diferentes personalizaciones se reflejen en la dirección URL.

Por ejemplo, las cookies no se almacenan en caché, por lo que si almacena el tamaño de la fuente en una cookie (o mecanismo similar), no se conservará para la página en caché. Como resultado, Dispatcher devuelve aleatoriamente documentos con cualquier tamaño de la fuente.

Incluir el tamaño de la fuente en la URL como selector evita este problema:

```xml
www.myCompany.com/news/main.large.html
```

>[!NOTE]
>
>Para la mayoría de los aspectos de diseño, también es posible utilizar hojas de estilo, secuencias de comandos del lado del cliente o ambas cosas. Cualquiera de los dos funciona bien con el almacenamiento en caché.
>
>Este método también es útil en la versión impresa, donde se puede usar una URL como la siguiente:
>
>`www.myCompany.com/news/main.print.html`
>
>Utilizando el script globbing de la definición de la plantilla, puede especificar un script separado que procese las páginas imprimidas.

## Invalidar archivos de imagen utilizados como títulos {#invalidating-image-files-used-as-titles}

Si procesa títulos de páginas u otros textos como imágenes, se recomienda almacenar los archivos para que se eliminen tras actualizar el contenido de la página:

1. Coloque el archivo de imagen en la misma carpeta que la página.
1. Utilice el siguiente formato de nomenclatura para el archivo de imagen:

   `<page file name>.<image file name>`

Por ejemplo, puede almacenar el título de la página myPage.html en el archivo myPage.title.gif. Este archivo se eliminará automáticamente si se actualiza la página, por lo que cualquier cambio en el título de la página se reflejará automáticamente en la caché.

>[!NOTE]
>
>El archivo de imagen no existe necesariamente en la instancia de AEM. Puede utilizar un script que cree dinámicamente el archivo de imagen. A continuación, Dispatcher almacenará el archivo en el servidor web.

## Invalidar archivos de imagen utilizados para la navegación {#invalidating-image-files-used-for-navigation}

Si utiliza imágenes para las entradas de navegación, el método es básicamente el mismo que con los títulos, aunque un poco más complejo. Almacene todas las imágenes de navegación con las páginas de destino. Si utiliza dos imágenes para la normal y la activa, puede utilizar los siguientes scripts:

* Scripts que muestra la página normal.
* Un script que procesa las solicitudes `.normal` y devuelve la imagen normal.
* Un script que procesa las solicitudes `.active` y devuelve la imagen activa.

Es importante crear estas imágenes con el mismo nombre que la página, para garantizar que una actualización de contenido elimine estas imágenes, así como la página.

En las páginas que no se modifiquen, las imágenes permanecerán en la caché, aunque las páginas en sí se invalidan automáticamente.

## Personalización {#personalization}

Dispatcher no puede almacenar en caché los datos personalizados, por lo que se recomienda limitar la personalización a donde sea necesario. Para explicar por qué:

* Si utiliza una página de inicio personalizable libremente, esa página deberá estar compuesta cada vez que un usuario la solicite.
* Si, por el contrario, ofrece una opción de 10 páginas de inicio diferentes, puede almacenar en caché cada una de ellas, mejorando así el rendimiento.

>[!NOTE]
>
>Si personaliza cada página (por ejemplo, colocando el nombre del usuario en la barra de título), no puede almacenarla en caché, lo que afecta de manera importante al rendimiento.
>
>Sin embargo, si es necesario, puede hacer lo siguiente:
>
>* utilizar iFrames para dividir la página en una parte que sea la misma para todos los usuarios y otra que sea la misma para todas las páginas del usuario. A continuación, puede almacenar en caché ambas partes.
>* utilice JavaScript del lado del cliente para mostrar información personalizada. Sin embargo, debe asegurarse de que la página se muestre correctamente si un usuario desactiva JavaScript.
>

## Conexiones fijas {#sticky-connections}

[Las conexiones fijas](dispatcher.md#TheBenefitsofLoadBalancing) garantizan que todos los documentos de un usuario se compongan en el mismo servidor. Si un usuario abandona esta carpeta y más tarde vuelve a ella, la conexión se mantiene. Defina una carpeta para guardar todos los documentos que requieran conexiones fijas para el sitio web. Intente no meter otros documentos en ella. Esto afectará al equilibrio de cargas si utiliza páginas personalizadas y datos de sesión.

## Tipos MIME {#mime-types}

Existen dos maneras en las que un explorador puede determinar el tipo de archivo:

1. Por su extensión (por ejemplo, .HTML, .GIF y .JPG)
1. Por el tipo MIME que el servidor envía con el archivo.

Para la mayoría de los archivos, el tipo MIME está implícito en la extensión del archivo:

1. Por su extensión (por ejemplo, .HTML, .GIF y .JPG)
1. Por el tipo MIME que el servidor envía con el archivo.

Si el nombre del archivo no tiene extensión, se mostrará como texto sin formato.

El tipo MIME forma parte del encabezado HTTP y, como tal, Dispatcher no lo almacena en caché. Su aplicación de AEM puede devolver archivos que no tengan una extensión de archivo reconocida. Si los archivos dependen del tipo MIME, es posible que se muestren incorrectamente.

Para asegurarse de que los archivos se almacenan en caché correctamente, siga estas directrices:

* Asegúrese de que los archivos siempre tengan la extensión adecuada.
* Evite los scripts genéricos del servidor de archivos, que tienen direcciones URL como download.jsp?file=2214. Vuelva a escribir el script de modo que utilice direcciones URL que contengan la especificación del archivo. En el ejemplo anterior, sería `download.2214.pdf`.

