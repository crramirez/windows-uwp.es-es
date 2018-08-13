---
author: laurenhughes
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: Instalación en streaming de aplicaciones para UWP
description: La instalación en streaming de la Plataforma universal de Windows (UWP) te permite especificar qué partes de la aplicación deben descargarse primero de Microsoft Store. Al descargar primero los archivos esenciales de la aplicación, el usuario puede iniciarla e interactuar con ella mientras el resto de archivos terminan de descargarse en segundo plano.
ms.author: lahugh
ms.date: 04/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: aplicación uwp transmisión por secuencias de Windows 10, uwp, transmisión por secuencias de instalación, instalar
ms.localizationpriority: medium
ms.openlocfilehash: 087226cad4bcf7ea0294d8878564c345d6cfb9d0
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "305294"
---
# <a name="uwp-app-streaming-install"></a>Instalación en streaming de aplicaciones para UWP
La instalación en streaming de la Plataforma universal de Windows (UWP) te permite especificar qué partes de la aplicación deben descargarse primero de Microsoft Store. Al descargar primero los archivos esenciales de la aplicación, el usuario puede iniciarla e interactuar con ella mientras el resto de archivos terminan de descargarse en segundo plano. 

Para usar UWP transmisión por secuencias instalar la aplicación que necesitará para dividir los archivos de su aplicación en las secciones. Para ello, va a crear un mapa de contenido de grupo, que es un archivo XML que se incluye con la aplicación, lo que le permite establecer la prioridad de descarga y orden. Vea el tema vinculado a continuación para obtener más información.

Para obtener una guía completa sobre cómo agregar UWP transmisión por secuencias instalar la aplicación a su aplicación UWP, consulte esta [serie de blog](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/).

| Tema | Descripción | 
|-------|-------------|
| [Crear y convertir una asignación de grupo de contenido de origen](create-cgm.md) | Para preparar la aplicación de la Plataforma universal de Windows (UWP) para la instalación en streaming de la aplicación para UWP, tienes que crear una asignación de grupo de contenido. En este artículo encontrarás detalles, recomendaciones y consejos que te serán de utilidad para crear y convertir una asignación de grupo de contenido. |