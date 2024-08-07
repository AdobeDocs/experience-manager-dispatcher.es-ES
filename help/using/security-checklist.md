---
title: Lista de comprobación de seguridad de Dispatcher
description: Obtenga información acerca de la lista de comprobación de seguridad de Dispatcher que debe completarse antes de continuar con la producción.
contentOwner: User
products: SG_EXPERIENCEMANAGER/DISPATCHER
topic-tags: dispatcher
content-type: reference
jcr-lastmodifiedby: remove-legacypath-6-1
index: y
internal: n
snippet: y
exl-id: 49009810-b5bf-41fd-b544-19dd0c06b013
source-git-commit: 0a1aa854ea286a30c3527be8fc7c0998726a663f
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 100%

---

# Lista de comprobación de seguridad de Dispatcher {#the-dispatcher-security-checklist}

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (ims-author-00AF43764F54BE740A490D44@AdobeID)
Last Modified Date: 2015-06-05T05:14:35.365-0400

<p>Food for thought listed on <a href="https://jira.corp.adobe.com/browse/DOC-5649">DOC-5649</a>. To be considered while proof-reading.</p> 
<p> </p>

 -->

Adobe recomienda completar la siguiente lista de comprobación antes de continuar con la producción.

>[!CAUTION]
>
>Complete la lista de comprobación de seguridad de su versión de AEM antes de empezar. Consulte la correspondiente [Documentación de Adobe Experience Manager](https://experienceleague.adobe.com/es_es/docs/experience-manager-65/content/security/security-checklist).

## Utilice la última versión de Dispatcher {#use-the-latest-version-of-dispatcher}

Instale la versión más reciente disponible para su plataforma. Actualice la instancia de Dispatcher para utilizar la última versión y aprovechar las mejoras del producto y la seguridad. Consulte [Instalación de Dispatcher](dispatcher-install.md).

>[!NOTE]
>
>Puede comprobar la versión actual de la instalación de Dispatcher mirando el archivo de registro de Dispatcher.
>
>`[Thu Apr 30 17:30:49 2015] [I] [23171(140735307338496)] Dispatcher initialized (build 4.1.9)`
>
>Para encontrar el archivo de registro, revise la configuración de Dispatcher en su `httpd.conf`.

## Restrinja clientes que puedan vaciar la caché {#restrict-clients-that-can-flush-your-cache}

Adobe recomienda que [limite los clientes que pueden vaciar la caché.](dispatcher-configuration.md#limiting-the-clients-that-can-flush-the-cache)

## Habilite HTTPS para la seguridad de la capa de transporte {#enable-https-for-transport-layer-security}

Adobe recomienda habilitar la capa de transporte HTTPS en las instancias de autor y publicación.

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (ims-author-00AF43764F54BE740A490D44@AdobeID)
Last Modified Date: 2015-06-26T04:41:28.841-0400

<p>Recommended to have SSL termination, front end SSL.</p> 
<p>Question is do we want to have SSL communication between dispatcher and AEM instances (publish and/or author).</p> 
<p>We might want to have two items:</p> 
<ul> 
 <li>MUST HTTPS clients -&gt; dispatcher / load balancer</li> 
 <li>NICE load balancer -&gt; dispatcher<br /> </li> 
 <li>NICE dispatcher -&gt; instances if sensitive information such as credit cards / or infrastructure requirements such as DMZ</li> 
</ul>

 -->

## Restrinja el acceso {#restrict-access}

Al configurar Dispatcher, restrinja el acceso externo en la medida de lo posible. Consulte [Ejemplo de sección /filter](dispatcher-configuration.md#main-pars_184_1_title) en la documentación de Dispatcher.

## Asegúrese de que se deniegue el acceso a las direcciones URL administrativas {#make-sure-access-to-administrative-urls-is-denied}

Asegúrese de utilizar filtros para bloquear el acceso externo a cualquier dirección URL administrativa, como la consola web.

Consulte la [Prueba de seguridad de Dispatcher](dispatcher-configuration.md#testing-dispatcher-security) para obtener una lista de las direcciones URL que deben bloquearse.

## Utilice Listas de permitidos en lugar de Listas de bloqueados {#use-allowlists-instead-of-blocklists}

Las listas de permitidos son una mejor manera de controlar el acceso, ya que suponen inherentemente que todas las solicitudes de acceso deben denegarse a menos que formen parte explícita de la lista de permitidos. Este modelo proporciona un control más restrictivo sobre las nuevas solicitudes que pueden no haberse revisado aún o tenido en cuenta durante una determinada fase de configuración.

## Ejecute Dispatcher con un usuario del sistema dedicado {#run-dispatcher-with-a-dedicated-system-user}

Al configurar Dispatcher, debe asegurarse de que al servidor web lo ejecute un usuario específico con menos privilegios. Se recomienda conceder el acceso de escritura únicamente a la carpeta de caché de Dispatcher.

Además, los usuarios de IIS deben configurar su sitio web de la siguiente manera:

1. En la configuración de ruta física del sitio web, seleccione **Conectar como usuario específico**.
1. Configure el usuario.

## Evite ataques de denegación de servicio (DoS) {#prevent-denial-of-service-dos-attacks}

Un ataque de denegación de servicio (DoS) es un intento de hacer que un recurso de equipo no esté disponible para los usuarios a los que va destinado.

En el nivel de Dispatcher, existen dos métodos de configuración para evitar ataques DoS: [Filtros](https://experienceleague.adobe.com/es/docs#/filter)

* Utilice el módulo mod_rewrite (por ejemplo, [Apache 2.4](https://httpd.apache.org/docs/2.4/mod/mod_rewrite.html)) para validar la URL (si las reglas de patrones de la URL no son demasiado complejas).

* Impida que Dispatcher almacene en caché las direcciones URL con extensiones falsas mediante [filtros](dispatcher-configuration.md#configuring-access-to-content-filter).\
  Por ejemplo, cambie las reglas de almacenamiento en caché para limitar el almacenamiento en caché a los tipos de MIME esperados:

   * `.html`
   * `.jpg`
   * `.gif`
   * `.swf`
   * `.js`
   * `.doc`
   * `.pdf`
   * `.ppt`

  Se puede ver un ejemplo de archivo de configuración para [restringir el acceso externo](#restrict-access). Incluye restricciones para los tipos MIME.

Para habilitar la funcionalidad completa en las instancias de publicación, configure filtros para evitar el acceso a los siguientes nodos:

* `/etc/`
* `/libs/`

A continuación, configure filtros para permitir el acceso a las siguientes rutas de nodos:

* `/etc/designs/*`
* `/etc/clientlibs/*`
* `/etc/segmentation.segment.js`
* `/libs/cq/personalization/components/clickstreamcloud/content/config.json`
* `/libs/wcm/stats/tracker.js`
* `/libs/cq/personalization/*` (JS, CSS y JSON)
* `/libs/cq/security/userinfo.json` (Información de usuario de CQ)
* `/libs/granite/security/currentuser.json` (**los datos no deben almacenarse en caché**)

* `/libs/cq/i18n/*` (Internalización)

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (ims-author-00AF43764F54BE740A490D44@AdobeID)
Last Modified Date: 2015-06-26T04:38:17.016-0400

<p>We need to highlight whether a path applies to all versions or specific ones.<br /> </p>

 -->

## Configure Dispatcher para prevenir ataques de tipo CSRF {#configure-dispatcher-to-prevent-csrf-attacks}

AEM ofrece un [marco de trabajo](https://experienceleague.adobe.com/es_es/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions#verification-steps) para evitar los ataques de falsificación de solicitudes entre sitios. Para hacer un uso adecuado de este marco de trabajo, admita el token CSRF de la lista de permitidos en Dispatcher haciendo lo siguiente:

1. Crear un filtro para permitir la ruta `/libs/granite/csrf/token.json`;
1. Agregue el encabezado `CSRF-Token` a la sección `clientheaders` de la configuración de Dispatcher.

## Impedir el clickjacking {#prevent-clickjacking}

Para evitar los ataques de clickjacking, Adobe recomienda que configure su servidor web para que proporcione el encabezado `X-FRAME-OPTIONS`HTTP establecido en `SAMEORIGIN`.

Para obtener más información sobre los ataques de clickjacking, consulte el [sitio de OWASP](https://owasp.org/www-community/attacks/Clickjacking).

## Realizar una prueba de penetración {#perform-a-penetration-test}

Adobe recomienda encarecidamente realizar una prueba de penetración de su infraestructura de AEM antes de continuar con la producción.

