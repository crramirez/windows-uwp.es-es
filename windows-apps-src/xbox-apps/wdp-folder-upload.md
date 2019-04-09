---
title: Referencia de API de carga de carpeta del Device Portal
description: Obtén información sobre cómo tener acceso mediante programación a la API de carga de carpeta.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e1a2c7f0-0040-4ce7-94de-17224736e20b
ms.localizationpriority: medium
ms.openlocfilehash: 870d203271cb75ecf5531106bb2c10b3736db9b9
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244051"
---
# <a name="upload-a-folder-to-the-development-directory"></a>Cargar una carpeta en el directorio de desarrollo

**Solicitud**

Puedes cargar una carpeta completa de una vez en el id. de la carpeta conocida para DevelopmentFiles (o en una subcarpeta dentro de esa carpeta).

Método      | URI de la solicitud
:------     | :------
EXPONER | /api/app/packagemanager/upload 

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro del URI      | Descripción
:------     | :-----
destinationFolder (necesario) | El nombre de la carpeta de destino de la carpeta que se va a cargar. Esta carpeta se colocará en d:\developmentfiles\LooseApps en la consola. El nombre de esta carpeta debe estar codificado en Base64, ya que puede contener separadores de ruta de acceso si la carpeta es una subcarpeta de LooseApps.


**Encabezados de solicitud**

- Ninguno

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

