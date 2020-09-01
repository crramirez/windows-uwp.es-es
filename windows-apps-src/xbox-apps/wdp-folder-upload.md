---
title: Referencia de API de carga de carpeta del Device Portal
description: Obtenga información sobre cómo puede cargar una carpeta en el directorio de desarrollo mediante la API de REST del portal de dispositivos Xbox.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e1a2c7f0-0040-4ce7-94de-17224736e20b
ms.localizationpriority: medium
ms.openlocfilehash: b71f60350bf5c8318adb2a4741bb1a275a4b0276
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157759"
---
# <a name="upload-a-folder-to-the-development-directory"></a>Cargar una carpeta en el directorio de desarrollo

**Solicitud**

Puedes cargar una carpeta completa de una vez en el id. de la carpeta conocida para DevelopmentFiles (o en una subcarpeta dentro de esa carpeta).

Método      | URI de solicitud
:------     | :------
POST | /api/app/packagemanager/upload 

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro del URI      | Descripción
:------     | :-----
destinationFolder (necesario) | El nombre de la carpeta de destino de la carpeta que se va a cargar. Esta carpeta se colocará en d:\developmentfiles\LooseApps en la consola. El nombre de esta carpeta debe estar codificado en Base64, ya que puede contener separadores de ruta de acceso si la carpeta es una subcarpeta de LooseApps.


**Encabezados de solicitud**

- None

**Cuerpo de la solicitud**

- Varias partes que conforman el cuerpo HTTP del contenido del directorio.

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Correcto
4XX | Códigos de error
5XX | Códigos de error

**Familias de dispositivos disponibles**

* Windows Xbox

