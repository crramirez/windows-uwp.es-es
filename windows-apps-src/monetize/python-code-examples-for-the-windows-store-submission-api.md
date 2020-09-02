---
ms.assetid: 8AC56AAF-8D8C-4193-A6B3-BB5D0669D994
description: Use los ejemplos de código de Python de esta sección para obtener más información sobre el uso de la API de envío de Microsoft Store.
title: Código Python para enviar aplicaciones, complementos y vuelos
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, UWP, API de envío de Microsoft Store, ejemplos de código, Python
ms.localizationpriority: medium
ms.openlocfilehash: f551a7de85e493f4fbc1a027fb3ab9c3ca2dd598
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363868"
---
# <a name="python-sample-submissions-for-apps-add-ons-and-flights"></a>Ejemplo de Python: envíos de aplicaciones, complementos y pilotos

En este artículo se proporcionan ejemplos de código de Python que muestran cómo usar la [API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) para estas tareas:

* [Obtención de un token de acceso de Azure AD](#token)
* [Crear un complemento](#create-add-on)
* [Crear un paquete piloto](#create-package-flight)
* [Crear un envío de aplicación](#create-app-submission)
* [Crear un envío de complemento](#create-add-on-submission)
* [Crear un envío de paquete piloto](#create-flight-submission)

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obtención de un token de acceso de Azure AD

En el ejemplo siguiente se muestra cómo [obtener un token de acceso Azure ad](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) que puede usar para llamar a métodos en la API de envío de Microsoft Store. Después de obtener un token, tiene 60 minutos para usar este token en las llamadas a la API de envío de Microsoft Store antes de que expire el token. Después de que el token expire, puedes generar uno nuevo.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="1-20":::

<span id="create-add-on" />

## <a name="create-an-add-on"></a>Crear un complemento

En el ejemplo siguiente se muestra cómo [crear](create-an-add-on.md) y [eliminar](delete-an-add-on.md) un complemento.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="26-52":::

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>Crear un paquete piloto

En el siguiente ejemplo se muestra cómo [crear](create-a-flight.md) y luego [eliminar](delete-a-flight.md) un paquete piloto.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="58-87":::

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Crear un envío de aplicación

En el ejemplo siguiente se muestra cómo usar varios métodos en el Microsoft Store API de envío para crear un envío de aplicación. Para ello, el código crea un nuevo envío como clon del último envío publicado y, a continuación, actualiza y confirma el envío clonado en el centro de Partners. Específicamente, el ejemplo realiza estas tareas:

1. Para empezar, el ejemplo [obtiene datos de la aplicación especificada](get-an-app.md).
2. A continuación, [elimina el envío pendiente de la aplicación](delete-an-app-submission.md), si existe uno.
3. Luego [crea un nuevo envío de la aplicación](create-an-app-submission.md) (el nuevo envío es una copia del último envío publicado).
4. Cambia algunos detalles para el nuevo envío y carga un nuevo paquete para el envío a Azure Blob Storage.
5. Después, [actualiza](update-an-app-submission.md) y [confirma](commit-an-app-submission.md) el nuevo envío al centro de Partners.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-an-app-submission.md) hasta que el envío se confirma correctamente.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="93-166":::

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Crear un envío de complemento

En el ejemplo siguiente se muestra cómo usar varios métodos en el Microsoft Store API de envío para crear un envío de complementos. Para ello, el código crea un nuevo envío como clon del último envío publicado y, a continuación, actualiza y confirma el envío clonado en el centro de Partners. Específicamente, el ejemplo realiza estas tareas:

1. Para empezar, el ejemplo [obtiene datos del complemento especificado](get-an-add-on.md).
2. A continuación, [elimina el envío pendiente del complemento](delete-an-add-on-submission.md), si existe uno.
3. Luego [crea un nuevo envío del complemento](create-an-add-on-submission.md) (el nuevo envío es una copia del último envío publicado).
4. Carga un archivo zip que contiene los iconos del envío a Azure Blob Storage. Para obtener más información, consulta las instrucciones sobre cómo cargar un archivo ZIP en Azure Blob Storage en [Crear un envío de complemento](manage-add-on-submissions.md#create-an-add-on-submission).
5. Después, [actualiza](update-an-add-on-submission.md) y [confirma](commit-an-add-on-submission.md) el nuevo envío al centro de Partners.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-an-add-on-submission.md) hasta que el envío se confirma correctamente.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="172-245":::

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Crear un envío de paquete piloto

En el ejemplo siguiente se muestra cómo usar varios métodos en la API de envío de Microsoft Store para crear un envío de paquetes piloto. Para ello, el código crea un nuevo envío como clon del último envío publicado y, a continuación, actualiza y confirma el envío clonado en el centro de Partners. Específicamente, el ejemplo realiza estas tareas:

1. Para empezar, el ejemplo [obtiene datos del paquete piloto especificado](get-a-flight.md).
2. A continuación, [elimina el envío pendiente del paquete piloto](delete-a-flight-submission.md), si existe uno.
3. Luego [crea un nuevo envío del paquete piloto](create-a-flight-submission.md) (el nuevo envío es una copia del último envío publicado).
4. Carga un nuevo paquete para el envío a Azure Blob Storage. Para obtener más información, consulta las instrucciones sobre cómo cargar un archivo ZIP en Azure Blob Storage en [Crear un envío de paquete piloto](manage-flight-submissions.md#create-a-package-flight-submission).
5. Después, [actualiza](update-a-flight-submission.md) y [confirma](commit-a-flight-submission.md) el nuevo envío al centro de Partners.
6. Por último, periódicamente [comprueba el estado del nuevo envío](get-status-for-a-flight-submission.md) hasta que el envío se confirma correctamente.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="251-325":::

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
