---
author: mcleanbyron
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: "Usa la API de promociones de la Tienda Windows para administrar las campañas publicitarias promocionales mediante programación para las aplicaciones registradas en tu cuenta del Centro de desarrollo de Windows o en la de tu organización."
title: "Ejecución de campañas publicitarias con los servicios de la Tienda Windows"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de promociones de la Tienda Windows, campañas publicitarias"
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: ec245f07098a662c80517de49ba5637a69b30f35
ms.lasthandoff: 02/08/2017

---

# <a name="run-ad-campaigns-using-windows-store-services"></a>Ejecución de campañas publicitarias con los servicios de la Tienda Windows

Usa la *API de promociones de la Tienda Windows* para administrar las campañas publicitarias promocionales mediante programación para las aplicaciones registradas en tu cuenta del Centro de desarrollo de Windows o en la de tu organización. Esta API permite crear, actualizar y supervisar las campañas y otros activos relacionados, como la selección de destino y los creativos. Esta API es especialmente útil para los desarrolladores que crean grandes volúmenes de campañas y que quieran hacerlo sin usar el panel del Centro de desarrollo de Windows. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas procedentes de la aplicación o el servicio.

Los siguientes pasos describen el proceso de principio a fin:

1.  Asegúrate de que se hayan completado todos los [requisitos previos](#prerequisites).
2.  Antes de llamar a un método en la API de promociones de la tienda Windows, [consigue un token de acceso de Azure AD](#obtain-an-azure-ad-access-token). Tras obtener un token, tienes 60 minutos para utilizar dicho token en llamadas a la API de promociones de la Tienda Windows antes de que expire. Una vez que el token expire, puedes generar uno nuevo.
3.  [Llama a la API de promociones de la Tienda Windows](#call-the-windows-store-promotions-api).

Como alternativa, puedes crear y administrar las campañas publicitarias mediante el panel del Centro de desarrollo de Windows, donde también se puede acceder a las campañas publicitarias que crees mediante programación a través de la API de promociones de la Tienda Windows. Para obtener más información acerca de la administración de campañas publicitarias en el panel, consulta [Crear una campaña publicitaria para tu aplicación](../publish/create-an-ad-campaign-for-your-app.md).

>**Nota**&nbsp;&nbsp;Cualquier desarrollador con una cuenta del Centro de desarrollo de Windows puede usar la API de promociones de la Tienda Windows para administrar las campañas publicitarias para sus aplicaciones. Las agencias de medios también pueden solicitar acceso a esta API para ejecutar campañas publicitarias en nombre de sus anunciantes. Si tienes una agencia de medios y quieres saber más sobre esta API o solicitar acceso a ella, envía tu solicitud a storepromotionsapi@microsoft.com.

<span id="prerequisites" />
## <a name="step-1-complete-prerequisites-for-using-the-windows-store-promotions-api"></a>Paso 1: Completa los requisitos previos para usar la API de promociones de la Tienda Windows

Antes de empezar a escribir código para llamar a la API de promociones de la Tienda Windows, asegúrate de que has completado los siguientes requisitos previos.

* Tú (o tu organización) debes tener un directorio de Azure AD y un permiso de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para el directorio. Si ya usas Office 365 u otros servicios empresariales de Microsoft, ya tienes un directorio de Azure AD. De lo contrario, puedes [crear un nuevo Azure AD desde el Centro de desarrollo](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users) sin ningún coste adicional.

* Debes asociar una aplicación de Azure AD con tu cuenta del Centro de desarrollo, recuperar el identificador de inquilino y el identificador de cliente correspondientes a la aplicación y generar la clave. La aplicación de Azure AD representa la aplicación o el servicio desde el que quieres llamar a la API de promociones de la Tienda Windows. Necesitas el identificador de inquilino, el de cliente y la clave para obtener un token de acceso de Azure AD que se pasará a la API.

  >**Nota**&nbsp;&nbsp;Solo debes realizar esta tarea una vez. Una vez que tengas el identificador de inquilino, el identificador de cliente y la clave, puedes volver a usarlos siempre que necesites crear un nuevo token de acceso de Azure AD.

Para asociar una aplicación de Azure AD con la cuenta del Centro de desarrollo y recuperar los valores requeridos:

1.  En el Centro de desarrollo, ve a **Configuración de la cuenta**, haz clic en **Administrar usuarios** y asocia la cuenta del Centro de desarrollo de tu organización al directorio de Azure AD de tu organización. Para obtener instrucciones detalladas, consulta [Administración de usuarios de la cuenta](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

2.  En la página **Administrar usuarios**, haz clic en **Agregar aplicaciones de Azure AD**, agrega la aplicación de Azure AD que represente la aplicación o el servicio que usarás para administrar las campañas promocionales de tu cuenta del Centro de desarrollo y asígnale el rol **Administrador**. Si esta aplicación ya existe en el directorio de Azure AD, puedes seleccionarla en la página **Agregar aplicaciones de Azure AD** para agregarla a tu cuenta del Centro de desarrollo. De lo contrario, puedes crear una nueva aplicación de Azure AD en la página **Adición de aplicaciones de Azure AD**. Para obtener más información, consulta [Adición y administración de aplicaciones de Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

3.  Vuelve a la página **Administración de usuarios**, haz clic en el nombre de la aplicación de Azure AD para ir a la configuración de la aplicación y copia los valores de **Identificador de inquilino** e **Identificador de cliente**.

4. Haz clic en **Agregar nueva clave**. En la siguiente pantalla, copia el valor **Clave**. No podrás acceder a esta información de nuevo después de salir de la página. Para obtener más información, consulta la información sobre la administración de claves en [Adición y administración de aplicaciones de Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

<span id="obtain-an-azure-ad-access-token" />
## <a name="step-2-obtain-an-azure-ad-access-token"></a>Paso 2: Obtén un token de acceso de Azure AD

Antes de llamar a cualquiera de los métodos en la API de promociones de la Tienda Windows, primero debes obtener un token de acceso de Azure AD para pasarlo al encabezado **Authorization** de cada método de la API. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes actualizar el token para que puedas continuar usándolo en llamadas adicionales a la API.

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

<span id="call-the-windows-store-promotions-api" />
## <a name="step-3-call-the-windows-store-promotions-api"></a>Paso 3: Llama a la API de promociones de la Tienda Windows

Cuando tengas un token de acceso de Azure AD, podrás llamar a la API de promociones de la Tienda Windows. Debes pasar el token de acceso al encabezado **Authorization** de cada método.

En el contexto de la API de promociones de la Tienda Windows, una campaña publicitaria consta de un objeto *campaña*, que contiene información de alto nivel sobre la campaña, y otros objetos que representan las *líneas de entrega*, los *perfiles de selección de destino* y los *creativos* para la campaña publicitaria. La API incluye diferentes conjuntos de los métodos que se agrupan por estos tipos de objetos. Para crear una campaña, normalmente llamas a un método POST diferente para cada uno de estos objetos. La API también proporciona métodos GET que puedes usar para recuperar cualquier objeto, y métodos PUT que sirven para editar la campaña, la línea de entrega y los objetos de perfil de selección de destino.

Para obtener más información sobre estos objetos y sus métodos relacionados, consulta la siguiente tabla.


| Objeto       | Descripción   |
|---------------|-----------------|
| Campañas |  Este objeto representa la campaña publicitaria y se encuentra en la parte superior de la jerarquía del modelo de objetos de las campañas publicitarias. Este objeto identifica el tipo de campaña que se ejecuta (de pago, interna o comunitaria), el objetivo de la campaña, las líneas de entrega de la campaña y otros detalles. Cada campaña puede asociarse con una sola aplicación.<br/><br/>Para obtener más información acerca de los métodos relacionados con este objeto, consulta [Administración de las campañas publicitarias](manage-ad-campaigns.md).<br/><br/>**Nota**&nbsp;&nbsp;Tras crear una campaña publicitaria, puedes recuperar los datos de rendimiento de dicha campaña mediante el método [Obtener los datos de rendimiento de la campaña de anuncios](get-ad-campaign-performance-data.md) en la [API de análisis de la Tienda Windows](access-analytics-data-using-windows-store-services.md).  |
| Líneas de entrega | Cada campaña tiene uno o más líneas de entrega, que se usan para comprar inventario y entregar los anuncios. Para cada línea de entrega, puedes establecer la selección de destino y el precio de la oferta, así como cuánto quieres gastar mediante el establecimiento de un presupuesto y la vinculación a los creativos que quieras usar.<br/><br/>Para obtener más información acerca de los métodos relacionados con este objeto, consulta [Administración de las líneas de entrega de las campañas publicitarias](manage-delivery-lines-for-ad-campaigns.md). |
| Perfiles de selección de destino | Cada línea de entrega tiene un perfil de selección de destino que especifica los usuarios, las regiones y los tipos de inventario que quieres como destino. Los perfiles de selección de destino se pueden crear como una plantilla y compartirse entre diversas líneas de entrega.<br/><br/>Para obtener más información acerca de los métodos relacionados con este objeto, consulta [Administración de los perfiles de selección de destino de las campañas publicitarias](manage-targeting-profiles-for-ad-campaigns.md). |
| Creativos | Cada línea de entrega tiene uno o más creativos, que representan los anuncios que se muestran a los clientes como parte de la campaña. Un creativo puede estar asociada con una o más líneas de entrega, incluso en distintas campañas publicitarias, siempre que represente en todo momento la misma aplicación.<br/><br/>Para obtener más información acerca de los métodos relacionados con este objeto, consulta [Administración de los creativos de las campañas publicitarias](manage-creatives-for-ad-campaigns.md). |


El siguiente diagrama muestra la relación entre las campañas, las líneas de entrega, los perfiles de selección de destino y los creativos.

![Jerarquía de una campaña publicitaria](images/ad-campaign-hierarchy.png)

## <a name="code-example"></a>Ejemplo de código

El siguiente ejemplo de código muestra cómo obtener un token de acceso de Azure AD y llamar a la API de promociones de la Tienda Windows desde una aplicación de consola C#. Para usar este ejemplo de código, asigna las variables *tenantId*, *clientId*, *clientSecret* y *appID* a los valores adecuados para tu escenario. En este ejemplo se necesita el [paquete Json.NET](http://www.newtonsoft.com/json) de Newtonsoft para deserializar los datos de JSON devueltos por la API de promociones de la Tienda Windows.

> [!div class="tabbedCodeSnippets"]
[!code-cs[PromotionsApi](./code/StoreServicesExamples_Promotions/cs/Program.cs#PromotionsApiExample)]

## <a name="related-topics"></a>Temas relacionados

* [Administración de las campañas publicitarias](manage-ad-campaigns.md)
* [Administración de las líneas de entrega de las campañas publicitarias](manage-delivery-lines-for-ad-campaigns.md)
* [Administración de los perfiles de selección de destino de las campañas publicitarias](manage-targeting-profiles-for-ad-campaigns.md)
* [Administración de los creativos de las campañas publicitarias](manage-creatives-for-ad-campaigns.md)
* [Obtener los datos de rendimiento de la campaña de anuncios](get-ad-campaign-performance-data.md)


 

