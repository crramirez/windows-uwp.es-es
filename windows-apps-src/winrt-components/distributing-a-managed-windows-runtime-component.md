---
title: Distribuir un componente de Windows Runtime administrado
description: Puedes distribuir tu componente de Windows Runtime mediante la copia de archivos.
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b4e05a1f24e6192d25c80c043cdb4a51e7ac61ec
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372367"
---
# <a name="distributing-a-managed-windows-runtime-component"></a>Distribuir un componente de Windows Runtime administrado



Puedes distribuir tu componente de Windows Runtime mediante la copia de archivos. Sin embargo, si el componente consta de muchos archivos, la instalación puede ser tediosa para los usuarios. Además, los errores en la colocación de archivos o la incapacidad para establecer referencias pueden causarles problemas. Puedes empaquetar un componente complejo como un SDK de extensión de Visual Studio, para que sea fácil de instalar y usar. Los usuarios solamente necesitan establecer una referencia para todo el paquete. Pueden localizar e instalar tu componente usando el cuadro de diálogo **Extensiones y actualizaciones**, como se describe en [Búsqueda y uso de extensiones de Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015), en la biblioteca de MSDN.

## <a name="planning-a-distributable-windows-runtime-component"></a>Planear un componente de Windows Runtime distribuible

Elige nombres únicos para archivos binarios, como tus archivos .winmd. Te recomendamos el formato siguiente para garantizar su singularidad:

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

Tus archivos binarios se instalarán en los paquetes de aplicación, posiblemente con archivos binarios de otros desarrolladores. Vea "Extension SDK" en [Cómo: Crear un Kit de desarrollo de Software](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015), en MSDN Library.

Para decidir cómo distribuir tu componente, considera su complejidad. Se recomienda un SDK de extensión o administrador de paquetes similar cuando:

-   Tu componente conste de varios archivos.
-   Proporciones versiones de tu componente para varias plataformas (x 86 y ARM, por ejemplo).
-   Proporciones tanto la depuración como las versiones de lanzamiento de tu componente.
-   Tu componente tenga archivos y ensamblados que se usen solo en el tiempo de diseño.

Un SDK de extensión es especialmente útil si se cumple más de una de las anteriores.

> **Tenga en cuenta**  para los componentes complejos, el sistema de administración de paquetes de NuGet ofrece una alternativa de código abierto a los SDK de extensión. Como el SDK de extensión, NuGet te permite crear paquetes que simplifican la instalación de componentes complejos. Para ver una comparación de paquetes NuGet y SDK de extensión de Visual Studio, consulta [Agregar referencias con el uso de NuGet frente a un SDK de extensión](https://docs.microsoft.com/visualstudio/ide/adding-references-using-nuget-versus-an-extension-sdk?view=vs-2015) en la biblioteca de MSDN.

## <a name="distribution-by-file-copy"></a>Distribución por copia de archivos

Si tu componente consta de un archivo .winmd único, o un archivo .winmd y un archivo de índice de recursos (.pri), simplemente puedes hacer que el archivo .winmd esté disponible para que los usuarios lo copien. Los usuarios pueden colocar el archivo donde quieran en un proyecto, usar el cuadro de diálogo **Agregar elemento existente** para agregar el archivo .winmd al proyecto y, a continuación, usar el cuadro de diálogo Administrador de referencias para crear una referencia. Si incluyes un archivo .pri o un archivo .xml, indica a los usuarios que coloquen estos archivos con el archivo .winmd.

> **Tenga en cuenta**  Visual Studio siempre genera un archivo .pri al compilar el componente de Windows en tiempo de ejecución, incluso si el proyecto no incluye todos los recursos. Si tiene una aplicación de prueba para el componente, puede determinar si se utiliza el archivo .pri examinando el contenido del paquete de aplicación en la Papelera de\\depurar\\carpeta AppX. De todos modos, si el archivo .pri de tu componente no aparece ahí, no es necesario que lo distribuyas. Como alternativa, puedes usar la herramienta [MakePRI.exe](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10)) para volcar el archivo de recursos de tu proyecto de componente de Windows Runtime. Por ejemplo, en la ventana del símbolo del sistema de Visual Studio, escribe: makepri dump /if MyComponent.pri /of MyComponent.pri.xml. Si lo deseas, puedes obtener más información acerca de los archivos .pri en [Resource Management System (Windows) (Sistema de administración de recursos para Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)).

