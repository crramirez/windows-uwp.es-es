---
author: laurenhughes
title: Desarrollar con paquetes de activos y plegado de paquete
description: Aprende a organizar de forma eficaz tu aplicación con paquetes de activos y plegado de paquete.
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, packaging, empaquetado, package layout, distribución de paquete, asset package, paquete de activos
ms.localizationpriority: medium
ms.openlocfilehash: 31c27430c850f861c8b97863521202a6dcab80f7
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "4356912"
---
# <a name="developing-with-asset-packages-and-package-folding"></a>Desarrollar con paquetes de activos y plegado de paquete 

> [!IMPORTANT]
> Si quieres enviar la aplicación a la Store, debes ponerte en contacto con [Soporte técnico para desarrolladores de Windows](https://developer.microsoft.com/windows/support) para obtener la aprobación para utilizar paquetes de activos y plegado de paquete.

Los paquetes de activos pueden reducir el tamaño general del empaquetado y el tiempo de publicación de tus aplicaciones en la Store. Puedes obtener más información sobre los paquetes de activos y cómo puede acelerar las iteraciones de desarrollo en [Introducción a los paquetes de activos](asset-packages.md).

Si estás pensando usar paquetes de activos para tu aplicación o ya sabes que quieres usarlos, probablemente te preguntes cómo cambiarán los paquetes de activos el proceso de desarrollo. En resumen, el desarrollo de aplicaciones sigue siendo igual, esto es posible gracias al plegado del paquete para paquetes de activos.

## <a name="file-access-after-splitting-your-app"></a>Acceso a los archivos después de dividir la aplicación

Para comprender cómo el plegado de paquete no afecta al proceso de desarrollo, vamos a retroceder primero para comprender lo que sucede cuando divides tu aplicación en varios paquetes (con paquetes de activos o paquetes de recursos). 

En un nivel alto, cuando divides algunos de los archivos de la aplicación en otros paquetes (que no sean paquetes de arquitectura), no podrás acceder a esos archivos directamente con respecto al lugar en el que se ejecuta el código. Esto es porque estos paquetes se instalan todos en directorios diferentes con respecto al lugar en el que está instalado el paquete de arquitectura. Por ejemplo, si vas a crear un juego y el juego se localizado en francés y alemán y se ha compilado para x86 y x64 equipos, deberías tener estos archivos de paquete de aplicación dentro de la recopilación de aplicación de tu juego:

-   MyGame_1.0_x86.appx
-   MyGame_1.0_x64.appx
-   MyGame_1.0_language-fr.appx
-   MyGame_1.0_language-de.appx

Cuando tu juego esté instalado en el equipo del usuario, cada archivo de paquete de la aplicación tendrá su propia carpeta en el directorio **WindowsApps** . Por lo tanto, para un usuario francés con Windows de 64 bits, tu juego tendrá este aspecto:

```example
C:\Program Files\WindowsApps\
|-- MyGame_1.0_x64
|   `-- …
|-- MyGame_1.0_language-fr
|   `-- …
`-- …(other apps)
```

Ten en cuenta que el paquete de la aplicación no estará archivos que no son aplicables al usuario instalado (el x86 y paquetes alemán). 

Para este usuario, el archivo ejecutable principal de tu juego estará dentro de la carpeta **MyGame_1.0_x64** y se ejecutará desde allí y, normalmente, solo tendrá acceso a los archivos de esta carpeta. Para obtener acceso a los archivos de la carpeta **MyGame_1.0_language-fr**, tendrías que usar las API de MRT o las API de PackageManager. Las API de MRT puede seleccionar automáticamente el archivo más adecuado de los idiomas instalados, puedes encontrar más información sobre las API de MRT en [Windows.ApplicationModel.Resources.Core](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core). Como alternativa, puedes encontrar la ubicación instalada del paquete de idioma francés usando la [clase PackageManager](https://docs.microsoft.com/uwp/api/Windows.Management.Deployment.PackageManager). Nunca debes dar por supuesta la ubicación instalada de los paquetes de la aplicación ya que puede cambiar y puede variar entre usuarios. 

## <a name="asset-package-folding"></a>plegado de paquete de activos

¿Cómo puedes tener acceso a los archivos de tus paquetes de activos? Bueno, puedes seguir usando las API de acceso a archivos que estés usando para acceder a cualquier otro archivo del paquete de arquitectura. Esto es porque los archivos de paquetes de activos se plegarán en el paquete de arquitectura cuando se instala a través del proceso de plegado de paquete. Además, dado que los archivos de paquete de activos deben ser originalmente archivos dentro de los paquetes de arquitectura, esto significa que no tendrías que cambiar el uso de la API cuando cambies de implementación de archivos sueltos a empaquetados en el proceso de desarrollo. 

Para conocer más sobre cómo funciona el plegado de paquete, comencemos con un ejemplo. Si tienes un proyecto de juego con la siguiente estructura de archivos:

```example
MyGame
|-- Audios
|   |-- Level1
|   |   `-- ...
|   `-- Level2
|       `-- ...
|-- Videos
|   |-- Level1
|   |   `-- ...
|   `-- Level2
|       `-- ...
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
```

Si quieres dividir tu juego en 3 paquetes: un paquete de arquitectura x64, un paquete de activos de audio y un paquete de activos para vídeos, tu juego se dividirá estos paquetes:

```example
MyGame_1.0_x64.appx
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
MyGame_1.0_Audios.appx
`-- Audios
    |-- Level1
    |   `-- ...
    `-- Level2
        `-- ...
MyGame_1.0_Videos.appx
`-- Videos
    |-- Level1
    |   `-- ...
    `-- Level2
        `-- ...
```

Cuando instales tu juego, el paquete x64 se implementará en primer lugar. A continuación, los dos paquetes de activos se implementarán en sus propias carpetas, como **MyGame_1.0_language-fr** en el ejemplo anterior. Sin embargo, debido al plegado de paquete, los archivos del paquete de activos también tendrán un vínculo físico para que aparezcan en la carpeta **MyGame_1.0_x64** (de forma que aunque los archivos aparezcan en dos ubicaciones, ya no ocuparán el doble de espacio en disco). La ubicación en la que aparecerán los archivos del paquete de activos es exactamente la ubicación en la que están en relación con la raíz del paquete. Por tanto, este es el aspecto que tendrá el diseño final del juego implementado:

```example 
C:\Program Files\WindowsApps\
|-- MyGame_1.0_x64
|   |-- Audios
|   |   |-- Level1
|   |   |   `-- ...
|   |   `-- Level2
|   |       `-- ...
|   |-- Videos
|   |   |-- Level1
|   |   |   `-- ...
|   |   `-- Level2
|   |       `-- ...
|   |-- Engine
|   |   `-- ...
|   |-- XboxLive
|   |   `-- ...
|   `-- Game.exe
|-- MyGame_1.0_Audios
|   `-- Audios
|       |-- Level1
|       |   `-- ...
|       `-- Level2
|           `-- ...
|-- MyGame_1.0_Videos
|   `-- Videos
|       |-- Level1
|       |   `-- ...
|       `-- Level2
|           `-- ...
`-- …(other apps)
```

Cuando uses el plegado de paquete para los paquetes de activos, puedes seguir accediendo a los archivos que has dividido en paquetes de activos de la misma forma (ten en cuenta que la carpeta de arquitectura tiene la misma estructura exacta que la carpeta del proyecto original) y puedes agregar paquetes de activos o mover archivos entre paquetes de activos sin que afecte al código. 

Ahora veremos un plegado de paquete más complicado. Supongamos que en su lugar quieres dividir los archivos basándote en el nivel y, si quieres mantener la misma estructura que la carpeta del proyecto original, los paquetes deben tener el siguiente aspecto:

```example
MyGame_1.0_x64.appx
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
MyGame_Level1.appx
|-- Audios
|   `-- Level1
|       `-- ...
`-- Videos
    `-- Level1
        `-- ...

MyGame_Level2.appx
|-- Audios
|   `-- Level2
|       `-- ...
`-- Videos
    `-- Level2
        `-- ...
```
Esto permitirá fusionar las carpetas de **Level1** y los archivos del paquete **MyGame_Level1** y las carpetas de **Level2** y los archivos del paquete **MyGame_Level2** en las carpetas **Audios** y **Videos** durante el plegado del paquete. Por tanto, como regla general, la ruta de acceso relativa designada para los archivos empaquetados en el archivo de asignación o [diseño de empaquetado](packaging-layout.md) para MakeAppx.exe es la ruta de acceso que debes utilizar para acceder a ellos después del plegado del paquete. 

Por último, si hay dos archivos en diferentes paquetes de activos que tienen las mismas rutas relativas, habrá un conflicto durante el plegado del paquete. Si se produce una colisión, se producirá un error en la implementación de la aplicación y fallará. Además, debido a que el plegado del paquete aprovecha las ventajas de los vínculos físicos, si utilizas paquetes de activos, la aplicación no podrá implementarse en unidades que no sean NTFS. Si sabes que es probable que los usuarios trasladen la aplicación a unidades extraíbles, no debes usar paquetes de activos. 


