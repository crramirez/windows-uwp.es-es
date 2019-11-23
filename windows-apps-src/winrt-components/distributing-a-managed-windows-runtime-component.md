---
title: Distribuir un componente de Windows Runtime administrado
description: Puedes distribuir tu componente de Windows Runtime mediante la copia de archivos.
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1d306c02d4fd99acaa49ec59230181ac0a20c9f3
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690385"
---
# <a name="distributing-a-managed-windows-runtime-component"></a>Distribuir un componente de Windows Runtime administrado

Puedes distribuir tu componente de Windows Runtime mediante la copia de archivos. Sin embargo, si el componente consta de muchos archivos, la instalación puede ser tediosa para los usuarios. Además, los errores en la colocación de archivos o la incapacidad para establecer referencias pueden causarles problemas. Puedes empaquetar un componente complejo como un SDK de extensión de Visual Studio, para que sea fácil de instalar y usar. Los usuarios solamente necesitan establecer una referencia para todo el paquete. Pueden localizar e instalar fácilmente el componente mediante el cuadro de diálogo **extensiones y actualizaciones** , como se describe en [búsqueda y uso de extensiones de Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015).

## <a name="planning-a-distributable-windows-runtime-component"></a>Planear un componente de Windows Runtime distribuible

Elige nombres únicos para archivos binarios, como tus archivos .winmd. Te recomendamos el formato siguiente para garantizar su singularidad:

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

Tus archivos binarios se instalarán en los paquetes de aplicación, posiblemente con archivos binarios de otros desarrolladores. Vea "SDK de extensión" en [Cómo: crear un kit de desarrollo de software](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015).

Para decidir cómo distribuir tu componente, considera su complejidad. Se recomienda un SDK de extensión o administrador de paquetes similar cuando:

-   Tu componente conste de varios archivos.
-   Proporciones versiones de tu componente para varias plataformas (x 86 y ARM, por ejemplo).
-   Proporciones tanto la depuración como las versiones de lanzamiento de tu componente.
-   Tu componente tenga archivos y ensamblados que se usen solo en el tiempo de diseño.

Un SDK de extensión es especialmente útil si se cumple más de una de las anteriores.

> **Nota**  para componentes complejos, el sistema de administración de paquetes de NuGet ofrece una alternativa de código abierto a los SDK de extensión. Como el SDK de extensión, NuGet te permite crear paquetes que simplifican la instalación de componentes complejos. Para obtener una comparación de los paquetes NuGet y los SDK de extensión de Visual Studio, vea [Agregar referencias mediante NuGet en lugar de un SDK de extensión](https://docs.microsoft.com/visualstudio/ide/adding-references-using-nuget-versus-an-extension-sdk?view=vs-2015).

## <a name="distribution-by-file-copy"></a>Distribución por copia de archivos

Si tu componente consta de un archivo .winmd único, o un archivo .winmd y un archivo de índice de recursos (.pri), simplemente puedes hacer que el archivo .winmd esté disponible para que los usuarios lo copien. Los usuarios pueden colocar el archivo donde quieran en un proyecto, usar el cuadro de diálogo **Agregar elemento existente** para agregar el archivo .winmd al proyecto y, a continuación, usar el cuadro de diálogo Administrador de referencias para crear una referencia. Si incluyes un archivo .pri o un archivo .xml, indica a los usuarios que coloquen estos archivos con el archivo .winmd.

> **Tenga en cuenta**  Visual Studio siempre genera un archivo. PRI al compilar el componente de Windows Runtime, incluso si el proyecto no incluye ningún recurso. Si tiene una aplicación de prueba para el componente, puede determinar si se usa el archivo. PRI examinando el contenido del paquete de la aplicación en la carpeta bin\\Debug\\AppX. De todos modos, si el archivo .pri de tu componente no aparece ahí, no es necesario que lo distribuyas. Como alternativa, puedes usar la herramienta [MakePRI.exe](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10)) para volcar el archivo de recursos de tu proyecto de componente de Windows Runtime. Por ejemplo, en la ventana del símbolo del sistema de Visual Studio, escribe: makepri dump /if MyComponent.pri /of MyComponent.pri.xml. Si lo deseas, puedes obtener más información acerca de los archivos .pri en [Resource Management System (Windows) (Sistema de administración de recursos para Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)).

