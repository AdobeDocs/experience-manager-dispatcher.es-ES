---
title: Configurar Dispatcher para prevenir ataques de tipo CSRF
description: Aprenda a configurar Dispatcher de AEM para evitar ataques de falsificación de solicitud entre sitios.
topic-tags: dispatcher
content-type: reference
exl-id: bcd38878-f977-46a6-b01a-03e4d90aef01
source-git-commit: 2d90738d01fef6e37a2c25784ed4d1338c037c23
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 46%

---

# Configurar Dispatcher para prevenir ataques de tipo CSRF {#configuring-dispatcher-to-prevent-csrf-attacks}

AEM ofrece un marco de trabajo para evitar los ataques de falsificación de solicitudes entre sitios. Para utilizar correctamente este marco, realice los siguientes cambios en la configuración de Dispatcher:

>[!NOTE]
>
>Asegúrese de actualizar los números de reglas en los siguientes ejemplos en función de la configuración existente. Recuerde que los distribuidores utilizan la última regla que coincide para conceder o denegar una autorización, de modo que coloque las reglas cerca de la parte inferior de la lista existente.

1. En el `/clientheaders` de su sección `author-farm.any` y `publish-farm.any`, agregue la siguiente entrada al final de la lista:\
   `CSRF-Token`
1. En la sección /filters de su `author-farm.any` y `publish-farm.any` o `publish-filters.any` , agregue la línea siguiente para permitir solicitudes de `/libs/granite/csrf/token.json` mediante Dispatcher.\
   `/0999 { /type "allow" /glob " * /libs/granite/csrf/token.json*" }`
1. En el `/cache /rules` de su sección `publish-farm.any`, agregue una regla para bloquear Dispatcher y evitar que almacene en caché el `token.json` archivo. Normalmente, los autores omiten el almacenamiento en caché, por lo que no es necesario agregar la regla a su `author-farm.any`.\
   `/0999 { /glob "/libs/granite/csrf/token.json" /type "deny" }`

Para comprobar que la configuración está funcionando, observe el archivo dispatcher.log en modo DEBUG para verificar que el archivo token.json no se está almacenando en caché y que los filtros no lo están bloqueando. Debería ver mensajes similares a:\
`... checking [/libs/granite/csrf/token.json]  `
`... request URL not in cache rules: /libs/granite/csrf/token.json`\
`... cache-action for [/libs/granite/csrf/token.json]: NONE`

También puede comprobar que las solicitudes se están realizando correctamente en su Apache `access_log`. Las solicitudes para &quot;/libs/granite/csrf/token.json&quot; deben devolver un código de estado HTTP 200.
