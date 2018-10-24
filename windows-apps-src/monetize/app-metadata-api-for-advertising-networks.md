---
author: Xansky
description: Aprende a usar la API de REST de metadatos de la aplicación para acceder a determinados metadatos de los tipos de aplicaciones. Esta API está pensada para que las redes de publicidad puedan recuperar información sobre las aplicaciones de Microsoft Store con el objetivo de mejorar su forma de vender espacio de anuncios a los anunciantes.
title: API de metadatos de la aplicación para redes de publicidad
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, red de publicidad, metadatos de la aplicación
ms.assetid: f0904086-d61f-4adb-82b6-25968cbec7f3
ms.localizationpriority: medium
ms.openlocfilehash: 16603bfe8c3fe0bfeaef1e19018798d0c6477b85
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "5440046"
---
# <a name="app-metadata-api-for-advertising-networks"></a>API de metadatos de la aplicación para redes de publicidad

Las redes de publicidad pueden usar la *API de metadatos de la aplicación* para, mediante programación, recuperar metadatos sobre las aplicaciones de Microsoft Store, incluidos detalles como la descripción y la categoría de la descripción de la Store de la aplicación, y si la aplicación está destinada a niños menores de 13 años. El acceso a esta API está actualmente restringido a los desarrolladores a los que Microsoft ha concedido permiso a la API.

