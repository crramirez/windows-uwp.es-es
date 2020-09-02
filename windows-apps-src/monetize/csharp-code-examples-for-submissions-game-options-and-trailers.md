---
description: Use los ejemplos de código de C# de esta sección para obtener más información sobre el envío de opciones de juego y finalizadores con la API de envío de Microsoft Store.
title: 'Ejemplo de C#: envío de aplicaciones con opciones y finalizadores de juego'
ms.date: 07/10/2017
ms.topic: article
keywords: 'Windows 10, UWP, API de envío de Microsoft Store, ejemplos de código, opciones de juego, finalizadores, listas avanzadas, C #'
ms.localizationpriority: medium
ms.openlocfilehash: 16d51a12c6d703be3a76c1c90d5ab6a2e9442603
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363198"
---
# <a name="c-sample-app-submission-with-game-options-and-trailers"></a>\#Ejemplo de C: envío de aplicaciones con opciones y finalizadores de juego

En este artículo se proporcionan ejemplos de código de C# que muestran cómo usar la [API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) para estas tareas:

* Obtenga un token de acceso Azure AD para usarlo con la API de envío de Microsoft Store.
* Crear un envío de aplicación
* Configure los datos de la lista de tiendas para el envío de la aplicación, incluidas las opciones de anuncios avanzados de [juegos](manage-app-submissions.md#gaming-options-object) y [finalizadores](manage-app-submissions.md#trailer-object) .
* Cargue el archivo ZIP que contiene los paquetes, las imágenes de lista y los archivos de finalizador para el envío de la aplicación.
* Confirme el envío de la aplicación.

Puedes revisar cada ejemplo para conocer más detalles sobre la tarea que muestra, o bien puedes compilar todos los ejemplos de código de este artículo en una aplicación de consola. Para compilar los ejemplos, cree una aplicación de consola de C# denominada **DevCenterApiSample** en Visual Studio, copie cada ejemplo en un archivo de código independiente en el proyecto y compile el proyecto.

## <a name="prerequisites"></a>Requisitos previos

Estos ejemplos tienen los siguientes requisitos:

* Agregue una referencia al ensamblado System. Web en el proyecto.
* Instale el [Newtonsoft.Jsen](https://www.newtonsoft.com/json) el paquete NuGet de Newtonsoft en el proyecto.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Crear un envío de aplicación

La ```CreateAndSubmitSubmissionExample``` clase define un ```Execute``` método público que llama a otros métodos de ejemplo para usar el Microsoft Store API de envío para crear y confirmar un envío de aplicación que contiene opciones de juego y un finalizador. Para adaptar este código para su propio uso:

* Asigne la ```tenantId``` variable al identificador de inquilino de la aplicación y asigne las ```clientId``` variables y ```clientSecret``` al identificador de cliente y la clave de la aplicación. Para obtener más información, consulte [Asociación de una aplicación Azure ad a la cuenta del centro de Partners](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account) .
* Asigne la ```applicationId``` variable al [identificador de almacén](in-app-purchases-and-trials.md#store-ids) de la aplicación para la que desea crear un envío.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/cs/CreateAndSubmitSubmissionExample.cs" id="CreateAndSubmitSubmissionExample":::

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obtención de un token de acceso de Azure AD

La ```DevCenterAccessTokenClient``` clase define un método auxiliar que usa los ```tenantId``` ```clientId``` valores, y ```clientSecret``` para crear un token de acceso Azure ad para usarlo con la API de envío Microsoft Store.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterAccessTokenClient.cs" id="DevCenterAccessTokenClient":::

<span id="utilities" />

## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>Métodos auxiliares para invocar la API de envío y cargar archivos de envío

La ```DevCenterClient``` clase define métodos auxiliares que invocan una variedad de métodos en el Microsoft Store API de envío y cargan el archivo zip que contiene los paquetes, las imágenes de lista y los archivos de finalizador para el envío de la aplicación.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterClient.cs" id="DevCenterClient":::

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
