---
title: Agregar Xbox Live a un proyecto XDK
description: Obtenga información sobre cómo agregar Xbox Live a un proyecto de Xbox Developer Kit (XDK) nueva o existente.
ms.assetid: fc6f987c-1a87-4ff5-b063-891591aa6653
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, xdk
ms.localizationpriority: medium
ms.openlocfilehash: f17765b09dcb0b6f5c89d168f2d3d9611a60ffa6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649050"
---
# <a name="add-xbox-live-to-a-new-or-existing-xdk-project"></a>Agregar Xbox Live a un proyecto XDK nuevo o existente

Este tema describe cómo agregar Xbox Live a un proyecto XDK nuevo o existente.

El proceso es:

- El programa de instalación de su entorno de desarrollo de Xbox One
- Obtener los identificadores
- Configurar la consola de desarrollo
- Agregue el TitleID y ¿SCID para el archivo binario


## <a name="setup-up-your-xbox-one-development-environment"></a>El programa de instalación de su entorno de desarrollo de Xbox One
En primer lugar, configurar la consola, siga la sección "Configuración de la Xbox un entorno de desarrollo" en la documentación de XDK

## <a name="get-your-ids"></a>Obtener los identificadores

Para habilitar los servicios de Xbox Live, deberá obtener varios identificadores para configurar el kit de desarrollo y su título. Es posible con el mismo proceso.

Obtendrá los identificadores siguiendo el proceso de [configuración del servicio de Xbox Live](../xbox-live-service-configuration.md)

## <a name="configure-your-development-console"></a>Configurar la consola de desarrollo

Una vez que los identificadores, siga el [configurar la consola de desarrollo](configure-your-development-console.md) guía para configurar la consola de desarrollo.

## <a name="add-the-titleid-and-scid-to-your-binary"></a>Agregue el TitleID y ¿SCID para el archivo binario
Mientras se configura el espacio aislado en un nivel de plataforma para cada Kit de desarrollo, el TitleID y ¿SCID están enlazados a un binario concreto. Para agregar un TitleID y ¿SCID para el archivo binario, modifique el Package.appxmanifest para ese binario agregando un nuevo nodo en el <Extensions> nodo como sigue:

```
<Applications>
    ...
    <Application ...>
      ...
      <Extensions>
        <mx:Extension Category="xbox.live">
           <mx:XboxLive TitleId="<your titleID>" PrimaryServiceConfigId="<your SCID>" RequireXboxLive="<boolean indicating Live requirement>" />
        </mx:Extension>
      </Extensions>
   </Application>
</Applications>
```

Para obtener más información sobre el archivo AppxManifest.xml, hacer referencia a plantillas de proyecto en Visual Studio para el desarrollo de una Xbox.

Vea el esquema de manifiesto de aplicación para obtener una descripción de la aplicación manifiesto del esquema.

**La marca RequireXboxLive** si no se iniciará el RequireXboxLive marca se establece en true, el título a menos que el nivel de conexión Windows.Networking.Connectivity devuelve como XboxLiveAccess y el título borra la autenticación con Xbox Live. Esto garantiza que el título ha tomado las últimas actualizaciones de contenido. Si se pierde la conexión mientras se está ejecutando el título, se suspende el título.

Títulos de "Internet Required" solo deben marcar RequireXboxLive como true y tenga en cuenta que marcar el título de esta manera no garantiza que los servicios necesarios para el título están activos y en ejecución.
