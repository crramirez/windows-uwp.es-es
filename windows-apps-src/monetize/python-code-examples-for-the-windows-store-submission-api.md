---
author: Xansky
ms.assetid: 8AC56AAF-8D8C-4193-A6B3-BB5D0669D994
description: Usa los ejemplos de código Python de esta sección para obtener más información sobre cómo usar la API de envío de Microsoft Store.
title: 'Muestra de Python: envíos de aplicaciones, complementos y pilotos'
ms.author: mhopkins
ms.date: 07/10/2017
ms.topic: article
keywords: windows 10, uwp, API de envío de Microsoft Store, ejemplos de código, python
ms.localizationpriority: medium
ms.openlocfilehash: 6fdd7dce766e2d1804c5b0973dcf3e51cbae99c0
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5823411"
---
# <a name="python-sample-submissions-for-apps-add-ons-and-flights"></a>Muestra de Python: envíos de aplicaciones, complementos y pilotos

En este artículo se proporcionan ejemplos de código Python que muestran cómo usar la [API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) para estas tareas:

* [Obtener un token de acceso de AzureAD](#token)
* [Crear un complemento](#create-add-on)
* [Crear un paquete piloto](#create-package-flight)
* [Crear un envío de aplicación](#create-app-submission)
* [Crear un envío de complemento](#create-add-on-submission)
* [Crear un envío de paquete piloto](#create-flight-submission)

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obtener un token de acceso de AzureAD

En el siguiente ejemplo se muestra cómo [obtener un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) que puedas usar para llamar a métodos en la API de envío de Microsoft Store. Después de obtener un token, tienes 60 minutos para utilizar este token en llamadas a la API de envío de Microsoft Store antes de que el token expire. Después de que el token expire, puedes generar uno nuevo.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L1-L20)]

<span id="create-add-on" />

## <a name="create-an-add-on"></a>Crear un complemento

En el siguiente ejemplo se muestra cómo [crear](create-an-add-on.md) y luego [eliminar](delete-an-add-on.md) un complemento.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L26-L52)]

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>Crear un paquete piloto

En el siguiente ejemplo se muestra cómo [crear](create-a-flight.md) y luego [eliminar](delete-a-flight.md) un paquete piloto.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L58-L87)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Crear un envío de aplicación

En el siguiente ejemplo se muestra cómo usar varios métodos en la API de envío de Microsoft Store para crear un envío de aplicación. Para hacerlo, el código crea un nuevo envío como clon del último envío publicado y, a continuación, actualiza y confirma el envío clonado en el Centro de desarrollo de Windows. Específicamente, el ejemplo realiza estas tareas:

1. Para empezar, el ejemplo [obtiene datos de la aplicación especificada](get-an-app.md).
2. A continuación, [elimina el envío pendiente de la aplicación](delete-an-app-submission.md), si existe uno.
3. Luego [crea un nuevo envío de la aplicación](create-an-app-submission.md) (el nuevo envío es una copia del último envío publicado).
4. Cambia algunos detalles para el nuevo envío y carga un nuevo paquete para el envío a Azure Blob Storage.
5. Después, [actualiza](update-an-app-submission.md) y [confirma](commit-an-app-submission.md) el nuevo envío al Centro de desarrollo de Windows.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-an-app-submission.md) hasta que el envío se confirma correctamente.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L93-L166)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Crear un envío de complemento

En el siguiente ejemplo se muestra cómo usar varios métodos en la API de envío de Microsoft Store para crear un envío de complemento. Para hacerlo, el código crea un nuevo envío como clon del último envío publicado y, a continuación, actualiza y confirma el envío clonado en el Centro de desarrollo de Windows. Específicamente, el ejemplo realiza estas tareas:

1. Para empezar, el ejemplo [obtiene datos del complemento especificado](get-an-add-on.md).
2. A continuación, [elimina el envío pendiente del complemento](delete-an-add-on-submission.md), si existe uno.
3. Luego [crea un nuevo envío del complemento](create-an-add-on-submission.md) (el nuevo envío es una copia del último envío publicado).
4. Carga un archivo zip que contiene los iconos del envío a Azure Blob Storage. Para obtener más información, consulta las instrucciones sobre cómo cargar un archivo ZIP en Azure Blob Storage en [Crear un envío de complemento](manage-add-on-submissions.md#create-an-add-on-submission).
5. Después, [actualiza](update-an-add-on-submission.md) y [confirma](commit-an-add-on-submission.md) el nuevo envío al Centro de desarrollo de Windows.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-an-add-on-submission.md) hasta que el envío se confirma correctamente.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L172-L245)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Crear un envío de paquete piloto

En el siguiente ejemplo se muestra cómo usar varios métodos en la API de envío de Microsoft Store para crear un envío de paquete piloto. Para hacerlo, el código crea un nuevo envío como clon del último envío publicado y, a continuación, actualiza y confirma el envío clonado en el Centro de desarrollo de Windows. Específicamente, el ejemplo realiza estas tareas:

1. Para empezar, el ejemplo [obtiene datos del paquete piloto especificado](get-a-flight.md).
2. A continuación, [elimina el envío pendiente del paquete piloto](delete-a-flight-submission.md), si existe uno.
3. Luego [crea un nuevo envío del paquete piloto](create-a-flight-submission.md) (el nuevo envío es una copia del último envío publicado).
4. Carga un nuevo paquete para el envío a Azure Blob Storage. Para obtener más información, consulta las instrucciones sobre cómo cargar un archivo ZIP en Azure Blob Storage en [Crear un envío de paquete piloto](manage-flight-submissions.md#create-a-package-flight-submission).
5. Después, [actualiza](update-a-flight-submission.md) y [confirma](commit-a-flight-submission.md) el nuevo envío al Centro de desarrollo de Windows.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-a-flight-submission.md) hasta que el envío se confirma correctamente.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L251-L325)]

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
