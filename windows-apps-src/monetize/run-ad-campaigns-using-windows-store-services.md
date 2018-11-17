---
author: Xansky
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: Usar la API de promociones de Microsoft Store para administrar mediante programación campañas de anuncios promocionales para las aplicaciones que están registradas en la cuenta del centro de partners de tu o tu organización.
title: Ejecutar campañas de anuncios con los servicios de la Store
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store promotions API, API de promociones de Microsoft Store, ad campaigns, campañas de anuncios
ms.localizationpriority: medium
ms.openlocfilehash: 81e77fb01347c2a6ec2630e08f3a00ee6cf47de7
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2018
ms.locfileid: "7151927"
---
# <a name="run-ad-campaigns-using-store-services"></a>Ejecutar campañas de anuncios con los servicios de la Store

Usar la *API de promociones de Microsoft Store* para administrar mediante programación campañas de anuncios promocionales para las aplicaciones que están registradas en la cuenta del centro de partners de tu o tu organización. Esta API permite crear, actualizar y supervisar las campañas y otros activos relacionados, como la selección de destino y los creativos. Esta API es especialmente útil para los desarrolladores que crean grandes volúmenes de campañas y que quieran hacerlo sin usar el centro de partners. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas procedentes de la aplicación o el servicio.

Los siguientes pasos describen el proceso de principio a fin:

