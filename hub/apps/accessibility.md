---
title: Accesibilidad en Windows 10
description: En esta página se proporciona la información necesaria para empezar a desarrollar aplicaciones de Windows accesibles.
ms.topic: article
ms.date: 09/12/2019
keywords: Accesibilidad en Windows 10, Accesibilidad, compilación de aplicaciones de Win32 accesibles, compilación de aplicaciones para UWP accesibles, compilación de aplicaciones para WPF accesibles, compilación de aplicaciones de WinForms accesibles
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: fdb1092139602edd00086e17ff4dc7a27cf361dd
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681896"
---
# <a name="accessibility-in-windows-10"></a>Accesibilidad en Windows 10

![hero-accessibility-bar-smaller.png](images/hero-accessibility-bar-smaller.png)

## <a name="build-accessibility-into-your-applications-to-empower-people-of-all-abilities"></a>Agrega accesibilidad en tus aplicaciones para que las puedan utilizar todos los usuarios, independientemente de sus capacidades

Los productos y servicios, incluidos los medios electrónicos, son accesibles cuando están diseñados para proporcionar experiencias completas y correctas para tantas personas como sea posible.

Compila aplicaciones de Windows inclusivas y accesibles, que ofrezcan funcionalidades y una facilidad de uso mejoradas, para personas con discapacidades (tanto temporales como permanentes) y según las preferencias personales, los estilos de trabajo específicos o las restricciones de los sitios (como, por ejemplo, si se está en espacios de trabajo compartidos, se conduce, se cocina o si hay deslumbramiento, etc.). Entre otras soluciones comunes, se incluyen aquellas para proporcionar información en formatos alternativos (como leyendas en un vídeo) o habilitar el uso de tecnologías de asistencia (como lectores de pantalla).

**Todas las personas deben tener acceso a las mismas salas de un edificio, tanto si necesitan usar las escaleras como el ascensor.**

En esta página se proporciona información sobre cómo los distintos marcos de desarrollo de Windows proporcionan compatibilidad con la accesibilidad para los desarrolladores que crean aplicaciones de Windows, los desarrolladores de tecnología de asistencia que compilan, por ejemplo, lectores de pantalla y lupas, y los ingenieros de pruebas de software que crean scripts automatizados para probar aplicaciones.

## <a name="platform-specific-documentation"></a>Documentación específica de la plataforma

