---
author: TylerMSFT
title: "Iniciar la aplicación Configuración de Windows"
description: "Aprende a iniciar la aplicación Configuración de Windows desde la aplicación. En este tema se describe el esquema de URI ms-settings. Usa este esquema de URI para iniciar la aplicación Configuración de Windows en páginas específicas de configuración."
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.author: twhitney
ms.date: 06/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 2a30e14f7c275c48f52253157fc9c67bf05d259e
ms.sourcegitcommit: 00c3f5a1208bd0125f5b275f972cf2a82d8eb9b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2017
---
# <a name="launch-the-windows-settings-app"></a>Iniciar la aplicación de configuración de Windows

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

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

Usa los siguientes URI para abrir varias páginas de la aplicación Configuración.

> Ten en cuenta que la disponibilidad de una página de configuración varía según la SKU de Windows. No todas las páginas de configuración disponibles en Windows 10 para escritorio están disponibles en Windows 10 Mobile y viceversa. La columna notas también captura requisitos adicionales que se deben cumplir para una página que esté disponible.

<table border="1">
 <tr>
  <th>Categoría</th>
  <th>Página Configuración</th>
  <th>URI</th>
  <th>Notas</th>
 </tr>
 <tr>
  <td rowspan="6">Cuentas</td>
  <td>Acceder a la red del trabajo o centro docente</td>
  <td>ms-settings:workplace</td>
  <td></td>
 </tr>
 <tr>
  <td>Cuentas de correo electrónico y aplicaciones</td>
  <td>ms-settings:emailandaccounts</td>
  <td></td>
 </tr>
 <tr>
  <td>Familia y otras personas</td>
  <td>ms-settings:otherusers</td>
  <td></td>
 </tr>
 <tr>
  <td>Opciones de inicio de sesión</td>
  <td>ms-settings:signinoptions</td>
  <td></td>
 </tr>
 <tr>
  <td>Sincronizar tu configuración</td>
  <td>ms-settings:sync</td>
  <td></td>
 </tr>
 <tr>
  <td>Tu información</td>
  <td>ms-settings:yourinfo</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="4">Aplicaciones</td>
  <td>Aplicaciones y funciones</td>
  <td>ms-settings:appsfeatures</td>
  <td></td>
 </tr>
 <tr>
  <td>Aplicaciones para sitios web</td>
  <td>ms-settings:appsforwebsites</td>
  <td></td>
 </tr>
 <tr>
  <td>Aplicaciones predeterminadas</td>
  <td>ms-settings:defaultapps</td>
  <td></td>
 </tr>
 <tr>
  <td>Aplicaciones y funciones</td>
  <td>ms-settings:optionalfeatures</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="12">Dispositivos</td>
  <td>USB</td>
  <td>ms-settings:usb</td>
  <td></td>
 </tr>
 <tr>
  <td>Audio y voz</td>
  <td>ms-settings:holographic-audio</td>
  <td>Solo disponible si la aplicación del Portal de realidad mixta está instalada (disponible en la Tienda Windows)</td>
 </tr>
 <tr>
  <td>AutoPlay</td>
  <td>ms-settings:autoplay</td>
  <td></td>
 </tr>
 <tr>
  <td>Panel táctil</td>
  <td>ms-settings:devices-touchpad</td>
  <td>Solo disponible si hay hardware de panel táctil</td>
 </tr>
 <tr>
  <td>Lápiz y Windows Ink</td>
  <td>ms-settings:pen</td>
  <td></td>
 </tr>
 <tr>
  <td>Impresoras y escáneres</td>
  <td>ms-settings:printers</td>
  <td></td>
 </tr>
 <tr>
  <td>Escritura</td>
  <td>ms-settings:typing</td>
  <td></td>
 </tr>
 <tr>
  <td>Rueda</td>
  <td>ms-settings:wheel</td>
  <td>Solo disponible si Dial está emparejado</td>
 </tr>
 <tr>
  <td>Cámara predeterminada</td>
  <td>ms-settings:camera</td>
  <td></td>
 </tr>
 <tr>
  <td>Bluetooth</td>
  <td>ms-settings:bluetooth</td>
  <td></td>
 </tr>
 <tr>
  <td>Dispositivos conectados</td>
  <td>ms-settings:connecteddevices</td>
  <td></td>
 </tr>
 <tr>
  <td>Ratón y panel táctil</td>
  <td>ms-settings:mousetouchpad</td>
  <td>La configuración del panel táctil solo está disponible en dispositivos que disponen de panel táctil.</td>
 </tr>
 <tr>
  <td rowspan="7">Accesibilidad</td>
  <td>Narrador</td>
  <td>ms-settings:easeofaccess-narrator</td>
  <td></td>
 </tr>
 <tr>
  <td>Lupa</td>
  <td>ms-settings:easeofaccess-magnifier</td>
  <td></td>
 </tr>
 <tr>
  <td>Contraste alto</td>
  <td>ms-settings:easeofaccess-highcontrast</td>
  <td></td>
 </tr>
 <tr>
  <td>Subtítulos</td>
  <td>ms-settings:easeofaccess-closedcaptioning</td>
  <td></td>
 </tr>
 <tr>
  <td>Teclado</td>
  <td>ms-settings:easeofaccess-keyboard</td>
  <td></td>
 </tr>
 <tr>
  <td>Ratón</td>
  <td>ms-settings:easeofaccess-mouse</td>
  <td></td>
 </tr>
 <tr>
  <td>Otras opciones</td>
  <td>ms-settings:easeofaccess-otheroptions</td>
 </tr>
 <tr>
  <td>Extras</td>
  <td>Extras</td>
  <td>ms-settings:extras</td>
  <td>Solo disponible si se instalan las "aplicaciones de configuración" (por ejemplo, de un tercero)</td>
 </tr>
 <tr>
  <td rowspan="4">Juegos</td>
  <td>Difusión</td>
  <td>ms-settings:gaming-broadcasting</td>
  <td></td>
 </tr>
 <tr>
  <td>Barra de juegos</td>
  <td>ms-settings:gaming-gamebar</td>
  <td></td>
 </tr>
 <tr>
  <td>Game DVR</td>
  <td>ms-settings:gaming-gamedvr</td>
  <td></td>
 </tr>
 <tr>
  <td>Modo de juego</td>
  <td>ms-settings:gaming-gamemode</td>
  <td></td>
 </tr>
 <tr>
  <td>Página Inicio</td>
  <td>Página inicial de Configuración</td>
  <td>ms-settings:</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="10">Redes e Internet</td>
  <td>Ethernet</td>
  <td>ms-settings:network-ethernet</td>
  <td></td>
 </tr>
 <tr>
  <td>VPN</td>
  <td>ms-settings:network-vpn</td>
  <td></td>
 </tr>
 <tr>
  <td>Marcación</td>
  <td>ms-settings:network-dialup</td>
  <td></td>
 </tr>
 <tr>
  <td>DirectAccess</td>
  <td>ms-settings:network-directaccess</td>
  <td>Solo disponible si se habilita DirectAccess</td>
 </tr>
 <tr>
  <td>Llamada por Wi-Fi</td>
  <td>ms-settings:network-wificalling</td>
  <td>Solo disponible si se habilitan las llamadas por Wi-Fi</td>
 </tr>
 <tr>
  <td>Uso de datos</td>
  <td>ms-settings:datausage</td>
  <td></td>
 </tr>
 <tr>
  <td>Red móvil y SIM</td>
  <td>ms-settings:network-cellular</td>
  <td></td>
 </tr>
 <tr>
  <td>Zona con cobertura inalámbrica móvil</td>
  <td>ms-settings:network-mobilehotspot</td>
  <td></td>
 </tr>
 <tr>
  <td>Proxy</td>
  <td>ms-settings:network-proxy</td>
  <td></td>
 </tr>
 <tr>
  <td>Estado</td>
  <td>ms-settings:network-status</td>
  <td></td>
 </tr>
 <tr>
  <td>Administrar redes conocidas</td>
  <td>ms-settings:network-wifisettings</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="3">Conexión inalámbrica y de red</td>
  <td>NFC</td>
  <td>ms-settings:nfctransactions</td>
  <td></td>
 </tr>
 <tr>
  <td>Wi-Fi</td>
  <td>ms-settings:network-wifi</td>
  <td>Solo disponible si el dispositivo tiene un adaptador Wi-Fi</td>
 </tr>
 <tr>
  <td>Modo avión</td>
  <td>ms-settings:network-airplanemode</td>
  <td>Usar ms-settings:proximity en Windows 8.x</td>
 </tr>
 <tr>
  <td rowspan="10">Personalización</td>
  <td>Inicio</td>
  <td>ms-settings:personalization-start</td>
  <td></td>
 </tr>
 <tr>
  <td>Temas</td>
  <td>ms-settings:themes</td>
  <td></td>
 </tr>
 <tr>
  <td>Resumen</td>
  <td>ms-settings:personalization-glance</td>
  <td></td>
 </tr>
 <tr>
  <td>Barra de navegación</td>
  <td>ms-settings:personalization-navbar</td>
  <td></td>
 </tr>
 <tr>
  <td>Personalización (categoría)</td>
   <td>ms-settings:personalization</td>
   <td></td>
 </tr>
 <tr>
  <td>Fondo</td>
   <td>ms-settings:personalization-background</td>
   <td></td>
 </tr>
 <tr>
  <td>Colores</td>
   <td>ms-settings:personalization-colors</td>
   <td></td>
 </tr>
 <tr>
  <td>Sonidos</td>
   <td>ms-settings:sounds</td>
   <td></td>
 </tr>
 <tr>
  <td>Pantalla de bloqueo</td>
   <td>ms-settings:lockscreen</td>
   <td></td>
 </tr>
 <tr>
  <td>Barra de tareas</td>
   <td>ms-settings:taskbar</td>
   <td></td>
 </tr>
 <tr>
  <td rowspan="22">Privacidad</td>
  <td>Diagnósticos de aplicaciones</td>
   <td>ms-settings:privacy-appdiagnostics</td>
   <td></td>
 </tr>
 <tr>
  <td>Notificaciones</td>
   <td>ms-settings:privacy-notifications</td>
   <td></td>
 </tr>
 <tr>
  <td>Tareas</td>
   <td>ms-settings:privacy-tasks</td>
   <td></td>
 </tr>
 <tr>
  <td>General</td>
   <td>ms-settings:privacy-general</td>
   <td></td>
 </tr>
 <tr>
  <td>Aplicaciones para accesorios</td>
   <td>ms-settings:privacy-accessoryapps</td>
   <td></td>
 </tr>
 <tr>
  <td>Identificador de publicidad</td>
   <td>ms-settings:privacy-advertisingid</td>
   <td></td>
 </tr>
 <tr>
  <td>Llamadas de teléfono</td>
   <td>ms-settings:privacy-phonecall</td>
   <td></td>
 </tr>
 <tr>
  <td>Ubicación</td>
   <td>ms-settings:privacy-location</td>
   <td></td>
 </tr>
 <tr>
  <td>Cámara</td>
   <td>ms-settings:privacy-webcam</td>
   <td></td>
 </tr>
 <tr>
  <td>Micrófono</td>
   <td>ms-settings:privacy-microphone</td>
   <td></td>
 </tr>
 <tr>
  <td>Movimiento</td>
   <td>ms-settings:privacy-motion</td>
   <td></td>
 </tr>
 <tr>
  <td>Voz, entrada manuscrita y escritura</td>
   <td>ms-settings:privacy-speechtyping</td>
   <td></td>
 </tr>
 <tr>
  <td>Información de cuenta</td>
   <td>ms-settings:privacy-accountinfo</td>
   <td></td>
 </tr>
 <tr>
  <td>Contactos</td>
   <td>ms-settings:privacy-contacts</td>
   <td></td>
 </tr>
 <tr>
  <td>Calendario</td>
   <td>ms-settings:privacy-calendar</td>
   <td></td>
 </tr>
 <tr>
  <td>Historial de llamadas</td>
   <td>ms-settings:privacy-callhistory</td>
   <td></td>
 </tr>
 <tr>
  <td>Correo electrónico</td>
  <td>ms-settings:privacy-email</td>
  <td></td>
 </tr>
 <tr>
  <td>Mensajes</td>
    <td>ms-settings:privacy-messaging</td>
  <td></td>
 </tr>
 <tr>
  <td>Señales de radio</td>
    <td>ms-settings:privacy-radios</td>
  <td></td>
 </tr>
 <tr>
  <td>Aplicaciones en segundo plano</td>
    <td>ms-settings:privacy-backgroundapps</td>
  <td></td>
 </tr>
 <tr>
  <td>Otros dispositivos</td>
    <td>ms-settings:privacy-customdevices</td>
  <td></td>
 </tr>
 <tr>
  <td>Comentarios y diagnósticos</td>
    <td>ms-settings:privacy-feedback</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="5">Surface Hub</td>
  <td>Cuentas</td>
    <td>ms-settings:surfacehub-accounts</td>
      <td></td>
  </tr>
  <tr>
    <td>Reuniones de equipo</td>
      <td>ms-settings:surfacehub-calling</td>
      <td></td>
  </tr>
  <tr>
    <td>Administración de dispositivos de equipo</td>
      <td>ms-settings:surfacehub-devicemanagenent</td>
      <td></td>
  </tr>
  <tr>
    <td>Liberador de sesiones</td>
      <td>ms-settings:surfacehub-sessioncleanup</td>
      <td></td>
  </tr>
  <tr>
    <td>Pantalla de inicio de sesión</td>
      <td>ms-settings:surfacehub-welcome</td>
      <td></td>
  </tr>
    <td rowspan="19">Sistema</td>
    <td>Experiencias compartidas</td>
      <td>ms-settings:crossdevice</td>
    <td></td>
 </tr>
 <tr>
  <td>Pantalla</td>
    <td>ms-settings:display</td>
  <td></td>
 </tr>
 <tr>
  <td>Multitarea</td>
    <td>ms-settings:multitasking</td>
  <td></td>
 </tr>
 <tr>
  <td>Proyección en este PC</td>
    <td>ms-settings:project</td>
  <td></td>
 </tr>
 <tr>
  <td>Modo tableta</td>
    <td>ms-settings:tabletmode</td>
  <td></td>
 </tr>
 <tr>
  <td>Barra de tareas</td>
    <td>ms-settings:taskbar</td>
  <td></td>
 </tr>
 <tr>
  <td>Teléfono</td>
    <td>ms-settings:phone-defaultapps</td>
  <td></td>
 </tr>
 <tr>
  <td>Pantalla</td>
    <td>ms-settings:screenrotation</td>
  <td></td>
 </tr>
 <tr>
  <td>Notificaciones y acciones</td>
    <td>ms-settings:notifications</td>
  <td></td>
 </tr>
 <tr>
  <td>Teléfono</td>
    <td>ms-settings:phone</td>
  <td></td>
 </tr>
 <tr>
  <td>Mensajes</td>
    <td>ms-settings:messaging</td>
  <td></td>
 </tr>
 <tr>
  <td>Ahorro de batería</td>
  <td>ms-settings:batterysaver</td>
  <td>Solo disponible en dispositivos que tengan batería, como una tableta.</td>
 </tr>
 <tr>
  <td>Uso de la batería</td>
  <td>ms-settings:batterysaver-usagedetails</td>
  <td>Solo disponible en dispositivos que tengan batería, como una tableta.</td>
 </tr>
 <tr>
  <td>Inicio/apagado y suspensión</td>
  <td>ms-settings:powersleep</td>
  <td></td>
 </tr>
 <tr>
  <td>Acerca de</td>
    <td>ms-settings:about</td>
  <td></td>
 </tr>
 <tr>
  <td>Almacenamiento</td>
    <td>ms-settings:storagesense</td>
  <td></td>
 </tr>
 <tr>
  <td>Sensor de almacenamiento</td>
    <td>ms-settings:storagepolicies</td>
  <td></td>
 </tr>
 <tr>
  <td>Cifrado</td>
    <td>ms-settings:deviceencryption</td>
  <td></td>
 </tr>
 <tr>
  <td>Mapas sin conexión</td>
    <td>ms-settings:maps</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="5">Hora e idioma</td>
  <td>Fecha y hora</td>
    <td>ms-settings:dateandtime</td>
  <td></td>
 </tr>
 <tr>
  <td>Región e idioma</td>
    <td>ms-settings:regionlanguage</td>
  <td></td>
 </tr>
 <tr>
     <td>Idioma de voz</td>
     <td>ms-settings:speech</td>
     <td></td>
 </tr>
 <tr>
     <td>Teclado Pinyin</td>
     <td>ms-settings:regionlanguage-chsime-pinyin</td>
     <td>Disponible si está instalado el Editor de métodos de entrada de Pinyin de Microsoft</td>
 </tr>
 <tr>
     <td>Modo de entrada Wubi</td>
     <td>ms-settings:regionlanguage-chsime-wubi</td>
     <td>Disponible si está instalado el Editor de métodos de entrada de Wubi de Microsoft</td>
 </tr>
 <tr>
  <td rowspan="13">Actualización y seguridad</td>
  <td>Configurar Windows Hello</td>
    <td>ms-settings:signinoptions-launchfaceenrollment<br>ms-settings:signinoptions-launchfingerprintenrollment</td>
  </tr>
  <tr>
    <td>Copia de seguridad</td>
      <td>ms-settings:backup</td>
    <td></td>
 </tr>
 <tr>
  <td>Encuentra mi dispositivo</td>
    <td>ms-settings:findmydevice</td>
  <td></td>
 </tr>
 <tr>
  <td>ProgramaWindowsInsider</td>
  <td>ms-settings:windowsinsider</td>
  <td>Solo presente si el usuario está inscrito en el WIP</td>
 </tr>
 <tr>
  <td>Windows Update</td>
  <td>ms-settings:windowsupdate</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Update</td>
    <td>ms-settings:windowsupdate-history</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Update</td>
    <td>ms-settings:windowsupdate-options</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Update</td>
    <td>ms-settings:windowsupdate-restartoptions</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Update</td>
    <td>ms-settings:windowsupdate-action</td>
  <td></td>
 </tr>
 <tr>
  <td>Activación</td>
    <td>ms-settings:activation</td>
  <td></td>
 </tr>
 <tr>
  <td>Recuperación</td>
    <td>ms-settings:recovery</td>
  <td></td>
 </tr>
 <tr>
  <td>Solución de problemas</td>
    <td>ms-settings:troubleshoot</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Defender</td>
    <td>ms-settings:windowsdefender</td>
  <td></td>
 </tr>
 <tr>
  <td>Para desarrolladores</td>
    <td>ms-settings:developers</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="2">Cuentas de usuario</td>
  <td>Windows Anywhere</td>
  <td>ms-settings:windowsanywhere</td>
  <td>El dispositivo debe admitir Windows Anywhere</td>
 </tr>
 <tr>
  <td>Aprovisionamiento</td>
  <td>ms-settings:workplace-provisioning</td>
  <td>Solo disponible si la empresa ha implementado un paquete de aprovisionamiento</td>
 </tr>
</table><br/>  