1.  Asegúrate de que se hayan completado todos los [requisitos previos](#prerequisites).
2.  Antes de llamar a un método en la API de promociones de Microsoft Store, [consigue un token de acceso de Azure AD](#obtain-an-azure-ad-access-token). Tras obtener un token, tienes 60minutos para utilizar dicho token en llamadas a la API de promociones de Microsoft Store antes de que expire. Una vez que el token expire, puedes generar uno nuevo.
3.  [Llama a la API de promociones de Microsoft Store](#call-the-windows-store-promotions-api).

Como alternativa, puede crear y administrar las campañas publicitarias mediante el centro de partners y las campañas publicitarias que crees mediante programación a través de la API también puede tener acceso en el centro de partners de promociones de Microsoft Store. Para obtener más información acerca de cómo administrar las campañas de anuncios del centro de partners, consulta [crear una campaña publicitaria para tu aplicación](../publish/create-an-ad-campaign-for-your-app.md).

> [!NOTE]
> Cualquier desarrollador con una cuenta del centro de partners puede usar la API de promociones de Microsoft Store para administrar las campañas publicitarias para sus aplicaciones. Las agencias de medios también pueden solicitar acceso a esta API para ejecutar campañas publicitarias en nombre de sus anunciantes. Si tienes una agencia de medios y quieres saber más sobre esta API o solicitar acceso a ella, envía tu solicitud a storepromotionsapi@microsoft.com.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-promotions-api"></a>Paso 1: Completa los requisitos previos para usar la API de promociones de Microsoft Store

Antes de empezar a escribir código para llamar a la API de promociones de Microsoft Store, asegúrate de que has completado los siguientes requisitos previos.

* Antes de correctamente puede crear e iniciar una campaña de anuncios con esta API, primero debe [crear una campaña de anuncios de pago con la página de **las campañas de anuncios** del centro de partners](../publish/create-an-ad-campaign-for-your-app.md)y tienes que agregar al menos un instrumento de pago en esta página. Después de hacer esto, podrás crear correctamente líneas de entrega facturables para campañas de anuncios con esta API. Las líneas de entrega para campañas de anuncios que creas con esta API facturarán siempre automáticamente al instrumento de pago elegido en la página de **las campañas de anuncios** en el centro de partners.

* Tú (o tu organización) debes tener un directorio de Azure AD y un permiso de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para el directorio. Si ya usas Office365 u otros servicios empresariales de Microsoft, ya tienes un directorio de AzureAD. De lo contrario, puedes [crear un nuevo Azure AD en el centro de partners](../publish/associate-azure-ad-with-dev-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sin ningún coste adicional.

* Debes asociar una aplicación de Azure AD con tu cuenta del centro de partners, recuperar el identificador de inquilino y el identificador de cliente para la aplicación y generar una clave. La aplicación de Azure AD representa la aplicación o el servicio desde el que quieres llamar a la API de promociones de Microsoft Store. Necesitas el identificador de inquilino, el de cliente y la clave para obtener un token de acceso de Azure AD que se pasará a la API.
    > [!NOTE]
    > Solo debes realizar esta tarea una vez. Una vez que tengas el identificador de inquilino, el identificador de cliente y la clave, puedes volver a usarlos siempre que necesites crear un nuevo token de acceso de Azure AD.

Para asociar una aplicación de Azure AD con tu cuenta del centro de partners y recuperar los valores necesarios:

1.  En el centro de partners, [asociar la cuenta del centro de partners de tu organización con el directorio de Azure AD de tu organización](../publish/associate-azure-ad-with-dev-center.md).

2.  A continuación, desde la página de **usuarios** en la sección de **configuración de la cuenta** del centro de partners, [Agregar la aplicación de Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) que representa la aplicación o el servicio que usarás para administrar las campañas de promoción para tu cuenta del centro de partners. Asegúrate de que se asignas a esta aplicación el rol de **Administrador**. Si la aplicación no existe aún en el directorio de Azure AD, puedes [crear una nueva aplicación de Azure AD en el centro de partners](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  Vuelve a la página **Usuarios**, haz clic en el nombre de la aplicación de Azure AD para ir a la configuración de la aplicación y copia los valores de **Identificador de inquilino** e **Identificador de cliente**.

4. Haz clic en **Agregar nueva clave**. En la siguiente pantalla, copia el valor **Clave**. No podrás acceder a esta información de nuevo después de salir de la página. Para obtener más información, consulta [Administrar claves para una aplicación de Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Paso 2: Obtener un token de acceso de Azure AD

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

El valor de *tenant\_id* de POST URI y los parámetros *client\_id* y *client\_secret* , especifica el identificador de inquilino, Id. de cliente y la clave de la aplicación que recuperaste del centro de partners en la sección anterior. Para el parámetro *resource*, debes especificar ```https://manage.devcenter.microsoft.com```.

Una vez que expire el token de acceso, puedes actualizarlo siguiendo las instrucciones que se muestran [aquí](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-promotions-api" />

## <a name="step-3-call-the-microsoft-store-promotions-api"></a>Paso 3: Llama a la API de promociones de Microsoft Store

Cuando tengas un token de acceso de Azure AD, podrás llamar a la API de promociones de Microsoft Store. Debes pasar el token de acceso al encabezado **Authorization** de cada método.

En el contexto de la API de promociones de Microsoft Store, una campaña publicitaria consta de un objeto *campaña*, que contiene información de alto nivel sobre la campaña, y otros objetos que representan las *líneas de entrega*, los *perfiles de selección de destino* y los *creativos* para la campaña publicitaria. La API incluye diferentes conjuntos de los métodos que se agrupan por estos tipos de objetos. Para crear una campaña, normalmente llamas a un método POST diferente para cada uno de estos objetos. La API también proporciona métodos GET que puedes usar para recuperar cualquier objeto, y métodos PUT que sirven para editar la campaña, la línea de entrega y los objetos de perfil de selección de destino.

Para obtener más información sobre estos objetos y sus métodos relacionados, consulta la siguiente tabla.


| Objeto       | Descripción   |
|---------------|-----------------|
| Campañas |  Este objeto representa la campaña publicitaria y se encuentra en la parte superior de la jerarquía del modelo de objetos de las campañas publicitarias. Este objeto identifica el tipo de campaña que se ejecuta (de pago, interna o comunitaria), el objetivo de la campaña, las líneas de entrega de la campaña y otros detalles. Cada campaña puede asociarse con una sola aplicación.<br/><br/>Para obtener más información acerca de los métodos relacionados con este objeto, consulta [Administración de las campañas publicitarias](manage-ad-campaigns.md).<br/><br/>**Nota**&nbsp;&nbsp;Tras crear una campaña publicitaria, puedes recuperar los datos de rendimiento de dicha campaña mediante el método [Obtener los datos de rendimiento de la campaña de anuncios](get-ad-campaign-performance-data.md) en la [API de análisis de Microsoft Store](access-analytics-data-using-windows-store-services.md).  |
| Líneas de entrega | Cada campaña tiene uno o más líneas de entrega, que se usan para comprar inventario y entregar los anuncios. Para cada línea de entrega, puedes establecer la selección de destino y el precio de la oferta, así como cuánto quieres gastar mediante el establecimiento de un presupuesto y la vinculación a los creativos que quieras usar.<br/><br/>Para obtener más información acerca de los métodos relacionados con este objeto, consulta [Administración de las líneas de entrega de las campañas publicitarias](manage-delivery-lines-for-ad-campaigns.md). |
| Perfiles de selección de destino | Cada línea de entrega tiene un perfil de selección de destino que especifica los usuarios, las regiones y los tipos de inventario que quieres como destino. Los perfiles de selección de destino se pueden crear como una plantilla y compartirse entre diversas líneas de entrega.<br/><br/>Para obtener más información acerca de los métodos relacionados con este objeto, consulta [Administración de los perfiles de selección de destino de las campañas publicitarias](manage-targeting-profiles-for-ad-campaigns.md). |
| Creativos | Cada línea de entrega tiene uno o más creativos, que representan los anuncios que se muestran a los clientes como parte de la campaña. Un creativo puede estar asociada con una o más líneas de entrega, incluso en distintas campañas publicitarias, siempre que represente en todo momento la misma aplicación.<br/><br/>Para obtener más información acerca de los métodos relacionados con este objeto, consulta [Administración de los creativos de las campañas publicitarias](manage-creatives-for-ad-campaigns.md). |


El siguiente diagrama muestra la relación entre las campañas, las líneas de entrega, los perfiles de selección de destino y los creativos.

![Jerarquía de una campaña publicitaria](images/ad-campaign-hierarchy.png)

## <a name="code-example"></a>Ejemplo de código

El siguiente ejemplo de código muestra cómo obtener un token de acceso de Azure AD y llamar a la API de promociones de Microsoft Store desde una aplicación de consola C#. Para usar este ejemplo de código, asigna las variables *tenantId*, *clientId*, *clientSecret* y *appID* a los valores adecuados para tu escenario. En este ejemplo se necesita el [paquete Json.NET](http://www.newtonsoft.com/json) de Newtonsoft para deserializar los datos de JSON devueltos por la API de promociones de Microsoft Store.

[!code-cs[PromotionsApi](./code/StoreServicesExamples_Promotions/cs/Program.cs#PromotionsApiExample)]

## <a name="related-topics"></a>Artículos relacionados

* [Administración de las campañas publicitarias](manage-ad-campaigns.md)
* [Administración de las líneas de entrega de las campañas publicitarias](manage-delivery-lines-for-ad-campaigns.md)
* [Administración de los perfiles de selección de destino de las campañas publicitarias](manage-targeting-profiles-for-ad-campaigns.md)
* [Administrar creativos de campañas de anuncios](manage-creatives-for-ad-campaigns.md)
* [Obtener los datos de rendimiento de la campaña de anuncios](get-ad-campaign-performance-data.md)


 
