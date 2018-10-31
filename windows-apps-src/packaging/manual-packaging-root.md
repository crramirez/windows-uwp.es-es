---
author: laurenhughes
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: Empaquetado manual de la aplicación
description: En esta sección se incluyen artículos o vínculos a artículos sobre el empaquetado manual de aplicaciones para la Plataforma universal de Windows (UWP).
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
keywords: windows 10, uwp, packaging, empaquetado
ms.localizationpriority: medium
ms.openlocfilehash: 0268e858ecbcaaee95796fa590d4a9994dcfb505
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5868340"
---
# <a name="manual-app-packaging"></a>Empaquetado manual de la aplicación

Si quieres crear y firmar un paquete de aplicación, pero no usaste Visual Studio para desarrollar tu aplicación, tendrás que usar las herramientas de empaquetado de aplicación manual.

> [!IMPORTANT] 
> Si usaste Visual Studio para desarrollar tu aplicación, es recomendable usar el Asistente de Visual Studio para crear y firmar el paquete de la aplicación. Para más información, consulta [Empaquetado de aplicaciones para UWP con Visual Studio](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

## <a name="purpose"></a>Propósito

En esta sección se incluyen artículos o vínculos a artículos sobre el empaquetado manual de aplicaciones para la Plataforma universal de Windows (UWP).

| Tema | Descripción |
|-------|-------------|
| [Crear un paquete de la aplicación con la herramienta MakeAppx.exe](create-app-package-with-makeappx-tool.md) | MakeAppx.exe crea, cifra, descifra y extrae los archivos de paquetes de aplicaciones y lotes. |
| [Crear un certificado para firmar paquetes](create-certificate-package-signing.md) | Crea y exporta un certificado para firmar paquetes de la aplicación con herramientas de PowerShell. |
| [Firmar un paquete de la aplicación con SignTool](sign-app-package-using-signtool.md) | Usa SignTool para firmar manualmente un paquete de la aplicación con un certificado. |

### <a name="advanced-topics"></a>Temas avanzados

Esta sección contiene temas más avanzados para crear componentes de una aplicación grande o compleja para un empaquetado e instalación más eficaces. 

> [!IMPORTANT]
> Si quieres enviar la aplicación a la Store, debes ponerte en contacto con [Soporte técnico de desarrolladores de Windows](https://developer.microsoft.com/windows/support) y obtener su aprobación para usar cualquiera de las características avanzadas de esta sección.


| Tema | Descripción |
|-------|-------------|
| [Introducción a los paquetes de activos](asset-packages.md) | Los paquetes de activos son un tipo de paquete que actúa como una ubicación centralizada para archivos comunes de la aplicación, lo que elimina la necesidad de archivos duplicados en todos los paquetes de arquitectura. |
| [Desarrollar con paquetes de activos y plegado de paquete](package-folding.md) | Aprende a organizar de forma eficaz tu aplicación con paquetes de activos y plegado de paquete. |
| [Paquetes de aplicaciones de lotes planos](flat-bundles.md) | Se describe cómo crear una recopilación plana para archivos de paquete de la aplicación. |
| [Creación del paquete con el diseño del empaquetado](packaging-layout.md) | El diseño del empaquetado es un solo documento que describe la estructura del empaquetado de la aplicación. Especifica los lotes de una aplicación (principal y opcional), los paquetes de los lotes y los archivos de los paquetes. |
