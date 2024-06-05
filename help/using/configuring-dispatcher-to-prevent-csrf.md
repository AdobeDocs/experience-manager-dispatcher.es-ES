---
title: Configurar Dispatcher de Adobe Experience Manager para prevenir ataques de tipo CSRF
description: Aprenda cómo configurar Dispatcher de Adobe Experience Manager para evitar ataques de falsificación de solicitudes en sitios múltiples.
topic-tags: dispatcher
content-type: reference
exl-id: bcd38878-f977-46a6-b01a-03e4d90aef01
source-git-commit: 0a1aa854ea286a30c3527be8fc7c0998726a663f
workflow-type: ht
source-wordcount: '235'
ht-degree: 100%

---

# Configurar Dispatcher de Adobe Experience Manager para prevenir ataques de tipo CSRF{#configuring-dispatcher-to-prevent-csrf-attacks}

AEM (Adobe Experience Manager) ofrece un marco de trabajo para evitar los ataques de falsificación de solicitudes en sitios múltiples. Para utilizar correctamente este marco de trabajo, realice los siguientes cambios en la configuración de Dispatcher:

>[!NOTE]
>
>Asegúrese de actualizar los números de reglas en los siguientes ejemplos en función de la configuración existente. Recuerde que Dispatcher utiliza la última regla que coincide para conceder o denegar una autorización, de modo que las reglas quedan cerca de la parte inferior de la lista existente.

1. En la sección `/clientheaders` de `author-farm.any` y `publish-farm.any`, agregue la siguiente entrada al final de la lista:\
   `CSRF-Token`
1. En la sección /filters de los archivos `author-farm.any` y `publish-farm.any` o `publish-filters.any`, agregue la siguiente línea para permitir las solicitudes de `/libs/granite/csrf/token.json` a través de Dispatcher.\
   `/0999 { /type "allow" /glob " * /libs/granite/csrf/token.json*" }`

1. En la sección `/cache /rules` de su `publish-farm.any`, agregue una regla para bloquear Dispatcher y evitar que almacene el archivo `token.json` en caché. Normalmente, los autores omiten el almacenamiento en caché, por lo que no debe agregar la regla a su `author-farm.any`.

   `/0999 { /glob "/libs/granite/csrf/token.json" /type "deny" }`

Para validar que la configuración esté funcionando, observe el archivo dispatcher.log en modo DEBUG. Puede ayudarle a validar que el archivo `token.json` para asegurarse de que los filtros no lo almacenen en caché o lo bloqueen. Debería ver mensajes similares a los siguientes:\
`... checking [/libs/granite/csrf/token.json]  `
`... request URL not in cache rules: /libs/granite/csrf/token.json`\
`... cache-action for [/libs/granite/csrf/token.json]: NONE`

También puede comprobar que las solicitudes se están realizando correctamente en `access_log` de Apache. Las solicitudes para “/libs/granite/csrf/token.json” deben devolver un código de estado HTTP 200.
