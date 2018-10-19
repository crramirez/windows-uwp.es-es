---
author: Xansky
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: Usa la API de envío de Microsoft Store para crear y administrar mediante programación los envíos para las aplicaciones que estén registradas en tu cuenta del Centro de desarrollo de Windows.
title: Crear y administrar envíos
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store
ms.localizationpriority: medium
ms.openlocfilehash: a6ba5678fc7b66e15d0e189cd75792f97c2353db
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "4950207"
---
# <a name="create-and-manage-submissions"></a>Crear y administrar envíos


Usa la *API de envío de Microsoft Store* para efectuar consultas mediante programación y crear envíos de aplicaciones, complementos y paquetes piloto para tu cuenta del Centro de desarrollo de Windows o la de tu organización. Esta API es útil si tu cuenta administra muchas aplicaciones o complementos, y quieres automatizar y optimizar el proceso de envío para estos activos. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas provenientes de la aplicación o el servicio.

Los siguientes pasos describen el proceso de principio a fin del uso de la API de envío de Microsoft Store:

1.  Asegúrate de que se hayan completado todos los [requisitos previos](#prerequisites).
3.  Antes de llamar a un método en la API de envío de Microsoft Store, [consigue un token de acceso de Azure AD](#obtain-an-azure-ad-access-token). Después de obtener un token, tienes 60 minutos para utilizar este token en llamadas a la API de envío de Microsoft Store antes de que el token expire. Después de que el token expire, puedes generar uno nuevo.
4.  [Llama a la API de envío de Microsoft Store](#call-the-windows-store-submission-api).

<span id="not_supported" />

> [!IMPORTANT]
> Si usas esta API para crear un envío para una aplicación, un paquete piloto o un complemento, asegúrate de realizar más cambios en el envío solo mediante la API, en lugar de en el panel del Centro de desarrollo. Si usas el panel para cambiar un envío que creaste originalmente mediante la API, ya no podrás cambiar o confirmar el envío con la API. En algunos casos, el envío podría quedar en un estado de error en el que no se puede continuar con el proceso de envío. Si esto ocurre, debes eliminar el envío y crear uno nuevo.

> [!IMPORTANT]
> No puedes usar esta API para publicar envíos para [compras por volumen a través de Microsoft Store para Empresas y Microsoft Store para Educación](../publish/organizational-licensing.md) o publicar envíos para [aplicaciones LOB](../publish/distribute-lob-apps-to-enterprises.md) directamente para empresas. Para estos dos escenarios, debes usar el panel del Centro de desarrollo de Windows para publicar el envío.

> [!NOTE]
> Esta API no puede usarse con aplicaciones o complementos que utilizan actualizaciones obligatorias de aplicaciones y complementos de consumibles gestionadas en la Store. Si usas la API de envío de Microsoft Store con una aplicación o complemento que usa una de estas funciones, la API devolverá un código de error 409. En este caso, debes usar el panel para administrar los envíos para la aplicación o el complemento.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-submission-api"></a>Paso 1: Completar requisitos previos para usar la API de envío de Microsoft Store

Antes de empezar a escribir código para llamar a la API de envío de Microsoft Store, asegúrate de que has completado los siguientes requisitos previos.

* Tú (o tu organización) debes tener un directorio de Azure AD y un permiso de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para el directorio. Si ya usas Office365 u otros servicios empresariales de Microsoft, ya tienes un directorio de AzureAD. De lo contrario, puedes [crear un nuevo Azure AD desde el Centro de desarrollo](../publish/associate-azure-ad-with-dev-center.md#create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account) sin ningún coste adicional.

* Debes [asociar una aplicación de Azure AD con tu cuenta del Centro de desarrollo de Windows](#associate-an-azure-ad-application-with-your-windows-dev-center-account) y obtener tu identificador de inquilino, identificador de cliente y la clave. Necesitas estos valores para obtener un token de acceso de Azure AD que usarás en llamadas a la API de envío de Microsoft Store.

* Preparación de la aplicación para su uso con la API de envío de Microsoft Store:

  * Si tu aplicación todavía no existe en el Centro de desarrollo, [crea tu aplicación en el panel del Centro de desarrollo](https://msdn.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name). No puedes usar la API de envío de Microsoft Store para crear una aplicación en el Centro de desarrollo; debes crear tu aplicación con el panel y, a continuación, puedes usar la API para acceder a la aplicación y crear envíos para este mediante programación. Sin embargo, puedes usar la API para crear complementos y paquetes piloto mediante programación antes de crear envíos para estos.

  * Antes de crear un envío para una aplicación determinada mediante esta API, primero debes [crear un envío de la aplicación en el panel del Centro de desarrollo](https://msdn.microsoft.com/windows/uwp/publish/app-submissions), incluidas las respuestas a las preguntas de la [clasificación por edades](https://msdn.microsoft.com/windows/uwp/publish/age-ratings). Después de realizar este paso, podrás crear mediante programación nuevos envíos para esta aplicación con la API. No es necesario crear un envío de complementos o de paquetes piloto antes de usar la API para esos tipos de envíos.

  * Si creas o actualizas un envío de aplicaciones y necesitas incluir un paquete de aplicaciones, [prepara el paquete de aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements).

  * Si vas a crear o actualizar el envío de una aplicación y necesitas incluir capturas de pantalla o imágenes para la descripción de Tienda, [prepara las imágenes y capturas de pantalla de la aplicación](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images).

  * Si vas a crear o actualizar un envío de complementos y debes incluir un icono, [prepara el icono](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon).

<span id="associate-an-azure-ad-application-with-your-windows-dev-center-account" />

### <a name="how-to-associate-an-azure-ad-application-with-your-windows-dev-center-account"></a>Asociación de una aplicación de Azure AD a tu cuenta del Centro de desarrollo de Windows

Antes de poder usar la API de envío de Microsoft Store, debes asociar una aplicación de Azure AD con tu cuenta del Centro de desarrollo, recuperar el identificador de inquilino y de cliente para la aplicación y generar una clave. La aplicación de Azure AD representa la aplicación o el servicio desde donde quieres originar la llamada a la API de envío de Microsoft Store. Necesitas el identificador de inquilino, el identificador de cliente y la clave para obtener un token de acceso de Azure AD que se pasa a la API.

> [!NOTE]
> Solo debes realizar esta tarea una vez. Una vez que tengas el identificador de inquilino, el identificador de cliente y la clave, puedes volver a usarlos siempre que necesites crear un nuevo token de acceso de Azure AD.

1.  En el Centro de desarrollo, [asocia tu cuenta del Centro de desarrollo de la organización con tu directorio de Azure AD de la organización](../publish/associate-azure-ad-with-dev-center.md).

2.  A continuación, desde la página **Usuarios** en la sección **Configuración de la cuenta** del Centro de desarrollo, [añada la aplicación de Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-dev-center-account) que representa la aplicación o el servicio que usarás para acceder a los envíos de tu cuenta del Centro de desarrollo. Asegúrate de que se asignas a esta aplicación el rol de **Administrador**. Si la aplicación no existe aún en el directorio de Azure AD, puedes [crear una nueva aplicación de Azure AD en el Centro de desarrollo](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-dev-center-account).  

3.  Vuelve a la página **Usuarios**, haz clic en el nombre de la aplicación de Azure AD para ir a la configuración de la aplicación y copia los valores de **Identificador de inquilino** e **Identificador de cliente**.

4. Haz clic en **Agregar nueva clave**. En la siguiente pantalla, copia el valor **Clave**. No podrás acceder a esta información de nuevo después de salir de la página. Para obtener más información, consulta [Administrar claves para una aplicación de Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Paso 2: Obtener un token de acceso de AzureAD

Antes de llamar a cualquiera de los métodos en la API de envío de Microsoft Store, primero debes obtener un token de acceso de Azure AD para pasarlo al encabezado **Autorización** de cada método en la API. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes actualizar el token para que puedas continuar usándolo en llamadas adicionales a la API.

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

Para el valor de *tenant\_id* de POST URI y los parámetros *client\_id* y *client\_secret*, especifica el identificador de inquilino, de cliente y la clave para la aplicación que recuperaste del Centro de desarrollo en la sección anterior. Para el parámetro *resource*, debes especificar ```https://manage.devcenter.microsoft.com```.

Una vez expire el token de acceso, puedes actualizarlo siguiendo las instrucciones que se muestran [aquí](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

Para ver ejemplos que muestran cómo obtener un token de acceso con código C#, Java o Python, consulta los [ejemplos de código](#code-examples) de la API de envío de Microsoft Store.

<span id="call-the-windows-store-submission-api">

## <a name="step-3-use-the-microsoft-store-submission-api"></a>Paso 3: Uso de la API de envío de Microsoft Store

Una vez que tengas un token de acceso de Azure AD, podrás llamar a métodos en la API de envío de Microsoft Store. La API incluye muchos métodos que se agrupan en escenarios de aplicaciones, complementos y paquetes piloto. Para crear o actualizar los envíos, normalmente llamas a varios métodos en la API de envío de Microsoft Store en un orden específico. Para obtener información sobre cada escenario y la sintaxis de cada método, consulta los artículos en la siguiente tabla.

> [!NOTE]
> Después de obtener un token de acceso, tienes 60 minutos para acceder a métodos en la API de envío de Microsoft Store antes de que el token expire.

| Escenario       | Descripción                                                                 |
|---------------|----------------------------------------------------------------------|
| Aplicaciones |  Recupera los datos de todas las aplicaciones que están registradas en tu cuenta del Centro de desarrollo de Windows y crea envíos para aplicaciones. Para obtener más información sobre estos métodos, consulta los siguientes artículos: <ul><li>[Obtención de datos de la aplicación](get-app-data.md)</li><li>[Administración de envíos de aplicaciones](manage-app-submissions.md)</li></ul> |
| Complementos | Obtén, crea, elimina complementos para las aplicaciones y, a continuación, obtén, crea o elimina envíos para los complementos. Para obtener más información sobre estos métodos, consulta los siguientes artículos: <ul><li>[Administración de complementos](manage-add-ons.md)</li><li>[Administrar envíos de complementos](manage-add-on-submissions.md)</li></ul> |
| Paquetes piloto | Obtén, crea, elimina paquetes piloto para las aplicaciones y, a continuación, obtén, crea o elimina envíos para los paquetes piloto. Para obtener más información sobre estos métodos, consulta los siguientes artículos: <ul><li>[Administración de paquetes piloto](manage-flights.md)</li><li>[Administración de envíos de paquetes piloto](manage-flight-submissions.md)</li></ul> |

<span id="code-samples"/>

## <a name="code-examples"></a>Ejemplos de código

Los siguientes artículos proporcionan ejemplos de código detallados que muestran cómo usar la API de envío de Microsoft Store en varios lenguajes diferentes:

* [Muestra de C#: envíos de aplicaciones, complementos y pilotos](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Muestra de C#: envío de aplicación con opciones de juego y tráileres](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Muestra de Java: envíos de aplicaciones, complementos y pilotos](java-code-examples-for-the-windows-store-submission-api.md)
* [Muestra de Java: envío de aplicación con opciones de juego y tráileres](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Muestra de Python: envíos de aplicaciones, complementos y pilotos](python-code-examples-for-the-windows-store-submission-api.md)
* [Muestra de Python: envío de aplicación con opciones de juego y tráileres](python-code-examples-for-submissions-game-options-and-trailers.md)

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
* Visita nuestra [página de soporte técnico](https://developer.microsoft.com/windows/support) y solicita una de las opciones de soporte técnico asistido para el panel del Centro de desarrollo. Si se le pide que elija un tipo y categoría de problema, elige **Envío y certificación de aplicaciones** y **Enviar una aplicación**, respectivamente.  

## <a name="related-topics"></a>Temas relacionados

* [Obtención de datos de la aplicación](get-app-data.md)
* [Administración de envíos de aplicaciones](manage-app-submissions.md)
* [Administración de complementos](manage-add-ons.md)
* [Administrar envíos de complementos](manage-add-on-submissions.md)
* [Administración de paquetes piloto](manage-flights.md)
* [Administración de envíos de paquetes piloto](manage-flight-submissions.md)
