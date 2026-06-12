---
title: Mejora de Dispatcher ETag para la revalidación de CDN
description: Disponibilidad, estado de compatibilidad y comportamiento de INTERNAL_AEM_DISPATCHER_ETAG_ENHANCEMENT en AEM as a Cloud Service.
exl-id: 4409d0f0-05db-42f3-ace9-1516f1970891
source-git-commit: cddffe2194beea628f71b6631faada5df4555267
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 0%

---

# Mejora de Dispatcher ETag para la revalidación de CDN

## Información general

El indicador `INTERNAL_AEM_DISPATCHER_ETAG_ENHANCEMENT` permite que Dispatcher evalúe el encabezado de solicitud `If-None-Match` en las visitas de caché. Cuando el valor `If-None-Match` entrante coincide con el `ETag` almacenado en caché, Dispatcher puede devolver `304 Not Modified` en lugar de `200 OK`.

Este comportamiento está diseñado para reducir las transferencias de carga útil innecesarias entre CDN y Dispatcher y aumentar la eficacia de la caché condicional.

## Disponibilidad

- Versión de Dispatcher: `2.0.264`
- Compilación de AEM SDK: `aem-sdk-2026.2.24464.20260214T050318Z-260100`

## Compatibilidad con AEM as a Cloud Service

En AEM as a Cloud Service, esta capacidad es compatible con el uso del cliente.

Los clientes pueden habilitarlo estableciendo la variable de entorno `INTERNAL_AEM_DISPATCHER_ETAG_ENHANCEMENT` en Cloud Manager. Adobe también puede habilitarlo en nombre del cliente cuando sea necesario.

Cuando está habilitado, y cuando la CDN envía `If-None-Match` y el `ETag` relevante está presente en la caché de Dispatcher, se esperan tasas de respuesta `304` más altas entre la CDN y Dispatcher. Este aumento es el resultado previsto.

## Ejemplo de configuración (encabezado ETag de caché)

Para que esta mejora sea efectiva, asegúrese de que Dispatcher almacena en caché el encabezado de respuesta `ETag` y de que su servidor web está configurado para evitar generar etiquetas ET basadas en el sistema de archivos.

Ejemplo de sección de caché `dispatcher.any`:

```text
/cache {
  /headers {
    "Cache-Control"
    "Content-Type"
    "Expires"
    "Last-Modified"
    "ETag"
  }
}
```

Ejemplo de directiva Apache en el contexto vhost de Dispatcher:

```apache
FileETag none
```

Para obtener instrucciones básicas sobre el almacenamiento en caché de encabezados, consulte [Almacenamiento en caché de encabezados de respuesta HTTP](dispatcher-configuration.md#caching-http-response-headers).

## Ejemplo de validación

Después de habilitar la variable de entorno e implementar los cambios de configuración:

1. Solicite una vez para calentar la caché y capturar los `ETag` devueltos.
1. Vuelva a solicitar con `If-None-Match: <etag-value>`.
1. Confirme que Dispatcher devuelve `304 Not Modified` para los flujos de revalidación de visitas en caché.

## Referencia pública (comportamiento relacionado)

Para obtener instrucciones de línea de base de cara al cliente sobre el almacenamiento en caché de encabezados y la administración de `ETag` en Dispatcher, consulte:

- [Configuración de Dispatcher: almacenamiento en caché de encabezados de respuesta HTTP](https://experienceleague.adobe.com/es/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration#caching-http-response-headers)

&quot;Esta funcionalidad está disponible en Dispatcher `2.0.264` (AEM SDK `2026.2.24464`). Cuando está habilitado, Dispatcher puede validar `If-None-Match` con valores de `ETag` en caché y devolver `304 Not Modified` en las visitas de caché. En AEM as a Cloud Service, esto es compatible y se puede habilitar mediante la configuración del entorno de Cloud Manager&quot;.