## <a name="distribution-by-extension-sdk"></a>Distribución por SDK de extensión

Normalmente, un componente complejo incluye los recursos de Windows, pero debes consultar el aviso sobre la detección de archivos .pri vacíos en el apartado anterior.

**Para crear un SDK de extensión**

1.  Asegúrate de tener instalado el SDK de Visual Studio. Puedes descargar el SDK de Visual Studio desde la página [Descargas de Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs).
2.  Crear un nuevo proyecto usando la plantilla VSIX Project. Puedes encontrar la plantilla en Visual C# o Visual Basic, en la categoría de extensibilidad. Esta plantilla se instala como parte del SDK de Visual Studio. ([Tutorial: Creación de un SDK con C# o Visual Basic](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-csharp-or-visual-basic?view=vs-2015) o [Tutorial: Creación de un SDK con C++](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-cpp?view=vs-2015), se muestra el uso de esta plantilla en un escenario muy sencillo. ) simple
3.  Determinar la estructura de carpetas para tu SDK. La estructura de carpetas comienza en el nivel de raíz de tu proyecto VSIX, con las carpetas **Referencias**, **Redist**, y **DesignTime**.

    -   En la carpeta **References** se ubican los archivos binarios que pueden programar los usuarios. El SDK de extensión crea referencias a estos archivos en los proyectos de Visual Studio de tus usuarios.
    -   **Redist** es la ubicación de otros archivos que deben distribuirse con tus archivos binarios, en las aplicaciones que se crean mediante tu componente.
    -   **DesignTime** es la ubicación de los archivos que se usan solo cuando los desarrolladores crean aplicaciones que usan tu componente.

    En cada una de estas carpetas, puedes crear carpetas de configuración. Los nombres permitidos son debug, retail y CommonConfiguration. La carpeta CommonConfiguration es para los archivos que son los mismos independientemente de si son utilizados por compilaciones retail o debug. Si solo distribuyes compilaciones retail de tu componente, puedes poner todo el contenido en CommonConfiguration y omitir las otros dos carpetas.

    En cada carpeta de configuración, puedes proporcionar carpetas de arquitectura para archivos específicos de plataforma. Si usas los mismos archivos para todas las plataformas, puedes proporcionar una sola carpeta denominada neutral. Puede encontrar los detalles de la estructura de carpetas, incluidos otros nombres de carpeta de arquitectura en [Cómo: Crear un Kit de desarrollo de Software](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015), en MSDN Library. (En dicho artículo se explican tanto los SDK de plataforma como los SDK de extensión. Es posible que le resulte útil para contraer el apartado sobre SDK de plataforma, para evitar confusiones. ) simple

4.  Crear un archivo de manifiesto de SDK. En el manifiesto se especifica el nombre y la información de la versión, las arquitecturas compatibles con tu SDK, las versiones de .NET Framework y otra información sobre la forma en que Visual Studio usa tu SDK. Puede encontrar más detalles y un ejemplo en [Cómo: Crear un Kit de desarrollo de Software](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015).
5.  Compilar y distribuir el SDK de extensión. Para obtener información detallada, incluidas la localización y la firma del paquete VSIX, consulta "VSIX Deployment" en la biblioteca de MSDN.

## <a name="related-topics"></a>Temas relacionados

* [Crear un Kit de desarrollo de Software](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)
* [Sistema de administración de paquetes de NuGet](https://github.com/NuGet/Home)
* [Sistema de administración de recursos (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))
* [Buscar y usar extensiones de Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)
* [Opciones de comando MakePRI.exe](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10))
