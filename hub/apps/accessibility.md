---
title: Accesibilidad en Windows 10
description: En esta página se proporciona información para empezar a desarrollar aplicaciones de Windows accesibles.
ms.topic: article
ms.date: 09/12/2019
keywords: Accesibilidad en Windows 10, accesibilidad, compilar aplicaciones de Win32 accesibles, compilar aplicaciones de UWP accesibles, compilar aplicaciones de WPF accesibles, compilar aplicaciones de WinForms accesibles
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: 76bbd3f0e04bbb2f729ad0950bae190b2fffb6ac
ms.sourcegitcommit: 6e7665b457ec4585db19b70acfa2554791ad6e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2019
ms.locfileid: "70987194"
---
# <a name="accessibility-in-windows-10"></a>Accesibilidad en Windows 10

![Hero-Accessibility-bar-smaller. png](images/hero-accessibility-bar-smaller.png)

## <a name="build-accessibility-into-your-applications-to-empower-people-of-all-abilities"></a>Cree accesibilidad en sus aplicaciones para permitir a los usuarios de todas las capacidades

Los productos y servicios, incluidos los medios electrónicos, son accesibles cuando están diseñados para proporcionar experiencias completas y correctas para tantas personas como sea posible.

Cree aplicaciones de Windows inclusivas y accesibles, con funcionalidad y facilidad de uso mejoradas, para personas con discapacidades (tanto temporales como permanentes), preferencias personales, estilos de trabajo específicos o restricciones de situación (como espacios de trabajo compartidos, conducción, cocina, deslumbramiento, etc.). Algunas soluciones comunes incluyen proporcionar información en formatos alternativos (como leyendas en un vídeo) o habilitar el uso de tecnologías de asistencia (como lectores de pantalla).

**Todos deben tener acceso a los mismos salones en un edificio, tanto si necesitan usar las escaleras como el ascensor.**

En esta página se proporciona información sobre el modo en que los distintos marcos de desarrollo de Windows proporcionan compatibilidad de accesibilidad para desarrolladores que crean aplicaciones de Windows, desarrolladores de tecnología de asistencia, como lectores de pantalla y ampliadores, y software Ingenieros de prueba que crean scripts automatizados para probar aplicaciones.

## <a name="platform-specific-documentation"></a>Documentación específica de la plataforma

