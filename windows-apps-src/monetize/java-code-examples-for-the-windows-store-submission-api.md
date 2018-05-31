---
author: mcleanbyron
ms.assetid: 4920D262-B810-409E-BA3A-F68AADF1B1BC
description: Usa los ejemplos de código Java de esta sección para obtener más información sobre cómo usar la API de envío de Microsoft Store.
title: 'Muestra de Java: envíos de aplicaciones, complementos y pilotos'
ms.author: mcleans
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de envío de Microsoft Store, ejemplos de código, java
ms.localizationpriority: medium
ms.openlocfilehash: efbf5e075737a1611e1114c82c1df982fe4751c2
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654454"
---
# <a name="java-sample-submissions-for-apps-add-ons-and-flights"></a>Muestra de Java: envíos de aplicaciones, complementos y pilotos

En este artículo se proporcionan ejemplos de código Java que muestran cómo usar la [API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) para estas tareas:

* [Obtener un token de acceso de AzureAD](#token)
* [Crear un complemento](#create-add-on)
* [Crear un paquete piloto](#create-package-flight)
* [Crear un envío de aplicación](#create-app-submission)
* [Crear un envío de complemento](#create-add-on-submission)
* [Crear un envío de paquete piloto](#create-flight-submission)

Puedes revisar cada ejemplo para conocer más detalles sobre la tarea que muestra, o bien puedes compilar todos los ejemplos de código de este artículo en una aplicación de consola. Para obtener un listado de código completa, consulta la sección [listado de código](java-code-examples-for-the-windows-store-submission-api.md#code-listing) sección al final de este artículo.

## <a name="prerequisites"></a>Requisitos previos

En estos ejemplos se usan las bibliotecas siguientes:

* [Apache Commons Logging 1.2](http://commons.apache.org/proper/commons-logging) (commons-logging-1.2.jar).
* [Apache HttpComponents Core 4.4.5 y Apache HttpComponents Client 4.5.2](https://hc.apache.org/) (httpcore-4.4.5.jar y httpclient-4.5.2.jar).
* [JSR 353 JSON Processing API 1.0](https://mvnrepository.com/artifact/javax.json/javax.json-api/1.0) y [JSR 353 JSON Processing Default Provider API 1.0.4](https://mvnrepository.com/artifact/org.glassfish/javax.json/1.0.4) (javax.json-api-1.0.jar y javax.json-1.0.4.jar).

## <a name="main-program-and-imports"></a>Programa principal e importaciones

En el siguiente ejemplo se muestran las instrucciones imports que se usan en todos los ejemplos de código y se implementa un programa de línea de comandos que llama a los otros métodos de ejemplo.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/MainExample.java#L1-L64)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obtener un token de acceso de AzureAD

En el siguiente ejemplo se muestra cómo [obtener un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) que puedas usar para llamar a métodos en la API de envío de Microsoft Store. Después de obtener un token, tienes 60 minutos para utilizar este token en llamadas a la API de envío de Microsoft Store antes de que el token expire. Después de que el token expire, puedes generar uno nuevo.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L65-L95)]

<span id="create-add-on" />

## <a name="create-an-add-on"></a>Crear un complemento

En el siguiente ejemplo se muestra cómo [crear](create-an-add-on.md) y luego [eliminar](delete-an-add-on.md) un complemento.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L310-L345)]

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>Crear un paquete piloto

En el siguiente ejemplo se muestra cómo [crear](create-a-flight.md) y luego [eliminar](delete-a-flight.md) un paquete piloto.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L185-L221)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Crear un envío de aplicación

En el siguiente ejemplo se muestra cómo usar varios métodos en la API de envío de Microsoft Store para crear un envío de aplicación. Para ello, el método ```SubmitNewApplicationSubmission``` crea un nuevo envío como clon del último envío publicado y, después, actualiza y confirma el envío clonado al Centro de desarrollo de Windows. Específicamente, el método ```SubmitNewApplicationSubmission``` realiza estas tareas:

1. Para empezar, el método [obtiene datos de la aplicación especificada](get-an-app.md).
2. A continuación, [elimina el envío pendiente de la aplicación](delete-an-app-submission.md), si existe uno.
3. Luego [crea un nuevo envío de la aplicación](create-an-app-submission.md) (el nuevo envío es una copia del último envío publicado).
4. Cambia algunos detalles para el nuevo envío y carga un nuevo paquete para el envío a Azure Blob Storage.
5. Después, [actualiza](update-an-app-submission.md) y [confirma](commit-an-app-submission.md) el nuevo envío al Centro de desarrollo de Windows.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-an-app-submission.md) hasta que el envío se confirma correctamente.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L97-L183)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Crear un envío de complemento

En el siguiente ejemplo se muestra cómo usar varios métodos en la API de envío de Microsoft Store para crear un envío de complemento. Para ello, el método ```SubmitNewInAppProductSubmission``` crea un nuevo envío como clon del último envío publicado y, después, actualiza y confirma el envío clonado al Centro de desarrollo de Windows. Específicamente, el método ```SubmitNewInAppProductSubmission``` realiza estas tareas:

1. Para empezar, el método [obtiene datos del complemento especificado](get-an-add-on.md).
2. A continuación, [elimina el envío pendiente del complemento](delete-an-add-on-submission.md), si existe uno.
3. Luego [crea un nuevo envío del complemento](create-an-add-on-submission.md) (el nuevo envío es una copia del último envío publicado).
4. Carga un archivo ZIP que contiene los iconos del envío a Azure Blob Storage.
5. Después, [actualiza](update-an-add-on-submission.md) y [confirma](commit-an-add-on-submission.md) el nuevo envío al Centro de desarrollo de Windows.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-an-add-on-submission.md) hasta que el envío se confirma correctamente.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L347-L431)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Crear un envío de paquete piloto

En el siguiente ejemplo se muestra cómo usar varios métodos en la API de envío de Microsoft Store para crear un envío de paquete piloto. Para ello, el método ```SubmitNewFlightSubmission``` crea un nuevo envío como clon del último envío publicado y, después, actualiza y confirma el envío clonado al Centro de desarrollo de Windows. Específicamente, el método ```SubmitNewFlightSubmission``` realiza estas tareas:

1. Para empezar, el método [obtiene datos del paquete piloto especificado](get-a-flight.md).
2. A continuación, [elimina el envío pendiente del paquete piloto](delete-a-flight-submission.md), si existe uno.
3. Luego [crea un nuevo envío del paquete piloto](create-a-flight-submission.md) (el nuevo envío es una copia del último envío publicado).
4. Carga un nuevo paquete para el envío a Azure Blob Storage.
5. Después, [actualiza](update-a-flight-submission.md) y [confirma](commit-a-flight-submission.md) el nuevo envío al Centro de desarrollo de Windows.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-a-flight-submission.md) hasta que el envío se confirma correctamente.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L223-L308)]

<span id="utilities" />

## <a name="utility-methods-to-upload-submission-files-and-handle-request-responses"></a>Métodos de utilidad para cargar archivos de envío y controlar las respuestas a solicitudes

Los siguientes métodos de utilidad muestran estas tareas:

* Cómo cargar un archivo ZIP que contiene los nuevos recursos para el envío de una aplicación o complemento a Azure Blob Storage. Para obtener más información sobre cómo cargar un archivo ZIP a Azure Blob Storage para envíos de aplicaciones y complementos, consulta las instrucciones correspondientes en [Crear un envío de aplicación](manage-app-submissions.md#create-an-app-submission), [Crear un envío de complemento](manage-add-on-submissions.md#create-an-add-on-submission) y [Crear un envío de paquete piloto](manage-flight-submissions.md#create-a-package-flight-submission).
* Cómo controlar las respuestas a solicitudes

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L433-L490)]

<span id="code-listing" />

## <a name="complete-code-listing"></a>Listado de código completo

El listado de código siguiente contiene todos los ejemplos anteriores organizados en un archivo de origen.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L1-L491)]

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
