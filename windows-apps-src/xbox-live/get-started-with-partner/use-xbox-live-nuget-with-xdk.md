---
title: Usar el paquete NuGet de Xbox Live con el XDK
description: Obtenga información sobre cómo usar el paquete API NuGet de Xbox Live para desarrollar los títulos XDK.
ms.assetid: 2c5ae514-393d-48bb-afd8-a897d35f7938
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, NuGet
ms.localizationpriority: medium
ms.openlocfilehash: c955ca42c09075e5125683588c335cfa47443f00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655040"
---
# <a name="use-the-xbox-live-api-nuget-package-to-develop-xdk-titles"></a>Use el paquete API NuGet de Xbox Live para desarrollar los títulos XDK

### <a name="1--ensure-you-have-the-latest-nuget-package-manager-installed"></a>1.  Asegurarse de que el Administrador de paquetes de NuGet más reciente instalado
1.  Comprobar la versión actual:
    - En la barra de menús, seleccione Herramientas -> extensiones y actualizaciones.
    - En la pestaña instalado, busque `NuGet Package Manager`
![](../images/nuget/nuget_uwp_install_1.png)
2.  Para actualizar su versión actual:
    - En la barra de menús, seleccione Herramientas -> extensiones y actualizaciones.
    - En las actualizaciones -> pestaña de la Galería de Visual Studio, seleccione `Update`
![](../images/nuget/nuget_uwp_install_2.png)

### <a name="2--add-reference-to-the-project"></a>2.  Agregar referencia al proyecto
1.  Agregar referencia al proyecto
    1.  Haga clic con el botón derecho en la solución del proyecto y seleccione "Administrar paquetes de NuGet"
<br/>
![](../images/nuget/nuget_xbox_install_4.png)
1.  Busque `Xbox Live` y seleccione el paquete adecuado y haga clic en `Install`.
  - La API de servicios de Xbox está disponible en versiones de UWP y XDK y de C++ y WinRT.  
  - Elija entre `Microsoft.Xbox.Live.SDK.*.UWP` y `Microsoft.Xbox.Live.SDK.*.XboxOneXDK`.  `XboxOneXDK` es para ID@Xbox y los desarrolladores administrados que usan el XDK una Xbox.  `UWP` es para los juegos UWP que pueden ejecutarse en PC, la Xbox One o de Windows Phone.  Puede leer más sobre la ejecución de UWP en Xbox One en [https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started)
  - Elija entre `Microsoft.Xbox.Live.SDK.Cpp.*` y `Microsoft.Xbox.Live.SDK.WinRT.*`. `Cpp` es para los motores de juegos de C++ con las API de Xbox Live.  `WinRT` es para los motores de juegos escritos en C++, C#, o Javascript con las API de Xbox Live.  Al usar WinRT con un motor de C++, se usaría C++ / c++ / CX que utiliza sombreros (^).  `Cpp` es la API que se recomienda que se usará para los motores de juegos de C++.    
![](../images/nuget/nuget_xbox_install_5.png)
![](../images/nuget/nuget_uwp_install_7.png)
1. Después de aceptar la TOS de licencia, espere hasta que se ha agregado correctamente el paquete.  Debería ver este registro en la ventana de salida del Administrador de paquetes:

```
========== Finished ==========
```

### <a name="3--optionally-include-header"></a>3.  Opcionalmente, incluya el encabezado
* Para `Microsoft.Xbox.Live.SDK.Cpp.*` proyectos basados en `#include <xsapi\services.h>` en el origen del proyecto.
* Para `Microsoft.Xbox.Live.SDK.WinRT.*` en función de los proyectos, no es necesario incluir los encabezados.   
