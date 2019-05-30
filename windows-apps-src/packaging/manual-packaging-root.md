---
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: Empaquetado de aplicación manual
description: En esta sección se incluyen artículos o vínculos a artículos sobre el empaquetado manual de aplicaciones para la Plataforma universal de Windows (UWP).
ms.date: 04/30/2018
ms.topic: article
keywords: windows 10, uwp, packaging, empaquetado
ms.localizationpriority: medium
ms.openlocfilehash: d90307560c315741751a5ab58ccdf0eaf35d97fc
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372791"
---
# <a name="manual-app-packaging"></a>Empaquetado de aplicación manual

Si quieres crear y firmar un paquete de aplicación, pero no usaste Visual Studio para desarrollar tu aplicación, tendrás que usar las herramientas de empaquetado de aplicación manual.

> [!IMPORTANT] 
> Si usaste Visual Studio para desarrollar tu aplicación, es recomendable usar el Asistente de Visual Studio para crear y firmar el paquete de la aplicación. Para más información, consulta [Empaquetado de aplicaciones para UWP con Visual Studio](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

## <a name="purpose"></a>Finalidad

En esta sección se incluyen artículos o vínculos a artículos sobre el empaquetado manual de aplicaciones para la Plataforma universal de Windows (UWP).

| Tema | Descripción |
|-------|-------------|
| [Crear un paquete de aplicación con la herramienta MakeAppx.exe](create-app-package-with-makeappx-tool.md) | MakeAppx.exe crea, cifra, descifra y extrae los archivos de paquetes de aplicaciones y lotes. |
| [Crear un certificado de firma del paquete](create-certificate-package-signing.md) | Crea y exporta un certificado para firmar paquetes de la aplicación con herramientas de PowerShell. |
| [Firmar un paquete de aplicación mediante SignTool](sign-app-package-using-signtool.md) | Usa SignTool para firmar manualmente un paquete de aplicación con un certificado. |

### <a name="advanced-topics"></a>Temas avanzados

Esta sección contiene temas más avanzados para crear componentes de una aplicación grande o compleja para un empaquetado e instalación más eficaces. 

> [!IMPORTANT]
> Si quieres enviar la aplicación a la Store, debes ponerte en contacto con [Soporte técnico de desarrolladores de Windows](https://developer.microsoft.com/windows/support) y obtener su aprobación para usar cualquiera de las características avanzadas de esta sección.


| Tema | Descripción |
|-------|-------------|
| [Introducción a los paquetes de activos](asset-packages.md) | Los paquetes de recurso son un tipo de paquete que actúa como una ubicación centralizada para archivos comunes de la aplicación, lo que elimina la necesidad de archivos duplicados en todos los paquetes de arquitectura. |
| [Desarrollo con paquetes de activo y plegamiento de paquete](package-folding.md) | Aprende a organizar de forma eficaz tu aplicación con paquetes de activos y plegado de paquete. |
| [Paquetes de aplicación sin formato](flat-bundles.md) | Describe cómo crear un paquete sin formato para archivos de paquete de la aplicación. |
| [Creación del paquete con el diseño de empaquetado](packaging-layout.md) | El diseño del empaquetado es un solo documento que describe la estructura del empaquetado de la aplicación. Especifica los lotes de una aplicación (principal y opcional), los paquetes de los lotes y los archivos de los paquetes. |
