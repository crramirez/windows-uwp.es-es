---
description: Use los ejemplos de código de Java de esta sección para obtener más información sobre el envío de opciones y finalizadores de juegos con la API de envío de Microsoft Store.
title: 'Ejemplo de Java: envío de aplicaciones con opciones y finalizadores de juego'
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, UWP, API de envío de Microsoft Store, ejemplos de código, opciones de juego, finalizadores, listas avanzadas, Java
ms.localizationpriority: medium
ms.openlocfilehash: 38351c06fd680a16eaf54b0f2f8c84460a275028
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363078"
---
# <a name="java-sample-app-submission-with-game-options-and-trailers"></a>Ejemplo de Java: envío de aplicación con opciones de juego y tráileres

En este artículo se proporcionan ejemplos de código de Java que muestran cómo usar la [API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) para estas tareas:

* Obtenga un token de acceso Azure AD para usarlo con la API de envío de Microsoft Store.
* Crear un envío de aplicación
* Configure los datos de la lista de tiendas para el envío de la aplicación, incluidas las opciones de anuncios avanzados de [juegos](manage-app-submissions.md#gaming-options-object) y [finalizadores](manage-app-submissions.md#trailer-object) .
* Cargue el archivo ZIP que contiene los paquetes, las imágenes de lista y los archivos de finalizador para el envío de la aplicación.
* Confirme el envío de la aplicación.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Crear un envío de aplicación

La `CreateAndSubmitSubmissionExample` clase implementa un `main` programa que llama a otros métodos de ejemplo para usar el Microsoft Store API de envío para crear y confirmar un envío de aplicación que contiene opciones de juego y un finalizador. Para adaptar este código para su propio uso:

* Asigne la `tenantId` variable al identificador de inquilino de la aplicación y asigne las `clientId` variables y `clientSecret` al identificador de cliente y la clave de la aplicación. Para obtener más información, consulte [Asociación de una aplicación Azure ad a la cuenta del centro de Partners](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account) .
* Asigne la `applicationId` variable al [identificador de almacén](in-app-purchases-and-trials.md#store-ids) de la aplicación para la que desea crear un envío.

> [!div class="tabbedCodeSnippets"]
:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/java/CreateAndSubmitSubmissionExample.java" range="1-313":::

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obtención de un token de acceso de Azure AD

La `DevCenterAccessTokenClient` clase define un método auxiliar que usa los `tenantId` `clientId` valores, y `clientSecret` para crear un token de acceso Azure ad para usarlo con la API de envío Microsoft Store.

> [!div class="tabbedCodeSnippets"]
:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/java/DevCenterAccessTokenClient.java" range="1-69":::


<span id="utilities" />

## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>Métodos auxiliares para invocar la API de envío y cargar archivos de envío

La `DevCenterClient` clase define métodos auxiliares que invocan una variedad de métodos en el Microsoft Store API de envío y cargan el archivo zip que contiene los paquetes, las imágenes de lista y los archivos de finalizador para el envío de la aplicación.

> [!div class="tabbedCodeSnippets"]
:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/java/DevCenterClient.java" range="1-224":::

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
