---
title: Distribuir un componente de Windows Runtime administrado
description: Puede distribuir el componente de Windows Runtime por copia de archivos.
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1d306c02d4fd99acaa49ec59230181ac0a20c9f3
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690385"
---
# <a name="distributing-a-managed-windows-runtime-component"></a>Distribuir un componente de Windows Runtime administrado

Puede distribuir el componente de Windows Runtime por copia de archivos. Sin embargo, si el componente consta de muchos archivos, la instalación puede resultar tediosa para los usuarios. Además, los errores que se produzcan al colocar archivos o al no establecer referencias pueden ocasionar problemas. Puede empaquetar un componente complejo como un SDK de extensión de Visual Studio para facilitar su instalación y uso. Los usuarios solo tienen que establecer una referencia para todo el paquete. Pueden localizar e instalar fácilmente el componente mediante el cuadro de diálogo **extensiones y actualizaciones** , como se describe en [búsqueda y uso de extensiones de Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015).

## <a name="planning-a-distributable-windows-runtime-component"></a>Planear un componente de Windows Runtime distribuible

Elija nombres únicos para los archivos binarios, como los archivos. winmd. Se recomienda el siguiente formato para garantizar la exclusividad:

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

Los archivos binarios se instalarán en los paquetes de aplicaciones, posiblemente con archivos binarios de otros desarrolladores. Vea "SDK de extensión" en [Cómo: crear un kit de desarrollo de software](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015).

Para decidir cómo distribuir el componente, tenga en cuenta el grado de complejidad. Se recomienda un SDK de extensión o un administrador de paquetes similar cuando:

-   El componente consta de varios archivos.
-   Las versiones del componente se proporcionan para varias plataformas (x86 y ARM, por ejemplo).
-   Proporcione las versiones de depuración y de lanzamiento del componente.
-   El componente tiene archivos y ensamblados que solo se usan en tiempo de diseño.

Un SDK de extensión es especialmente útil si se cumplen más de una de las anteriores.

> **Nota**  para componentes complejos, el sistema de administración de paquetes de NuGet ofrece una alternativa de código abierto a los SDK de extensión. Al igual que los SDK de extensión, NuGet le permite crear paquetes que simplifican la instalación de componentes complejos. Para obtener una comparación de los paquetes NuGet y los SDK de extensión de Visual Studio, vea [Agregar referencias mediante NuGet en lugar de un SDK de extensión](https://docs.microsoft.com/visualstudio/ide/adding-references-using-nuget-versus-an-extension-sdk?view=vs-2015).

## <a name="distribution-by-file-copy"></a>Distribución por copia de archivos

Si el componente consta de un archivo. winmd único, o un archivo. winmd y un archivo de índice de recursos (. PRI), simplemente puede hacer que el archivo. winmd esté disponible para que los usuarios lo copien. Los usuarios pueden colocar el archivo donde quiera en un proyecto, usar el cuadro de diálogo **Agregar elemento existente** para agregar el archivo. winmd al proyecto y, a continuación, usar el cuadro de diálogo Administrador de referencias para crear una referencia. Si incluye un archivo. PRI o un archivo. XML, indique a los usuarios que coloquen esos archivos con el archivo. winmd.

> **Tenga en cuenta**  Visual Studio siempre genera un archivo. PRI al compilar el componente de Windows Runtime, incluso si el proyecto no incluye ningún recurso. Si tiene una aplicación de prueba para el componente, puede determinar si se usa el archivo. PRI examinando el contenido del paquete de la aplicación en la carpeta bin\\Debug\\AppX. Si el archivo. PRI del componente no aparece allí, no es necesario distribuirlo. Como alternativa, puede usar la herramienta [MakePRI. exe](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10)) para volcar el archivo de recursos de su proyecto de componente de Windows Runtime. Por ejemplo, en la ventana del símbolo del sistema de Visual Studio, escriba: makepri dump/if mi Component. PRI/of Component. PRI. Xml. puede leer más información sobre los archivos. PRI en el [sistema de administración de recursos (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)).

## <a name="distribution-by-extension-sdk"></a>Distribución por SDK de extensión

Un componente complejo normalmente incluye recursos de Windows, pero vea la nota sobre la detección de archivos. PRI vacíos en la sección anterior.

**Para crear un SDK de extensión**

1.  Asegúrese de que tiene instalado el SDK de Visual Studio. Puede descargar el SDK de Visual Studio desde la página de [descargas de Visual Studio](https://visualstudio.microsoft.com/downloads/download-visual-studio-vs) .
2.  Cree un nuevo proyecto con la plantilla de Proyecto VSIX. Puede encontrar la plantilla en Visual C# o en Visual Basic, en la categoría extensibilidad. Esta plantilla se instala como parte del SDK de Visual Studio. ([Tutorial: crear un SDK mediante C# o Visual Basic](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-csharp-or-visual-basic?view=vs-2015) o [Tutorial: crear un SDK con C++ ](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-cpp?view=vs-2015), muestra el uso de esta plantilla en un escenario muy sencillo. )
3.  Determine la estructura de carpetas para el SDK. La estructura de carpetas comienza en el nivel raíz del Proyecto VSIX, con las carpetas **References**, **Redist**y **DesignTime** .

    -   **Referencias** es la ubicación de los archivos binarios con los que los usuarios pueden programar. El SDK de extensión crea referencias a estos archivos en los proyectos de Visual Studio de los usuarios.
    -   **Redist** es la ubicación de otros archivos que deben distribuirse con los archivos binarios, en las aplicaciones que se crean con el componente.
    -   **DesignTime** es la ubicación de los archivos que se usan solo cuando los desarrolladores están creando aplicaciones que usan el componente.

    En cada una de estas carpetas, puede crear carpetas de configuración. Los nombres permitidos son debug, Retail y CommonConfiguration. La carpeta CommonConfiguration es para los archivos que son iguales independientemente de que los usen las compilaciones de depuración o de venta directa. Si solo va a distribuir las compilaciones comerciales del componente, puede colocar todo en CommonConfiguration y omitir las otras dos carpetas.

    En cada carpeta de configuración, puede proporcionar carpetas de arquitectura para archivos específicos de la plataforma. Si usa los mismos archivos para todas las plataformas, puede proporcionar una sola carpeta denominada neutral. Puede encontrar detalles de la estructura de carpetas, incluidos otros nombres de carpetas de arquitectura, en [Cómo: crear un kit de desarrollo de software](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015). (En este artículo se describen los SDK de la plataforma y los SDK de extensión. Puede que le resulte útil contraer la sección acerca de los SDK de la plataforma para evitar confusiones. )

4.  Cree un archivo de manifiesto del SDK. El manifiesto especifica el nombre y la información de versión, las arquitecturas que admite el SDK, las versiones de .NET y otra información sobre la manera en que Visual Studio usa el SDK. Puede encontrar detalles y un ejemplo en [Cómo: crear un kit de desarrollo de software](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015).
5.  Compile y distribuya el SDK de la extensión. Para obtener información detallada, incluida la localización y la firma del paquete VSIX, vea [implementación de VSIX](https://docs.microsoft.com/visualstudio/misc/how-to-manually-package-an-extension-vsix-deployment?view=vs-2015).

## <a name="related-topics"></a>Temas relacionados

* [Creación de un kit de desarrollo de software](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)
* [sistema de administración de paquetes NuGet](https://github.com/NuGet/Home)
* [Sistema de administración de recursos (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))
* [Buscar y usar extensiones de Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)
* [Opciones del comando MakePRI. exe](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10))
