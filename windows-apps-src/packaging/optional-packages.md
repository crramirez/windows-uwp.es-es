---
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: Paquetes opcionales y creación de conjuntos relacionados
description: Los paquetes opcionales tienen contenido que se puede integrar con un paquete principal. Estos son útiles para el contenido descargable (DLC), para dividir una aplicación grande que tenga restricciones de tamaño o para enviar cualquier contenido adicional aparte de la aplicación original.
ms.date: 09/30/2018
ms.topic: article
keywords: Windows 10, uwp, paquetes opcionales, conjunto relacionado, extensión de paquete, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: e19f9673090501d59e260a698f9968a8f98f1cd5
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7692666"
---
# <a name="optional-packages-and-related-set-authoring"></a>Paquetes opcionales y creación de conjuntos relacionados
Los paquetes opcionales tienen contenido que se puede integrar con un paquete principal. Estos son útiles para el contenido descargable (DLC), dividir una aplicación grande que tenga restricciones de tamaño, o para enviar cualquier contenido adicional aparte de la aplicación original.

Conjuntos relacionados son una extensión de paquetes opcionales: permiten aplicar un conjunto estricto de versiones en los paquetes principales y opcionales. También permiten que cargar código nativo (C++) de los paquetes opcionales. 

## <a name="prerequisites"></a>Requisitos previos

- Visual Studio 2017, versión 15.1
- Windows 10 versión 1703
- Windows 10, versión 1703 SDK

Para obtener todas las herramientas de desarrollo más recientes, consulta [las descargas y herramientas para Windows 10](https://developer.microsoft.com/windows/downloads).

> [!NOTE]
> Para enviar una aplicación que usa paquetes opcionales o conjuntos relacionados en la Microsoft Store, tendrás que permiso. Paquetes opcionales y conjuntos relacionados pueden usarse para las aplicaciones de línea de negocio (LOB) o enterprise sin el permiso del centro de partners si no vas a enviar a la tienda. Consulta [Soporte técnico de desarrolladores de Windows](https://developer.microsoft.com/windows/support) para obtener el permiso necesario para enviar una aplicación que usa paquetes opcionales y conjuntos relacionados.

### <a name="code-sample"></a>Ejemplo de código
Mientras que estás leyendo este artículo, se recomienda que sigue los pasos indicados en el [ejemplo de código de paquete opcional](https://github.com/AppInstaller/OptionalPackageSample) en GitHub para obtener una descripción de los paquetes opcionales cómo práctica y relacionadas con conjuntos de trabajo en Visual Studio.

## <a name="optional-packages"></a>Paquetes opcionales
Para crear un paquete opcional en Visual Studio, tendrás que:
1. Asegúrate de que la **Versión mínima de plataforma de destino** de la aplicación se establece en: 10.0.15063.0.
2. Desde el proyecto de **paquete principal** , abre el `Package.appxmanifest` archivo. Ve a la pestaña "Empaquetado" y toma nota de tu **nombre de familia de paquete**, que es todo antes del carácter "_".
3. Desde el proyecto de **paquete opcional** , haz clic en el `Package.appxmanifest` y seleccione **Abrir con > Editor XML (texto)**.
4. Busca el `<Dependencies>` elemento en el archivo. Agrega lo siguiente:

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

Reemplazar `[MainPackageDependency]` por el **nombre de familia de paquete** del paso 2. Esto permitirá especificar que el **paquete opcional** depende de su **paquete principal**.

Una vez que tengas las dependencias del paquete configurar de los pasos 1 a 4, puedes seguir desarrollando como de costumbre. Si quieres cargar código desde el paquete opcional en el paquete principal, tendrás que crear un conjunto relacionado. Consulta la sección [establece relacionados](#related_sets) para obtener más detalles.

Visual Studio puede configurarse para volver a implementar el paquete principal cada vez que se implementa un paquete opcional. Para establecer la dependencia de compilación en Visual Studio, debes:

- Haz clic en el proyecto de paquete opcional y selecciona **dependencias de compilación > dependencias del proyecto …**
- Comprobar el proyecto de paquete principal y selecciona "Aceptar". 

Ahora, cada vez que se escribe F5 o compilar un proyecto de paquete opcional, Visual Studio se compila el proyecto de paquete principal en primer lugar. Esto garantiza que el proyecto principal y opcionales proyectos están sincronizados.

## Conjuntos relacionados<a name="related_sets"></a>

Si quieres cargar código desde un paquete opcional en el paquete principal, tendrás que crear un conjunto relacionado. Para crear un conjunto relacionado, el paquete principal y un paquete opcional deben estar estrechamente relacionados. Los metadatos de conjuntos relacionados se especifican en el archivo .appxbundle o .msixbundle del paquete principal. Visual Studio te ayuda a obtener los metadatos correcto de los archivos. Para configurar la solución de aplicación para conjuntos relacionados, usa los siguientes pasos:

1. Haz clic en el proyecto de paquete principal, selecciona **Agregar > nuevo elemento …**
2. En la ventana, busca las plantillas instaladas para ".txt" y agrega un nuevo archivo de texto.
> [!IMPORTANT]
> El nuevo archivo de texto debe llamarse: `Bundle.Mapping.txt`.
3. En el `Bundle.Mapping.txt` archivo especificará rutas de acceso relativas a los proyectos de paquete opcional o paquetes externos. Un ejemplo `Bundle.Mapping.txt` archivo debe ser similar al siguiente:

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

Cuando la solución está configurada de esta forma, Visual Studio creará un manifiesto de paquete para el paquete principal con todos los metadatos necesarios para conjuntos relacionados. 

Ten en cuenta que como paquetes opcionales, un `Bundle.Mapping.txt` archivo para conjuntos relacionados solo funcionará en Windows 10, versión 1703. Además, la versión de Min de plataforma de destino de la aplicación debe establecerse en 10.0.15063.0.

## Problemas conocidos<a name="known_issues"></a>

Actualmente no se admite la depuración de un proyecto opcional de conjuntos relacionados en Visual Studio. Para evitar este problema, puedes implementar e iniciar la activación (Ctrl + F5) y asociar manualmente el depurador a un proceso. Para asociar al depurador, ve el menú "Debug" en Visual Studio, selecciona "Asociar al proceso …" y asociar al depurador al **proceso de aplicación principal**.