:::row:::
   :::column:::
      ![Plataforma universal de Windows (UWP)](images/platform-uwp.png)

      **Plataforma universal de Windows (UWP)**

      Desarrolle aplicaciones y herramientas accesibles en la plataforma moderna para aplicaciones y juegos de Windows 10 en cualquier dispositivo de Windows (incluidos PC, teléfonos, Xbox One, HoloLens, etc.) y publíquelos en el Microsoft Store.

      [Diseño de software inclusivo](https://docs.microsoft.com/windows/uwp/accessibility/designing-inclusive-software)

      [Desarrollo de aplicaciones inclusivas de Windows](https://docs.microsoft.com/windows/uwp/accessibility/developing-inclusive-windows-apps)

      [Pruebas de accesibilidad](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-testing)

      [Accesibilidad en la Tienda](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-in-the-store)
   :::column-end:::
   :::column:::
      ![Aplicaciones de la plataforma Win32](images/platform-win32.png)

      **Plataforma Win32**

      Desarrolle aplicaciones y herramientas accesibles en la plataforma original para aplicacionesC++ de C/Windows.

      [Novedades de la automatización y la accesibilidad de Windows](https://docs.microsoft.com/windows/desktop/accessibility-whatsnew)

      [Desarrollo de aplicaciones accesibles para Windows](https://docs.microsoft.com/windows/desktop/accessibility-appdev)

      [Desarrollo de marcos de interfaz de usuario accesibles para Windows](https://docs.microsoft.com/windows/desktop/accessibility-uiframeworkdev)

      [Desarrollo de tecnología de asistencia para Windows](https://docs.microsoft.com/windows/desktop/accessibility-atdev)

      [Probar la accesibilidad](https://docs.microsoft.com/windows/desktop/accessibility-testwithuia)

      [Tecnología de automatización y accesibilidad heredada: MSAA en automatización de la interfaz de usuario](https://docs.microsoft.com/windows/desktop/accessibility-legacy)

      [Características de accesibilidad de Windows](https://docs.microsoft.com/windows/desktop/winauto/about-windows-accessibility-features)

      [Directrices para diseñar aplicaciones accesibles](https://docs.microsoft.com/windows/desktop/uxguide/inter-accessibility)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![Plataforma WPF](images/platform-wpf2-small.png)

      **Windows Presentation Foundation (WPF)**

      Desarrolle aplicaciones y herramientas accesibles en la plataforma establecida para aplicaciones Windows administradas con un modelo de interfaz de usuario XAML y el .NET Framework.

      [Prácticas recomendadas de accesibilidad](https://docs.microsoft.com/dotnet/framework/ui-automation/accessibility-best-practices)

      [Aspectos básicos de la automatización de la interfaz de usuario](https://docs.microsoft.com/dotnet/framework/ui-automation/index)

      [Proveedores de UI Automation para código administrado](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-providers-for-managed-code)

      [Clientes de UI Automation para código administrado](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-clients-for-managed-code)

      [Patrones de control de UI Automation](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-patterns)

      [Patrón de texto de automatización de la interfaz de usuario](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-text-pattern)

      [Tipos de control de UI Automation](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-types)

      [Especificación de UI Automation y compromiso de la comunidad](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-specification-and-community-promise)
   :::column-end:::
   :::column:::
      ![Aplicaciones de Windows Forms Platform](images/platform-winforms.png)

      **Windows Forms (WinForms)**

      Desarrolle aplicaciones y herramientas de acceso para aplicaciones Windows administradas con un modelo de interfaz de usuario XAML y el .NET Framework.

      [Accesibilidad Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)

      [Creación de una aplicación de Windows accesible](https://docs.microsoft.com/dotnet/framework/winforms/advanced/walkthrough-creating-an-accessible-windows-based-application)

      [Propiedades de los controles de Windows Forms que admiten instrucciones de accesibilidad](https://docs.microsoft.com/dotnet/framework/winforms/advanced/properties-on-windows-forms-controls-that-support-accessibility-guidelines)

      [Proporcionar información de accesibilidad para los controles de Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/controls/providing-accessibility-information-for-controls-on-a-windows-form)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **Accesibilidad web**

      Diseñar, compilar y probar sitios web accesibles en Microsoft Edge.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Introducción a accesibilidad web](https://docs.microsoft.com/microsoft-edge/accessibility)

      [Diseñar sitios web accesibles](https://docs.microsoft.com/microsoft-edge/accessibility/design)
   :::column-end:::
   :::column:::
      [Creación de sitios web accesibles](https://docs.microsoft.com/microsoft-edge/accessibility/build)

      [Prueba de sitios web accesibles](https://docs.microsoft.com/microsoft-edge/accessibility/test)
   :::column-end:::
:::row-end:::

## <a name="samples"></a>Muestras

Descargue y ejecute ejemplos completos de Windows que muestren varias características y funcionalidades de accesibilidad.

:::row:::
   :::column:::
      [Explorador de ejemplo de código](https://docs.microsoft.com/en-us/samples/browse/)

      El explorador de ejemplos nuevo reemplaza a la galería de código de MSDN.
   :::column-end:::
   :::column:::
      [Galería de código de MSDN (retirada)](https://code.msdn.microsoft.com/site/search?query=accessibility&f%5B0%5D.Value=accessibility&f%5B0%5D.Type=SearchText&ac=2)

      Descargue ejemplos para Windows, Windows Phone, Microsoft Azure, Office, SharePoint, Silverlight y otros productos.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Ejemplos de Windows clásico en GitHub](https://github.com/microsoft/Windows-classic-samples/search?q=accessibility&unscoped_q=accessibility)

      En estos ejemplos se muestra el modelo de programación y la funcionalidad de Windows y Windows Server. 
   :::column-end:::
   :::column:::
      [Ejemplos de Plataforma universal de Windows (UWP) en GitHub](https://github.com/microsoft/Windows-universal-samples/search?q=accessibility&unscoped_q=accessibility)

      En estos ejemplos se muestran los patrones de uso de la API para el Plataforma universal de Windows (UWP) en el kit de desarrollo de software (SDK) de Windows para Windows 10.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [XAML Controls Gallery](https://github.com/microsoft/Xaml-Controls-Gallery)

      Esta aplicación muestra los distintos controles XAML admitidos en el sistema de diseño de Fluent.
   :::column-end:::
:::row-end:::

## <a name="videos"></a>Vídeos

Varios vídeos en los que se explica cómo crear aplicaciones de Windows accesibles para cuestiones de accesibilidad generales y cómo Microsoft las resuelve.

:::row:::
   :::column:::
      **Cómo empezar a trabajar con accesibilidad en aplicaciones de Windows**
   :::column-end:::
   :::column:::
      **Un minuto de desarrollo: Desarrollo de aplicaciones para accesibilidad**
   :::column-end:::
   :::column:::
      **Introducción a Disability y accesibilidad**
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
      **De hack to product, control ocular para Windows 10**
   :::column-end:::
   :::column:::
      **Accesibilidad en Windows 10**
   :::column-end:::
   :::column:::
      **Introducción a la compilación de aplicaciones UWP accesibles**
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
      **Diseñar características de accesibilidad de Windows**
   :::column-end:::
   :::column:::
      **Características de accesibilidad de Windows 10 otorgar a todos**
   :::column-end:::
   :::column:::
      **Facilitar la visualización de los punteros del mouse**
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

## <a name="other-resources"></a>Otros recursos:

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
      [Blogs de accesibilidad](https://blogs.microsoft.com/accessibility/)
   :::column-end:::
   :::column:::
      [Blogs de automatización de la interfaz de usuario de Windows](https://blogs.msdn.microsoft.com/winuiautomation/)
   :::column-end:::
:::row-end:::

:::row:::
   :::column span="3":::
      **Comunidad y soporte técnico**

      Un lugar donde los desarrolladores y usuarios de Windows reúnen y aprenden juntos.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Comunidad de Windows: accesibilidad](https://community.windows.com/search?q=accessibility)
   :::column-end:::
   :::column:::
      [Foro de accesibilidad de Windows y desarrollo de automatización](https://social.msdn.microsoft.com/Forums/windows/home?forum=windowsaccessibilityandautomation)
   :::column-end:::
   :::column:::
      [Departamento de soporte técnico Answer](https://www.microsoft.com/Accessibility/disability-answer-desk)
   :::column-end:::
:::row-end:::
