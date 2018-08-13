---
author: laurenhughes
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: Paquetes opcionales y creación de conjuntos relacionados
description: Los paquetes opcionales tienen contenido que se puede integrar con un paquete principal. Estos son útiles para el contenido descargable (DLC), para dividir una aplicación grande que tenga restricciones de tamaño o para enviar cualquier contenido adicional aparte de la aplicación original.
ms.author: lahugh
ms.date: 04/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, paquetes opcionales, conjunto relacionado, extensión de paquete, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: d66a511211396190393e31bfd553149a1e89fad0
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "406084"
---
# <a name="optional-packages-and-related-set-authoring"></a>Paquetes opcionales y creación de conjuntos relacionados
Los paquetes opcionales tienen contenido que se puede integrar con un paquete principal. Estos son útiles para el contenido descargable (DLC), dividir una aplicación de gran tamaño para las restricciones de tamaño, o para cualquier contenido adicional de envío independientes desde su aplicación original.

Conjuntos relacionados son una extensión de paquetes opcionales: le permiten aplicar un conjunto estricto de versiones en paquetes principales y opcionales. También le permiten cargar código nativo (C++) de paquetes opcionales. 

## <a name="prerequisites"></a>Requisitos previos

- Visual Studio 2017, versión 15.1
- Windows 10, versión 1703
- 10 de Windows, versión 1703 SDK

Para obtener todas las herramientas de desarrollo más recientes, vea [descargas y herramientas para Windows 10](https://developer.microsoft.com/windows/downloads).

> [!NOTE]
> Para enviar una aplicación que usa paquetes opcionales o conjuntos relacionados para el Microsoft Store, necesitará permiso. Aun así, puedes usar los paquetes opcionales y los conjuntos relacionados en aplicaciones de línea de negocio (LOB) o de empresa sin el permiso del Centro de desarrollo, si no los vas a enviar a la Store. Consulta [Soporte técnico de desarrolladores de Windows](https://developer.microsoft.com/windows/support) para obtener el permiso necesario para enviar una aplicación que usa paquetes opcionales y conjuntos relacionados.

### <a name="code-sample"></a>Ejemplo de código
Mientras está leyendo este artículo, se recomienda que siga junto con el [ejemplo de código de paquete opcional](https://github.com/AppInstaller/OptionalPackageSample) de depósito para tener un conocimiento práctico de paquetes de modo opcionales y relacionadas con los conjuntos de trabajo dentro de Visual Studio.

## <a name="optional-packages"></a>Paquetes opcionales
Para crear un paquete opcional en Visual Studio, necesitará:
1. Asegúrese de que su aplicación **Versión mínima de plataforma de destino** se establece en: 10.0.15063.0.
2. Desde su proyecto **paquete principal** , abra el `Package.appxmanifest` archivo. Vaya a la ficha "Empaquetado" y tome nota de su **nombre de la familia de paquete**, que es todo lo que precede el carácter "_".
3. Desde su proyecto **paquete opcional** , haga clic en el `Package.appxmanifest` y seleccione **Abrir con > Editor XML (texto)**.
4. Busque la `<Dependencies>` elemento en el archivo. Agregue lo siguiente:

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

Reemplace `[MainPackageDependency]` con su **nombre de la familia de paquete** del paso 2. Esto permitirá especificar que el **paquete opcional** depende de su **paquete principal**.

Una vez que tenga las dependencias de paquete configurar de los pasos 1 a 4, puede continuar desarrollando como lo haría normalmente. Si desea cargar el código desde el paquete opcional en el paquete principal, debe crear un conjunto relacionado. Vea la sección [establece relacionado](#related_sets) para obtener más detalles.

Visual Studio puede configurarse para volver a implementar el paquete principal cada vez que implementa un paquete opcional. Para establecer la dependencia de generación en Visual Studio, deberá:

- Haga clic con el botón secundario del mouse en el proyecto de paquete opcional y seleccione **crear dependencias > dependencias del proyecto...**
- Proteger el proyecto de paquete principal y seleccione "OK". 

Ahora, cada vez que escriba F5 o crea un proyecto en el paquete opcional, Visual Studio generará el paquete principal proyecto en primer lugar. Esto le permitirá garantizar que el proyecto principal y proyectos opcionales están sincronizados.

## Conjuntos relacionados<a name="related_sets"></a>

Si desea cargar el código desde un paquete opcional en el paquete principal, debe crear un conjunto relacionado. Para crear un conjunto relacionado, su paquete principal y el paquete opcional deben estar estrechamente acoplados. Los metadatos para conjuntos relacionados se especifican en el `.appxbundle` archivo del paquete principal. Visual Studio le ayuda a obtener los metadatos correctos en los archivos. Para configurar la solución de su aplicación para conjuntos relacionados, siga estos pasos:

1. Haga clic con el botón secundario del mouse en el proyecto de paquete principal, seleccione **Agregar > nuevo elemento...**
2. Desde la ventana Buscar las plantillas instaladas de ".txt" y agregue un nuevo archivo de texto.
> [!IMPORTANT]
> El nuevo archivo de texto debe ser el nombre: `Bundle.Mapping.txt`.
3. En el `Bundle.Mapping.txt` archivo especifique las rutas de acceso relativas a los proyectos de paquete opcional o paquetes externos. Un ejemplo de `Bundle.Mapping.txt` archivo debe tener un aspecto similar al siguiente:

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

Cuando la solución está configurada de este modo, Visual Studio creará un manifiesto de paquete para el paquete principal con todos los metadatos necesarios para conjuntos relacionados. 

Tenga en cuenta que, como paquetes de opcionales, una `Bundle.Mapping.txt` archivo para conjuntos relacionados sólo funcionará en Windows 10, versión 1703. Además, la versión de Min de plataforma de destino de la aplicación debe establecerse en 10.0.15063.0.

## Problemas conocidos<a name="known_issues"></a>

Actualmente no se admite la depuración de un proyecto opcional conjunto relacionado en Visual Studio. Para solucionar este problema, puede implementar e iniciar la activación (Ctrl + F5) y adjuntar manualmente el depurador a un proceso. Para adjuntar al depurador, vaya el menú "Debug" en Visual Studio, seleccione "Adjunta al proceso..." y adjuntar al depurador al **proceso de la aplicación principal**.