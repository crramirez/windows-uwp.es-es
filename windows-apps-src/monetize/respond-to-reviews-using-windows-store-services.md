---
ms.assetid: c92c0ea8-f742-4fc1-a3d7-e90aac11953e
description: Usa la API de opiniones de Microsoft Store para enviar respuestas mediante programación a opiniones sobre tu aplicación en la Store.
title: Responder a opiniones con servicios de la Store Windows
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store reviews API, API de opiniones de Microsoft Store, respond to reviews, responder a las opiniones
ms.localizationpriority: medium
ms.openlocfilehash: 95de2cc1de1b71a435fc8d4388f599c417132814
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "8795553"
---
# <a name="respond-to-reviews-using-store-services"></a>Responder a opiniones con servicios de la Store Windows

Usa la *API de opiniones de Microsoft Store* para responder mediante programación a las opiniones de la aplicación en la Store. Esta API es especialmente útil para los desarrolladores que quieren masiva de responden a muchas opiniones sin usar el centro de partners. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas procedentes de la aplicación o el servicio.

Los siguientes pasos describen el proceso de principio a fin:

1.  Asegúrate de que se hayan completado todos los [requisitos previos](#prerequisites).
2.  Antes de llamar a un método en la API de opiniones de Microsoft Store, [obtén un token de acceso de Azure AD](#obtain-an-azure-ad-access-token). Después de obtener un token, tienes 60minutos para utilizar dicho token en llamadas a la API de opiniones de Microsoft Store antes de que expire. Después de que el token expire, puedes generar uno nuevo.
3.  [Llama a la API de opiniones de Microsoft Store](#call-the-windows-store-reviews-api).

> [!NOTE]
> Además de usar API para responder mediante programación a las opiniones, también puedes responder a las opiniones [mediante el centro de partners](../publish/respond-to-customer-reviews.md)de opiniones de Microsoft Store.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-reviews-api"></a>Paso 1: Completar los requisitos previos para usar la API de opiniones de Microsoft Store

Antes de empezar a escribir código para llamar a la API de opiniones de Microsoft Store, asegúrate de que has completado los siguientes requisitos previos.

* Tú (o tu organización) debes tener un directorio de Azure AD y un permiso de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para el directorio. Si ya usas Office365 u otros servicios empresariales de Microsoft, ya tienes un directorio de AzureAD. De lo contrario, puede [crear un nuevo Azure AD en el centro de partners](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sin ningún coste adicional.

* Debes asociar una aplicación de Azure AD con tu cuenta del centro de partners, recuperar el identificador de inquilino y el identificador de cliente para la aplicación y generar una clave. La aplicación de Azure AD representa la aplicación o el servicio desde donde quieres originar la llamada a la API de opiniones de Microsoft Store. Necesitas el identificador de inquilino, el de cliente y la clave para obtener un token de acceso de Azure AD que se pasa a la API.
    > [!NOTE]
    > Solo debes realizar esta tarea una vez. Una vez que tengas el identificador de inquilino, el identificador de cliente y la clave, puedes volver a usarlos siempre que necesites crear un nuevo token de acceso de Azure AD.

Para asociar una aplicación de Azure AD con tu cuenta del centro de partners y recuperar los valores necesarios:

1.  En el centro de partners, [asociar la cuenta del centro de partners de tu organización con el directorio de Azure AD de tu organización](../publish/associate-azure-ad-with-partner-center.md).

2.  A continuación, desde la página de **usuarios** en la sección de **configuración de la cuenta** del centro de partners, [Agregar la aplicación de Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) que representa la aplicación o el servicio que usarás para responder a opiniones. Asegúrate de que se asignas a esta aplicación el rol de **Administrador**. Si la aplicación no existe aún en el directorio de Azure AD, puedes [crear una nueva aplicación de Azure AD en el centro de partners](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  Vuelve a la página **Usuarios**, haz clic en el nombre de la aplicación de Azure AD para ir a la configuración de la aplicación y copia los valores de **Identificador de inquilino** e **Identificador de cliente**.

4. Haz clic en **Agregar nueva clave**. En la siguiente pantalla, copia el valor **Clave**. No podrás acceder a esta información de nuevo después de salir de la página. Para obtener más información, consulta [Administrar claves para una aplicación de Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Paso 2: Obtener un token de acceso de Azure AD

Antes de llamar a cualquiera de los métodos en la API de opiniones de Microsoft Store, primero debes obtener un token de acceso de Azure AD para pasarlo al encabezado **Authorization** de cada método en la API. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes actualizar el token para que puedas continuar usándolo en llamadas adicionales a la API.

Para obtener el token de acceso, sigue las instrucciones en [Llamadas de servicio a servicio utilizando las credenciales del cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) para enviar un HTTP POST al punto de conexión ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Este es un ejemplo de solicitud.

```syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

El valor de *tenant\_id* en POST URI y los parámetros *client\_id* y *client\_secret* , especifica el identificador de inquilino, Id. de cliente y la clave de la aplicación que recuperó del centro de partners en la sección anterior. Para el parámetro *resource*, debes especificar ```https://manage.devcenter.microsoft.com```.

Una vez que expire el token de acceso, puedes actualizarlo siguiendo las instrucciones que se muestran [aquí](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-reviews-api" />

## <a name="step-3-call-the-microsoft-store-reviews-api"></a>Paso 3: Llamar a la API de opiniones de Microsoft Store

Cuando tengas un token de acceso de Azure AD, podrás llamar a la API de opiniones de Microsoft Store. Debes pasar el token de acceso al encabezado **Authorization** de cada método.

La API de opiniones de Microsoft Store contiene varios métodos que puedes utilizar para determinar si tienes permiso para responder a una opinión determinada y enviar respuestas a una o varias opiniones. Sigue este proceso para usar la API:

1. Obtén los identificadores de las opiniones a las que quieres responder. Los identificadores de opinión están disponibles en los datos de respuesta del método [obtener opiniones de la aplicación](get-app-reviews.md) en la API de análisis de Microsoft Store y en la [descarga sin conexión](../publish/download-analytic-reports.md) del [informe de opiniones](../publish/reviews-report.md).
2. Llama al método [obtener información de respuesta de opiniones de la aplicación](get-response-info-for-app-reviews.md) para determinar si tienes permiso para responder a las opiniones. Cuando un cliente envía una opinión, puede elegir no recibir respuestas. No puedes responder a opiniones enviadas por clientes que han elegido no recibir respuestas a opiniones.
3. Llama al método [enviar respuestas a opiniones de la aplicación](submit-responses-to-app-reviews.md) para responder mediante programación a las opiniones.


## <a name="related-topics"></a>Temas relacionados

* [Obtener opiniones de la aplicación](get-app-reviews.md)
* [Obtener información de respuesta de opiniones de la aplicación](get-response-info-for-app-reviews.md)
* [Enviar respuestas a opiniones de la aplicación](submit-responses-to-app-reviews.md)

 
