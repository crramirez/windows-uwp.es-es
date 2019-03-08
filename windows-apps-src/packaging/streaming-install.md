---
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: Instalación en streaming de aplicaciones para UWP
description: La instalación en streaming de la Plataforma universal de Windows (UWP) te permite especificar qué partes de la aplicación deben descargarse primero de Microsoft Store. Al descargar primero los archivos esenciales de la aplicación, el usuario puede iniciarla e interactuar con ella mientras el resto de archivos terminan de descargarse en segundo plano.
ms.date: 04/05/2017
ms.topic: article
keywords: windows 10, uwp, instalación en streaming, instalación en streaming de aplicaciones para uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3fa33410be31b1732a04c51d14dbbd114e1f5e0c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608660"
---
# <a name="uwp-app-streaming-install"></a>Instalación en streaming de aplicaciones para UWP
La instalación en streaming de la Plataforma universal de Windows (UWP) te permite especificar qué partes de la aplicación deben descargarse primero de Microsoft Store. Al descargar primero los archivos esenciales de la aplicación, el usuario puede iniciarla e interactuar con ella mientras el resto de archivos terminan de descargarse en segundo plano. 

Para usar la instalación en streaming de aplicaciones para UWP, tienes que dividir los archivos de la aplicación en secciones. Para ello, debes crear una asignación de grupo de contenido, que es un archivo XML que se incluye en la aplicación y que te permite establecer la prioridad de descarga y el orden. Para obtener más información, consulta el tema que se vincula a continuación.

Si quieres obtener una guía completa acerca de cómo agregar la instalación en streaming de aplicaciones para UWP a tu aplicación para UWP, consulta esta [serie de blogs](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/).

| Tema | Descripción | 
|-------|-------------|
| [Crear y convertir un mapa de contenido del grupo de origen](create-cgm.md) | Para preparar la aplicación de la Plataforma universal de Windows (UWP) para la instalación en streaming de la aplicación para UWP, tienes que crear una asignación de grupo de contenido. En este artículo encontrarás detalles, recomendaciones y consejos que te serán de utilidad para crear y convertir una asignación de grupo de contenido. |