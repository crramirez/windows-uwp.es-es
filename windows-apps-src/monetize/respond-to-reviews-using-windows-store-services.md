---
ms.assetid: c92c0ea8-f742-4fc1-a3d7-e90aac11953e
description: Use la API de Microsoft Store Reviews para enviar mediante programación las respuestas a las revisiones de la aplicación en la tienda.
title: Respuesta a las revisiones mediante servicios de almacenamiento
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store API Reviews, responder a las revisiones
ms.localizationpriority: medium
ms.openlocfilehash: 5b39ec67c4821b870a0f404e7199b69152b3a89c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174959"
---
# <a name="respond-to-reviews-using-store-services"></a>Respuesta a las revisiones mediante servicios de almacenamiento

Use la *API de Microsoft Store Reviews* para responder mediante programación a las revisiones de la aplicación en la tienda. Esta API es especialmente útil para los desarrolladores que desean responder de forma masiva a muchas revisiones sin usar el centro de Partners. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas provenientes de la aplicación o el servicio.

Los siguientes pasos describen el proceso de principio a fin:

1.  Asegúrate de que se hayan completado todos los [requisitos previos](#prerequisites).
2.  Antes de llamar a un método en la API de Microsoft Store Reviews, [obtenga un token de acceso Azure ad](#obtain-an-azure-ad-access-token). Después de obtener un token, tiene 60 minutos para usar este token en las llamadas a la API de Microsoft Store Reviews antes de que expire el token. Después de que el token expire, puedes generar uno nuevo.
3.  [Llame a la API de Microsoft Store Reviews](#call-the-windows-store-reviews-api).

> [!NOTE]
> Además de usar la API de Microsoft Store Reviews para responder mediante programación a las revisiones, puede responder de forma alternativa a las revisiones [mediante el centro de Partners](../publish/respond-to-customer-reviews.md).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-reviews-api"></a>Paso 1: completar los requisitos previos para usar la API de Microsoft Store Reviews

Antes de empezar a escribir código para llamar a la API de Microsoft Store Reviews, asegúrese de que ha completado los requisitos previos siguientes.

* Usted (o su organización) tiene que tener un directorio de Azure AD y el permiso de [Administrador global](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para el directorio. Si usa Microsoft 365 u otros servicios empresariales de Microsoft, ya tiene el directorio de Azure AD. De lo contrario, puede [crear un nuevo Azure ad en el centro de Partners](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sin cargo adicional.

* Debe asociar una aplicación Azure AD a la cuenta del centro de Partners, recuperar el identificador de inquilino y el identificador de cliente de la aplicación y generar una clave. La aplicación Azure AD representa la aplicación o el servicio del que desea llamar a la API de Microsoft Store Reviews. Necesita el identificador de inquilino, el identificador de cliente y la clave para obtener un token de acceso de Azure AD para pasar a la API.
    > [!NOTE]
    > Solo tiene que realizar esta tarea una vez. Una vez que tenga el identificador de inquilino, el identificador de cliente y la clave, puede volver a usarlos cada vez que tenga que crear un nuevo token de acceso de Azure AD.

Para asociar una aplicación Azure AD a la cuenta del centro de Partners y recuperar los valores necesarios:

1.  En el Centro de partners, [asocie la cuenta del Centro de partners de la organización con el directorio de Azure AD de la organización](../publish/associate-azure-ad-with-partner-center.md).

2.  A continuación, en la página **usuarios** de la sección **configuración** de la cuenta del centro de partners, [agregue la Azure ad aplicación](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) que representa la aplicación o el servicio que usará para responder a las revisiones. Asegúrese de asignar a esta aplicación el rol **Administrador**. Si la aplicación aún no existe en el directorio de Azure AD, puede [crear una nueva aplicación de Azure AD en el Centro de partners](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  Vuelva a la página **Usuarios**, haga clic en el nombre de la aplicación de Azure AD para ir a la configuración de la aplicación y, a continuación, copie los valores de **Identificador de inquilino** e **Identificador de cliente**.

4. Haga clic en **Agregar nueva clave**. En la pantalla siguiente, copie el valor de **Clave**. Después de salir de esta página no podrá tener acceso de nuevo a esta información. Para más información, consulte [Administrar claves para una aplicación de Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Paso 2: Obtención de un token de acceso de Azure AD

Antes de llamar a cualquiera de los métodos de la API de Microsoft Store Reviews, primero debe obtener un token de acceso Azure AD que pase al encabezado **Authorization** de cada método de la API. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes actualizar el token para que puedas continuar usándolo en llamadas adicionales a la API.

Para obtener el token de acceso, sigue las instrucciones en [Llamadas de servicio a servicio utilizando las credenciales del cliente](/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow) para enviar un HTTP POST al punto de conexión ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Este es un ejemplo de solicitud.

```syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Para el valor de * \_ identificador de inquilino* en el URI de post y los parámetros de * \_ identificador de cliente* y * \_ secreto de cliente* , especifique el identificador de inquilino, el identificador de cliente y la clave de la aplicación que recuperó del centro de Partners en la sección anterior. Para el parámetro *resource*, tiene que especificar ```https://manage.devcenter.microsoft.com```.

Una vez que expire el token de acceso, puedes actualizarlo siguiendo las instrucciones que se muestran [aquí](/azure/active-directory/azuread-dev/v1-protocols-oauth-code#refreshing-the-access-tokens).

<span id="call-the-windows-store-reviews-api" />

## <a name="step-3-call-the-microsoft-store-reviews-api"></a>Paso 3: llamar a la API de Microsoft Store Reviews

Una vez que tenga un Azure AD token de acceso, estará listo para llamar a la API de Microsoft Store Reviews. Debes pasar el token de acceso al encabezado **Authorization** de cada método.

La API de Microsoft Store Reviews contiene varios métodos que se pueden usar para determinar si se permite responder a una revisión determinada y enviar respuestas a una o más revisiones. Siga este proceso para usar esta API:

1. Obtenga los identificadores de las revisiones a las que desea responder. Los identificadores de revisión están disponibles en los datos de respuesta del método [Get App Reviews](get-app-reviews.md) de la API de Microsoft Store Analytics y en la [descarga sin conexión](../publish/download-analytic-reports.md) del [Informe de revisiones](../publish/reviews-report.md).
2. Llame al método [Get Response info for App Reviews](get-response-info-for-app-reviews.md) para determinar si se le permite responder a las revisiones. Cuando un cliente envía una revisión, puede optar por no recibir respuestas a su revisión. No puede responder a las revisiones enviadas por los clientes que han elegido no recibir respuestas de revisión.
3. Llame al método [enviar respuestas a las revisiones](submit-responses-to-app-reviews.md) de la aplicación para responder a las revisiones mediante programación.


## <a name="related-topics"></a>Temas relacionados

* [Obtener las opiniones de la aplicación](get-app-reviews.md)
* [Obtención de información de respuesta para las revisiones de la aplicación](get-response-info-for-app-reviews.md)
* [Enviar respuestas a las revisiones de la aplicación](submit-responses-to-app-reviews.md)

 