## <a name="distribution-by-extension-sdk"></a>Distribución por SDK de extensión

Normalmente, un componente complejo incluye los recursos de Windows, pero debes consultar el aviso sobre la detección de archivos .pri vacíos en el apartado anterior.

**Para crear un SDK de extensión**

1.  Asegúrate de tener instalado el SDK de Visual Studio. Puedes descargar el SDK de Visual Studio desde la página [Descargas de Visual Studio](https://visualstudio.microsoft.com/downloads/download-visual-studio-vs).
2.  Crear un nuevo proyecto usando la plantilla VSIX Project. Puedes encontrar la plantilla en Visual C# o Visual Basic, en la categoría de extensibilidad. Esta plantilla se instala como parte del SDK de Visual Studio. ([Tutorial: crear un SDK con C# o Visual Basic](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-csharp-or-visual-basic?view=vs-2015) o [Tutorial: crear un SDK con C++](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-cpp?view=vs-2015), demuestra el uso de esta plantilla en un escenario muy simple. ) simple
3.  Determinar la estructura de carpetas para tu SDK. La estructura de carpetas comienza en el nivel de raíz de tu proyecto VSIX, con las carpetas **Referencias**, **Redist**, y **DesignTime**.

    -   En la carpeta **References** se ubican los archivos binarios que pueden programar los usuarios. El SDK de extensión crea referencias a estos archivos en los proyectos de Visual Studio de tus usuarios.
    -   **Redist** es la ubicación de otros archivos que deben distribuirse con tus archivos binarios, en las aplicaciones que se crean mediante tu componente.
    -   **DesignTime** es la ubicación de los archivos que se usan solo cuando los desarrolladores crean aplicaciones que usan tu componente.

    En cada una de estas carpetas, puedes crear carpetas de configuración. Los nombres permitidos son debug, retail y CommonConfiguration. La carpeta CommonConfiguration es para los archivos que son los mismos independientemente de si son utilizados por compilaciones retail o debug. Si solo distribuyes compilaciones retail de tu componente, puedes poner todo el contenido en CommonConfiguration y omitir las otros dos carpetas.

    En cada carpeta de configuración, puedes proporcionar carpetas de arquitectura para archivos específicos de plataforma. Si usas los mismos archivos para todas las plataformas, puedes proporcionar una sola carpeta denominada neutral. Puede encontrar detalles de la estructura de carpetas, incluidos otros nombres de carpetas de arquitectura, en [Cómo: crear un kit de desarrollo de software](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015). (En dicho artículo se explican tanto los SDK de plataforma como los SDK de extensión. Es posible que le resulte útil para contraer el apartado sobre SDK de plataforma, para evitar confusiones. ) simple

4.  Crear un archivo de manifiesto de SDK. El manifiesto especifica el nombre y la información de versión, las arquitecturas que admite el SDK, las versiones de .NET y otra información sobre la manera en que Visual Studio usa el SDK. Puedes encontrar información detallada y un ejemplo en [Cómo: crear un kit de desarrollo de software](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015).
5.  Compilar y distribuir el SDK de extensión. Para obtener información detallada, incluida la localización y la firma del paquete VSIX, vea [implementación de VSIX](https://docs.microsoft.com/visualstudio/misc/how-to-manually-package-an-extension-vsix-deployment?view=vs-2015).

## <a name="related-topics"></a>Temas relacionados

* [Creación de un kit de desarrollo de software](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)
* [sistema de administración de paquetes NuGet](https://github.com/NuGet/Home)
* [Sistema de administración de recursos (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))
* [Buscar y usar extensiones de Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)
* [Opciones del comando MakePRI. exe](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10))
