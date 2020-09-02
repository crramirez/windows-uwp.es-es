---
ms.assetid: FABA802F-9CB2-4894-9848-9BB040F9851F
description: Use los ejemplos de código de C# de esta sección para obtener más información sobre el uso de la API de envío de Microsoft Store.
title: 'Ejemplo de C#: envíos de aplicaciones, complementos y vuelos'
ms.date: 08/03/2017
ms.topic: article
keywords: 'Windows 10, UWP, API de envío de Microsoft Store, ejemplos de código, C #'
ms.localizationpriority: medium
ms.openlocfilehash: ac16d6932a2f20e701d7446ac8c21c316cfe5d4a
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364128"
---
# <a name="c-sample-submissions-for-apps-add-ons-and-flights"></a>\#Ejemplo de C: envíos de aplicaciones, complementos y vuelos

En este artículo se proporcionan ejemplos de código de C# que muestran cómo usar la [API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) para estas tareas:

* [Crear un envío de aplicación](#create-app-submission)
* [Crear un envío de complemento](#create-add-on-submission)
* [Actualización de un envío de complemento](#update-add-on-submission)
* [Crear un envío de paquete piloto](#create-flight-submission)

Puedes revisar cada ejemplo para conocer más detalles sobre la tarea que muestra, o bien puedes compilar todos los ejemplos de código de este artículo en una aplicación de consola. Para compilar los ejemplos, crear una aplicación de consola de C# denominada **DeveloperApiCSharpSample** en Visual Studio, copia cada ejemplo en un archivo de código independiente en el proyecto y compila el proyecto.

## <a name="prerequisites"></a>Requisitos previos

En estos ejemplos se usan las bibliotecas siguientes:

* Microsoft.WindowsAzure.Storage.dll. Esta biblioteca está disponible en el [SDK de Azure para .NET](https://azure.microsoft.com/downloads/) o puedes obtenerla instalando el [paquete de NuGet WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage).
* [Newtonsoft.Jsen](https://www.newtonsoft.com/json) Paquete NuGet de Newtonsoft.

## <a name="main-program"></a>Programa principal

En el ejemplo siguiente se implementa un programa de línea de comandos que llama a los otros métodos de ejemplo de este artículo para mostrar diferentes maneras de usar la API de envío de Microsoft Store. Para adaptar este programa a tu propio uso:

* Asigne las ```ApplicationId``` ```InAppProductId``` propiedades, y ```FlightId``` al identificador de la aplicación, el complemento y el paquete piloto que desea administrar.
* Asigna las propiedades ```ClientId``` y ```ClientSecret``` al identificador de cliente y la clave de la aplicación, y reemplaza la cadena *tenantid* de la dirección URL de ```TokenEndpoint``` por el identificador de inquilino de la aplicación. Para obtener más información, consulte [Asociación de una aplicación Azure ad a la cuenta del centro de Partners](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account) .

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/Program.cs" id="Main":::

<span id="clientconfiguration" />

## <a name="clientconfiguration-helper-class"></a>Clase auxiliar ClientConfiguration

La aplicación de ejemplo usa la ```ClientConfiguration``` clase auxiliar para pasar Azure Active Directory datos y datos de la aplicación a cada uno de los métodos de ejemplo que usan la API de envío de Microsoft Store.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/ClientConfiguration.cs" id="ClientConfiguration":::

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Crear un envío de aplicación

En el ejemplo siguiente se implementa una clase que usa varios métodos en la API de envío de Microsoft Store para actualizar un envío de aplicación. El ```RunAppSubmissionUpdateSample``` método de la clase crea un nuevo envío como clon del último envío publicado y, a continuación, actualiza y confirma el envío clonado en el centro de Partners. Específicamente, el método ```RunAppSubmissionUpdateSample``` realiza estas tareas:

1. Para empezar, el método [obtiene datos de la aplicación especificada](get-an-app.md).
2. A continuación, [elimina el envío pendiente de la aplicación](delete-an-app-submission.md), si existe uno.
3. Luego [crea un nuevo envío de la aplicación](create-an-app-submission.md) (el nuevo envío es una copia del último envío publicado).
4. Cambia algunos detalles para el nuevo envío y carga un nuevo paquete para el envío a Azure Blob Storage.
5. Después, [actualiza](update-an-app-submission.md) y [confirma](commit-an-app-submission.md) el nuevo envío al centro de Partners.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-an-app-submission.md) hasta que el envío se confirma correctamente.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/AppSubmissionUpdateSample.cs" id="AppSubmissionUpdateSample":::

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Crear un envío de complemento

En el ejemplo siguiente se implementa una clase que usa varios métodos en el Microsoft Store API de envío para crear un nuevo envío de complementos. El método ```RunInAppProductSubmissionCreateSample``` de la clase realiza estas tareas:

1. Para empezar, el método [crea un nuevo complemento](create-an-add-on.md).
2. A continuación, [crea un nuevo envío para el complemento](create-an-add-on-submission.md).
3. Carga un archivo zip que contiene los iconos del envío a Azure Blob Storage.
4. Después, [confirma el nuevo envío al centro de Partners](commit-an-add-on-submission.md).
5. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-an-add-on-submission.md) hasta que el envío se confirma correctamente.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/InAppProductSubmissionCreateSample.cs" id="InAppProductSubmissionCreateSample":::

<span id="update-add-on-submission" />

## <a name="update-an-add-on-submission"></a>Actualización de un envío de complemento

En el ejemplo siguiente se implementa una clase que usa varios métodos en la API de envío de Microsoft Store para actualizar un envío de complemento existente. El ```RunInAppProductSubmissionUpdateSample``` método de la clase crea un nuevo envío como clon del último envío publicado y, a continuación, actualiza y confirma el envío clonado en el centro de Partners. Específicamente, el método ```RunInAppProductSubmissionUpdateSample``` realiza estas tareas:

1. Para empezar, el método [obtiene datos del complemento especificado](get-an-add-on.md).
2. A continuación, [elimina el envío pendiente del complemento](delete-an-add-on-submission.md), si existe uno.
3. Luego [crea un nuevo envío del complemento](create-an-add-on-submission.md) (el nuevo envío es una copia del último envío publicado).
5. Después, [actualiza](update-an-add-on-submission.md) y [confirma](commit-an-add-on-submission.md) el nuevo envío al centro de Partners.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-an-add-on-submission.md) hasta que el envío se confirma correctamente.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/InAppProductSubmissionUpdateSample.cs" id="InAppProductSubmissionUpdateSample":::

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Crear un envío de paquete piloto

En el ejemplo siguiente se implementa una clase que usa varios métodos en la API de envío de Microsoft Store para actualizar un envío de paquetes piloto. El ```RunFlightSubmissionUpdateSample``` método de la clase crea un nuevo envío como clon del último envío publicado y, a continuación, actualiza y confirma el envío clonado en el centro de Partners. Específicamente, el método ```RunFlightSubmissionUpdateSample``` realiza estas tareas:

1. Para empezar, el método [obtiene datos del paquete piloto especificado](get-a-flight.md).
2. A continuación, [elimina el envío pendiente del paquete piloto](delete-a-flight-submission.md), si existe uno.
3. Luego [crea un nuevo envío del paquete piloto](create-a-flight-submission.md) (el nuevo envío es una copia del último envío publicado).
4. Carga un nuevo paquete para el envío a Azure Blob Storage.
5. Después, [actualiza](update-a-flight-submission.md) y [confirma](commit-a-flight-submission.md) el nuevo envío al centro de Partners.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-a-flight-submission.md) hasta que el envío se confirma correctamente.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/FlightSubmissionUpdateSample.cs" id="FlightSubmissionUpdateSample":::

<span id="ingestionclient" />

## <a name="ingestionclient-helper-class"></a>Clase auxiliar IngestionClient

La clase ```IngestionClient``` proporciona métodos auxiliares que otros métodos usan en la aplicación de muestra para realizar las siguientes tareas:

* [Obtener un token de acceso Azure ad](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) que se puede usar para llamar a métodos en la API de envío de Microsoft Store. Después de obtener un token, tiene 60 minutos para usar este token en las llamadas a la API de envío de Microsoft Store antes de que expire el token. Después de que el token expire, puedes generar uno nuevo.
* Carga un archivo ZIP que contenga los nuevos recursos para el envío de una aplicación o complemento a Azure Blob Storage. Para obtener más información sobre cómo cargar un archivo ZIP en Azure Blob Storage para el envío de aplicaciones y complementos, consulta las instrucciones correspondientes en [Creación de un envío de aplicación](manage-app-submissions.md#create-an-app-submission) y [Creación de un envío de complemento](manage-add-on-submissions.md#create-an-add-on-submission).
* Procese las solicitudes HTTP para la API de envío de Microsoft Store.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/IngestionClient.cs" id="IngestionClient":::

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
