---
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: Creación de paquetes opcionales y de conjuntos relacionados
description: Los paquetes opcionales tienen contenido que se puede integrar con un paquete principal. Estos son útiles para el contenido descargable (DLC), para dividir una aplicación grande que tenga restricciones de tamaño o para enviar cualquier contenido adicional aparte de la aplicación original.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp, paquetes opcionales, conjunto relacionado, extensión de paquete, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: f62d6c99acc75033403fac7a498308cea6f7d3f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594030"
---
# <a name="optional-packages-and-related-set-authoring"></a>Creación de paquetes opcionales y de conjuntos relacionados
Los paquetes opcionales tienen contenido que se puede integrar con un paquete principal. Estos son útiles para el contenido descargable (DLC), para dividir una aplicación grande que tenga restricciones de tamaño o para enviar cualquier contenido adicional aparte de la aplicación original.

Los conjuntos relacionados son una extensión de los paquetes opcionales: te permiten aplicar un conjunto estricto de versiones tanto en los paquetes principales como en los opcionales. Asimismo, también te permiten cargar código nativo (C++) de paquetes opcionales. 

## <a name="prerequisites"></a>Requisitos previos

- Visual Studio 2017, versión 15.1
- Windows 10, versión 1703
- Windows 10, SDK versión 1703

Para obtener las herramientas de desarrollo más recientes, consulta [Descargas y herramientas para Windows 10](https://developer.microsoft.com/windows/downloads).

> [!NOTE]
> Para enviar una aplicación que utiliza paquetes opcionales o conjuntos relacionados a la Microsoft Store, necesita permiso. Paquetes opcionales y los conjuntos relacionados pueden usarse para aplicaciones de línea de negocio (LOB) o enterprise sin permiso del centro de partners si no se enviarán a la Store. Consulta [Soporte técnico de desarrolladores de Windows](https://developer.microsoft.com/windows/support) para obtener el permiso necesario para enviar una aplicación que usa paquetes opcionales y conjuntos relacionados.

### <a name="code-sample"></a>Ejemplo de código
Además de este artículo, te recomendamos que eches un vistazo a la [muestra de código del paquete opcional](https://github.com/AppInstaller/OptionalPackageSample) en GitHub para obtener un ejemplo práctico del funcionamiento de los paquetes opcionales y los conjuntos relacionados en Visual Studio.

## <a name="optional-packages"></a>Paquetes opcionales
Para crear un paquete opcional en Visual Studio, deberás realizar los siguientes pasos:
1. Asegúrese de que la aplicación **versión mínima de plataforma de destino** está establecido en: 10.0.15063.0 o superior.
2. En el proyecto del **paquete principal**, abre el archivo `Package.appxmanifest`. Ve a la pestaña "Empaquetado" y apunta el **nombre de familia de paquete**, que es todo el contenido que hay antes del carácter "_".
3. En el proyecto del **paquete opcional**, haz clic con el botón derecho en `Package.appxmanifest` y selecciona **Abrir con > Editor XML (texto)**.
4. Busca el elemento `<Dependencies>` en el archivo. Agrega lo siguiente:

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

Reemplaza `[MainPackageDependency]` con el **nombre de familia de paquete** del paso 2. Esto permitirá especificar que el **paquete opcional** depende del **paquete principal**.

Cuando configures las dependencias de los paquetes del paso 1 al 4, podrás seguir desarrollando con normalidad. Si quieres cargar el código desde el paquete opcional en el paquete principal, deberás crear un conjunto relacionado. Consulta la sección [Conjuntos relacionados](#related_sets) para obtener más información.

Puedes configurar Visual Studio para que vuelva a implementar el paquete principal cada vez que implementes un paquete opcional. Para establecer la dependencia de compilación en Visual Studio, haz lo siguiente:

- Haz clic con el botón derecho en el proyecto del paquete opcional y selecciona **Dependencias de compilación > Dependencias del proyecto...**
- Selecciona el proyecto del paquete principal y "Aceptar". 

Una vez hecho esto, cada vez que pulses F5 o compiles un proyecto de paquete opcional, Visual Studio compilará primero el proyecto del paquete principal. Gracias a ello, te asegurarás de que tanto el proyecto principal como el opcional están sincronizados.

## Conjuntos relacionados<a name="related_sets"></a>

Si quieres cargar el código desde un paquete opcional en el paquete principal, deberás crear un conjunto relacionado. Para crear un conjunto relacionado, los paquetes principal y opcional deben estar estrechamente acoplados. Los metadatos de conjuntos relacionados se especifican en el archivo .appxbundle o .msixbundle del paquete principal. Visual Studio te ayudará a obtener los metadatos correctos en los archivos. Para configurar la solución de la aplicación para conjuntos relacionados, realiza los siguientes pasos:

1. Haz clic con el botón derecho en el proyecto del paquete principal y selecciona **Agregar > Nuevo elemento...**
2. En la ventana que aparecerá, busca ".txt" en las plantillas instaladas y agrega un nuevo archivo de texto.
> [!IMPORTANT]
> El nuevo archivo de texto debe llamarse: `Bundle.Mapping.txt`.

3. En el archivo `Bundle.Mapping.txt` tendrás que especificar las rutas de acceso relativas a los proyectos de paquete opcional o a los paquetes externos. Un archivo `Bundle.Mapping.txt` debería tener este aspecto:

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

Cuando configures la solución de este modo, Visual Studio creará un manifiesto de paquete para el paquete principal, que contendrá todos los metadatos que necesitarán los conjuntos relacionados. 

Tenga en cuenta que, como paquetes opcionales, un `Bundle.Mapping.txt` archivo para conjuntos relacionados solo funcionará en Windows 10, versión 1703 o posterior. Además, la versión de Min de plataforma de destino de la aplicación debe establecerse en 10.0.15063.0 o superior.

## Problemas conocidos<a name="known_issues"></a>

Actualmente no se puede depurar un proyecto opcional de conjunto relacionado en Visual Studio. Para evitar este problema, puedes implementar e iniciar la activación (Ctrl + F5) y asociar manualmente el depurador a un proceso. Para adjuntar el depurador, ve al menú "Depurar" en Visual Studio, selecciona "Adjuntar al proceso..." y adjunta el depurador al **proceso de aplicación principal**.