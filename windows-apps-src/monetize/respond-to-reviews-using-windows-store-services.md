---
author: mcleanbyron
ms.assetid: c92c0ea8-f742-4fc1-a3d7-e90aac11953e
description: "Usa la API de opiniones de la Tienda Windows para enviar respuestas mediante programación a opiniones sobre tu aplicación en la Tienda."
title: Responder a opiniones con servicios de la Tienda Windows
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP, Windows Store reviews API, API de opiniones de la Tienda Windows, respond to reviews, responder a las opiniones
ms.openlocfilehash: 6a345fe3d8d5f8e9df7a01d94a8101d31aa312e5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="respond-to-reviews-using-store-services"></a>Responder a opiniones con servicios de la Tienda Windows

Usa la *API de opiniones de la Tienda Windows* para responder mediante programación a las opiniones de la aplicación en la Tienda. Esta API es especialmente útil para los desarrolladores que quieren responder de forma masiva a muchas opiniones sin usar el panel del Centro de desarrollo de Windows. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas procedentes de la aplicación o el servicio.

Los siguientes pasos describen el proceso de principio a fin:

1.  Asegúrate de que se hayan completado todos los [requisitos previos](#prerequisites).
2.  Antes de llamar a un método en la API de opiniones de la Tienda Windows [obtén un token de acceso de Azure AD](#obtain-an-azure-ad-access-token). Después de obtener un token, tienes 60minutos para utilizar dicho token en llamadas a la API de opiniones de la Tienda Windows antes de que expire. Después de que el token expire, puedes generar uno nuevo.
3.  [Llama a la API de opiniones de la Tienda Windows](#call-the-windows-store-reviews-api).

>**Nota**&nbsp;&nbsp;Además de usar la API de opiniones de la Tienda Windows para responder mediante programación a las opiniones, también puedes responder a las opiniones [mediante el panel del Centro de desarrollo de Windows](../publish/respond-to-customer-reviews.md).

<span id="prerequisites" />
## <a name="step-1-complete-prerequisites-for-using-the-windows-store-reviews-api"></a>Paso 1: Completar los requisitos previos para usar la API de opiniones de la Tienda Windows

Antes de empezar a escribir código para llamar a la API de opiniones de la Tienda Windows, asegúrate de que has completado los siguientes requisitos previos.

* Tú (o tu organización) debes tener un directorio de Azure AD y un permiso de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para el directorio. Si ya usas Office365 u otros servicios empresariales de Microsoft, ya tienes un directorio de AzureAD. De lo contrario, puedes [crear un nuevo Azure AD desde el Centro de desarrollo](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users) sin ningún coste adicional.

* Debes asociar una aplicación de Azure AD con tu cuenta del Centro de desarrollo, recuperar el identificador de inquilino y el identificador de cliente correspondientes a la aplicación y generar la clave. La aplicación de Azure AD representa la aplicación o el servicio desde donde quieres originar la llamada a la API de opiniones de la Tienda Windows. Necesitas el identificador de inquilino, el de cliente y la clave para obtener un token de acceso de Azure AD que se pasa a la API.

  >**Nota**&nbsp;&nbsp;Solo debes realizar esta tarea una vez. Una vez que tengas el identificador de inquilino, el identificador de cliente y la clave, puedes volver a usarlos siempre que necesites crear un nuevo token de acceso de Azure AD.

Para asociar una aplicación de Azure AD con la cuenta del Centro de desarrollo y recuperar los valores requeridos:

1.  En el Centro de desarrollo, ve a **Configuración de la cuenta**, haz clic en **Administrar usuarios** y asocia la cuenta del Centro de desarrollo de tu organización al directorio de AzureAD de tu organización. Para obtener instrucciones detalladas, consulta [Administración de usuarios de la cuenta](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

2.  En la página **Administrar usuarios**, haz clic en **Agregar aplicaciones de Azure AD**, agrega la aplicación de Azure AD que represente la aplicación o el servicio que usarás para administrar las campañas promocionales de tu cuenta del Centro de desarrollo y asígnale el rol **Administrador**. Si esta aplicación ya existe en el directorio de Azure AD, puedes seleccionarla en la página **Agregar aplicaciones de Azure AD** para agregarla a tu cuenta del Centro de desarrollo. De lo contrario, puedes crear una nueva aplicación de Azure AD en la página **Adición de aplicaciones de Azure AD**. Para obtener más información, consulta [Adición y administración de aplicaciones de Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

3.  Vuelve a la página **Administración de usuarios**, haz clic en el nombre de la aplicación de Azure AD para ir a la configuración de la aplicación y copia los valores de **Identificador de inquilino** e **Identificador de cliente**.

4. Haz clic en **Agregar nueva clave**. En la siguiente pantalla, copia el valor **Clave**. No podrás acceder a esta información de nuevo después de salir de la página. Para obtener más información, consulta la información sobre la administración de claves en [Adición y administración de aplicaciones de Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

<span id="obtain-an-azure-ad-access-token" />
## <a name="step-2-obtain-an-azure-ad-access-token"></a>Paso 2: Obtener un token de acceso de Azure AD

Antes de llamar a cualquiera de los métodos en la API de opiniones de la Tienda Windows, primero debes obtener un token de acceso de Azure AD para pasarlo al encabezado **Authorization** de cada método en la API. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes actualizar el token para que puedas continuar usándolo en llamadas adicionales a la API.

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

Para el valor de *tenant\_id* de POST URI y los parámetros *client\_id* y *client\_secret*, especifica el identificador de inquilino, de cliente y la clave para la aplicación que recuperaste del Centro de desarrollo en la sección anterior. Para el parámetro *resource*, debes especificar ```https://manage.devcenter.microsoft.com```.

Una vez que expire el token de acceso, puedes actualizarlo siguiendo las instrucciones que se muestran [aquí](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-reviews-api" />
## <a name="step-3-call-the-windows-store-reviews-api"></a>Paso 3: Llamar a la API de opiniones de la Tienda Windows

Cuando tengas un token de acceso de Azure AD, podrás llamar a la API de opiniones de la Tienda Windows. Debes pasar el token de acceso al encabezado **Authorization** de cada método.

La API de opiniones de la Tienda Windows contiene varios métodos que puedes utilizar para determinar si tienes permiso para responder a una opinión determinada y enviar respuestas a una o varias opiniones. Sigue este proceso para usar la API:

1. Obtén los identificadores de las opiniones a las que quieres responder. Los identificadores de opinión están disponibles en los datos de respuesta del método [obtener opiniones de la aplicación](get-app-reviews.md) en la API de análisis de la Tienda Windows y en la [descarga sin conexión](../publish/download-analytic-reports.md) del [informe de opiniones](../publish/reviews-report.md).
2. Llama al método [obtener información de respuesta de opiniones de la aplicación](get-response-info-for-app-reviews.md) para determinar si tienes permiso para responder a las opiniones. Cuando un cliente envía una opinión, puede elegir no recibir respuestas. No puedes responder a opiniones enviadas por clientes que han elegido no recibir respuestas a opiniones.
3. Llama al método [enviar respuestas a opiniones de la aplicación](submit-responses-to-app-reviews.md) para responder mediante programación a las opiniones.


## <a name="related-topics"></a>Temas relacionados

* [Obtener opiniones de la aplicación](get-app-reviews.md)
* [Obtener información de respuesta de opiniones de la aplicación](get-response-info-for-app-reviews.md)
* [Enviar respuestas a opiniones de la aplicación](submit-responses-to-app-reviews.md)

 
