---
Description: Esta guía explica cómo configurar la solución de Visual Studio para editar, depurar y empaquetar aplicación de escritorio.
Search.Product: eADQiWindows 10XVcnh
title: Empaquetar una aplicación de escritorio con Visual Studio
ms.date: 08/30/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: 04a16b5e824621b0e7f32c8cb012db326f591d48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655970"
---
# <a name="package-a-desktop-application-by-using-visual-studio"></a>Empaquetar una aplicación de escritorio con Visual Studio

Puedes usar Visual Studio para generar un paquete para tu aplicación de escritorio. A continuación, puede publicar ese paquete a la Microsoft Store o transferirla localmente a uno o varios de los equipos.

La versión más reciente de Visual Studio proporciona una nueva versión del proyecto de empaquetado que elimina todos los pasos manuales se solían ser necesarios para empaquetar tu aplicación. Tan solo tienes que agrear un proyecto de empaquetado, hacer referencia al proyecto de escritorio y luego presionar F5 para depurar la aplicación. No es necesario realizar ajustes manuales. Esta nueva experiencia optimizada es una gran mejora de la experiencia que estaba disponible en la versión anterior de Visual Studio.

>[!IMPORTANT]
>La capacidad para crear un paquete de aplicación de Windows para su aplicación de escritorio (conocido también como el puente de escritorio) se introdujo en Windows 10, versión 1607, y solo puede usarse en proyectos que tienen como destino Windows 10 Anniversary Update (10.0; Compilación 14393) o una versión posterior de Visual Studio.

## <a name="first-prepare-your-application"></a>Primero, prepara tu aplicación

Revise a esta guía antes de empezar a crear un paquete para la aplicación: [Preparar empaquetar una aplicación de escritorio](desktop-to-uwp-prepare.md).

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

6. Compila el proyecto de empaquetado para garantizar que no aparece ningún error.  Si recibe errores, abra **Configuration Manager** y asegúrese de que los proyectos de la misma plataforma de destino.

   ![Administrador de configuración](images/desktop-to-uwp/config-manager.png)

7. Usa el asistente [Crear paquetes de aplicaciones](../packaging/packaging-uwp-apps.md) para generar un archivo appxupload.

   Puede cargar ese archivo directamente en el Store.

**Vídeo**

&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/fJkbYPyd08w]

## <a name="next-steps"></a>Pasos siguientes

**Encuentre respuestas a sus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Proporcionar comentarios o hacer sugerencias**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Ejecutar, depurar o probar su aplicación de escritorio**

Consulte [ejecutar, depurar y probar una aplicación de escritorio empaquetada](desktop-to-uwp-debug.md)

**Mejore su aplicación de escritorio mediante la adición de las API de UWP**

Consulta [Mejorar tu aplicación de escritorio para Windows 10](desktop-to-uwp-enhance.md).

**Ampliar su aplicación de escritorio mediante la adición de proyectos de UWP y componentes de Windows en tiempo de ejecución**

Consulta [Ampliar tu aplicación de escritorio con componentes de UWP modernos](desktop-to-uwp-extend.md).

**Distribuir la aplicación**

Consulte [distribuir una aplicación de escritorio empaquetada](desktop-to-uwp-distribute.md)
