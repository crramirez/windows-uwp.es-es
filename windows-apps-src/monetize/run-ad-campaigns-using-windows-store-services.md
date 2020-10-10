---
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: Use la API de promociones de Microsoft Store para administrar mediante programación campañas publicitarias promocionales para las aplicaciones que están registradas en su cuenta del centro de Partners de su organización.
title: Ejecutar campañas de ad mediante los servicios de almacenamiento
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, API de promociones de Microsoft Store, campañas de ad
ms.localizationpriority: medium
ms.openlocfilehash: 2be721137e6c09913eafd2c58bab07f1ae6f2728
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878518"
---
# <a name="run-ad-campaigns-using-store-services"></a>Ejecutar campañas de ad mediante los servicios de almacenamiento

Use la *API de promociones de Microsoft Store* para administrar mediante programación campañas publicitarias promocionales para las aplicaciones que están registradas en su cuenta del centro de Partners de su organización. Esta API le permite crear, actualizar y supervisar sus campañas y otros recursos relacionados, como la creación de destinos y creativos. Esta API es especialmente útil para los desarrolladores que crean grandes volúmenes de campañas y que quieren hacerlo sin usar el centro de Partners. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas provenientes de la aplicación o el servicio.

Los siguientes pasos describen el proceso de principio a fin:

