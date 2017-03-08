---
author: TylerMSFT
title: "Iniciar la aplicación Configuración de Windows"
description: "Aprende a iniciar la aplicación Configuración de Windows desde la aplicación. En este tema se describe el esquema de URI ms-settings. Usa este esquema de URI para iniciar la aplicación Configuración de Windows en páginas específicas de configuración."
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: e4cc17bf268ddb470c3c64dfe3e471053d8fca55
ms.lasthandoff: 02/07/2017

---

# <a name="launch-the-windows-settings-app"></a>Iniciar la aplicación de configuración de Windows

\[ Actualizado para las aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**API importantes**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

Aprende a iniciar la aplicación Configuración de Windows desde la aplicación. En este tema se describe el esquema de URI **ms-settings:**. Usa este esquema de URI para iniciar la aplicación Configuración de Windows en las páginas de configuración específicas.

El inicio de la aplicación Configuración es una parte importante de la programación de una aplicación compatible con la privacidad. Si la aplicación no puede obtener acceso a un recurso con información confidencial, se recomienda proporcionar al usuario un vínculo a la configuración de privacidad de ese recurso. Para obtener más información, consulta [Directrices para aplicaciones compatibles con la privacidad](https://msdn.microsoft.com/library/windows/apps/hh768223).

## <a name="how-to-launch-the-settings-app"></a>Cómo iniciar la aplicación Configuración

Para iniciar la aplicación **Configuración**, usa el esquema de URI `ms-settings:`, como se muestra en los siguientes ejemplos.

En este ejemplo, se usa un control de hipervínculo XAML para iniciar la página de configuración de privacidad para el micrófono mediante el URI `ms-settings:privacy-microphone`.

```xml
<!--Set Visibility to Visible when access to the microphone is denied -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access the microphone. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-microphone">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the microphone privacy settings."/>
</TextBlock>
```

Como alternativa, la aplicación puede llamar al método [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) para iniciar la aplicación **Configuración** desde el código. Este ejemplo muestra cómo iniciar la página de configuración de privacidad de la cámara mediante el URI `ms-settings:privacy-webcam` .

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

El código anterior inicia la página de configuración de privacidad de la cámara:

![Configuración de privacidad de la cámara.](images/privacyawarenesssettingsapp.png)

Para obtener más información sobre el inicio de URI, consulta [Iniciar la aplicación predeterminada de un URI](launch-default-app.md).

## <a name="ms-settings-uri-scheme-reference"></a>Referencia del esquema de URI ms-settings:

Usa los siguientes URI para abrir varias páginas de la aplicación Configuración. Ten en cuenta que la columna SKU compatibles indica si la página de configuración existe en Windows 10 para las ediciones de escritorio (Home, Pro, Enterprise y Education), Windows 10 Mobile o ambos.

<table border="1">
    <tr>
        <th>Categoría</th>
        <th>Página de configuración</th>
        <th>SKU compatibles</th>
        <th>URI</th>
    </tr>
    <tr>
        <td>Página principal</td>
        <td>Página de aterrizaje de Configuración</td>
        <td>Ambos</td>
        <td>ms-settings:</td>
    </tr>
    <tr>
        <td rowspan="10">Sistema</td>
        <td>Pantalla</td>
        <td>Ambos</td>
        <td>ms-settings:screenrotation</td>
    </tr>
    <tr>
        <td>Notificaciones y acciones</td>
        <td>Ambos</td>
        <td>ms-settings:notifications</td>
    </tr>
    <tr>
        <td>Teléfono</td>
        <td>Solo para móviles</td>
        <td>ms-settings:phone</td>
    </tr>
    <tr>
        <td>Mensajes</td>
        <td>Solo para móviles</td>
        <td>ms-settings:messaging</td>
    </tr>
    <tr>
        <td>Ahorro de batería</td>
        <td>Ambos<br>Solo está disponible en dispositivos que tengan batería, como una tableta.</td>
        <td>ms-settings:batterysaver</td>
    </tr>
    <tr>
        <td>Uso de la batería</td>
        <td>Ambos<br>Solo está disponible en dispositivos que tengan batería, como una tableta.</td>
        <td>ms-settings:batterysaver-usagedetails</td>
    </tr>
    <tr>
        <td>Inicio/apagado y suspensión</td>
        <td>Solo para dispositivos de escritorio</td>
        <td>ms-settings:powersleep</td>
    </tr>
    <tr>
        <td>Acerca de</td>
        <td>Ambos</td>
        <td>ms-settings:about</td>
    </tr>
    <tr>
        <td>Cifrado</td>
        <td>Ambos</td>
        <td>ms-settings:deviceencryption</td>
    </tr>
    <tr>
        <td>Mapas sin conexión</td>
        <td>Ambos</td>
        <td>ms-settings:maps</td>
    </tr>
    <tr>
        <td rowspan="4">Dispositivos</td>
        <td>Cámara predeterminada</td>
        <td>Solo para dispositivos móviles</td>
        <td>ms-settings:camera</td>
    </tr>
    <tr>
        <td>Bluetooth</td>
        <td>Solo para dispositivos de escritorio</td>
        <td>ms-settings:bluetooth</td>
    </tr>
    <tr>
        <td>Dispositivos conectados</td>
        <td>Solo para dispositivos de escritorio</td>
        <td>ms-settings:connecteddevices</td>
    </tr>
    <tr>
        <td>Mouse y panel táctil</td>
        <td>Ambos<br>La configuración del panel táctil solo está disponible en dispositivos que disponen de panel táctil.</td>
        <td>ms-settings:mousetouchpad</td>
    </tr>
    <tr>
        <td rowspan="3">Conexión inalámbrica y de red</td>
        <td>NFC</td>
        <td>Ambos</td>
        <td>ms-settings:nfctransactions</td>
    </tr>
    <tr>
        <td>Wi-Fi</td>
        <td>Ambos</td>
        <td>ms-settings:network-wifi</td>
    </tr>
    <tr>
        <td>Modo avión</td>
        <td>Ambos</td>
        <td>ms-settings:network-airplanemode</td>
    </tr>
    <tr>
        <td rowspan="5">Redes e Internet</td>
        <td>Uso de datos</td>
        <td>Ambos</td>
        <td>ms-settings:datausage</td>
    </tr>
    <tr>
        <td>Red móvil y SIM</td>
        <td>Ambos</td>
        <td>ms-settings:network-cellular</td>
    </tr>
    <tr>
        <td>Zona con cobertura inalámbrica móvil</td>
        <td>Ambos</td>
        <td>ms-settings:network-mobilehotspot</td>
    </tr>
    <tr>
        <td>Proxy</td>
        <td>Solo para dispositivos de escritorio</td>
        <td>ms-settings:network-proxy</td>
    </tr>
    <tr>
        <td>Estado</td>
        <td>Solo para dispositivos de escritorio</td>
        <td>ms-settings:network-status</td>
    </tr>
    <tr>
        <td rowspan="5">Personalización</td>
        <td>Personalización (categoría)</td>
        <td>Ambos</td>
        <td>ms-settings:personalization</td>
    </tr>
    <tr>
        <td>Segundo plano</td>
        <td>Solo para el escritorio</td>
        <td>ms-settings:personalization-background</td>
    </tr>
    <tr>
        <td>Colores</td>
        <td>Ambos</td>
        <td>ms-settings:personalization-colors</td>
    </tr>
    <tr>
        <td>Sonidos</td>
        <td>Solo para móviles </td>
        <td>ms-settings:sounds</td>
    </tr>
    <tr>
        <td>Pantalla de bloqueo</td>
        <td>Ambos</td>
        <td>ms-settings:lockscreen</td>
    </tr>
    <tr>
        <td rowspan="7">Cuentas</td>
        <td>Acceder a la red del trabajo o colegio</td>
        <td>Ambos</td>
        <td>ms-settings:workplace</td>
    </tr>
    <tr>
        <td>Cuentas de correo electrónico y aplicaciones</td>
        <td>Ambos</td>
        <td>ms-settings:emailandaccounts</td>
    </tr>
    <tr>
        <td>Familia y otras personas</td>
        <td>Ambos</td>
        <td>ms-settings:otherusers</td>
    </tr>
    <tr>
        <td>Opciones de inicio de sesión</td>
        <td>Ambos</td>
        <td>ms-settings:signinoptions</td>
    </tr>
    <tr>
        <td>Sincronizar tu configuración</td>
        <td>Ambos</td>
        <td>ms-settings:sync</td>
    </tr>
    <tr>
        <td>Otras personas</td>
        <td>Ambos</td>
        <td>ms-settings:otherusers</td>
    </tr>
    <tr>
        <td>Tu información</td>
        <td>Ambos</td>
        <td>ms-settings:yourinfo</td>
    </tr>
    <tr>
        <td rowspan="2">Hora e idioma</td>
        <td>Fecha y hora</td>
        <td>Ambos</td>
        <td>ms-settings:dateandtime</td>
    </tr>
    <tr>
        <td>Región e idioma</td>
        <td>Solo para el escritorio</td>
        <td>ms-settings:regionlanguage</td>
    </tr>
    <tr>
        <td rowspan="7">Accesibilidad</td>
        <td>Narrador</td>
        <td>Ambos</td>
        <td>ms-settings:easeofaccess-narrator</td>
    </tr>
    <tr>
        <td>Lupa</td>
        <td>Ambos</td>
        <td>ms-settings:easeofaccess-magnifier</td>
    </tr>
    <tr>
        <td>Contraste alto </td>
        <td>Ambos</td>
        <td>ms-settings:easeofaccess-highcontrast</td>
    </tr>
    <tr>
        <td>Subtítulos (CC)</td>
        <td>Ambos</td>
        <td>ms-settings:easeofaccess-closedcaptioning</td>
    </tr>
    <tr>
        <td>Teclado</td>
        <td>Ambos</td>
        <td>ms-settings:easeofaccess-keyboard</td>
    </tr>
    <tr>
        <td>Mouse</td>
        <td>Ambos</td>
        <td>ms-settings:easeofaccess-mouse</td>
    </tr>
    <tr>
        <td>Otras opciones</td>
        <td>Ambos</td>
        <td>ms-settings:easeofaccess-otheroptions</td>
    </tr>
    <tr>
        <td rowspan="15">Privacidad</td>
        <td>Ubicación</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-location</td>
    </tr>
    <tr>
        <td>Cámara</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-webcam</td>
    </tr>
    <tr>
        <td>Micrófono</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-microphone</td>
    </tr>
    <tr>
        <td>Movimiento</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-motion</td>
    </tr>
    <tr>
        <td>Voz, entrada manuscrita y escritura</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-speechtyping</td>
    </tr>
    <tr>
        <td>Información de cuenta</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-accountinfo</td>
    </tr>
    <tr>
        <td>Contactos</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-contacts</td>
    </tr>
    <tr>
        <td>Calendario</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-calendar</td>
    </tr>
    <tr>
        <td>Historial de llamadas</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-callhistory</td>
    </tr>
    <tr>
        <td>Correo electrónico</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-email</td>
    </tr>
    <tr>
        <td>Mensajes</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-messaging</td>
    </tr>
    <tr>
        <td>Señales de radio</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-radios</td>
    </tr>
    <tr>
        <td>Aplicaciones en segundo plano</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-backgroundapps</td>
    </tr>
    <tr>
        <td>Otros dispositivos</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-customdevices</td>
    </tr>
    <tr>
        <td>Comentarios y diagnósticos</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-feedback</td>
    </tr>
    <tr>
        <td>Actualización y seguridad</td>
        <td>Para desarrolladores</td>
        <td>Ambos</td>
        <td>ms-settings:developers</td>
    </tr>
</table><br/>

