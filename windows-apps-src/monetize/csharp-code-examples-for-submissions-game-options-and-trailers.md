---
author: Xansky
description: Usa los ejemplos de código de C# de esta sección para obtener más información sobre cómo enviar opciones de juego y tráileres usando la API de envío de Microsoft Store.
title: 'Muestra de C#: envío de aplicación con opciones de juego y tráileres'
ms.author: mhopkins
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, API de envío de Microsoft Store, ejemplos de código, opciones de juego, tráileres, descripciones avanzadas, C#
ms.localizationpriority: medium
ms.openlocfilehash: e22081435bea8c73f509719aec1ce31d9157a315
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "4752254"
---
# <a name="c-sample-app-submission-with-game-options-and-trailers"></a>Muestra de C\#: envío de aplicación con opciones de juego y tráileres

En este artículo se proporcionan ejemplos de código C# que muestran cómo usar la [API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) para estas tareas:

* Obtén un token de acceso de Azure AD para usarlo con la API de envío de Microsoft Store.
* Crear un envío de aplicación
* Configura los datos de la descripción de la Store para el envío de aplicación, incluida las opciones de descripciones avanzadas de [juegos](manage-app-submissions.md#gaming-options-object) y [tráileres](manage-app-submissions.md#trailer-object).
* Carga el archivo ZIP que contiene los paquetes, las imágenes de descripciones y los archivos de tráileres para el envío de aplicación.
* Confirma el envío de aplicación.

Puedes revisar cada ejemplo para conocer más detalles sobre la tarea que muestra, o bien puedes compilar todos los ejemplos de código de este artículo en una aplicación de consola. Para compilar los ejemplos, crea una aplicación de consola de C# denominada **DevCenterApiSample** en Visual Studio, copia cada ejemplo en un archivo de código independiente en el proyecto y compila el proyecto.

## <a name="prerequisites"></a>Requisitos previos

Estos ejemplos cumplen los siguientes requisitos:

* Agrega una referencia al ensamblado System.Web en el proyecto.
* Instala el paquete NuGet [v4.5.11](http://www.newtonsoft.com/json) de Newtonsoft en el proyecto.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Crear un envío de aplicación

La clase ```CreateAndSubmitSubmissionExample``` define un método ```Execute``` público que llama a otros métodos de ejemplo para usar la API de envío de Microsoft Store para crear y confirmar un envío de aplicación que contiene opciones de juego y un tráiler. Para adaptar este código a tu propio uso:

* Asigna la variable ```tenantId``` al identificador de inquilino para tu aplicación y asigna las variables ```clientId``` y ```clientSecret``` al identificador de cliente y clave para tu aplicación. Para obtener más información, consulta [Asociación de una aplicación de Azure AD a tu cuenta del Centro de desarrollo de Windows](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-windows-dev-center-account)
* Asigna la variable ```applicationId``` al [Id. de la Store](in-app-purchases-and-trials.md#store-ids) de la aplicación para la cual quieres crear un envío.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/CreateAndSubmitSubmissionExample.cs#CreateAndSubmitSubmissionExample)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obtener un token de acceso de AzureAD

La clase ```DevCenterAccessTokenClient``` define un método auxiliar que usa tus valores ```tenantId```, ```clientId``` y ```clientSecret``` para crear un token de acceso de Azure AD para usarlo con la API de envío de Microsoft Store.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterAccessTokenClient.cs#DevCenterAccessTokenClient)]

<span id="utilities" />

## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>Métodos auxiliares para invocar la API de envío y los archivos de envío de carga

La clase ```DevCenterClient``` define métodos auxiliares que invocan una variedad de métodos en la API de envío de Microsoft Store y cargan el archivo ZIP que contiene los paquetes, las imágenes de descripciones y los archivos de tráileres para el envío de aplicación.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterClient.cs#DevCenterClient)]

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
