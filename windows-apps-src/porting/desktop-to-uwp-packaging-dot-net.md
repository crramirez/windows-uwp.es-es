---
author: normesta
Description: This guide explains how to configure your Visual Studio Solution to edit, debug, and package desktop app for the Desktop Bridge.
Search.Product: eADQiWindows 10XVcnh
title: Empaquetar una aplicación mediante Visual Studio (Puente de dispositivo de escritorio)
ms.author: normesta
ms.date: 08/30/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: d7ae77c499cb8398aa5557f0d422899fbe8b252d
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2018
ms.locfileid: "1816260"
---
# <a name="package-an-app-by-using-visual-studio-desktop-bridge"></a>Empaquetar una aplicación mediante Visual Studio (Puente de dispositivo de escritorio a UWP)

Puedes usar Visual Studio para generar un paquete para tu aplicación de escritorio. Luego, puedes publicar que ese paquete en Microsoft Store o transferirlo localmente a uno o más equipos.

La versión más reciente de Visual Studio proporciona una nueva versión del proyecto de empaquetado que elimina todos los pasos manuales se solían ser necesarios para empaquetar tu aplicación. Tan solo tienes que agregar un proyecto de empaquetado, hacer referencia al proyecto de escritorio y luego presionar F5 para depurar la aplicación. No es necesario realizar ajustes manuales. Esta nueva experiencia optimizada es una gran mejora de la experiencia que estaba disponible en la versión anterior de Visual Studio.

>[!IMPORTANT]
>El Puente de dispositivo de escritorio se introdujo en Windows 10, versión 1607, y solo se puede usar en proyectos destinados a la Actualización de aniversario de Windows 10 (10.0, compilación 14393) o una versión posterior de Visual Studio.

## <a name="first-consider-how-youll-distribute-your-app"></a>En primer lugar, piensa en la manera de distribuir la aplicación.

Si vas a publicar la aplicación en la [Microsoft Store](https://www.microsoft.com/store/apps), comienza rellenando [este formulario](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge). Microsoft se pondrá en contacto contigo para iniciar el proceso de incorporación. Como parte de este proceso, podrás reservar un nombre en la Store y obtener la información que necesitas para empaquetar la aplicación.

Además, asegúrate de consultar esta guía antes de empezar a crear un paquete para tu aplicación: [Preparar para empaquetar una aplicación (Puente de dispositivo de escritorio)](desktop-to-uwp-prepare.md).

<a id="new-packaging-project"/>

## <a name="create-a-package"></a>Crear un paquete

1. En Visual Studio, abre la solución que contiene tu proyecto de aplicación de escritorio.

2. Agrega un **Proyecto de paquete de aplicación de Windows** a la solución.

   No tendrás que agregarle ningún código. Solo está ahí para generar un paquete para ti. Nos referiremos a este proyecto como el "proyecto de empaquetado".

   ![Proyecto de empaquetado](images/desktop-to-uwp/packaging-project.png)

   >[!NOTE]
   >Este proyecto aparece solo en Visual Studio 2017 versión 15.5 o posterior.

3. Establece la **versión de destino** de este proyecto a cualquier versión que quieras, pero asegúrate de establecer la **Versión mínima** a **Actualización de aniversario de Windows 10**.

   ![Cuadro de diálogo del selector de versión de empaquetado](images/desktop-to-uwp/packaging-version.png)

4. En el proyecto de empaquetado, haz clic con el botón derecho en la carpeta **Applications** y, a continuación, elige **Agregar referencia**.

   ![Agregar referencia de proyecto](images/desktop-to-uwp/add-project-reference.png)

5. Elija el proyecto de aplicación de escritorio y después selecciona el botón **Aceptar**.

   ![Proyecto de escritorio](images/desktop-to-uwp/reference-project.png)

   Puedes incluir varias aplicaciones de escritorio en el paquete, pero solo una de ellas puede iniciarse cuando los usuarios elijan el icono de la aplicación. En el nodo **Applications**, haz clic con el botón derecho en la aplicación que quieres que los usuarios inicien cuando elijas el icono de la aplicación y después elige **Establecer como punto de entrada**.

   ![Establecer punto de entrada](images/desktop-to-uwp/entry-point-set.png)

6. Compila el proyecto de empaquetado para garantizar que no aparece ningún error.

7. Usa el asistente [Crear paquetes de aplicaciones](../packaging/packaging-uwp-apps.md) para generar un archivo appxupload.

   Puedes cargar dicho archivo directamente a la Store.

**Vídeo**

<iframe src="https://www.youtube.com/embed/fJkbYPyd08w" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="next-steps"></a>Pasos siguientes

**Encuentra respuestas a tus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Enviar comentarios o realizar sugerencias acerca de las características**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Ejecutar, depurar o probar la aplicación**

Consulta [Ejecutar, depurar y probar una aplicación de escritorio empaquetada (Puente de dispositivo de escritorio)](desktop-to-uwp-debug.md)

**Mejorar tu aplicación de escritorio agregando las API de UWP**

Consulta [Mejorar tu aplicación de escritorio para Windows 10](desktop-to-uwp-enhance.md).

**Amplía tu aplicación de escritorio agregando proyectos UWP y Componentes de Windows Runtime**

Consulta [Ampliar tu aplicación de escritorio con componentes de UWP modernos](desktop-to-uwp-extend.md).

**Distribuir la aplicación**

Consulta [Distribuir una aplicación de escritorio empaquetada (Puente de dispositivo de escritorio)](desktop-to-uwp-distribute.md)
