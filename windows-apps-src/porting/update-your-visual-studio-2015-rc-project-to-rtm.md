---
author: mcleblanc
description: "Si tienes un proyecto de Windows 10 que creaste con Microsoft Visual Studio 2015 RC, tienes dos opciones para actualizar los archivos del proyecto al formato adecuado para Visual Studio 2015 RTM."
title: "Actualización de tu proyecto de UWP Microsoft Visual Studio 2015 RC a RTM"
ms.assetid: 104E36CE-36DE-4E9C-A944-711C200B44EF
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ad7754a55a34ba3921d9455209cc76ab8ae0757b
ms.lasthandoff: 02/07/2017

---

# <a name="update-your-uwp-microsoft-visual-studio-2015-rc-project-to-rtm"></a>Actualización de tu proyecto de UWP Microsoft Visual Studio 2015 RC a RTM

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Si tienes un proyecto de Windows 10 que creaste con Microsoft Visual Studio 2015 RC, tienes dos opciones para actualizar los archivos del proyecto al formato adecuado para Visual Studio 2015 RTM. El método recomendado es crear un nuevo proyecto de Windows 10 en Visual Studio 2015 RTM y copiar ahí los archivos. También puedes seguir la documentación avanzada para editar los archivos de proyecto existentes y pasarlos al nuevo formato.

## <a name="what-you-see-when-you-open-a-windows-10visual-studio-2015-rc-project-in-visual-studio-2015-rtm"></a>Lo que ves cuando abres un proyecto de Visual Studio 2015 RC de Windows 10 en Visual Studio 2015 RTM

Cuando abras un proyecto de Visual Studio 2015 RC de Windows 10 en Visual Studio 2015 RTM, verás un mensaje el que se indica que se requiere actualización en el **Explorador de soluciones**.

![se requiere una actualización](images/vsrc-to-rtm/solution-explorer.png)

Si obtienes acceso al menú contextual del proyecto en el **Explorador de soluciones** y eliges **Volver a cargar el proyecto**, verás este cuadro de diálogo.

![se requiere una actualización de Visual Studio](images/vsrc-to-rtm/reload-project.png)

## <a name="create-a-new-project-and-copy-files-into-it"></a>Crear un nuevo proyecto y copiar archivos en él

1.  Inicia Visual Studio 2015 RTM y crea un nuevo proyecto de aplicación vacía (universal de Windows). Recuerda que, de manera predeterminada, tu nuevo proyecto genera un paquete de la aplicación (un archivo .appx) destinado a la familia de dispositivos universales. Cámbialo si tienes como objetivo una o más familias de dispositivos específicas.
2.  En tu proyecto de Visual Studio 2015 RC, identifica todos los archivos de código fuente y los archivos de activos visuales que quieres copiar. Con el Explorador de archivos, copia los modelos de datos, los modelos de vista, los recursos visuales, los diccionarios de recursos, la estructura de carpetas y cualquier otro elemento que necesites (incluido AssemblyInfo.cs), en tu nuevo proyecto. Copia o crea subcarpetas en el disco según sea necesario.
3.  Copia vistas (por ejemplo, MainPage.xaml y MainPage.xaml.cs) en el nuevo proyecto también. De nuevo, crea nuevas subcarpetas según sea necesario y quita las vistas existentes del proyecto. Pero antes de sobrescribir o quitar una vista que Visual Studio ha generado, conserva una copia, ya que puede resultar útil consultarla más adelante.
4.  En el **Explorador de soluciones**, asegúrate de que **Mostrar todos los archivos** esté activado. Selecciona los archivos que has copiado, haz clic con el botón secundario en ellos y haz clic en **Incluir en el proyecto**. Esto incluirá automáticamente sus carpetas contenedoras. Entonces puedes desactivar **Mostrar todos los archivos** si quieres. Un flujo de trabajo alternativo, si lo prefieres, es usar el comando **Agregar elemento existente**, después de crear las subcarpetas necesarias en el **Explorador de soluciones** de Visual Studio. Vuelve a comprobar que los recursos visuales tienen la opción **Acción de compilación** establecida en **Contenido** y la opción **Copiar en el directorio de resultados** establecida en **No copiar**.
5.  Agrega referencias a cualquier SDK de extensión al que hagas referencia en el proyecto de RC y copia los cambios desde el Package.appxmanifest anterior (por ejemplo, las capacidades que declaraste) al que se muestra en el nuevo proyecto de RTM.

## <a name="advanced-edit-your-existing-project-files"></a>Opciones avanzadas: edita tus archivos de proyecto existentes

Una diferencia significativa entre el formato de proyecto de Windows 10 de Visual Studio 2015 RC y Visual Studio 2015 RTM es que el formato RTM usa [NuGet](http://docs.nuget.org/) versión 3. Ten en cuenta esta diferencia si tienes intención de actualizar manualmente el proyecto.

Si quieres actualizar manualmente el proyecto, o si estás interesado en saber las diferencias entre los formatos de proyecto de Visual Studio 2015 RC y Visual Studio 2015 RTM, consulta [Migrar aplicaciones a la Plataforma universal de Windows (UWP)](http://msdn.microsoft.com/library/mt148501.aspx).


