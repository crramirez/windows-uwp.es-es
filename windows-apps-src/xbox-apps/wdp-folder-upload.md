---
author: WilliamsJason
title: Referencia de API de carga de carpeta del Device Portal
description: "Obtén información sobre cómo tener acceso mediante programación a la API de carga de carpeta."
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: e1a2c7f0-0040-4ce7-94de-17224736e20b
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 8c2251c5c78fed10959b89de0d81ff563d0fa3e3
ms.lasthandoff: 02/08/2017

---

# <a name="upload-a-folder-to-the-development-directory"></a>Cargar una carpeta en el directorio de desarrollo

**Solicitud**

Puedes cargar una carpeta completa de una vez en el id. de la carpeta conocida para DevelopmentFiles (o en una subcarpeta dentro de esa carpeta).

Método      | URI de la solicitud
:------     | :------
POST | /api/app/packagemanager/upload 
<br />
**Parámetros del URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro del URI      | Descripción
:------     | :-----
destinationFolder (necesario) | El nombre de la carpeta de destino de la carpeta que se va a cargar. Esta carpeta se colocará en d:\developmentfiles\LooseApps en la consola. El nombre de esta carpeta debe estar codificado en Base64, ya que puede contener separadores de ruta de acceso si la carpeta es una subcarpeta de LooseApps.
<br />

**Encabezados de la solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Varias partes que conforman el cuerpo HTTP del contenido del directorio.

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Correcto
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Xbox