Este artículo proporciona instrucciones sobre cómo solicitar acceso a la API mediante el [portal de la API de metadatos de la aplicación](https://admetadata.portal.azure-api.net/), cómo obtener tu clave de suscripción para acceder a la API y cómo llamar a la API.

## <a name="request-access"></a>Solicitar acceso

Las redes de publicidad pueden solicitar acceso a la API de metadatos de la aplicación, para ello, deben seguir estas instrucciones:

1. Ve a la página [https://admetadata.portal.azure-api.net/signup](https://admetadata.portal.azure-api.net/signup) del portal de API de metadatos de la aplicación.
2. Escribe la información solicitada y haz clic en el botón **Registrarse**.
3. En el mismo sitio, haz clic en la pestaña **Productos** y, a continuación, haz clic en **Detalles de la aplicación para publicidad**.
4. En la siguiente página, haz clic en el botón **Suscribirse**. Esto envía tu solicitud a Microsoft para acceder a la API de metadatos de la aplicación.

Después de enviar la solicitud, recibirás un correo electrónico en 24 horas aproximadamente que te avisa si se ha concedido o denegado tu solicitud.

<span id="get-key" />

## <a name="get-your-subscription-key"></a>Obtener tu clave de suscripción

Si se te concede acceso a la API de metadatos de la aplicación, sigue estas instrucciones para obtener tu clave de suscripción. Esta clave se debe pasar en el encabezado de solicitud de llamadas a la API.

1. Ve a la página [https://admetadata.portal.azure-api.net/signin](https://admetadata.portal.azure-api.net/signin) del portal de la API de metadatos de la aplicación e inicia sesión con tu correo electrónico y tu contraseña.
2. Haz clic en tu nombre en la esquina superior derecha del sitio y, a continuación, haz clic en **Perfil**.
3. En la sección **Tus suscripciones** de la página, haz clic en **Mostrar** junto a **Clave principal**. Esta es tu clave de suscripción. Copia la clave para que puedas usarla más adelante cuando llames a la API.

<span id="call-the-api" />

## <a name="call-the-api"></a>Llamar a la API

Una vez que tengas la clave de suscripción, estás listo para llamar a la API con la sintaxis de REST de HTTP desde el lenguaje de programación de tu elección. Para obtener información sobre la sintaxis de la API, consulta la siguiente sección [Sintaxis de API](#syntax). Para ver ejemplos de código en C#, JavaScript, Python y otros idiomas, haz clic en la ficha **API** del portal de la API de metadatos de la aplicación, haz clic en **Detalles de la aplicación**y, a continuación, consulta la sección **Ejemplos de código** en la parte inferior de la página.

Como alternativa, puedes llamar a la API mediante la interfaz de usuario proporcionada por el portal de API de metadatos de la aplicación:
  1. En el portal, haz clic en la pestaña **API** y, a continuación, haz clic en **Detalles de la aplicación**.
  2. En la página siguiente, escribe el [app_id](#request-parameters) de la aplicación para la que quieres recuperar los metadatos en el campo **app_id** y escribe la clave de suscripción en el campo **Ocp_Apim_Subscription-Key**.
  3. Haz clic en **Enviar**. La respuesta aparece en la parte inferior de la página.


<span id="syntax" />

## <a name="api-syntax"></a>Sintaxis de API

Este método tiene la siguiente sintaxis de solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://admetadata.azure-api.net/v1/app/{app_id}``` |

<span/>
 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Ocp-Apim-Subscription-Key | cadena | Obligatorio. Clave de suscripción que has [recuperado desde el portal de API de metadatos de la aplicación](#get-key).  |

<span/>

### <a name="request-parameters"></a>Parámetros de la solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------|
| app_id | cadena | Obligatorio. El identificador de la aplicación para la que quieres recuperar los metadatos. Este puede ser uno de los valores siguientes:<br/><br/><ul><li>El Id. de la Store de la aplicación. Un ejemplo de un Id. de la Store sería 9NBLGGH29DM8.</li><li>El id. del producto (en ocasiones, también llamado el *identificador de la aplicación*) para una aplicación que originalmente se creó para Windows 8.x o Windows Phone 8.x. El id. del producto es un GUID.</li></ul> |

<span/>

### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo recuperar metadatos para una aplicación con el valor de Id. de la Store 9NBLGGH29DM8.

```syntax
GET https://admetadata.azure-api.net/v1/app/9NBLGGH29DM8 HTTP/1.1
Ocp-Apim-Subscription-Key: <subscription key>
```

### <a name="response-body"></a>Cuerpo de la respuesta

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON de una llamada satisfactoria a este método.

```json
{
    "storeId":"9NBLGGH29DM8",
    "name":"Contoso Sports App",
    "description":"Catch the latest scores and replays using Contoso Sports App!",
    "phoneStoreGuid":"920217d7-90da-4019-99e8-46e4a6bd2594",
    "windowsStoreGuid":"d7e982e7-fbf3-42b5-9dad-72b65bd4e248",
    "storeCategory":"Sports",
    "iabCategory":"Sports",
    "iabCategoryId":"IAB17",
    "coppa":false,
    "downloadUrl":"https://www.microsoft.com/store/apps/9nblggh29dm8",
    "isLive":true,
    "iconUrls":[
      "//store-images.microsoft.com/image/apps.15753.13510798883747357.b6981489-6fdd-4fec-b655-a822247d5a76.33529096-a723-4204-9da9-a5dd8b328d4e"
    ],
    "type":"App",
    "devices":[
      "PC",
      "Phone"
    ],
    "platformVersions":[
      "Windows.Universal"
    ],
    "screenshotUrls":[
      "//store-images.microsoft.com/image/apps.15901.19810723133740207.c9781069-6fef-5fba-a055-c922051d59b6.40129896-d083-5604-ab79-aba68bfa084f"
    ]
}
```

Para obtener más información acerca de los valores del cuerpo de respuesta, consulta la siguiente tabla.

| Valor      | Tipo   | Descripción    |
|------------|--------|--------------------|
| storeId           | cadena  | Id. de la Store de la aplicación. Un ejemplo de un Id. de la Store sería 9NBLGGH29DM8.     |  
| name           | cadena  | El nombre de la aplicación.   |
| description           | cadena  | La descripción de la Store para la aplicación.  |
| phoneStoreGuid           | cadena  | El id. del producto (Windows Phone 8.x) de la aplicación. Este identificador es un GUID.  |
| windowsStoreGuid           | cadena  | El id. del producto (Windows 8.x) de la aplicación. Este identificador es un GUID. |
| storeCategory           | cadena  | La categoría de la aplicación en la Store. Para los valores admitidos, consulta la [Tabla de categoría y subcategoría](../publish/category-and-subcategory-table.md) para aplicaciones de la Store.  |
| iabCategory           | cadena  | La categoría de contenido de la aplicación tal como está definida por Interactive Advertising Bureau (IAB). Por ejemplo, **Noticias** o **Deportes**. Para obtener una lista de categorías de contenido, consulta la página sobre la [taxonomía de contenido de IAB Tech Lab](https://www.iab.com/guidelines/iab-quality-assurance-guidelines-qag-taxonomy) en el sitio web de IAB.   |
| iabCategoryId           | cadena  | El identificador de la categoría de contenido de la aplicación. Por ejemplo, **IAB12** es el identificador de la categoría de noticias e **IAB17** es el identificador de la categoría de deportes. Para obtener una lista de identificadores de categorías de contenido, consulta la sección 5.1 en la [especificación de API de OpenRTB](http://www.iab.com/wp-content/uploads/2015/05/OpenRTB_API_Specification_Version_2_3_1.pdf). |
| coppa           | booleano  | Devuelve verdadero si la aplicación está dirigida a niños menores de 13 años y, por tanto, tiene obligaciones en virtud de la Ley de protección de la privacidad infantil en línea (COPPA); de lo contrario, devuelve false.  |
| downloadUrl           | cadena  | Vínculo a la descripción de la aplicación en la Store. Este vínculo está en formato ```https://www.microsoft.com/store/apps/<Store ID>```.  |
| isLive           | booleano  | True si la aplicación está actualmente disponible en la Store; de lo contrario, false.  |
| iconUrls           | matriz  |  Una matriz de una o más cadenas que contienen las rutas de acceso relativas a las direcciones URL de los iconos asociadas con la aplicación. Para recuperar los iconos, debes anteponer *http* o *https* a las direcciones URL.  |
| type           | cadena  | Una de las siguientes cadenas: **App** o **Game**.  |
| devices           |  matriz  | Una matriz de una o más de las siguientes cadenas que especifica los tipos de dispositivo que admite la aplicación: **PC**, **Phone**, **Xbox**, **IoT**, **Server** y **Holographic**.  |
| platformVersions           | matriz  |  Una matriz de una o más de las siguientes cadenas que especifican las plataformas que admite la aplicación: **Windows.Universal**, **Windows.Windows8x** y **Windows.WindowsPhone8x**.  |
| screenshotUrls           | matriz  | Una matriz de una o más cadenas que contienen las rutas de acceso relativas a las direcciones URL de captura de pantalla para esta aplicación. Para recuperar las capturas de pantalla, debes anteponer *http* o *https* a las direcciones URL.  |

<span/>



 

 
