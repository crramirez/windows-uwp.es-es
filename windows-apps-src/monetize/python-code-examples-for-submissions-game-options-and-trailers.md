---
author: mcleanbyron
description: "Usa los ejemplos de código de Python de esta sección para obtener más información sobre cómo enviar opciones de juego y tráileres usando la API de envío de la Tienda Windows."
title: "Muestra de Python: envío de aplicación con opciones de juego y tráileres"
ms.author: mcleans
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, API de envío de la Tienda Windows, ejemplos de código, opciones de juego, tráileres, descripciones avanzadas, python"
ms.openlocfilehash: 225d6721760737388c0a14a3a63cc0c9608d53a9
ms.sourcegitcommit: a7a1b41c7dce6d56250ce3113137391d65d9e401
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2017
---
# <a name="python-sample-app-submission-with-game-options-and-trailers"></a>Muestra de Python: envío de aplicación con opciones de juego y tráileres

En este artículo se proporcionan ejemplos de código Python que muestran cómo usar la [API de envío de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md) para estas tareas:

* Obtén un token de acceso de Azure AD para usarlo con la API de envío de la Tienda Windows.
* Crear un envío de aplicación
* Configura los datos de la descripción de la Tienda para el envío de aplicación, incluida las opciones de descripciones avanzadas de [juegos](manage-app-submissions.md#gaming-options-object) y [tráileres](manage-app-submissions.md#trailer-object).
* Carga el archivo ZIP que contiene los paquetes, las imágenes de descripciones y los archivos de tráileres para el envío de aplicación.
* Confirma el envío de aplicación.

<span id="create-app-submission" />
## <a name="create-an-app-submission"></a>Crear un envío de aplicación

Este código llama a otras clases y funciones de ejemplo para usar la API de envío de la Tienda Windows para crear y confirmar un envío de aplicación que contiene opciones de juego y un tráiler. Para adaptar este código a tu propio uso:

* Asigna la variable ```tenant``` al identificador de inquilino para tu aplicación y asigna las variables ```client``` y ```secret``` al identificador de cliente y clave para tu aplicación. Para obtener más información, consulta [Asociación de una aplicación de Azure AD a tu cuenta del Centro de desarrollo de Windows](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-windows-dev-center-account)
* Asigna la variable ```application_id``` al [Id. de la Tienda](in-app-purchases-and-trials.md#store_ids) de la aplicación para la cual quieres crear un envío.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/CreateAndSubmitAppSubmissionExample.py#L1-L74)]

<span id="token" />
## <a name="obtain-an-azure-ad-access-token-and-invoke-the-submission-api"></a>Obtener un token de acceso de Azure AD e invocar la API de envío

En el ejemplo siguiente se definen las clases siguientes:

* La clase ```DevCenterAccessTokenClient``` define un método auxiliar que usa tus valores ```tenantId```, ```clientId``` y ```clientSecret``` para crear un token de acceso de Azure AD para usarlo con la API de envío de la Tienda Windows.
* La clase ```DevCenterClient``` define métodos auxiliares que invocan una variedad de métodos en la API de envío de la Tienda Windows y cargan el archivo ZIP que contiene los paquetes, las imágenes de descripciones y los archivos de tráileres para el envío de aplicación.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/devcenterclient.py#L1-L126)]

<span id="token" />
## <a name="get-app-submission-listing-data"></a>Obtener datos de la descripción del envío de aplicación

El siguiente ejemplo define las funciones auxiliares que devuelven datos de la descripción con formato JSON para un nuevo envío de aplicación de muestra.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/submissiondatasamples.py#L1-L170)]

## <a name="related-topics"></a>Temas relacionados

* [Creación y administración de envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)