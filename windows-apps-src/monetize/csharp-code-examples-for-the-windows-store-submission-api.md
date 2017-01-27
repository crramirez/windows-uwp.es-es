---
author: mcleanbyron
ms.assetid: FABA802F-9CB2-4894-9848-9BB040F9851F
description: "Usa los ejemplos de código de C# de esta sección para obtener más información sobre cómo usar la API de envío de la Tienda Windows."
title: "Ejemplos de código de C# para la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: ccc7cfea885cc9c8803cfc70d2e043192a7fee84
ms.openlocfilehash: d2fc29f8f2fc6cc78c1cb04c68844215a3e3eafe

---

# <a name="c-code-examples-for-the-windows-store-submission-api"></a>Ejemplos de código de C\# para la API de envío de la Tienda Windows

En este artículo se proporcionan ejemplos de código de C# para usar la *API de envío de la Tienda Windows*. Para obtener más información sobre esta API, consulta [Create and manage submissions using Windows Store services (Crear y administrar envíos mediante el uso de servicios de la Tienda Windows)](create-and-manage-submissions-using-windows-store-services.md).

Estos ejemplos de código muestran las siguientes tareas:

* [Actualización de un envío de aplicación](#update-app-submission)
* [Creación de un nuevo envío de complemento](#create-add-on-submission)
* [Actualización de un envío de complemento](#update-add-on-submission)
* [Actualización de un envío de paquete piloto](#update-flight-submission)

Puedes revisar cada ejemplo para conocer más detalles sobre la tarea que muestra, o bien puedes compilar todos los ejemplos de código de este artículo en una aplicación de consola. Para compilar los ejemplos, crear una aplicación de consola de C# denominada **DeveloperApiCSharpSample** en Visual Studio, copia cada ejemplo en un archivo de código independiente en el proyecto y compila el proyecto.

## <a name="prerequisites"></a>Requisitos previos

En estos ejemplos se usan las bibliotecas siguientes:

* Microsoft.WindowsAzure.Storage.dll. Esta biblioteca está disponible en el [SDK de Azure para .NET](https://azure.microsoft.com/downloads/) o puedes obtenerla instalando el [paquete de NuGet WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage).
* [Json.NET](http://www.newtonsoft.com/json) de Newtonsoft.

## <a name="main-program"></a>Programa principal

En el ejemplo siguiente se implementa un programa de línea de comandos que llama a los métodos de otros ejemplos de este artículo para mostrar las diferentes formas de usar la API de envío de la Tienda Windows. Para adaptar este programa a tu propio uso:

* Asigna las propiedades ```ApplicationId```, ```InAppProductId``` y ```FlightId``` al identificador de la aplicación, el complemento (los complementos también se denominan productos en aplicación o IAP) y el paquete piloto que quieras administrar. Estos identificadores están disponibles en el panel del Centro de desarrollo.
* Asigna las propiedades ```ClientId``` y ```ClientSecret``` al identificador de cliente y la clave de la aplicación, y reemplaza la cadena *tenantid* de la dirección URL de ```TokenEndpoint``` por el identificador de inquilino de la aplicación. Para obtener más información, consulta [Asociación de una aplicación de Azure AD a tu cuenta del Centro de desarrollo de Windows](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-windows-dev-center-account)

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/Program.cs#Main)]

<span id="clientconfiguration" />
## <a name="clientconfiguration-helper-class"></a>Clase auxiliar ClientConfiguration

La aplicación de muestra usa la clase auxiliar ```ClientConfiguration``` para pasar datos de Azure Active Directory y datos de la aplicación a cada uno de los métodos de ejemplo que usan la API de envío de la Tienda Windows.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/ClientConfiguration.cs#ClientConfiguration)]

<span id="update-app-submission" />
## <a name="update-an-app-submission"></a>Actualización de un envío de aplicación

En el ejemplo siguiente se implementa una clase que usa varios métodos en la API de envío de la Tienda Windows para actualizar un envío de aplicación. El método ```RunAppSubmissionUpdateSample``` de la clase crea un nuevo envío como clon del último envío publicado y, a continuación, actualiza y confirma el envío clonado al Centro de desarrollo de Windows. Específicamente, el método ```RunAppSubmissionUpdateSample``` realiza estas tareas:

1. Para empezar, el método [obtiene datos de la aplicación especificada](get-an-app.md).
2. A continuación, [elimina el envío pendiente de la aplicación](delete-an-app-submission.md), si existe uno.
3. Luego, [crea un nuevo envío de la aplicación](create-an-app-submission.md) (el nuevo envío es una copia del último envío publicado).
4. Cambia algunos detalles para el nuevo envío y carga un nuevo paquete para el envío a Azure Blob Storage.
5. Después, [actualiza](update-an-app-submission.md) y [confirma](commit-an-app-submission.md) el nuevo envío al Centro de desarrollo de Windows.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-an-app-submission.md) hasta que el envío se confirma correctamente.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/AppSubmissionUpdateSample.cs#AppSubmissionUpdateSample)]

<span id="create-add-on-submission" />
## <a name="create-a-new-add-on-submission"></a>Creación de un nuevo envío de complemento

En el ejemplo siguiente se implementa una clase que usa varios métodos en la API de envío de la Tienda Windows para crear un nuevo envío de complemento. El método ```RunInAppProductSubmissionCreateSample``` de la clase realiza estas tareas:

1. Para empezar, el método [crea un nuevo complemento](create-an-add-on.md).
2. A continuación, [crea un nuevo envío para el complemento](create-an-add-on-submission.md).
3. Carga un archivo ZIP que contiene los iconos del envío a Azure Blob Storage.
4. Luego [confirma el nuevo envío al Centro de desarrollo de Windows](commit-an-add-on-submission.md).
5. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-an-add-on-submission.md) hasta que el envío se confirma correctamente.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/InAppProductSubmissionCreateSample.cs#InAppProductSubmissionCreateSample)]

<span id="update-add-on-submission" />
## <a name="update-an-add-on-submission"></a>Actualización de un envío de complemento

En el ejemplo siguiente se implementa una clase que usa varios métodos en la API de envío de la Tienda Windows para actualizar un envío de un complemento existente. El método ```RunInAppProductSubmissionUpdateSample``` de la clase crea un nuevo envío como clon del último envío publicado y, a continuación, actualiza y confirma el envío clonado en el Centro de desarrollo de Windows. Específicamente, el método ```RunInAppProductSubmissionUpdateSample``` realiza estas tareas:

1. Para empezar, el método [obtiene datos del complemento especificado](get-an-add-on.md).
2. A continuación, [elimina el envío pendiente del complemento](delete-an-add-on-submission.md), si existe uno.
3. Luego [crea un nuevo envío del complemento](create-an-add-on-submission.md) (el nuevo envío es una copia del último envío publicado).
5. Después, [actualiza](update-an-add-on-submission.md) y [confirma](commit-an-add-on-submission.md) el nuevo envío al Centro de desarrollo de Windows.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-an-add-on-submission.md) hasta que el envío se confirma correctamente.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/InAppProductSubmissionUpdateSample.cs#InAppProductSubmissionUpdateSample)]

<span id="update-flight-submission" />
## <a name="update-a-package-flight-submission"></a>Actualización de un envío de paquete piloto

En el ejemplo siguiente se implementa una clase que usa varios métodos en la API de envío de la Tienda Windows para actualizar un envío de paquete piloto. El método ```RunFlightSubmissionUpdateSample``` de la clase crea un nuevo envío como clon del último envío publicado y, a continuación, actualiza y confirma el envío clonado en el Centro de desarrollo de Windows. Específicamente, el método ```RunFlightSubmissionUpdateSample``` realiza estas tareas:

1. Para empezar, el método [obtiene datos del paquete piloto especificado](get-a-flight.md).
2. A continuación, [elimina el envío pendiente del paquete piloto](delete-a-flight-submission.md), si existe uno.
3. Luego [crea un nuevo envío del paquete piloto](create-a-flight-submission.md) (el nuevo envío es una copia del último envío publicado).
4. Carga un nuevo paquete para el envío a Azure Blob Storage.
5. Después, [actualiza](update-a-flight-submission.md) y [confirma](commit-a-flight-submission.md) el nuevo envío al Centro de desarrollo de Windows.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-a-flight-submission.md) hasta que el envío se confirma correctamente.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/FlightSubmissionUpdateSample.cs#FlightSubmissionUpdateSample)]

<span id="ingestionclient" />
## <a name="ingestionclient-helper-class"></a>Clase auxiliar IngestionClient

La clase ```IngestionClient``` proporciona métodos auxiliares que otros métodos usan en la aplicación de muestra para realizar las siguientes tareas:

* [Obtener un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) que puede usarse para llamar a métodos en la API de envío de la Tienda Windows. Después de obtener un token, tienes 60 minutos para utilizar este token en llamadas a la API de envío de Tienda Windows antes de que el token expire. Después de que el token expire, puedes generar uno nuevo.
* Carga un archivo ZIP que contenga los nuevos recursos para el envío de una aplicación o complemento a Azure Blob Storage. Para obtener más información sobre cómo cargar un archivo ZIP en Azure Blob Storage para el envío de aplicaciones y complementos, consulta las instrucciones correspondientes en [Creación de un envío de aplicación](manage-app-submissions.md#create-an-app-submission) y [Creación de un envío de complemento](manage-add-on-submissions.md#create-an-add-on-submission).
* Procesa las solicitudes HTTP para la API de envío de la Tienda Windows.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/IngestionClient.cs#IngestionClient)]

## <a name="related-topics"></a>Temas relacionados

* [Creación y administración de envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)



<!--HONumber=Dec16_HO3-->


