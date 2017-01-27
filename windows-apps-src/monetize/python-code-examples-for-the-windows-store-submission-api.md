---
author: mcleanbyron
ms.assetid: 8AC56AAF-8D8C-4193-A6B3-BB5D0669D994
description: "Usa los ejemplos de código de Python de esta sección para obtener más información sobre cómo usar la API de envío de la Tienda Windows."
title: "Ejemplos de código de Python para la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: ccc7cfea885cc9c8803cfc70d2e043192a7fee84
ms.openlocfilehash: 1f8d415744d53120191f02a17aed4b7b04fa2f57

---

# <a name="python-code-examples-for-the-windows-store-submission-api"></a>Ejemplos de código de Python para la API de envío de la Tienda Windows

En este artículo se proporcionan ejemplos de código de Python para usar la *API de envío de la Tienda Windows*. Para obtener más información sobre esta API, consulta [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md).

Estos ejemplos de código muestran las siguientes tareas:

* [Obtener un token de acceso de Azure AD](#token)
* [Crear un complemento](#create-add-on)
* [Crear un paquete piloto](#create-package-flight)
* [Crear un envío de aplicación](#create-app-submission)
* [Crear un envío de complemento](#create-add-on-submission)
* [Crear un envío de paquete piloto](#create-flight-submission)

<span id="token" />
## <a name="obtain-an-azure-ad-access-token"></a>Obtener un token de acceso de Azure AD

En el siguiente ejemplo se muestra cómo [obtener un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) que puedas usar para llamar a métodos en la API de envío de la Tienda Windows. Después de obtener un token, tienes 60 minutos para utilizar este token en llamadas a la API de envío de Tienda Windows antes de que el token expire. Después de que el token expire, puedes generar uno nuevo.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L1-L20)]

<span id="create-add-on" />
## <a name="create-an-add-on"></a>Crear un complemento

En el siguiente ejemplo se muestra cómo [crear](create-an-add-on.md) y luego [eliminar](delete-an-add-on.md) un complemento (los complementos también se conocen como productos desde la aplicación o IAP).

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L26-L52)]

<span id="create-package-flight" />
## <a name="create-a-package-flight"></a>Crear un paquete piloto

En el siguiente ejemplo se muestra cómo [crear](create-a-flight.md) y luego [eliminar](delete-a-flight.md) un paquete piloto.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L58-L87)]

<span id="create-app-submission" />
## <a name="create-an-app-submission"></a>Crear un envío de aplicación

En el siguiente ejemplo se muestra cómo usar varios métodos en la API de envío de la Tienda Windows para crear un envío de aplicación. Para hacerlo, el código crea un nuevo envío como clon del último envío publicado y, a continuación, actualiza y confirma el envío clonado en el Centro de desarrollo de Windows. Específicamente, el ejemplo realiza estas tareas:

1. Para empezar, el ejemplo [obtiene datos de la aplicación especificada](get-an-app.md).
2. A continuación, [elimina el envío pendiente de la aplicación](delete-an-app-submission.md), si existe uno.
3. Luego [crea un nuevo envío de la aplicación](create-an-app-submission.md) (el nuevo envío es una copia del último envío publicado).
4. Cambia algunos detalles para el nuevo envío y carga un nuevo paquete para el envío a Azure Blob Storage.
5. Después, [actualiza](update-an-app-submission.md) y [confirma](commit-an-app-submission.md) el nuevo envío al Centro de desarrollo de Windows.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-an-app-submission.md) hasta que el envío se confirma correctamente.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L93-L166)]

<span id="create-add-on-submission" />
## <a name="create-an-add-on-submission"></a>Crear un envío de complemento

En el siguiente ejemplo se muestra cómo usar varios métodos en la API de envío de la Tienda Windows para crear un envío de complemento. Para hacerlo, el código crea un nuevo envío como clon del último envío publicado y, a continuación, actualiza y confirma el envío clonado en el Centro de desarrollo de Windows. Específicamente, el ejemplo realiza estas tareas:

1. Para empezar, el ejemplo [obtiene datos del complemento especificado](get-an-add-on.md).
2. A continuación, [elimina el envío pendiente del complemento](delete-an-add-on-submission.md), si existe uno.
3. Luego [crea un nuevo envío del complemento](create-an-add-on-submission.md) (el nuevo envío es una copia del último envío publicado).
4. Carga un archivo zip que contiene los iconos del envío a Azure Blob Storage. Para obtener más información, consulta las instrucciones sobre cómo cargar un archivo ZIP en Azure Blob Storage en [Crear un envío de complemento](manage-add-on-submissions.md#create-an-add-on-submission).
5. Después, [actualiza](update-an-add-on-submission.md) y [confirma](commit-an-add-on-submission.md) el nuevo envío al Centro de desarrollo de Windows.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-an-add-on-submission.md) hasta que el envío se confirma correctamente.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L172-L245)]

<span id="create-flight-submission" />
## <a name="create-a-package-flight-submission"></a>Crear un envío de paquete piloto

En el siguiente ejemplo se muestra cómo usar varios métodos en la API de envío de la Tienda Windows para crear un envío de paquete piloto. Para hacerlo, el código crea un nuevo envío como clon del último envío publicado y, a continuación, actualiza y confirma el envío clonado en el Centro de desarrollo de Windows. Específicamente, el ejemplo realiza estas tareas:

1. Para empezar, el ejemplo [obtiene datos del paquete piloto especificado](get-a-flight.md).
2. A continuación, [elimina el envío pendiente del paquete piloto](delete-a-flight-submission.md), si existe uno.
3. Luego [crea un nuevo envío del paquete piloto](create-a-flight-submission.md) (el nuevo envío es una copia del último envío publicado).
4. Carga un nuevo paquete para el envío a Azure Blob Storage. Para obtener más información, consulta las instrucciones sobre cómo cargar un archivo ZIP en Azure Blob Storage en [Crear un envío de paquete piloto](manage-flight-submissions.md#create-a-package-flight-submission).
5. Después, [actualiza](update-a-flight-submission.md) y [confirma](commit-a-flight-submission.md) el nuevo envío al Centro de desarrollo de Windows.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-a-flight-submission.md) hasta que el envío se confirma correctamente.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L251-L325)]

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante los servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)



<!--HONumber=Dec16_HO3-->


