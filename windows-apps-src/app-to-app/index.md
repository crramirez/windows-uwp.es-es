---
ms.assetid: E0728EB0-DFC3-4203-A367-8997B16E2328
description: En esta sección se explica cómo compartir datos entre aplicaciones para la Plataforma universal de Windows (UWP), como por ejemplo, cómo usar el contrato para contenido compartido, cómo copiar y pegar y cómo arrastrar y colocar.
title: Comunicación entre aplicaciones
ms.date: 02/12/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2de9d5ae04c526c7c35fda4a7aef713814b4fbc2
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "77258331"
---
# <a name="app-to-app-communication"></a>Comunicación entre aplicaciones

En esta sección se explica cómo compartir datos entre aplicaciones para la Plataforma universal de Windows (UWP) (por ejemplo, cómo usar el contrato para contenido compartido, cómo copiar y pegar y cómo arrastrar y colocar) y servicios de aplicaciones.

El contrato para contenido compartido es una forma mediante la cual los usuarios pueden intercambiar datos rápidamente entre aplicaciones. Por ejemplo, es posible que un usuario quiera compartir una página web con sus amigos mediante una aplicación de red social, o guardar un vínculo en una aplicación de notas para consultarlo más adelante. Si tu aplicación tiene escenarios para recibir contenido que el usuario puede completar rápidamente en el contexto de otra aplicación, deberías usar un contrato para contenido compartido.

Una aplicación puede admitir la función Compartir de dos modos. En primer lugar, puede tratarse de una [aplicación de origen que proporciona contenido que el usuario quiere compartir](share-data.md). En segundo lugar, puede tratarse de una [aplicación de destino que el usuario selecciona como el destino del contenido compartido](receive-data.md). Una aplicación puede ser de origen y de destino. Si quieres que tu aplicación comparta contenido como aplicación de origen, debes decidir qué formatos de datos puede proporcionar.

Además del contrato para contenido compartido, las aplicaciones también pueden integrar técnicas clásicas para transferir datos, tales como [arrastrar y colocar](../design/input/drag-and-drop.md) o [copiar y pegar](copy-and-paste.md). Además de la comunicación entre aplicaciones para UWP, estos métodos también admiten el uso compartido entre aplicaciones de escritorio.

Las aplicaciones para UWP también pueden crear [servicios de aplicaciones ](../launch-resume/how-to-create-and-consume-an-app-service.md) que proporcionan funcionalidades a otras aplicaciones para UWP. Un servicio de aplicaciones se ejecuta como tarea en segundo plano en la aplicación host y puede proporcionar su servicio a otras aplicaciones. Por ejemplo, un servicio de aplicaciones podría proporcionar un servicio de escáner de códigos de barras que podrían usar otras aplicaciones.

## <a name="in-this-section"></a>En esta sección

| Tema | Descripción |
|-------|-------------|
| [Compartir datos](share-data.md) | En este artículo se explica cómo admitir el contrato para contenido compartido en una aplicación para UWP. El contrato para contenido compartido es una manera sencilla de compartir rápidamente los datos, como texto, vínculos, fotos y vídeos, entre aplicaciones. Por ejemplo, es posible que un usuario quiera compartir una página web con sus amigos mediante una aplicación de red social, o guardar un vínculo en una aplicación de notas para consultarlo más adelante. |
| [Recibir datos](receive-data.md) | En este artículo se explica cómo recibir contenido compartido en la aplicación para UWP desde otra aplicación mediante el contrato para contenido compartido. Este contrato para contenido compartido permite que la aplicación se presente como una opción cuando el usuario invoca Compartir. |
| [Copiar y pegar](copy-and-paste.md) | En este artículo se explica cómo admitir las funciones copiar y pegar en aplicaciones para UWP usando el portapapeles. Copiar y pegar es la forma clásica de intercambiar datos entre aplicaciones o dentro de una aplicación y casi todas las aplicaciones admiten operaciones del portapapeles hasta cierto punto. |
| [Arrastrar y colocar](../design/input/drag-and-drop.md) | En este artículo se explica cómo agregar la funcionalidad de arrastrar y colocar elementos en la aplicación para UWP. Arrastrar y soltar es una forma clásica y natural de interactuar con contenido, como imágenes y archivos. Una vez implementada, la opción para arrastrar y colocar elementos funciona perfectamente en todos los sentidos; por ejemplo, de una aplicación a otra, de una aplicación al escritorio y del escritorio a una aplicación. |
| [Crear y usar un servicio de aplicación](../launch-resume/how-to-create-and-consume-an-app-service.md) | En este artículo se explica cómo crear un servicio de aplicaciones en una aplicación para UWP que proporciona servicios a otras aplicaciones para UWP.  |

## <a name="see-also"></a>Consulta también

- [Desarrollar aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/develop/)
