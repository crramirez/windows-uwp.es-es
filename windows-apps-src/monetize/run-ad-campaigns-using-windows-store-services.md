---
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: Use la API de promociones de Microsoft Store para administrar mediante programación campañas publicitarias promocionales para las aplicaciones que están registradas en su cuenta del centro de Partners de su organización.
title: Ejecutar campañas de anuncios con los servicios de la Store
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store promotions API, API de promociones de Microsoft Store, ad campaigns, campañas de anuncios
ms.localizationpriority: medium
ms.openlocfilehash: 54a9fcf524231f641ca92cb037bb6dcd01b8502f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260187"
---
# <a name="run-ad-campaigns-using-store-services"></a>Ejecutar campañas de anuncios con los servicios de la Store

Use la *API de promociones de Microsoft Store* para administrar mediante programación campañas publicitarias promocionales para las aplicaciones que están registradas en su cuenta del centro de Partners de su organización. Esta API permite crear, actualizar y supervisar las campañas y otros activos relacionados como destino y creativos. Esta API es especialmente útil para los desarrolladores que crean grandes volúmenes de campañas y que quieren hacerlo sin usar el centro de Partners. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas provenientes de la aplicación o el servicio.

Los siguientes pasos describen el proceso de principio a fin:

1.  Asegúrate de que se hayan completado todos los [requisitos previos](#prerequisites).
2.  Antes de llamar a un método en la API de promociones de Microsoft Store, [consigue un token de acceso de Azure AD](#obtain-an-azure-ad-access-token). Tras obtener un token, tienes 60 minutos para utilizar dicho token en llamadas a la API de promociones de Microsoft Store antes de que expire. Una vez que el token expire, puedes generar uno nuevo.
3.  [Llama a la API de promociones de Microsoft Store](#call-the-windows-store-promotions-api).

También puede crear y administrar campañas de ad con el centro de Partners, y también se puede tener acceso a las campañas de ad creadas mediante programación a través de la API de promociones de Microsoft Store en el centro de Partners. Para obtener más información acerca de la administración de campañas de AD en el centro de Partners, consulte [creación de una campaña de AD para la aplicación](../publish/create-an-ad-campaign-for-your-app.md).

> [!NOTE]
> Cualquier desarrollador con una cuenta del centro de Partners puede usar la API de promociones de Microsoft Store para administrar campañas de AD para sus aplicaciones. Las agencias de medios también pueden solicitar acceso a esta API para ejecutar campañas publicitarias en nombre de sus anunciantes. Si tienes una agencia de medios y quieres saber más sobre esta API o solicitar acceso a ella, envía tu solicitud a storepromotionsapi@microsoft.com.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-promotions-api"></a>Paso 1: Completa los requisitos previos para usar la API de promociones de Microsoft Store

Antes de empezar a escribir código para llamar a la API de promociones de Microsoft Store, asegúrate de que has completado los siguientes requisitos previos.

* Antes de poder crear e iniciar correctamente una campaña de ad mediante esta API, primero debe [crear una campaña de pago de pago mediante la página **campañas de ad** del centro de Partners](../publish/create-an-ad-campaign-for-your-app.md), y debe agregar al menos un instrumento de pago en esta página. Después de hacer esto, podrás crear correctamente líneas de entrega facturables para campañas de anuncios con esta API. Las líneas de entrega de las campañas de ad que cree con esta API facturarán automáticamente el instrumento de pago predeterminado elegido en la página **campañas de ad** del centro de Partners.

* Tú (o tu organización) debes tener un directorio de Azure AD y un permiso de [Administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para el directorio. Si ya usas Office 365 u otros servicios empresariales de Microsoft, ya tienes un directorio de Azure AD. De lo contrario, puede [crear un nuevo Azure ad en el centro de Partners](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sin cargo adicional.

* Debe asociar una aplicación Azure AD a la cuenta del centro de Partners, recuperar el identificador de inquilino y el identificador de cliente de la aplicación y generar una clave. La aplicación de Azure AD representa la aplicación o el servicio desde el que quieres llamar a la API de promociones de Microsoft Store. Necesitas el identificador de inquilino, de cliente y la clave para obtener un token de acceso de Azure AD que se pasa a la API.
    > [!NOTE]
    > Solo debes realizar esta tarea una vez. Una vez que tengas el identificador de inquilino, de cliente y la clave, puedes volver a usarlos cuando necesites crear un nuevo token de acceso de Azure AD.

Para asociar una aplicación Azure AD a la cuenta del centro de Partners y recuperar los valores necesarios:

1.  En el centro de Partners, [asocie la cuenta del centro de Partners de su organización con el directorio de Azure ad de su organización](../publish/associate-azure-ad-with-partner-center.md).

2.  A continuación, en la página **usuarios** de la sección **configuración** de la cuenta del centro de partners, [agregue la Azure ad aplicación](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) que representa la aplicación o el servicio que usará para administrar campañas de promoción para su cuenta del centro de Partners. Asegúrate de que se asignas a esta aplicación el rol de **Administrador**. Si la aplicación aún no existe en el directorio de Azure AD, puede [crear una nueva aplicación de Azure ad en el centro de Partners](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  Vuelve a la página **Usuarios**, haz clic en el nombre de la aplicación de Azure AD para ir a la configuración de la aplicación y copia los valores de **Identificador de inquilino** e **Identificador de cliente**.

4. Haz clic en **Agregar nueva clave**. En la siguiente pantalla, copia el valor **Clave**. No podrás acceder a esta información de nuevo después de salir de la página. Para obtener más información, consulta [Administrar claves para una aplicación de Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Paso 2: Obtención de un token de acceso de Azure AD

Antes de llamar a cualquiera de los métodos en la API de promociones de Microsoft Store, primero debes obtener un token de acceso de Azure AD para pasarlo al encabezado **Authorization** de cada método de la API. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes actualizar el token para que puedas continuar usándolo en llamadas adicionales a la API.

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

Para el valor de *identificador de\_de inquilino* en el URI de entrada y los parámetros de cliente *\_ID.* y *cliente\_secreto* , especifique el identificador de inquilino, el identificador de cliente y la clave de la aplicación que recuperó del centro de Partners en la sección anterior. Para el parámetro *resource*, debes especificar ```https://manage.devcenter.microsoft.com```.

Una vez que expire el token de acceso, puedes actualizarlo siguiendo las instrucciones que se muestran [aquí](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-promotions-api" />

## <a name="step-3-call-the-microsoft-store-promotions-api"></a>Paso 3: Llama a la API de promociones de Microsoft Store

Cuando tengas un token de acceso de Azure AD, podrás llamar a la API de promociones de Microsoft Store. Debes pasar el token de acceso al encabezado **Authorization** de cada método.

En el contexto de la API de promociones de Microsoft Store, una campaña publicitaria consta de un objeto *campaña*, que contiene información de alto nivel sobre la campaña, y otros objetos que representan las *líneas de entrega*, los *perfiles de selección de destino* y los *creativos* para la campaña publicitaria. La API incluye diferentes conjuntos de los métodos que se agrupan por estos tipos de objetos. Para crear una campaña, normalmente llamas a un método POST diferente para cada uno de estos objetos. La API también proporciona métodos GET que puedes usar para recuperar cualquier objeto, y métodos PUT que sirven para editar la campaña, la línea de entrega y los objetos de perfil de selección de destino.

Para obtener más información sobre estos objetos y sus métodos relacionados, consulta la siguiente tabla.


| Objeto       | Descripción   |
|---------------|-----------------|
| Campañas |  Este objeto representa la campaña publicitaria y se encuentra en la parte superior de la jerarquía del modelo de objetos de las campañas publicitarias. Este objeto identifica el tipo de campaña que se ejecuta (de pago, interna o comunitaria), el objetivo de la campaña, las líneas de entrega de la campaña y otros detalles. Cada campaña puede asociarse con una sola aplicación.<br/><br/>Para obtener más información acerca de los métodos relacionados con este objeto, consulta [Administración de las campañas publicitarias](manage-ad-campaigns.md).<br/><br/>**Nota**&nbsp;&nbsp;Tras crear una campaña publicitaria, puedes recuperar los datos de rendimiento de dicha campaña mediante el método [Obtener los datos de rendimiento de la campaña de anuncios](get-ad-campaign-performance-data.md) en la [API de análisis de Microsoft Store](access-analytics-data-using-windows-store-services.md).  |
| Líneas de entrega | Cada campaña tiene uno o más líneas de entrega, que se usan para comprar inventario y entregar los anuncios. Para cada línea de entrega, puedes establecer la selección del destino, el precio de oferta y decidir cuánto quieres gastar. Para ello, establece un presupuesto y vincular los creativos que quieres usar.<br/><br/>Para obtener más información acerca de los métodos relacionados con este objeto, consulta [Administración de las líneas de entrega de las campañas publicitarias](manage-delivery-lines-for-ad-campaigns.md). |
| Perfiles de selección de destino | Cada línea de entrega tiene un perfil de selección de destino que especifica los usuarios, las regiones y los tipos de inventario que quieres como destino. Los perfiles de selección de destino se pueden crear como una plantilla y compartirse entre diversas líneas de entrega.<br/><br/>Para obtener más información acerca de los métodos relacionados con este objeto, consulta [Administración de los perfiles de selección de destino de las campañas publicitarias](manage-targeting-profiles-for-ad-campaigns.md). |
| Creativos | Cada línea de entrega tiene uno o más creativos, que representan los anuncios que se muestran a los clientes como parte de la campaña. Un creativo puede estar asociado a una o varias líneas de entrega, incluso entre campañas de anuncios, siempre que represente siempre a la misma aplicación.<br/><br/>Para obtener más información acerca de los métodos relacionados con este objeto, consulta [Administración de los creativos de las campañas publicitarias](manage-creatives-for-ad-campaigns.md). |


El siguiente diagrama muestra la relación entre las campañas, las líneas de entrega, los perfiles de selección de destino y los creativos.

![Jerarquía de una campaña publicitaria](images/ad-campaign-hierarchy.png)

## <a name="code-example"></a>Ejemplo de código

El siguiente ejemplo de código muestra cómo obtener un token de acceso de Azure AD y llamar a la API de promociones de Microsoft Store desde una aplicación de consola C#. Para usar este ejemplo de código, asigna las variables *tenantId*, *clientId*, *clientSecret* y *appID* a los valores adecuados de tu escenario. En este ejemplo se necesita el [paquete Json.NET](https://www.newtonsoft.com/json) de Newtonsoft para deserializar los datos de JSON devueltos por la API de promociones de Microsoft Store.

[!code-csharp[PromotionsApi](./code/StoreServicesExamples_Promotions/cs/Program.cs#PromotionsApiExample)]

## <a name="related-topics"></a>Temas relacionados

* [Administrar campañas de ad](manage-ad-campaigns.md)
* [Administrar líneas de entrega para campañas de ad](manage-delivery-lines-for-ad-campaigns.md)
* [Administrar perfiles de destino para campañas de ad](manage-targeting-profiles-for-ad-campaigns.md)
* [Administrar creativos para campañas de ad](manage-creatives-for-ad-campaigns.md)
* [Obtención de datos de rendimiento de campaña de ad](get-ad-campaign-performance-data.md)


 
