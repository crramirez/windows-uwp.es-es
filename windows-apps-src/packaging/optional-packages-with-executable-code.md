---
author: laurenhughes
title: Paquetes opcionales con código ejecutable
description: Aprende a usar Visual Studio para crear un paquete opcional con código ejecutable.
ms.author: lahugh
ms.date: 9/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, instalador de aplicación, AppInstaller, instalación de prueba, conjunto relacionado, paquetes opcionales
ms.localizationpriority: medium
ms.openlocfilehash: f5660649b6f82135cdb45a8678a3f871a0f5e61d
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "5482347"
---
# <a name="optional-packages-with-executable-code"></a>Paquetes opcionales con código ejecutable
 
Los paquetes opcionales con código ejecutable son útiles para dividir una aplicación grande o compleja, o para agregar a una aplicación que ya se haya publicado. Con Visual Studio de 2017, versión 15.7 y.NET Native 2.1, puedes cargar código ejecutable de paquetes opcionales tanto de C++ como de C#.

## <a name="prerequisites"></a>Requisitos previos
- Visual Studio 2017, versión 15.7
- Windows 10, versión 1709
- Windows 10, SDK versión 1709

Para obtener las herramientas de desarrollo más recientes, consulta [Descargas y herramientas para Windows 10](https://developer.microsoft.com/windows/downloads). 

> [!NOTE]
> Para enviar una aplicación que usa paquetes opcionales o conjuntos relacionados a la Store, necesitarás tener permiso. Aun así, puedes usar los paquetes opcionales y los conjuntos relacionados en aplicaciones de línea de negocio (LOB) o de empresa sin el permiso del Centro de desarrollo, si no los vas a enviar a la Store. Consulta [Soporte técnico de desarrolladores de Windows](https://developer.microsoft.com/windows/support) para obtener el permiso necesario para enviar una aplicación que usa paquetes opcionales y conjuntos relacionados.

## <a name="c-optional-packages-with-executable-code"></a>Paquetes opcionales de C++ con código ejecutable

Para cargar código de un paquete opcional de C++, consulta el repositorio [OptionalPackageSample](https://github.com/AppInstaller/OptionalPackageSample) de GitHub. El [OptionalPackageDLL](https://github.com/AppInstaller/OptionalPackageSample/tree/master/OptionalPackageDLL) muestra cómo crear un proyecto con código que se puede ejecutar desde el paquete principal. El proyecto MyMainApp demuestra cómo [cargar código](https://github.com/AppInstaller/OptionalPackageSample/blob/bf6b4915ff1f3b8abfdaacb1ad9e77184c49fe18/MyMainApp/MainPage.xaml.cpp#L182) desde el archivo OptionalPackageDLL.dll.

## <a name="c-optional-packages-with-executable-code"></a>Paquetes opcionales de C# con código ejecutable

Para comenzar a crear un paquete de código opcional en C#, sigue los pasos siguientes para configurar la solución:

1. Crea una nueva aplicación para UWP con la versión mínima que establecida en el SDK de Windows 10 Fall Creators Update (compilación 16299) o superior.

2. Agrega un proyecto de **Paquete de código opcional (Windows universal)** nuevo a la solución. Asegúrate de que la **Versión mínima** y la **Versión de destino** coinciden con las de la aplicación principal.

3. Si deseas enviar tus aplicaciones a la Store, haz clic con el botón derecho en ambos proyectos y selecciona **Store-> Asociar aplicación con la Tienda...**

4. Abre el archivo `Package.appxmanifest` de la aplicación principal y busca el valor de `Identity Name`. Anota este valor para el siguiente paso.

5. Abre el archivo `Package.appxmanifest` del paquete de aplicación opcional y busca el valor de `uap3:MainAppPackageDependency Name`. Actualiza el valor de `uap3:MainAppPackageDependency Name` para que coincida con el valor de `Identity Name` del paquete de la aplicación principal del paso anterior. 

    Este es un ejemplo de la `Identity` del `Package.appxmanifest` de la aplicación principal.
    ```XML
    <Identity Name="12345.MainAppProject" Publisher="CN=PublisherName" Version="1.0.0.0" />
    ```

    `uap3:MainPackageDependency` del paquete de la aplicación opcional debe actualizarse para que coincida con la aplicación principal `Identity`.
    ```XML
    <uap3:MainPackageDependency Name="12345.MainAppProjectTest" />
    ```

6. Agrega un archivo `Bundle.mapping.txt` a la aplicación principal. Sigue los pasos de esta sección [Conjuntos relacionados](https://docs.microsoft.com/windows/uwp/packaging/optional-packages#related-sets) para crear un conjunto relacionado que contenga ambas aplicaciones. 

7. Compila el proyecto de paquete opcional y, a continuación, navega hasta la carpeta de referencia del paquete en la salida de la compilación en `..\[PathToOptionalPackageProject]\bin\[architecture]\[configuration]\Reference`. Ten en cuenta que puedes elegir cualquier arquitectura en la ruta de acceso a la carpeta de la carpeta Referencia puesto que el archivo `.winmd` (paso 8) es independiente de la arquitectura.

8. Agrega una referencia desde el proyecto de la aplicación principal al archivo `.winmd` que se encuentra en esta carpeta. Cada vez que cambie el área de superficie de la API en el proyecto del paquete opcional, este archivo `.winmd` **debe** actualizarse. Esta referencia proporciona al proyecto de la aplicación principal la información necesaria para compilar.

9. En el proyecto de aplicación principal, vea a las propiedades de compilación del proyecto y selecciona **Compilar con cadena de herramientas nativa de .NET**. Actualmente, solo se admite la depuración en .NET Native para la creación de paquetes de código opcional en C#. Ve a las propiedades de depuración del proyecto y selecciona **Implementar paquetes opcionales**. Esto garantizará que ambos paquetes estén sincronizados cuando implementes el proyecto de aplicación principal.

Una vez que hayas terminado con estos pasos, puedes agregar código al proyecto de paquete opcional como si fuera un proyecto de componente de WinRT administrado. Para tener acceso al código d el proyecto de aplicación principal, llama a los métodos públicos que se exponen en el proyecto de paquete opcional.