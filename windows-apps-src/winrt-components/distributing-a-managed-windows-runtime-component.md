---
title: Distribuir un componente de Windows Runtime administrado
description: Puedes distribuir tu componente de Windows Runtime mediante la copia de archivos.
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ef51e2235d8ac5c46af6093809d241d5c137d57d
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8883347"
---
# <a name="distributing-a-managed-windows-runtime-component"></a>Distribuir un componente de Windows Runtime administrado



Puedes distribuir tu componente de Windows Runtime mediante la copia de archivos. Sin embargo, si el componente consta de muchos archivos, la instalación puede ser tediosa para los usuarios. Además, los errores en la colocación de archivos o la incapacidad para establecer referencias pueden causarles problemas. Puedes empaquetar un componente complejo como un SDK de extensión de Visual Studio, para que sea fácil de instalar y usar. Los usuarios solamente necesitan establecer una referencia para todo el paquete. Pueden localizar e instalar tu componente usando el cuadro de diálogo **Extensiones y actualizaciones**, como se describe en [Búsqueda y uso de extensiones de Visual Studio](https://msdn.microsoft.com/library/vstudio/dd293638.aspx), en la biblioteca de MSDN.

## <a name="planning-a-distributable-windows-runtime-component"></a>Planear un componente de Windows Runtime distribuible

Elige nombres únicos para archivos binarios, como tus archivos .winmd. Te recomendamos el formato siguiente para garantizar su singularidad:

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

Tus archivos binarios se instalarán en los paquetes de aplicación, posiblemente con archivos binarios de otros desarrolladores. Consulta el apartado "SDK de extensión" en [Cómo crear un kit de desarrollo de software](https://msdn.microsoft.com/library/hh768146.aspx), en la biblioteca de MSDN.

Para decidir cómo distribuir tu componente, considera su complejidad. Se recomienda un SDK de extensión o administrador de paquetes similar cuando:

-   Tu componente conste de varios archivos.
-   Proporciones versiones de tu componente para varias plataformas (x 86 y ARM, por ejemplo).
-   Proporciones tanto la depuración como las versiones de lanzamiento de tu componente.
-   Tu componente tenga archivos y ensamblados que se usen solo en el tiempo de diseño.

Un SDK de extensión es especialmente útil si se cumple más de una de las anteriores.

> **Nota**para los componentes complejos, el sistema de administración de paquetes de NuGet ofrece una alternativa de código abierto para el SDK de extensión. Como el SDK de extensión, NuGet te permite crear paquetes que simplifican la instalación de componentes complejos. Para ver una comparación de paquetes NuGet y SDK de extensión de Visual Studio, consulta [Agregar referencias con el uso de NuGet frente a un SDK de extensión](https://msdn.microsoft.com/library/jj161096.aspx) en la biblioteca de MSDN.

## <a name="distribution-by-file-copy"></a>Distribución por copia de archivos

Si tu componente consta de un archivo .winmd único, o un archivo .winmd y un archivo de índice de recursos (.pri), simplemente puedes hacer que el archivo .winmd esté disponible para que los usuarios lo copien. Los usuarios pueden colocar el archivo donde quieran en un proyecto, usar el cuadro de diálogo **Agregar elemento existente** para agregar el archivo .winmd al proyecto y, a continuación, usar el cuadro de diálogo Administrador de referencias para crear una referencia. Si incluyes un archivo .pri o un archivo .xml, indica a los usuarios que coloquen estos archivos con el archivo .winmd.

> **Nota**Visual Studio genera siempre un archivo .pri al compilar el componente de Windows Runtime, incluso si el proyecto no incluya ningún recurso. Si tienes una aplicación de prueba para tu componente, puedes determinar si se usa el archivo .pri examinando el contenido del paquete de la aplicación en la carpeta bin\\debug\\AppX. De todos modos, si el archivo .pri de tu componente no aparece ahí, no es necesario que lo distribuyas. Como alternativa, puedes usar la herramienta [MakePRI.exe](https://msdn.microsoft.com/library/windows/apps/jj552945.aspx) para volcar el archivo de recursos de tu proyecto de componente de Windows Runtime. Por ejemplo, en la ventana del símbolo del sistema de Visual Studio, escribe: makepri dump /if MyComponent.pri /of MyComponent.pri.xml. Si lo deseas, puedes obtener más información acerca de los archivos .pri en [Resource Management System (Windows) (Sistema de administración de recursos para Windows)](https://msdn.microsoft.com/library/windows/apps/jj552947.aspx).

## <a name="distribution-by-extension-sdk"></a>Distribución por SDK de extensión

Normalmente, un componente complejo incluye los recursos de Windows, pero debes consultar el aviso sobre la detección de archivos .pri vacíos en el apartado anterior.

**Para crear un SDK de extensión**

1.  Asegúrate de tener instalado el SDK de Visual Studio. Puedes descargar el SDK de Visual Studio desde la página [Descargas de Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs).
2.  Crear un nuevo proyecto usando la plantilla VSIX Project. Puedes encontrar la plantilla en Visual C# o Visual Basic, en la categoría de extensibilidad. Esta plantilla se instala como parte del SDK de Visual Studio. ([Tutorial: crear un SDK con C# o Visual Basic](https://msdn.microsoft.com/library/jj127119.aspx) o [Tutorial: crear un SDK con C++](https://msdn.microsoft.com/library/jj127117.aspx), demuestra el uso de esta plantilla en un escenario muy simple. )
3.  Determinar la estructura de carpetas para tu SDK. La estructura de carpetas comienza en el nivel de raíz de tu proyecto VSIX, con las carpetas **Referencias**, **Redist**, y **DesignTime**.

    -   **Referencias** es la ubicación para los archivos binarios que pueden programar tus usuarios. El SDK de extensión crea referencias a estos archivos en los proyectos de Visual Studio de tus usuarios.
    -   **Redist** es la ubicación de otros archivos que deben distribuirse con tus archivos binarios, en las aplicaciones que se crean mediante tu componente.
    -   **DesignTime** es la ubicación de los archivos que se usan solo cuando los desarrolladores crean aplicaciones que usan tu componente.

    En cada una de estas carpetas, puedes crear carpetas de configuración. Los nombres permitidos son debug, retail y CommonConfiguration. La carpeta CommonConfiguration es para los archivos que son los mismos independientemente de si son utilizados por compilaciones retail o debug. Si solo distribuyes compilaciones retail de tu componente, puedes poner todo el contenido en CommonConfiguration y omitir las otros dos carpetas.

    En cada carpeta de configuración, puedes proporcionar carpetas de arquitectura para archivos específicos de plataforma. Si usas los mismos archivos para todas las plataformas, puedes proporcionar una sola carpeta denominada neutral. Puedes encontrar detalles de la estructura de carpetas, incluidos otros nombres de carpetas de arquitectura en [Cómo: crear un kit de desarrollo de software](https://msdn.microsoft.com/library/hh768146.aspx), en la biblioteca de MSDN. (En dicho artículo se explican tanto los SDK de plataforma como los SDK de extensión. Es posible que le resulte útil para contraer el apartado sobre SDK de plataforma, para evitar confusiones. )

4.  Crear un archivo de manifiesto de SDK. En el manifiesto se especifica el nombre y la información de la versión, las arquitecturas compatibles con tu SDK, las versiones de .NET Framework y otra información sobre la forma en que Visual Studio usa tu SDK. Puedes encontrar información detallada y un ejemplo en [Cómo: crear un kit de desarrollo de software](https://msdn.microsoft.com/library/hh768146.aspx).
5.  Compilar y distribuir el SDK de extensión. Para obtener información detallada, incluidas la localización y la firma del paquete VSIX, consulta "VSIX Deployment" en la biblioteca de MSDN.

## <a name="related-topics"></a>Temas relacionados

* [Crear un kit de desarrollo de software](https://msdn.microsoft.com/library/hh768146.aspx)
* [Sistema de administración de paquetes NuGet](https://github.com/NuGet/Home)
* [Sistema de administración de recursos (Windows)](https://msdn.microsoft.com/library/windows/apps/jj552947.aspx)
* [Buscar y usar extensiones de Visual Studio](https://msdn.microsoft.com/library/dd293638.aspx)
* [Opciones de comandos de MakePRI.exe](https://msdn.microsoft.com/library/windows/apps/jj552945.aspx)