:::row:::
   :::column:::
      ![Plataforma universal de Windows (UWP)](images/platform-uwp.png)

      **Plataforma universal de Windows (UWP)**

      Desarrolla aplicaciones y herramientas accesibles en la plataforma moderna para aplicaciones y juegos de Windows 10 en cualquier dispositivo de Windows (incluidos equipos, teléfonos, Xbox One, HoloLens, etc.) y publícalos en Microsoft Store.

      [Diseño de software inclusivo](https://docs.microsoft.com/windows/uwp/accessibility/designing-inclusive-software)

      [Desarrollo de aplicaciones inclusivas de Windows](https://docs.microsoft.com/windows/uwp/accessibility/developing-inclusive-windows-apps)

      [Pruebas de accesibilidad](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-testing)

      [Accesibilidad en Store](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-in-the-store)
   :::column-end:::
   :::column:::
      ![Aplicaciones de la plataforma Win32](images/platform-win32.png)

      **Plataforma Win32**

      Desarrolla aplicaciones y herramientas accesibles en la plataforma original para aplicaciones en C++ y C de Windows.

      [Novedades de la automatización y la accesibilidad de Windows](https://docs.microsoft.com/windows/desktop/accessibility-whatsnew)

      [Desarrollo de aplicaciones accesibles para Windows](https://docs.microsoft.com/windows/desktop/accessibility-appdev)

      [Desarrollo de marcos de interfaz de usuario accesibles para Windows](https://docs.microsoft.com/windows/desktop/accessibility-uiframeworkdev)

      [Desarrollo de tecnología de asistencia para Windows](https://docs.microsoft.com/windows/desktop/accessibility-atdev)

      [Pruebas de accesibilidad](https://docs.microsoft.com/windows/desktop/accessibility-testwithuia)

      [Tecnología de automatización y accesibilidad heredada: MSAA en automatización de la interfaz de usuario](https://docs.microsoft.com/windows/desktop/accessibility-legacy)

      [Características de accesibilidad de Windows](https://docs.microsoft.com/windows/desktop/winauto/about-windows-accessibility-features)

      [Directrices para diseñar aplicaciones accesibles](https://docs.microsoft.com/windows/desktop/uxguide/inter-accessibility)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![Plataforma WPF](images/platform-wpf2-small.png)

      **Windows Presentation Foundation (WPF)**

      Desarrolla aplicaciones y herramientas accesibles en la plataforma establecida para las aplicaciones de Windows administradas con un modelo de interfaz de usuario de XAML y .NET Framework.

      [Procedimientos recomendados de accesibilidad](https://docs.microsoft.com/dotnet/framework/ui-automation/accessibility-best-practices)

      [Aspectos básicos de la automatización de la interfaz de usuario](https://docs.microsoft.com/dotnet/framework/ui-automation/index)

      [Proveedores de Automatización de la interfaz de usuario para código administrado](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-providers-for-managed-code)

      [Clientes de Automatización de la interfaz de usuario para código administrado](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-clients-for-managed-code)

      [Patrones de control de Automatización de la interfaz de usuario](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-patterns)

      [Patrón de texto de Automatización de la interfaz de usuario](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-text-pattern)

      [Tipos de control de Automatización de la interfaz de usuario](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-types)

      [Especificación de Automatización de la interfaz de usuario y compromiso de la comunidad](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-specification-and-community-promise)
   :::column-end:::
   :::column:::
      ![Aplicaciones de la plataforma Windows Forms](images/platform-winforms.png)

      **Windows Forms (WinForms)**

      Desarrolla aplicaciones y herramientas accesibles para las aplicaciones de Windows administradas con un modelo de interfaz de usuario de XAML y .NET Framework.

      [Accesibilidad de formularios de Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)

      [Creación de una aplicación de Windows accesible](https://docs.microsoft.com/dotnet/framework/winforms/advanced/walkthrough-creating-an-accessible-windows-based-application)

      [Propiedades de los controles de formularios de Windows Forms que admiten las directrices de accesibilidad](https://docs.microsoft.com/dotnet/framework/winforms/advanced/properties-on-windows-forms-controls-that-support-accessibility-guidelines)

      [Provisión de información de accesibilidad de controles en un formulario de Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/controls/providing-accessibility-information-for-controls-on-a-windows-form)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **Accesibilidad web**

      Diseña, compila y prueba sitios web accesibles en Microsoft Edge.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Introducción a la accesibilidad web](https://docs.microsoft.com/microsoft-edge/accessibility)

      [Diseño de sitios web accesibles](https://docs.microsoft.com/microsoft-edge/accessibility/design)
   :::column-end:::
   :::column:::
      [Creación de sitios web accesibles](https://docs.microsoft.com/microsoft-edge/accessibility/build)

      [Pruebas de sitios web accesibles](https://docs.microsoft.com/microsoft-edge/accessibility/test)
   :::column-end:::
:::row-end:::

## <a name="samples"></a>Ejemplos

Descarga y ejecuta ejemplos completos de Windows que muestren varias características y funcionalidades de accesibilidad.

:::row:::
   :::column:::
      [Explorador de ejemplo de código](https://docs.microsoft.com/samples/browse/)

      El nuevo explorador de ejemplos reemplaza la galería de código de MSDN.
   :::column-end:::
   :::column:::
      [Galería de código de MSDN (retirada)](https://code.msdn.microsoft.com/site/search?query=accessibility&f%5B0%5D.Value=accessibility&f%5B0%5D.Type=SearchText&ac=2)

      Descarga ejemplos para Windows, Windows Phone, Microsoft Azure, Office, SharePoint, Silverlight y otros productos.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Ejemplos de Windows clásico en GitHub](https://github.com/microsoft/Windows-classic-samples/search?q=accessibility&unscoped_q=accessibility)

      En estos ejemplos se muestra el modelo de programación y la funcionalidad de Windows y Windows Server. 
   :::column-end:::
   :::column:::
      [Ejemplos de la Plataforma universal de Windows (UWP) en GitHub](https://github.com/microsoft/Windows-universal-samples/search?q=accessibility&unscoped_q=accessibility)

      En estos ejemplos se muestran los patrones de uso de la API para el Plataforma universal de Windows (UWP) en el Kit de desarrollo de software (SDK) de Windows para Windows 10.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [XAML Controls Gallery](https://github.com/microsoft/Xaml-Controls-Gallery)

      Esta aplicación muestra los distintos controles XAML admitidos en el sistema Fluent Design.
   :::column-end:::
:::row-end:::

## <a name="videos"></a>Vídeos

A continuación, le proporcionamos varios vídeos en los que se explica cómo crear aplicaciones de Windows accesibles para cuestiones de accesibilidad generales y cómo Microsoft las resuelve.

:::row:::
   :::column:::
      **Cómo empezar a trabajar con la accesibilidad en aplicaciones de Windows**
   :::column-end:::
   :::column:::
      **One Dev Minute: Desarrollo de aplicaciones para la accesibilidad**
   :::column-end:::
   :::column:::
      **Introducción a las discapacidades y la accesibilidad**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2017/P4072/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Kl4CT4DaypM]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **De ser candidato en Hackathon a convertirse en un producto: el control ocular para Windows 10**
   :::column-end:::
   :::column:::
      **Accesibilidad en Windows 10**
   :::column-end:::
   :::column:::
      **Introducción a la creación de aplicaciones para UWP accesibles**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/AShNPfmAkvY]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P541/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P497/player]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **Diseño de características de accesibilidad de Windows**
   :::column-end:::
   :::column:::
      **Las características de accesibilidad de Windows 10 otorgan posibilidades a todo el mundo**
   :::column-end:::
   :::column:::
      **Simplificación de la visualización de los punteros del mouse**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Y_NJbE7wxlk]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/BseTf-4q9GA]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/4UzaF7_T3bw]
   :::column-end:::
:::row-end:::

## <a name="other-resources"></a>Otros recursos

:::row:::
   :::column span="3":::
      **Blogs y noticias**

      La versión más reciente del mundo de accesibilidad de Microsoft.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [En las noticias](https://news.microsoft.com/presskits/accessibility/)
   :::column-end:::
   :::column:::
      [Blogs de accesibilidad](https://blogs.microsoft.com/accessibility/).
   :::column-end:::
   :::column:::
      [Blogs de Automatización de la interfaz de usuario de Windows](https://blogs.msdn.microsoft.com/winuiautomation/)
   :::column-end:::
:::row-end:::

:::row:::
   :::column span="3":::
      **Comunidad y soporte técnico**

      Un lugar donde los desarrolladores y usuarios de Windows se reúnen y aprenden juntos.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Comunidad de Windows: accesibilidad](https://community.windows.com/search?q=accessibility)
   :::column-end:::
   :::column:::
      [Foro de de desarrollo de la automatización y la accesibilidad de Windows](https://social.msdn.microsoft.com/Forums/windows/home?forum=windowsaccessibilityandautomation)
   :::column-end:::
   :::column:::
      [Answer Desk para personas con discapacidad](https://www.microsoft.com/Accessibility/disability-answer-desk)
   :::column-end:::
:::row-end:::
