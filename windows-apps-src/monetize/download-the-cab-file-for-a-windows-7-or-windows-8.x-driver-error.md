---
author: mcleanbyron
ms.assetid: 48D891CD-706C-4759-AB33-B0663774A829
description: "Usa este método en la API de análisis de la Tienda Windows para descargar el archivo .cab para un error de controlador de Windows 7 o Windows 8.x. Este método está previsto solo para IHV."
title: Descargar el archivo .cab para un error de controlador de Windows 7 o Windows 8.x
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, Windows 10, uwp, UWP, Windows Store analytics API, API de análisis de la Tienda Windows, download CAB, descargar .cab"
ms.openlocfilehash: 622ad656a64381e80549bc286a4d30490e488631
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="download-the-cab-file-for-a-windows-7-or-windows-8x-driver-error"></a>Descargar el archivo .cab para un error de controlador de Windows 7 o Windows 8.x

Usa este método en la API de análisis de la Tienda Windows para descargar el archivo .cab que está asociado con un error concreto de controlador de Windows 7 o Windows 8.x. Antes de poder usar este método, tienes que emplear el método para [obtener datos para un error de controlador de Windows 7 o Windows 8.x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md) y recuperar el id. del archivo .cab que quieres descargar.

Puedes obtener otra información sobre errores de controlador de Windows 7 o Windows 8.x con los métodos para [obtener datos de informes de errores para controladores de Windows 7 y Windows 8.x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md) y [obtener los detalles para un error de controlador de Windows 7 o Windows 8.x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md) en la API de análisis de la Tienda Windows.

> [!NOTE]
> Solo las cuentas de desarrollador que pertenecen al [programa Centro de desarrollo de hardware de Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) pueden usar este método.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.
* Obtén el id. del archivo .cab que quieres descargar. Para obtener este id., usa el método para [obtener los detalles para un error de controlador de Windows 7 o Windows 8.x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md) para recuperar los detalles de un error específico de controlador y usa el valor **cabIdHash** en el cuerpo de la respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/cabdownload``` |

<span/> 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/> 

### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Necesario  |
|---------------|--------|---------------|------|
| cabIdHash | cadena | El id. exclusivo del archivo .cab que quieres descargar. Para obtener este id., usa el método para [obtener los detalles para un error de controlador de Windows 7 o Windows 8.x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md) para recuperar los detalles de un error específico de la aplicación y usa el valor **cabIdHash** en el cuerpo de la respuesta de ese método. |  Sí  |

<span/>
 
### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo descargar un archivo .cab con este método.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/cabdownload?cabIdHash=c1a51104-d682-4230-be14-7278b18e3555 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

Este método devuelve un código de respuesta 302 (redirigir) y el encabezado **Ubicación** de la respuesta se asigna al URI de la firma de acceso compartido (SAS) del archivo .cab. El autor de la llamada se redirige a este URI para descargar automáticamente el archivo .cab.

## <a name="related-topics"></a>Temas relacionados

* [Obtener datos de informes de errores para controladores de Windows 7 y Windows 8.x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md)
* [Obtener los detalles para un error de controlador de Windows 7 o Windows 8.x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md)
