---
author: laurenhughes
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: Instalación en streaming de aplicaciones para UWP
description: La instalación en streaming de la Plataforma universal de Windows (UWP) te permite especificar qué partes de la aplicación deben descargarse primero de Microsoft Store. Al descargar primero los archivos esenciales de la aplicación, el usuario puede iniciarla e interactuar con ella mientras el resto de archivos terminan de descargarse en segundo plano.
ms.author: lahugh
ms.date: 04/05/2017
ms.topic: article
keywords: aplicación para uwp de streaming de Windows 10, uwp, streaming de instalación, instalar
ms.localizationpriority: medium
ms.openlocfilehash: e4915d2fb4d1133cd190d766d38c79934d9f3956
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "6185733"
---
# <a name="uwp-app-streaming-install"></a>Instalación en streaming de aplicaciones para UWP
La instalación en streaming de la Plataforma universal de Windows (UWP) te permite especificar qué partes de la aplicación deben descargarse primero de Microsoft Store. Al descargar primero los archivos esenciales de la aplicación, el usuario puede iniciarla e interactuar con ella mientras el resto de archivos terminan de descargarse en segundo plano. 

Para usar la instalación en Streaming UWP tendrás que dividir los archivos de la aplicación en secciones. Para ello, se creará una asignación de grupo de contenido, que es un archivo XML que se empaqueta con tu aplicación, lo que le permite establecer la prioridad de descarga y el orden. Consulta el tema que se vinculan a continuación para obtener más información.

Para obtener una guía completa sobre cómo agregar instalación en Streaming UWP a tu aplicación para UWP, echa un vistazo a esta [serie de blogs](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/).

| Tema | Descripción | 
|-------|-------------|
| [Crear y convertir una asignación de grupo de contenido de origen](create-cgm.md) | Para preparar la aplicación de la Plataforma universal de Windows (UWP) para la instalación en streaming de la aplicación para UWP, tienes que crear una asignación de grupo de contenido. En este artículo encontrarás detalles, recomendaciones y consejos que te serán de utilidad para crear y convertir una asignación de grupo de contenido. |