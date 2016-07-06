---
author: WilliamsJason
title: Referencia de API de carga de carpeta del Device Portal
description: "Obtén información sobre cómo tener acceso mediante programación a la API de carga de carpeta."
ms.sourcegitcommit: fdc25fa4bd7bd5bfa598b993f23cd0ae9783dd0e
ms.openlocfilehash: 942ddc13b0deba382ad7758bc30bd9a5b0cceb11

---

# Cargar una carpeta en el directorio de desarrollo

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




<!--HONumber=Jun16_HO4-->


