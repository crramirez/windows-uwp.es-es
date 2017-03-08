---
author: mcleanbyron
description: "Aprende a actualizar la aplicación para que use las últimas bibliotecas admitidas de Microsoft Advertising y a asegurarte de que siga recibiendo anuncios de banner."
title: "Actualizar la aplicación a las bibliotecas más recientes de Microsoft Advertising"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anuncios, publicidad, AdControl, AdMediatorControl, migrar
ms.assetid: f8d5b2ad-fcdb-4891-bd68-39eeabdf799c
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 3cdb1f41fda7bd4e4af1ce9e5f8fb4396da53f63
ms.lasthandoff: 02/08/2017

---

# <a name="update-your-app-to-the-latest-microsoft-advertising-libraries"></a>Actualizar la aplicación a las bibliotecas más recientes de Microsoft Advertising

Solo los SDK siguientes son compatibles para mostrar anuncios de banner de Microsoft en tus aplicaciones usando un **AdControl** o **AdMediatorControl**:

* [Microsoft Store Services SDK](http://aka.ms/store-services-sdk) (para aplicaciones para UWP)
* [SDK de Microsoft Advertising para Windows y Windows Phone 8.x](http://aka.ms/store-8-sdk) (para aplicaciones de Windows 8.1 y Windows Phone 8.x)

Antes de que estos SDK estuviesen disponibles, publicamos varias versiones de SDK de publicidad antigua para aplicaciones de Windows y Windows Phone. Ya no se admiten estas versiones anteriores de SDK de publicidad. En el futuro, tenemos previsto dejara de publicar anuncios de banner para aplicaciones que usan los SDK anteriores.

Si tienes una aplicación existente (ya está en la Tienda o aún está en la etapa de desarrollo) que muestra anuncios de banner mediante los controles **AdControl** o **AdMediatorControl**, tal vez debas actualizar la aplicación para que use el SDK de publicidad más reciente en tu plataforma de destino y, así, siga recibiendo los anuncios de banner en el futuro Sigue las instrucciones de este artículo para determinar si la aplicación se ve afectada por este cambio y para conocer cómo actualizar la aplicación si fuera necesario.

Si la aplicación se ve afectada por este cambio y no se actualiza la aplicación para usar el SDK de publicidad más reciente, verás el siguiente comportamiento si dejamos de publicar anuncios de banner en aplicaciones que usan versiones de SDK de publicidad no admitidas:

* Ya no se ofrecerán anuncios de banner a los controles **AdControl** o **AdMediatorControl** de tu aplicación y ya no obtendrás ingresos publicitarios por esos controles.

* Cuando el control **AdControl** o **AdMediatorControl** de tu aplicación solicite un anuncio nuevo, se generará el evento **ErrorOccurred** del control y la propiedad **ErrorCode** de los argumentos del evento tendrá el valor **NoAdAvailable**.

Para proporcionar contexto adicional sobre este cambio, ya no admitimos versiones anteriores del SDK de publicidad que no admiten un conjunto mínimo de capacidades, incluida la posibilidad de ofrecer contenido multimedia enriquecido de HTML5 a través de la [especificación Mobile Rich-media Ad Interface Definitions (MRAID) 1.0](http://www.iab.com/wp-content/uploads/2015/08/IAB_MRAID_VersionOne.pdf) de Interactive Advertising Bureau (IAB). Muchos de nuestros anunciantes buscan estas capacidades, y estamos haciendo este cambio con el fin de ayudar a que nuestro ecosistema de aplicaciones sea más atractivo para los anunciantes y, en última instancia, generar más ingresos para ti.

Si se produce algún problema o necesitas ayuda, [ponte en contacto con soporte técnico](http://go.microsoft.com/fwlink/?LinkId=393643).

>**Nota**&nbsp;&nbsp;Si tu aplicación ya usa el [Microsoft Store Services SDK](http://aka.ms/store-services-sdk) (para aplicaciones para UWP) o el [SDK de Microsoft Advertising para Windows y Windows Phone 8.x](http://aka.ms/store-8-sdk) (para aplicaciones de Windows 8.1 y Windows Phone 8.x), o si actualizaste anteriormente la aplicación para que use uno de estos SDK de publicidad más reciente disponible y no es necesario hacer más cambios en la aplicación.

## <a name="prerequisites"></a>Requisitos previos

* El código fuente completo y los archivos del proyecto de Visual Studio de la aplicación que usa los controles **AdControl** o **AdMediatorControl**.

* El paquete .appx o .xap de la aplicación.

  >**Nota**&nbsp;&nbsp;Si ya no tienes el paquete .appx o .xap de la aplicación, pero todavía tienes un equipo de desarrollo con la versión de Visual Studio y el SDK de publicidad que se usó para crear la aplicación, puedes volver a generar el paquete .appx o .xap en Visual Studio.

<span id="part-1" />
## <a name="part-1-determine-whether-you-need-to-update-your-app"></a>Parte 1: determinar si es necesario actualizar la aplicación

Sigue las instrucciones de las siguientes secciones para determinar si necesitas actualizar la aplicación.

### <a name="your-app-uses-adcontrol"></a>La aplicación usa AdControl

Si la aplicación usa **AdControl** para mostrar anuncios de banner, sigue estas instrucciones.

**Aplicaciones para UWP para Windows 10**

1. Crea una copia del paquete .appx de la aplicación para no alterar el original, cambia el nombre de la copia para que tenga una extensión .zip y extrae el contenido del archivo.

2. Comprueba el contenido extraído del paquete de la aplicación:

  * Si ves un archivo Microsoft.Advertising.dll, la aplicación usa un SDK anterior y debes actualizar el proyecto siguiendo las instrucciones que aparecen en las secciones a continuación. Ve a la [Parte 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).

  * Si no ves un archivo Microsoft.Advertising.dll, la aplicación para UWP ya usa el SDK de publicidad más reciente disponible y no es necesario hacer cambios en el proyecto.

<span/>

**Aplicaciones de Windows 8.1 o Windows Phone 8.x**

1. Crea una copia del paquete .appx o .xap de la aplicación para no alterar el original, cambia el nombre de la copia para que tenga una extensión .zip y extrae el contenido del archivo.

2. Para las aplicaciones XAML o HTML/JavaScript, comprueba el contenido extraído del paquete de la aplicación:

  * Si ves un archivo Microsoft.Advertising.winmd, pero ningún archivo UniversalXamlAdControl.\*.dll (para aplicaciones XAML) o el archivo UniversalSharedLibrary.Windows.dll (para aplicaciones JavaScript y HTML), la aplicación usa un SDK anterior y debes actualizar el proyecto siguiendo las instrucciones que aparecen en las secciones a continuación. Ve a la [Parte 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).

  * De lo contrario, continúa con el paso siguiente.

2. Abre Windows PowerShell, escribe el siguiente comando y asigna el argumento ```-Path``` a la ruta de acceso completa al contenido extraído del paquete de la aplicación. Este comando muestra todas las bibliotecas de publicidad a las que se hace referencia en el proyecto y la versión de cada biblioteca.

  > [!div class="tabbedCodeSnippets"]
  ```syntax
  get-childitem -Path "<path to your extracted package>" * -Recurse -include *advert*.dll,*admediator*.dll,*xamladcontrol*.dll,*universalsharedlibrary*.dll | where-object {$_.Name -notlike "*resources*" -and $_.Name -notlike "*design*" } | foreach-object { "{0}`t{1}" -f $_.FullName, [System.Diagnostics.FileVersionInfo]::GetVersionInfo($_).FileVersion }
  ```

2. Busca el archivo que aparece en la tabla siguiente para la plataforma de destino de la aplicación y compara las versiones de ese archivo con la versión que aparece en la tabla.

  <table>
    <colgroup>
      <col width="33%" />
      <col width="33%" />
      <col width="33%" />
    </colgroup>
    <thead>
      <tr class="header">
        <th align="left">Plataforma de destino</th>
        <th align="left">Archivos</th>
        <th align="left">Versión</th>
      </tr>
    </thead>
    <tbody>
      <tr class="odd">
        <td align="left"><p>Windows 8.1 XAML</p></td>
        <td align="left"><p>UniversalXamlAdControl.Windows.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows Phone 8.1 XAML</p></td>
        <td align="left"><p>UniversalXamlAdControl.WindowsPhone.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows 8.1 JavaScript/HTML<br/>Windows Phone 8.1 JavaScript/HTML</p></td>
        <td align="left"><p>UniversalSharedLibrary.Windows.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows Phone 8.1 Silverlight</p></td>
        <td align="left"><p>Microsoft.Advertising.\*.dll</p></td>
        <td align="left"><p>8.1.50112.0</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows Phone 8.0 Silverlight</p></td>
        <td align="left"><p>Microsoft.Advertising.\*.dll</p></td>
        <td align="left"><p>6.2.40501.0</p></td>
      </tr>
    </tbody>
  </table>

3. Si el archivo tiene una versión igual o posterior a la versión que aparece en la tabla antes mencionada, no es necesario hacer cambios en el proyecto.

  Si el archivo tiene un número de versión inferior, debes actualizar el proyecto mediante las instrucciones que aparecen en las secciones siguientes. Ve a la [Parte 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).

<span/>

**Aplicaciones de Windows 8.0**

* Puede que las aplicaciones destinadas a Windows 8.0 ya no presenten anuncios de banner en el futuro. Para evitar perder impresiones, te recomendamos convertir el proyecto en una aplicación para UWP destinada a Windows 10. La mayoría del tráfico de las aplicaciones de Windows 8.0 ahora se ejecuta en dispositivos con Windows 10.

<span/>

**Aplicaciones de Windows Phone 7.x**

* Puede que las aplicaciones destinadas a Windows Phone 7.x ya no presenten anuncios de banner en el futuro. Para evitar perder impresiones, te recomendamos convertir el proyecto para orientarlo a aplicaciones de Windows Phone 8.1 o convertirlo en una aplicación para UWP destinada a Windows 10. La mayoría del tráfico de las aplicaciones de Windows 7.x ahora se ejecuta en dispositivos con Windows Phone 8.1 o Windows 10.

<span/>

### <a name="your-app-uses-admediatorcontrol"></a>La aplicación usa AdMediatorControl

Si la aplicación usa **AdMediatorControl** para mostrar anuncios de banner, sigue las instrucciones a continuación para determinar si es necesario actualizar la aplicación.

**Aplicaciones para UWP para Windows 10**

* **AdMediatorControl** ya no se admite en aplicaciones para UWP. Debes migrar para usar **AdControl**. Para ello, sigue las instrucciones que aparecen en las secciones a continuación. Ve a la [Parte 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).

<span/>

**Aplicaciones de Windows 8.1 o Windows Phone 8.1**

1. Crea una copia del paquete .appx o .xap de la aplicación para no alterar el original, cambia el nombre de la copia para que tenga una extensión .zip y extrae el contenido del archivo.

2. Abre Windows PowerShell, escribe el siguiente comando y asigna el argumento ```-Path``` a la ruta de acceso completa al contenido extraído del paquete de la aplicación. Este comando muestra todas las bibliotecas de publicidad a las que se hace referencia en el proyecto y la versión de cada biblioteca.

  > [!div class="tabbedCodeSnippets"]
  ```syntax
  get-childitem -Path "<path to your extracted package>" * -Recurse -include *advert*.dll,*admediator*.dll,*xamladcontrol*.dll,*universalsharedlibrary*.dll | where-object {$_.Name -notlike "*resources*" -and $_.Name -notlike "*design*" } | foreach-object { "{0}`t{1}" -f $_.FullName, [System.Diagnostics.FileVersionInfo]::GetVersionInfo($_).FileVersion }
  ```

2. Si la versión de los archivos Microsoft.AdMediator.\*.dll que aparecen en la salida es la versión 2.0.1603.18005 o posterior, no es necesario hacer cambios en el proyecto.

  Si estos archivos tienen un número de versión inferior, debes actualizar el proyecto mediante las instrucciones que aparecen en las secciones siguientes. Ve a la [Parte 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).

<span id="part-2" />
## <a name="part-2-install-the-latest-sdk"></a>Parte 2: instalar el SDK más reciente

Si la aplicación usa una versión anterior del SDK, sigue estas instrucciones para asegurarte de que tienes el SDK más reciente en el equipo de desarrollo.

1. Asegúrate de que el equipo de desarrollo tenga instalado Visual Studio 2015 (para proyectos de UWP, Windows 8.1 o Windows Phone 8.x) o Visual Studio 2013 (para proyectos de Windows 8.1 o Windows Phone 8.x).

  >**Nota**&nbsp;&nbsp;Si Visual Studio está abierto en el equipo de desarrollo, ciérralo antes de realizar los siguientes pasos.

1.    Desinstala todas las versiones anteriores del SDK de Microsoft Advertising y del SDK de Ad Mediator del equipo de desarrollo.

2.    Abre una ventana **Símbolo del sistema** y ejecuta los siguientes comandos para limpiar todas las versiones de SDK que se hayan instalado con Visual Studio, pero que podrían no aparecer en la lista de programas instalados en el equipo:

  > [!div class="tabbedCodeSnippets"]
  ```syntax
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.    Instala el SDK más reciente de la aplicación:
  * En el caso de las aplicaciones para UWP en Windows 10, instala [Microsoft Store Services SDK](http://aka.ms/store-services-sdk).
  * En el caso de las aplicaciones destinadas a una versión anterior del sistema operativo, instala el [SDK de Microsoft Advertising para Windows y Windows Phone 8.x](http://aka.ms/store-8-sdk).

## <a name="part-3-update-your-project"></a>Parte 3: actualizar el proyecto

Sigue estas instrucciones para actualizar el proyecto.

### <a name="uwp-projects-for-windows-10"></a>Proyectos de UWP para Windows 10

<span/>

Si la aplicación usa **AdMediatorControl**, [refactoriza la aplicación para que use AdControl](migrate-from-admediatorcontrol-to-adcontrol.md) en su lugar. **AdMediatorControl** ya no se admite en aplicaciones para UWP.

Si la aplicación usa **AdControl**, quita todas las referencias existentes a las bibliotecas de Microsoft Advertising del proyecto y sigue las instrucciones de [AdControl en XAML](adcontrol-in-xaml-and--net.md) o [AdControl en HTML](adcontrol-in-html-5-and-javascript.md) para agregar las referencias necesarias. Esto garantizará que el proyecto use las bibliotecas correctas. Puedes conservar el código y el marcado XAML existentes.

<span/>

### <a name="windows-81-or-windows-phone-81-xaml-or-javascripthtml-projects"></a>Proyectos de Windows 8.1 o Windows Phone 8.1 (XAML o HTML/JavaScript)

<span/>

1. Quita todas las referencias de Microsoft.Advertising.\* y Microsoft.AdMediator.\* del proyecto. Si usaste la plantilla de proyecto universal, es posible que tengas dos referencias (una para Windows y otra para Windows Phone).

2. Si la aplicación usa **AdMediatorControl**, vuelve a agregar las referencias de biblioteca siguiendo las instrucciones que se describen en [Agregar y usar el control de mediación de anuncios](https://msdn.microsoft.com/library/windows/apps/xaml/dn864355.aspx). Si la aplicación usa **AdControl**, vuelve a agregar las referencias de biblioteca siguiendo las instrucciones que se describen en [AdControl en XAML](adcontrol-in-xaml-and--net.md) o [AdControl en HTML](adcontrol-in-html-5-and-javascript.md).

<span/>

Ten en cuenta esto:

* Si la aplicación se compiló anteriormente en la plataforma **Cualquier CPU**, debes volver a compilar el proyecto para una plataforma específica de una arquitectura (x 86, x64 o ARM).

* Si tienes una aplicación XAML de Windows Phone 8.x que anteriormente usó una versión del SDK en la que la clase **AdControl** se definió en el espacio de nombres **Microsoft.Advertising.Mobile.UI**, debes actualizar el código para que haga referencia a la clase **AdControl** del espacio de nombres **Microsoft.Advertising.WinRT.UI** (esta clase movió espacios de nombres en las versiones más recientes del SDK).

* A excepción del problema anterior, puedes conservar el código y el marcado XAML existentes.

<span/>

### <a name="windows-phone-8x-silverlight-projects"></a>Proyectos de Windows Phone 8.x Silverlight

<span/>

1. Quita todas las referencias de Microsoft.Advertising.\* y Microsoft.AdMediator.\* del proyecto.

2. Si la aplicación usa **AdMediatorControl**, vuelve a agregar las referencias de biblioteca siguiendo las instrucciones que se describen en [Agregar y usar el control de mediación de anuncios](https://msdn.microsoft.com/library/windows/apps/xaml/dn864355.aspx). Si la aplicación usa **AdControl**, vuelve a agregar las referencias de biblioteca siguiendo las instrucciones que se describen en [AdControl en Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md).

<span/>

Ten en cuenta esto:

* Puedes conservar el código y el marcado XAML existentes.

* En **Explorador de soluciones**, comprueba las propiedades de la referencia **Microsoft.Advertising.Mobile.UI** en el proyecto. Debe ser la versión 6.2.40501.0 si la aplicación está destinada a Windows Phone 8.0 o la versión 8.1.50112.0 si la aplicación está destinada a Windows Phone 8.1.

* Para las aplicaciones de Windows Phone 8.x Silverlight, no se admite la prueba de unidades de producción en un emulador. Te recomendamos que hagas las pruebas en un dispositivo.

## <a name="part-4-test-and-republish-your-app"></a>Parte 4: probar y volver a publicar la aplicación

Prueba la aplicación para asegurarte de que muestra anuncios de banner según lo previsto.

Si la versión anterior de la aplicación ya está disponible en la Tienda, [crea un nuevo envío](../publish/app-submissions.md) para la aplicación actualizada en el panel del Centro de desarrollo de Windows para publicar la aplicación.





 

