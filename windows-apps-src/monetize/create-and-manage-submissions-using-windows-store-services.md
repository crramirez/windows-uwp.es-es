---
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: Use la API de envío de Microsoft Store para crear y administrar envíos de aplicaciones que están registrados en su cuenta del centro de partners mediante programación.
title: Crear y administrar usuarios
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store
ms.localizationpriority: medium
ms.openlocfilehash: 82e5ba10b8f0480f4d996840df26817e324111d8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613120"
---
# <a name="create-and-manage-submissions"></a>Crear y administrar usuarios


Use la *API de envío de Microsoft Store* consultar mediante programación y crear presentaciones para aplicaciones, complementos y paquete vuelos para la cuenta del centro de partners de su o su organización. Esta API es útil si tu cuenta administra muchas aplicaciones y complementos y quieres automatizar y optimizar el proceso de envío de estos activos. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas provenientes de la aplicación o el servicio.

Los siguientes pasos describen el proceso de principio a fin del uso de la API de envío de Microsoft Store:

1.  Asegúrate de que se hayan completado todos los [requisitos previos](#prerequisites).
3.  Antes de llamar a un método en la API de envío de Microsoft Store, [consigue un token de acceso de Azure AD](#obtain-an-azure-ad-access-token). Después de obtener un token, tienes 60 minutos para utilizar este token en llamadas a la API de envío de Microsoft Store antes de que el token expire. Después de que el token expire, puedes generar uno nuevo.
4.  [Llama a la API de envío de Microsoft Store](#call-the-windows-store-submission-api).

<span id="not_supported" />

> [!IMPORTANT]
> Si usa esta API para crear una presentación para una aplicación, un vuelo de paquete o un complemento, asegúrese de realizar más cambios en el envío mediante la API, en lugar de en el centro de partners. Si usa el centro de partners para cambiar un envío que creó originalmente mediante la API, ya no podrá cambiar o confirmar ese envío mediante la API. En algunos casos, el envío podría quedar en un estado de error en el que no se puede continuar con el proceso de envío. Si esto ocurre, debes eliminar el envío y crear uno nuevo.

> [!IMPORTANT]
> No puedes usar esta API para publicar envíos para [compras por volumen a través de Microsoft Store para Empresas y Microsoft Store para Educación](../publish/organizational-licensing.md) o publicar envíos para [aplicaciones LOB](../publish/distribute-lob-apps-to-enterprises.md) directamente para empresas. Para ambos escenarios, debe usar el envío de publicación en el centro de partners.

> [!NOTE]
> Esta API no puede usarse con aplicaciones o complementos que utilizan actualizaciones obligatorias de aplicaciones y complementos de consumibles gestionadas en la Store. Si usas la API de envío de Microsoft Store con una aplicación o complemento que usa una de estas funciones, la API devolverá un código de error 409. En este caso, debe usar Centro de partners para administrar los envíos de la aplicación o complemento.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-submission-api"></a>Paso 1: Complete los requisitos previos para usar la API de envío de Microsoft Store

Antes de empezar a escribir código para llamar a la API de envío de Microsoft Store, asegúrate de que has completado los siguientes requisitos previos.

* Tú (o tu organización) debes tener un directorio de Azure AD y un permiso de [Administrador global](https://go.microsoft.com/fwlink/?LinkId=746654) para el directorio. Si ya usas Office 365 u otros servicios empresariales de Microsoft, ya tienes un directorio de Azure AD. En caso contrario, puede [cree un nuevo anuncio de Azure en el centro de partners](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sin ningún cargo adicional.

* Debe [asociar una aplicación de Azure AD con su cuenta de centro de partners](#associate-an-azure-ad-application-with-your-windows-partner-center-account) y obtener su inquilino, identificador, identificador de cliente y la clave. Necesitas estos valores para obtener un token de acceso de Azure AD que usarás en llamadas a la API de envío de Microsoft Store.

* Preparación de la aplicación para su uso con la API de envío de Microsoft Store:

  * Si la aplicación aún no existe en el centro de partners, deberá [crear la aplicación mediante la reserva de su nombre en el centro de partners](https://msdn.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name). No se puede usar la API de envío de Microsoft Store para crear una aplicación en el centro de partners; debe trabajar en el centro de partners para crearlo y, a continuación, después de que puede usar la API para acceder a la aplicación y crear mediante programación de envíos para él. Sin embargo, puedes usar la API para crear complementos y paquetes piloto mediante programación antes de crear envíos para estos.

  * Antes de poder crear una presentación para una aplicación determinada mediante esta API, primero debe [crear una presentación de la aplicación en el centro de partners](https://msdn.microsoft.com/windows/uwp/publish/app-submissions), incluidos responder a la [age clasificaciones](https://msdn.microsoft.com/windows/uwp/publish/age-ratings) cuestionario. Después de realizar este paso, podrás crear mediante programación nuevos envíos para esta aplicación con la API. No es necesario crear un envío de complementos o de paquetes piloto antes de usar la API para esos tipos de envíos.

  * Si creas o actualizas un envío de aplicaciones y necesitas incluir un paquete de aplicaciones, [prepara el paquete de aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements).

  * Si vas a crear o actualizar el envío de una aplicación y necesitas incluir capturas de pantalla o imágenes para la descripción de Tienda, [prepara las imágenes y capturas de pantalla de la aplicación](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images).

  * Si vas a crear o actualizar un envío de complementos y debes incluir un icono, [prepara el icono](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon).

<span id="associate-an-azure-ad-application-with-your-windows-partner-center-account" />

### <a name="how-to-associate-an-azure-ad-application-with-your-partner-center-account"></a>Cómo asociar una aplicación de Azure AD con su cuenta de centro de partners

Para poder usar la API de envío de Microsoft Store, debe asociar una aplicación de Azure AD con su cuenta de centro de partners, recuperar al inquilino de identificador y el identificador de cliente para la aplicación y generar una clave. La aplicación de Azure AD representa la aplicación o el servicio desde donde quieres originar la llamada a la API de envío de Microsoft Store. Necesitas el identificador de inquilino, de cliente y la clave para obtener un token de acceso de Azure AD que se pasa a la API.

> [!NOTE]
> Solo debes realizar esta tarea una vez. Una vez que tengas el identificador de inquilino, de cliente y la clave, puedes volver a usarlos cuando necesites crear un nuevo token de acceso de Azure AD.

1.  En el centro de partners, [asociar la cuenta del centro de partners de su organización con el directorio de Azure AD de su organización](../publish/associate-azure-ad-with-partner-center.md).

2.  A continuación, en el **usuarios** página en el **configuración de la cuenta** sección del centro de partners, [agregar la aplicación de Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) que representa la aplicación o servicio que se va a usar para envíos de acceso para la cuenta del centro de partners. Asegúrate de que se asignas a esta aplicación el rol de **Administrador**. Si la aplicación no existe todavía en el directorio de Azure AD, puede [crear una nueva aplicación de Azure AD en el centro de partners](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).  

3.  Vuelve a la página **Usuarios**, haz clic en el nombre de la aplicación de Azure AD para ir a la configuración de la aplicación y copia los valores de **Identificador de inquilino** e **Identificador de cliente**.

4. Haz clic en **Agregar nueva clave**. En la siguiente pantalla, copia el valor **Clave**. No podrás acceder a esta información de nuevo después de salir de la página. Para obtener más información, consulta [Administrar claves para una aplicación de Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Paso 2: Obtener un token de acceso de Azure AD

Antes de llamar a cualquiera de los métodos en la API de envío de Microsoft Store, primero debes obtener un token de acceso de Azure AD para pasarlo al encabezado **Autorización** de cada método en la API. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes actualizar el token para que puedas continuar usándolo en llamadas adicionales a la API.

Para obtener el token de acceso, sigue las instrucciones en [Llamadas de servicio a servicio utilizando las credenciales del cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) para enviar un HTTP POST al punto de conexión ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Este es un ejemplo de solicitud.

```
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Para el *inquilino\_Id. de* en el URI de entrada y la *cliente\_id* y *cliente\_secreto* parámetros, especifique el inquilino ID, Id. de cliente y la clave para la aplicación que obtuvo en el centro de partners en la sección anterior. Para el parámetro *resource*, debes especificar ```https://manage.devcenter.microsoft.com```.

Una vez que expire el token de acceso, puedes actualizarlo siguiendo las instrucciones que se muestran [aquí](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

Para ver ejemplos que muestran cómo obtener un token de acceso con código C#, Java o Python, consulta los [ejemplos de código](#code-examples) de la API de envío de Microsoft Store.

<span id="call-the-windows-store-submission-api">

## <a name="step-3-use-the-microsoft-store-submission-api"></a>Paso 3: Usar la API de envío de Microsoft Store

Una vez que tengas un token de acceso de Azure AD, podrás llamar a métodos en la API de envío de Microsoft Store. La API incluye muchos métodos que se agrupan en escenarios de aplicaciones, complementos y paquetes piloto. Para crear o actualizar los envíos, normalmente llamas a varios métodos en la API de envío de Microsoft Store en un orden específico. Para obtener información sobre cada escenario y la sintaxis de cada método, consulta los artículos en la siguiente tabla.

> [!NOTE]
> Después de obtener un token de acceso, tienes 60 minutos para acceder a métodos en la API de envío de Microsoft Store antes de que el token expire.

| Escenario       | Descripción                                                                 |
|---------------|----------------------------------------------------------------------|
| Aplicaciones |  Recuperar datos para todas las aplicaciones que están registradas en su cuenta del centro de partners y creación presentaciones para las aplicaciones. Para obtener más información sobre estos métodos, consulta los siguientes artículos: <ul><li>[Obtener datos de la aplicación](get-app-data.md)</li><li>[Administrar envíos de aplicaciones](manage-app-submissions.md)</li></ul> |
| Complementos | Obtén, crea, elimina complementos para las aplicaciones y, a continuación, obtén, crea o elimina envíos para los complementos. Para obtener más información sobre estos métodos, consulta los siguientes artículos: <ul><li>[Administrar complementos](manage-add-ons.md)</li><li>[Administrar envíos de complemento](manage-add-on-submissions.md)</li></ul> |
| Paquetes piloto | Obtén, crea, elimina paquetes piloto para las aplicaciones y, a continuación, obtén, crea o elimina envíos para los paquetes piloto. Para obtener más información sobre estos métodos, consulta los siguientes artículos: <ul><li>[Administrar los vuelos de paquete](manage-flights.md)</li><li>[Administrar envíos de vuelos de paquete](manage-flight-submissions.md)</li></ul> |

<span id="code-samples"/>

## <a name="code-examples"></a>Ejemplos de código

Los siguientes artículos proporcionan ejemplos de código detallados que muestran cómo usar la API de envío de Microsoft Store en varios lenguajes diferentes:

* [C#ejemplo: envíos de aplicaciones, complementos y vuelos](csharp-code-examples-for-the-windows-store-submission-api.md)
* [C#ejemplo: envío de la aplicación con las opciones del juego y finalizadores](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Ejemplo de Java: envíos de aplicaciones, complementos y vuelos](java-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplo de Java: envío de la aplicación con las opciones del juego y finalizadores](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Ejemplo de Python: envíos de aplicaciones, complementos y vuelos](python-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplo de Python: envío de la aplicación con las opciones del juego y finalizadores](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>Módulo de StoreBroker PowerShell

Como alternativa a llamar a la API de envío de Microsoft Store directamente, también proporcionamos un módulo de PowerShell de código abierto que implementa una interfaz de línea de comandos en la parte superior de la API. Este módulo se denomina [StoreBroker](https://aka.ms/storebroker). Puedes usar este módulo para administrar tus envíos de aplicaciones, paquetes piloto y complementos desde la línea de comandos en lugar de llamar directamente a la API de envío de Microsoft Store o, simplemente, puedes examinar el origen para ver más ejemplos de cómo llamar a esta API. El módulo StoreBroker se usa activamente dentro de Microsoft como método principal para enviar muchas aplicaciones propias a la Store.

Para obtener más información, consulta nuestra [página de StoreBroker en GitHub](https://aka.ms/storebroker).

## <a name="troubleshooting"></a>Solución de problemas

| Problema      | Resolución                                          |
|---------------|---------------------------------------------|
| Después de llamar a la API de envío de Microsoft Store de PowerShell, los datos de respuesta para la API está dañados si se convierte en formato JSON en un objeto de PowerShell mediante la cmdlet [ConvertFrom Json](https://technet.microsoft.com/library/hh849898.aspx) y, a continuación, volver al formato JSON mediante la cmdlet [ConvertTo Json](https://technet.microsoft.com/library/hh849922.aspx). |  De manera predeterminada, el parámetro de *-Profundidad* para la cmdlet [ConvertTo Json](https://technet.microsoft.com/library/hh849922.aspx) se establece en 2 niveles de objetos, lo cual es demasiado superficial para la mayoría de los objetos JSON devueltos por la API de envío de Microsoft Store. Cuando llames a la cmdlet [ConvertTo Json](https://technet.microsoft.com/library/hh849922.aspx), establece el parámetro de *-Profundidad* en un número mayor, como 20. |

## <a name="additional-help"></a>Ayuda adicional

Si tienes preguntas sobre la API de envío de Microsoft Store o necesitas ayuda para administrar tus envíos con esta API, usa los siguientes recursos:

* Pregunta en nuestros [foros](https://social.msdn.microsoft.com/Forums/windowsapps/home?forum=wpsubmit).
* Visite nuestro [página de soporte técnico](https://developer.microsoft.com/windows/support) y solicite una de las opciones de soporte técnico asistido para centro de partners. Si se le pide que elija un tipo y categoría de problema, elige **Envío y certificación de aplicaciones** y **Enviar una aplicación**, respectivamente.  

## <a name="related-topics"></a>Temas relacionados

* [Obtener datos de la aplicación](get-app-data.md)
* [Administrar envíos de aplicaciones](manage-app-submissions.md)
* [Administrar complementos](manage-add-ons.md)
* [Administrar envíos de complemento](manage-add-on-submissions.md)
* [Administrar los vuelos de paquete](manage-flights.md)
* [Administrar envíos de vuelos de paquete](manage-flight-submissions.md)