1.  Asegúrate de que se hayan completado todos los [requisitos previos](#prerequisites).
2.  Antes de llamar a un método en la API de Microsoft Store promociones, [obtenga un token de acceso Azure ad](#obtain-an-azure-ad-access-token). Después de obtener un token, tiene 60 minutos para usar este token en las llamadas a la API de promociones de Microsoft Store antes de que expire el token. Después de que el token expire, puedes generar uno nuevo.
3.  [Llame a la API de promociones de Microsoft Store](#call-the-windows-store-promotions-api).

También puede crear y administrar campañas de ad con el centro de Partners, y también se puede tener acceso a las campañas de ad creadas mediante programación a través de la API de promociones de Microsoft Store en el centro de Partners. Para obtener más información acerca de la administración de campañas de AD en el centro de Partners, consulte [creación de una campaña de AD para la aplicación](./index.md).

> [!NOTE]
> Cualquier desarrollador con una cuenta del centro de Partners puede usar la API de promociones de Microsoft Store para administrar campañas de AD para sus aplicaciones. Las agencias de medios también pueden solicitar acceso a esta API para ejecutar campañas de AD en nombre de sus anunciantes. Si es una agencia de medios que quiere saber más sobre esta API o solicitar acceso a ella, envíe su solicitud a storepromotionsapi@microsoft.com .

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-promotions-api"></a>Paso 1: completar los requisitos previos para usar la API de promociones de Microsoft Store

Antes de empezar a escribir código para llamar a la API de Microsoft Store promociones, asegúrese de que ha completado los siguientes requisitos previos.

* Antes de poder crear e iniciar correctamente una campaña de ad mediante esta API, primero debe [crear una campaña de pago de pago mediante la página **campañas de ad** del centro de Partners](./index.md), y debe agregar al menos un instrumento de pago en esta página. Después de hacerlo, podrá crear correctamente líneas de entrega facturable para campañas de ad mediante esta API. Las líneas de entrega de las campañas de ad que cree con esta API facturarán automáticamente el instrumento de pago predeterminado elegido en la página **campañas de ad** del centro de Partners.

* Usted (o su organización) tiene que tener un directorio de Azure AD y el permiso de [Administrador global](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para el directorio. Si usa Microsoft 365 u otros servicios empresariales de Microsoft, ya tiene el directorio de Azure AD. De lo contrario, puede [crear un nuevo Azure ad en el centro de Partners](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sin cargo adicional.

* Debe asociar una aplicación Azure AD a la cuenta del centro de Partners, recuperar el identificador de inquilino y el identificador de cliente de la aplicación y generar una clave. La aplicación Azure AD representa la aplicación o el servicio del que desea llamar a la API de promociones de Microsoft Store. Necesita el identificador de inquilino, el identificador de cliente y la clave para obtener un token de acceso de Azure AD para pasar a la API.
    > [!NOTE]
    > Solo tiene que realizar esta tarea una vez. Una vez que tenga el identificador de inquilino, el identificador de cliente y la clave, puede volver a usarlos cada vez que tenga que crear un nuevo token de acceso de Azure AD.

Para asociar una aplicación Azure AD a la cuenta del centro de Partners y recuperar los valores necesarios:

1.  En el Centro de partners, [asocie la cuenta del Centro de partners de la organización con el directorio de Azure AD de la organización](../publish/associate-azure-ad-with-partner-center.md).

2.  A continuación, en la página **usuarios** de la sección **configuración** de la cuenta del centro de partners, [agregue la Azure ad aplicación](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) que representa la aplicación o el servicio que usará para administrar campañas de promoción para su cuenta del centro de Partners. Asegúrese de asignar a esta aplicación el rol **Administrador**. Si la aplicación aún no existe en el directorio de Azure AD, puede [crear una nueva aplicación de Azure AD en el Centro de partners](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  Vuelva a la página **Usuarios**, haga clic en el nombre de la aplicación de Azure AD para ir a la configuración de la aplicación y, a continuación, copie los valores de **Identificador de inquilino** e **Identificador de cliente**.

4. Haga clic en **Agregar nueva clave**. En la pantalla siguiente, copie el valor de **Clave**. Después de salir de esta página no podrá tener acceso de nuevo a esta información. Para más información, consulte [Administrar claves para una aplicación de Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Paso 2: Obtención de un token de acceso de Azure AD

Antes de llamar a cualquiera de los métodos de la API de Microsoft Store promociones, primero debe obtener un token de acceso Azure AD que pase al encabezado **Authorization** de cada método de la API. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes actualizar el token para que puedas continuar usándolo en llamadas adicionales a la API.

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

<span id="call-the-windows-store-promotions-api" />

## <a name="step-3-call-the-microsoft-store-promotions-api"></a>Paso 3: llamar a la API de promociones de Microsoft Store

Una vez que tenga un Azure AD token de acceso, estará listo para llamar a la API de promociones de Microsoft Store. Debes pasar el token de acceso al encabezado **Authorization** de cada método.

En el contexto de la API de promociones de Microsoft Store, una campaña de ad consta de un objeto de *campaña* que contiene información de alto nivel sobre la campaña y objetos adicionales que representan las *líneas de entrega*, los *perfiles de destino*y los *creativos* de la campaña de anuncios. La API incluye diferentes conjuntos de métodos agrupados por estos tipos de objeto. Para crear una campaña, normalmente se llama a un método POST diferente para cada uno de estos objetos. La API también proporciona métodos GET que puede usar para recuperar cualquier objeto y métodos PUT que puede usar para editar la campaña, la línea de entrega y los objetos de Perfil de destino.

Para obtener más información sobre estos objetos y sus métodos relacionados, vea la tabla siguiente.


| Object       | Descripción   |
|---------------|-----------------|
| Campañas |  Este objeto representa la campaña de AD y se encuentra en la parte superior de la jerarquía del modelo de objetos para las campañas de ad. Este objeto identifica el tipo de campaña que se está ejecutando (de pago, de casa o de comunidad), el objetivo de la campaña, las líneas de entrega de la campaña y otros detalles. Cada campaña solo se puede asociar a una aplicación.<br/><br/>Para obtener más información sobre los métodos relacionados con este objeto, vea [administrar campañas de ad](manage-ad-campaigns.md).<br/><br/>**Nota:** &nbsp; &nbsp; Después de crear una campaña de ad, puede recuperar los datos de rendimiento de la campaña mediante el método [obtener datos de rendimiento de campaña de ad](get-ad-campaign-performance-data.md) de la [API de Microsoft Store Analytics](access-analytics-data-using-windows-store-services.md).  |
| Líneas de entrega | Cada campaña tiene una o más líneas de entrega que se usan para comprar el inventario y entregar los anuncios. Para cada línea de entrega, puede establecer destinatarios, establecer el precio de la oferta y decidir cuánto desea gastar si establece un presupuesto y un vínculo a los creativos que quiere usar.<br/><br/>Para obtener más información sobre los métodos relacionados con este objeto, consulte [Administración de líneas de entrega para campañas de ad](manage-delivery-lines-for-ad-campaigns.md). |
| Perfiles de destino | Cada línea de entrega tiene un perfil de destino que especifica los usuarios, las zonas geográficas y los tipos de inventario a los que desea dirigirse. Los perfiles de destino se pueden crear como una plantilla y compartirse entre líneas de entrega.<br/><br/>Para obtener más información sobre los métodos relacionados con este objeto, consulte [Administración de perfiles de destino para campañas de ad](manage-targeting-profiles-for-ad-campaigns.md). |
| Creativos | Cada línea de entrega tiene uno o más creativos que representan los anuncios que se muestran a los clientes como parte de la campaña. Una creatividad puede estar asociada a una o varias líneas de entrega, incluso a través de campañas de anuncios, siempre que siempre represente la misma aplicación.<br/><br/>Para obtener más información sobre los métodos relacionados con este objeto, consulte [Administración de creativos para campañas de ad](manage-creatives-for-ad-campaigns.md). |


En el diagrama siguiente se ilustra la relación entre las campañas, las líneas de entrega, los perfiles de destino y los creativos.

![Jerarquía de campañas de ad](images/ad-campaign-hierarchy.png)

## <a name="code-example"></a>Ejemplo de código

En el ejemplo de código siguiente se muestra cómo obtener un token de acceso Azure AD y cómo llamar a la API de promociones de Microsoft Store desde una aplicación de consola de C#. Para usar este ejemplo de código, asigna las variables *tenantId*, *clientId*, *clientSecret* y *appID* a los valores adecuados de tu escenario. Este ejemplo requiere el [paquete JSON.net](https://www.newtonsoft.com/json) de Newtonsoft para deserializar los datos JSON devueltos por la API de promociones de Microsoft Store.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Promotions/cs/Program.cs" id="PromotionsApiExample":::

## <a name="related-topics"></a>Temas relacionados

* [Administrar campañas publicitarias](manage-ad-campaigns.md)
* [Administrar líneas de entrega para campañas de ad](manage-delivery-lines-for-ad-campaigns.md)
* [Administrar perfiles de destino para campañas de ad](manage-targeting-profiles-for-ad-campaigns.md)
* [Administrar creativos para campañas de ad](manage-creatives-for-ad-campaigns.md)
* [Obtener los datos de rendimiento de la campaña de anuncios](get-ad-campaign-performance-data.md)


 