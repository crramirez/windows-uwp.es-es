---
ms.assetid: 08C8F359-E8B6-4A45-8F4B-8A1962F0CE38
description: Microsoft Visual Studio es a Windows lo que Xcode a iOS y Mac OS. En este tutorial, te ayudamos a que te sientas cómodo usando Visual Studio.
title: Creación de un proyecto en Visual Studio
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6be772573321e7f1cbf5e5861d5f7b15a1ba5183
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174899"
---
# <a name="getting-started-creating-a-project"></a>Introducción: Creación de un proyecto

## <a name="creating-a-project"></a>Creación de un proyecto

Microsoft Visual Studio es a Windows lo que Xcode a iOS y Mac OS. En este tutorial, te ayudamos a que te sientas cómodo usando Visual Studio. Muestra los fundamentos que necesitarás para comenzar. Cada vez que crees una aplicación, seguirás unos pasos similares a estos.

En el siguiente vídeo se comparan Xcode y Visual Studio.

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Comparing-Xcode-to-Visual-Studio/player]

También encontrarás este [blog sobre la compilación de aplicaciones para Windows](https://blogs.windows.com/buildingapps/2016/01/27/visual-studio-walkthrough-for-ios-developers/) muy útil.

Crear una aplicación para Windows 10 (denominada más formalmente como aplicación de la plataforma universal de Windows [UWP]) es como crear una aplicación de iOS con guiones gráficos. Las aplicaciones de Windows 10 se suelen construir en varias páginas y cada una de ellas contiene una parte distinta de la interfaz de usuario, como un sitio web. Cada página suele tener dos archivos de origen asociados: uno para almacenar la interfaz de usuario en [Introducción a XAML](../xaml-platform/xaml-overview.md), y otro que contiene el código fuente, que suele ser C#. A medida que el usuario interactúe con la aplicación, irá pasando por estas páginas. En este tutorial vas a crear una aplicación con dos páginas.

**Nota:**    Una característica importante de las aplicaciones de Windows 10 es el hecho de que el mismo código fuente y el mismo conjunto de API están a su disposición independientemente de la plataforma. Como sabes, cuando escribes una aplicación de iOS universal para iPhone y iPad, puedes determinar en el tiempo de ejecución la plataforma en que se ejecuta la aplicación y realizar la acción apropiada. De manera similar, las aplicaciones de Windows 10 saben reconocer en tiempo de ejecución el dispositivo en que se ejecutan. Con una aplicación de UWP, no es necesario usar \# ifdef en el código fuente para crear compilaciones de teléfono en lugar de escritorio. Para mayor comodidad, las aplicaciones de Windows 10 también usan sus controles de interfaz de usuario de forma inteligente en función del dispositivo: por ejemplo, la aplicación puede hacer referencia a un control de selector de fecha, y el control adoptará automáticamente el aspecto y funcionamiento dependiendo de si la pantalla es de escritorio o de teléfono. Sin embargo, el código fuente no se modifica.

Veamos cómo crear una aplicación de Windows 10. En primer lugar, ejecuta Visual Studio. La primera vez que lo hagas, Visual Studio te pedirá que obtengas una licencia de desarrollador. Una licencia de desarrollador le permite instalar y probar aplicaciones para UWP en el equipo local antes de enviarlas al Microsoft Store. Para obtener una licencia, sigue las instrucciones en pantalla para iniciar sesión con una cuenta Microsoft. Si no tienes una, haz clic en el vínculo **Regístrese** del cuadro de diálogo **Licencia de desarrollador** y sigue las instrucciones en pantalla.

Si lo comparamos con Xcode, lo primero que ves cuando inicias Xcode es la pantalla de **bienvenida a Xcode**, parecida a la ilustración siguiente.

![pantalla de bienvenida de Xcode](images/ios-to-uwp/ios-to-uwp-xcode-welcome.png)

Visual Studio es muy similar. Verás la **página de inicio** que se muestra en la figura siguiente.

![pantalla inicio de visual studio](images/ios-to-uwp/ios-to-uwp-vs-welcome.png)

Para crear una aplicación nueva, empieza por crear un proyecto siguiendo uno de estos pasos:

-   En el área **Inicio**, pulsa **Nuevo proyecto**.
-   Pulsa el menú **Archivo** y, a continuación, pulsa **Nuevo proyecto**.

Como comparativa, cuando crees un proyecto en Xcode, verás una lista de plantillas de proyecto como las que se muestran en la ilustración siguiente.

![cuadro de diálogo de nuevo proyecto de xcode](images/ios-to-uwp/ios-to-uwp-xcode-choose-template.png)

En Visual Studio, también puedes elegir entre distintas plantillas de proyecto, como se muestra en la ilustración siguiente.

![cuadro de diálogo nuevo proyecto de Visual Studio ](images/ios-to-uwp/ios-to-uwp-vs-choose-template.png) para este tutorial, puntee en **Visual C#** y, a continuación, puntee en **Windows**, **Windows universal** y **aplicación vacía (Windows universal)**. En el cuadro **Nombre**, escribe "MyApp" y después pulsa **Aceptar**. Visual Studio crea el primer proyecto y después lo muestra. Ya puedes empezar a diseñar la aplicación y a agregarle código.

## <a name="next-step"></a>Paso siguiente

[Introducción: Elección de un lenguaje de programación](getting-started-choosing-a-programming-language.md)