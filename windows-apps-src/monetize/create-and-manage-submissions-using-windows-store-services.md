---
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: Use la API de envío de Microsoft Store para crear y administrar mediante programación envíos de aplicaciones que estén registradas en su cuenta del centro de Partners.
title: Crear y administrar usuarios
ms.date: 06/04/2018
ms.topic: article
keywords: API de envío de Microsoft Store de Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: af0d36f2fa76fe9bb5bd253436f3d434a860e7ec
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155629"
---
# <a name="create-and-manage-submissions"></a>Crear y administrar usuarios

Use la *API de envío de Microsoft Store* para consultar y crear envíos de aplicaciones, complementos y paquetes piloto mediante programación para su cuenta del centro de Partners de su organización. Esta API es útil si tu cuenta administra muchas aplicaciones y complementos y quieres automatizar y optimizar el proceso de envío de estos activos. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas provenientes de la aplicación o el servicio.

En los pasos siguientes se describe el proceso de un extremo a otro del uso de la API de envío de Microsoft Store:

1.  Asegúrate de que se hayan completado todos los [requisitos previos](#prerequisites).
3.  Antes de llamar a un método en la API de envío de Microsoft Store, [obtenga un token de acceso Azure ad](#obtain-an-azure-ad-access-token). Después de obtener un token, tiene 60 minutos para usar este token en las llamadas a la API de envío de Microsoft Store antes de que expire el token. Después de que el token expire, puedes generar uno nuevo.
4.  [Llame a la API de envío de Microsoft Store](#call-the-windows-store-submission-api).

<span id="not_supported" />

> [!IMPORTANT]
> Si usa esta API para crear un envío para una aplicación, paquete piloto o complemento, asegúrese de realizar más cambios en el envío solo mediante la API, en lugar de hacerlo en el centro de Partners. Si usa el centro de partners para cambiar un envío que creó originalmente con la API, ya no podrá cambiar o confirmar el envío mediante la API. En algunos casos, el envío podría quedar en un estado de error en el que no se puede continuar en el proceso de envío. Si esto ocurre, debe eliminar el envío y crear un nuevo envío.

> [!IMPORTANT]
> No puede usar esta API para publicar envíos de [compras por volumen a través de la Microsoft Store para empresas y Microsoft Store para educación](../publish/organizational-licensing.md) o para publicar envíos de [aplicaciones de LOB](../publish/distribute-lob-apps-to-enterprises.md) directamente en las empresas. En ambos casos, debe usar publicar el envío en el centro de Partners.

> [!NOTE]
> Esta API no se puede usar con aplicaciones o complementos que usan actualizaciones de aplicaciones obligatorias y complementos consumibles administrados por el almacenamiento. Si usa la API de envío de Microsoft Store con una aplicación o un complemento que usa una de estas características, la API devolverá un código de error 409. En este caso, debe usar el centro de partners para administrar los envíos de la aplicación o el complemento.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-submission-api"></a>Paso 1: completar los requisitos previos para usar la API de envío de Microsoft Store

Antes de empezar a escribir código para llamar a la API de envío de Microsoft Store, asegúrese de que ha completado los requisitos previos siguientes.

* Usted (o su organización) tiene que tener un directorio de Azure AD y el permiso de [Administrador global](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para el directorio. Si usa Microsoft 365 u otros servicios empresariales de Microsoft, ya tiene el directorio de Azure AD. De lo contrario, puede [crear un nuevo Azure ad en el centro de Partners](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sin cargo adicional.

* Tiene que [asociar una aplicación Azure AD a su cuenta del Centro de partners](#associate-an-azure-ad-application-with-your-windows-partner-center-account) y obtener el identificador de inquilino, el identificador de cliente y la clave. Necesitará estos valores para obtener un token de acceso de Azure AD, que usará en las llamadas a la API de envío de Microsoft Store.

* Preparación de la aplicación para su uso con la API de envío de Microsoft Store:

  * Si la aplicación aún no existe en el centro de Partners, debe [reservar su nombre en el centro de partners para crear la aplicación](../publish/create-your-app-by-reserving-a-name.md). No se puede usar el Microsoft Store API de envío para crear una aplicación en el centro de Partners. debe trabajar en el centro de partners para crearlo y, después, puede usar la API para tener acceso a la aplicación y crear envíos para ella mediante programación. Sin embargo, puedes usar la API para crear complementos y paquetes piloto mediante programación antes de crear envíos para estos.

  * Antes de poder crear un envío para una aplicación determinada mediante esta API, primero debe [crear un envío para la aplicación en el centro de Partners](../publish/app-submissions.md), lo que incluye responder al cuestionario de [clasificación por edades](../publish/age-ratings.md) . Después de realizar este paso, podrás crear mediante programación nuevos envíos para esta aplicación con la API. No es necesario crear un envío de complementos o de paquetes piloto antes de usar la API para esos tipos de envíos.

  * Si creas o actualizas un envío de aplicaciones y necesitas incluir un paquete de aplicaciones, [prepara el paquete de aplicaciones](../publish/app-package-requirements.md).

  * Si vas a crear o actualizar el envío de una aplicación y necesitas incluir capturas de pantalla o imágenes para la descripción de Tienda, [prepara las imágenes y capturas de pantalla de la aplicación](../publish/app-screenshots-and-images.md).

  * Si vas a crear o actualizar un envío de complementos y debes incluir un icono, [prepara el icono](../publish/create-add-on-store-listings.md).

<span id="associate-an-azure-ad-application-with-your-windows-partner-center-account" />

### <a name="how-to-associate-an-azure-ad-application-with-your-partner-center-account"></a>Cómo asociar una aplicación de Azure AD con la cuenta del Centro de partners

Antes de poder usar la API de envío de Microsoft Store, debe asociar una aplicación Azure AD a su cuenta del centro de Partners, recuperar el identificador de inquilino y el identificador de cliente de la aplicación y generar una clave. La aplicación Azure AD representa la aplicación o el servicio del que desea llamar a la API de envío de Microsoft Store. Necesita el identificador de inquilino, el identificador de cliente y la clave para obtener un token de acceso de Azure AD para pasar a la API.

> [!NOTE]
> Solo tiene que realizar esta tarea una vez. Una vez que tenga el identificador de inquilino, el identificador de cliente y la clave, puede volver a usarlos cada vez que tenga que crear un nuevo token de acceso de Azure AD.

1.  En el Centro de partners, [asocie la cuenta del Centro de partners de la organización con el directorio de Azure AD de la organización](../publish/associate-azure-ad-with-partner-center.md).

2.  A continuación, en la página **Usuarios** en la sección **Configuración** de la cuenta del Centro de partners, [agregue la aplicación de Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) que representa la aplicación o el servicio que usará para obtener acceso a los envíos de la cuenta del Centro de partners. Asegúrese de asignar a esta aplicación el rol **Administrador**. Si la aplicación aún no existe en el directorio de Azure AD, puede [crear una nueva aplicación de Azure AD en el Centro de partners](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).  

3.  Vuelva a la página **Usuarios**, haga clic en el nombre de la aplicación de Azure AD para ir a la configuración de la aplicación y, a continuación, copie los valores de **Identificador de inquilino** e **Identificador de cliente**.

4. Haga clic en **Agregar nueva clave**. En la pantalla siguiente, copie el valor de **Clave**. Después de salir de esta página no podrá tener acceso de nuevo a esta información. Para más información, consulte [Administrar claves para una aplicación de Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Paso 2: Obtención de un token de acceso de Azure AD

Antes de llamar a cualquiera de los métodos de la API de envío de Microsoft Store, primero debe obtener un token de acceso Azure AD que pase al encabezado **Authorization** de cada método de la API. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes actualizar el token para que puedas continuar usándolo en llamadas adicionales a la API.

Para obtener el token de acceso, sigue las instrucciones en [Llamadas de servicio a servicio utilizando las credenciales del cliente](/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow) para enviar un HTTP POST al punto de conexión ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Este es un ejemplo de solicitud.

```json
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

Para obtener ejemplos que muestran cómo obtener un token de acceso mediante código de C#, Java o Python, consulte los [ejemplos de código](#code-examples)de API de envío de Microsoft Store.

<span id="call-the-windows-store-submission-api">

## <a name="step-3-use-the-microsoft-store-submission-api"></a>Paso 3: Uso de la API de envío de Microsoft Store

Una vez que tenga un Azure AD token de acceso, puede llamar a los métodos en la API de envío de Microsoft Store. La API incluye muchos métodos que se agrupan en escenarios de aplicaciones, complementos y paquetes piloto. Para crear o actualizar envíos, normalmente se llama a varios métodos en la API de envío de Microsoft Store en un orden específico. Para obtener información sobre cada escenario y la sintaxis de cada método, consulta los artículos en la siguiente tabla.

> [!NOTE]
> Después de obtener un token de acceso, tiene 60 minutos para llamar a métodos en la API de envío Microsoft Store antes de que expire el token.

| Escenario       | Descripción                                                                 |
|---------------|----------------------------------------------------------------------|
| Aplicaciones |  Recupere los datos de todas las aplicaciones registradas en su cuenta del centro de Partners y cree envíos para las aplicaciones. Para obtener más información sobre estos métodos, consulta los siguientes artículos: <ul><li>[Obtención de datos de la aplicación](get-app-data.md)</li><li>[Administración de envíos de aplicaciones](manage-app-submissions.md)</li></ul> |
| Complementos | Obtén, crea, elimina complementos para las aplicaciones y, a continuación, obtén, crea o elimina envíos para los complementos. Para obtener más información sobre estos métodos, consulta los siguientes artículos: <ul><li>[Administración de complementos](manage-add-ons.md)</li><li>[Manage add-on submissions (Administrar envíos de complemento)](manage-add-on-submissions.md)</li></ul> |
| Paquetes piloto | Obtén, crea, elimina paquetes piloto para las aplicaciones y, a continuación, obtén, crea o elimina envíos para los paquetes piloto. Para obtener más información sobre estos métodos, consulta los siguientes artículos: <ul><li>[Administración de paquetes piloto](manage-flights.md)</li><li>[Manage package flight submissions (Administrar envíos de paquetes piloto)](manage-flight-submissions.md)</li></ul> |

<span id="code-samples"/>

## <a name="code-examples"></a>Ejemplos de código

En los artículos siguientes se proporcionan ejemplos de código detallados que muestran cómo usar la API de envío de Microsoft Store en varios lenguajes de programación diferentes:

* [Ejemplo de C#: envíos de aplicaciones, complementos y pilotos](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplo de C#: envío de aplicación con opciones de juego y tráileres](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Ejemplo de Java: envíos de aplicaciones, complementos y pilotos](java-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplo de Java: envío de aplicación con opciones de juego y tráileres](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Ejemplo de Python: envíos de aplicaciones, complementos y pilotos](python-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplo de Python: envío de aplicación con opciones de juego y tráileres](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>Módulo de PowerShell de StoreBroker

Como alternativa a llamar a la API de envío de Microsoft Store directamente, también proporcionamos un módulo de PowerShell de código abierto que implementa una interfaz de línea de comandos sobre la API. Este módulo se denomina [StoreBroker](https://github.com/Microsoft/StoreBroker). Puede usar este módulo para administrar los envíos de aplicaciones, vuelos y complementos desde la línea de comandos en lugar de llamar directamente a la API de envío de Microsoft Store, o simplemente puede examinar el origen para ver más ejemplos de cómo llamar a esta API. El módulo StoreBroker se usa activamente en Microsoft como la manera principal en que se envían a la tienda muchas aplicaciones propias.

Para obtener más información, consulte nuestra [Página de StoreBroker en github](https://github.com/Microsoft/StoreBroker).

## <a name="troubleshooting"></a>Solución de problemas

| Incidencia      | Solución                                          |
|---------------|---------------------------------------------|
| Después de llamar a la API de envío de Microsoft Store desde PowerShell, los datos de respuesta de la API están dañados si se convierten del formato JSON a un objeto de PowerShell mediante el cmdlet [ConvertFrom-JSON](/powershell/module/5.1/microsoft.powershell.utility/ConvertFrom-Json) y luego vuelven al formato JSON mediante el cmdlet [ConvertTo-JSON](/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json) . |  De forma predeterminada, el parámetro *-Depth* del cmdlet [ConvertTo-JSON](/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json) se establece en 2 niveles de objetos, que es demasiado superficial para la mayoría de los objetos JSON devueltos por la API de envío de Microsoft Store. Cuando llames a la cmdlet [ConvertTo Json](/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json), establece el parámetro de *-Profundidad* en un número mayor, como 20. |

## <a name="additional-help"></a>Ayuda adicional

Si tiene alguna pregunta sobre la API de envío de Microsoft Store o necesita ayuda para administrar los envíos con esta API, use los siguientes recursos:

* Pregunta en nuestros [foros](https://social.msdn.microsoft.com/Forums/windowsapps/home?forum=wpsubmit).
* Visite nuestra [Página de soporte técnico](https://developer.microsoft.com/windows/support) y solicite una de las opciones de soporte técnico asistido para el centro de Partners. Si se le pide que elija un tipo y categoría de problema, elige **Envío y certificación de aplicaciones** y **Enviar una aplicación**, respectivamente.  

## <a name="related-topics"></a>Temas relacionados

* [Obtención de datos de la aplicación](get-app-data.md)
* [Administración de envíos de aplicaciones](manage-app-submissions.md)
* [Administración de complementos](manage-add-ons.md)
* [Manage add-on submissions (Administrar envíos de complemento)](manage-add-on-submissions.md)
* [Administración de paquetes piloto](manage-flights.md)
* [Manage package flight submissions (Administrar envíos de paquetes piloto)](manage-flight-submissions.md)