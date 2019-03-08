---
title: XboxServices.config
description: Describe el archivo XboxServices.config para asociar su juego para UWP con una configuración de Xbox Live.
ms.date: 03/29/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, configuración del servicio, xboxservices.config
ms.localizationpriority: medium
ms.openlocfilehash: 8ff538d691627bf4bb12b3ef6f8b1360e59ac701
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606690"
---
# <a name="xboxservicesconfig-file-description"></a>Descripción del archivo XboxServices.config

Al desarrollar una Xbox Live había habilitado juego para UWP, el proyecto debe incluir un archivo XboxServices.config.  Este archivo permite que el SDK de Xbox Live asociar su juego con la aplicación Centro de partners y la configuración de servicios de Xbox Live. Este archivo contiene un objeto JSON esa información de detalles como el Id. de configuración de servicio, identificador de título, etcetera.

Si usas Unity para diseñar un juego de programa de creadores de Xbox Live mediante el complemento Xbox Live, este archivo se crea automáticamente el Asistente para asociación de Xbox Live.

## <a name="xboxservicesconfig-fields"></a>XboxServices.config fields

>[!NOTE]
> El archivo creado por el Asistente para asociación de Xbox Live puede incluir campos adicionales más allá de los que se describen a continuación, pero no se usan por el servicio.

Los siguientes campos se definen en el objeto JSON en el archivo de configuración:

Campo | Descripción
--- | ---
PrimaryServiceConfigId  |  La configuración del servicio Xbox Live ID. (¿SCID). En [centro de partners](https://partner.microsoft.com/dashboard), puede encontrar este valor en el **Xbox Live** página (para el programa de creadores) o **el programa de instalación de Xbox Live** página (por completo Xbox Live games), en el **Servicios** sección de la aplicación.
TitleId  |  El identificador decimal del título de la aplicación. En [centro de partners](https://partner.microsoft.com/dashboard), puede encontrar este valor en el **Xbox Live** página (para el programa de creadores) o **el programa de instalación de Xbox Live** página (por completo Xbox Live games), en el **Servicios** sección de la aplicación.
XboxLiveCreatorsTitle  |  Si es "true", indica que la aplicación es una aplicación de programa de creadores de Xbox Live. En caso contrario, "false".
Ámbito  |  **(Opcional)**  Define el ámbito de funcionalidad que se usa la aplicación. Consulte a continuación para obtener una descripción adicional.

### <a name="scope-field"></a>Campo de ámbito

El **ámbito** campo es un campo opcional que puede usar para indicar la funcionalidad de su juego.


Si el **ámbito** campo no se especifica, el ámbito está establecido en un valor predeterminado que depende del valor de la **XboxLiveCreatorsTitle** campo, como se describe en la tabla siguiente:

XboxLiveCreatorsTitle value | Valor de ámbito predeterminado
--- | ---
"true"  |  "xbl.signin xbl.friends"
"false"  |  "xboxlive.signin"



La lista siguiente describe los valores válidos para el **ámbito** campo.

Valor de ámbito | Descripción
--- | ---
xbl.signin  | Incluye el inicio de sesión en la funcionalidad de juegos del programa de creadores. Se requiere para juegos del programa de creadores.
xbl.friends | Incluye la funcionalidad de marcadores sociales para juegos del programa de creadores y amigos.
xboxlive.signin | Incluye el inicio de sesión en la funcionalidad de juegos que tienen acceso a toda la funcionalidad de Xbox Live. Se requiere para juegos que no son creadores programa.

Actualmente, la única razón para especificar el **ámbito** campo es si va a hacer que un programa de creadores de Xbox Live game y su juego no necesita tener acceso a las listas de amigos o marcadores sociales (marcadores que se limitan a sus amigos). Si este es el caso, puede agregar la siguiente línea al archivo XboxServices.config:

```
  "Scope" : "xbl.signin"
```

Agregar esta línea, impide que la aplicación para UWP que solicite permiso para tener acceso a las listas de confianza cuando se inicia la aplicación por primera vez.

## <a name="example-xboxservicesconfig-file"></a>Archivo de ejemplo XboxServices.config

```
{
  "PrimaryServiceConfigId": "00000000-0000-0000-0000-000064382e34",
  "TitleId": 9039138423,
  "XboxLiveCreatorsTitle": true,
  "Scope" : "xbl.signin xbl.friends"
}
```
