---
title: Iniciar la aplicación Configuración de Windows
description: Aprende a iniciar la aplicación Configuración de Windows desde la aplicación. En este tema se describe el esquema de URI ms-settings. Usa este esquema de URI para iniciar la aplicación Configuración de Windows en páginas específicas de configuración.
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 6387cc75047371666ac55b9fb70ae73d3e4c4d64
ms.sourcegitcommit: cc108c791842789464c38a10e5d596c9bd878871
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/20/2019
ms.locfileid: "75302669"
---
# <a name="launch-the-windows-settings-app"></a>Iniciar la aplicación Configuración de Windows

**API importantes**

-   [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync)
-   [**PreferredApplicationPackageFamilyName**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname)
-   [**DesiredRemainingView**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.desiredremainingview)

Aprende a iniciar la aplicación Configuración de Windows. En este tema, se describe el esquema de URI **ms-settings:** . Usa este esquema de URI para iniciar la aplicación Configuración de Windows en páginas específicas de configuración.

El inicio de la aplicación Configuración es una parte importante de la programación de una aplicación compatible con la privacidad. Si la aplicación no puede obtener acceso a un recurso con información confidencial, se recomienda proporcionar al usuario un vínculo a la configuración de privacidad de ese recurso. Para obtener más información, consulta [Directrices para aplicaciones compatibles con la privacidad](https://docs.microsoft.com/windows/uwp/security/index).

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

Como alternativa, la aplicación puede llamar al método [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) para iniciar la aplicación **Configuración**. Este ejemplo muestra cómo iniciar la página de configuración de privacidad de la cámara mediante el URI `ms-settings:privacy-webcam` .

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

El código anterior inicia la página de configuración de privacidad de la cámara:

![Configuración de privacidad de la cámara.](images/privacyawarenesssettingsapp.png)

Para obtener más información sobre el inicio de URI, consulta [Iniciar la aplicación predeterminada de un URI](launch-default-app.md).

## <a name="ms-settings-uri-scheme-reference"></a>Referencia del esquema de URI ms-settings:

Usa los siguientes URI para abrir varias páginas de la aplicación Configuración.

> Ten en cuenta que la disponibilidad de una página de configuración varía según la SKU de Windows. No todas las páginas de configuración disponibles en Windows 10 para escritorio están disponibles en Windows 10 Mobile y viceversa. La columna notas también captura requisitos adicionales que se deben cumplir para una página que esté disponible.

<!-- TODO: 
* ms-settings:controlcenter
* ms-settings:holographic
* ms-settings:keyboard-advanced
* ms-settings:regionlanguage-adddisplaylanguage (crashed)
* ms-settings:regionlanguage-setdisplaylanguage (crashed)
* ms-settings:signinoptions-launchpinenrollment
* ms-settings:storagecleanup
* ms-settings:update-security -->

## <a name="accounts"></a>Cuentas

|Página de configuración| URI |
|-------------|-----|
| Acceder a la red del trabajo o centro docente | ms-settings:workplace |
| Cuentas de correo electrónico y aplicaciones  | ms-settings:emailandaccounts |
| Familia y otras personas | ms-settings:otherusers |
| Configuración de un quiosco | MS-Settings: assignedaccess |
| Opciones de inicio de sesión | ms-settings:signinoptions<br>ms-settings:signinoptions-dynamiclock |
| Sincronizar la configuración | ms-settings:sync |
| Configurar Windows Hello | ms-settings:signinoptions-launchfaceenrollment<br>ms-settings:signinoptions-launchfingerprintenrollment |
| Tu información | ms-settings:yourinfo |

## <a name="apps"></a>Aplicaciones

|Página de configuración| URI |
|-------------|-----|
| Aplicaciones y características | ms-settings:appsfeatures |
| Características de la aplicación | ms-settings:appsfeatures-app (Restablecer, administrar contenido de complementos y descargable, etc. para la aplicación)|
| Aplicaciones para sitios web | ms-settings:appsforwebsites |
| Aplicaciones predeterminadas | ms-settings:defaultapps |
| Administrar características opcionales | ms-settings:optionalfeatures |
| Mapas sin conexión | ms-settings:maps<br/>MS-Settings: Maps-downloadmaps (mapas de descarga) |
| Aplicaciones de inicio | ms-settings:startupapps |
| Reproducción de vídeo | ms-settings:videoplayback |

## <a name="cortana"></a>Cortana

|Página de configuración| URI |
|-------------|-----|
| Cortana entre mis dispositivos | ms-settings:cortana-notifications |
| Más detalles | ms-settings:cortana-moredetails |
| Historial de & de permisos | ms-settings:cortana-permissions |
| Buscar ventanas | MS-Settings: Cortana-WindowsSearch |
| Hablar con Cortana | ms-settings:cortana-language<br/>MS-Settings: Cortana<br/>MS-Settings: Cortana-talktocortana |

> [!NOTE] 
> Esta sección de configuración del escritorio se llamará búsqueda cuando el equipo esté establecido en regiones en las que Cortana no esté disponible actualmente o se haya deshabilitado Cortana. En este caso, no se mostrarán las páginas específicas de Cortana (Cortana en mis dispositivos y la comunicación con Cortana). 

## <a name="devices"></a>Dispositivos

|Página de configuración| URI |
|-------------|-----|
| AutoPlay | ms-settings:autoplay |
| Bluetooth, | ms-settings:bluetooth |
| Dispositivos conectados | ms-settings:connecteddevices |
| Cámara predeterminada | MS-Settings: Camera (**en desuso en Windows 10, versión 1809 y posteriores**) |
| Ratón y panel táctil | ms-settings:mousetouchpad (la configuración del panel táctil solo está disponible en dispositivos que disponen de panel táctil) |
| Lápiz y Windows Ink | ms-settings:pen |
| Impresoras y escáneres | ms-settings:printers |
| Panel táctil | ms-settings:devices-touchpad (la configuración del panel táctil solo está disponible en dispositivos que disponen de hardware de panel táctil) |
| Escritura | ms-settings:typing |
| USB | ms-settings:usb |
| Rueda | ms-settings:wheel (solo disponible si Dial está emparejado) |
| Tu teléfono | ms-settings:mobile-devices  |

## <a name="ease-of-access"></a>Accesibilidad

|Página de configuración| URI |
|-------------|-----|
| Audio | ms-settings:easeofaccess-audio |
| Subtítulos | ms-settings:easeofaccess-closedcaptioning |
| Filtros de color | MS-Settings: easeofaccess-ColorFilter |
| Cursor & tamaño del puntero | MS-Settings: easeofaccess-cursorandpointersize |
| Pantalla | ms-settings:easeofaccess-display |
| Control ocular | ms-settings:easeofaccess-eyecontrol |
| Fuentes | ms-settings:fonts |
| Contraste alto | ms-settings:easeofaccess-highcontrast |
| Teclado | ms-settings:easeofaccess-keyboard |
| Lupa | ms-settings:easeofaccess-magnifier |
| Mouse | ms-settings:easeofaccess-mouse |
| Narrador | ms-settings:easeofaccess-narrator |
| Otras opciones | MS-Settings: easeofaccess-otheroptions (**en desuso en Windows 10, versión 1809 y posteriores**) |
| Voz | ms-settings:easeofaccess-speechrecognition |

## <a name="extras"></a>Extras

|Página de configuración| URI |
|-------------|-----|
| Extras | MS-Settings: extras (solo disponible si las "aplicaciones de configuración" están instaladas, por ejemplo, por un tercero) |

## <a name="gaming"></a>Juegos

|Página de configuración| URI |
|-------------|-----|
| Difusión | ms-settings:gaming-broadcasting |
| Barra de juegos | ms-settings:gaming-gamebar |
| Game DVR | ms-settings:gaming-gamedvr |
| Modo de juego | ms-settings:gaming-gamemode |
| Reproducir un juego en pantalla completa | ms-settings:quietmomentsgame |
| TruePlay | MS-Settings: Gaming-trueplay (**en desuso en Windows 10, versión 1809 y posteriores**) |
| Redes Xbox | ms-settings:gaming-xboxnetworking |

## <a name="home-page"></a>Página Inicio

|Página de configuración| URI |
|-------------|-----|
| Página principal de configuración | ms-settings: |

## <a name="mixed-reality"></a>Realidad mixta

> [!NOTE]
> Estas opciones solo están disponibles si está instalada la aplicación del portal de realidad mixta.

| Página de configuración | URI |
|---------------|-----|
| Audio y voz | ms-settings:holographic-audio |
| Entorno | MS-Settings: Privacy-Holographic-Environment |
| Pantalla de auriculares | MS-Settings: Holographic-auriculares |
| Desinstalar | MS-Settings: Holographic-Management |

## <a name="network--internet"></a>Redes e Internet

|Página de configuración| URI |
|-------------|-----|
| Modo avión | ms-settings:network-airplanemode<br/>ms-settings:proximity |
| Red móvil y SIM | ms-settings:network-cellular |
| Uso de datos | ms-settings:datausage |
| Marcación | ms-settings:network-dialup |
| DirectAccess | ms-settings:network-directaccess (solo está disponible si se habilita DirectAccess) |
| Ethernet | ms-settings:network-ethernet |
| Administrar redes conocidas | ms-settings:network-wifisettings |
| Zona con cobertura inalámbrica móvil | ms-settings:network-mobilehotspot |
| NFC | ms-settings:nfctransactions |
| Proxy | ms-settings:network-proxy |
| Estado | ms-settings:network-status<br/>MS-Settings: red |
| VPN | ms-settings:network-vpn |
| Wi-Fi | ms-settings:network-wifi (solo está disponible si el dispositivo tiene un adaptador Wi-Fi) |
| Llamada por Wi-Fi | ms-settings:network-wificalling (solo está disponible si se habilitan las llamadas por Wi-Fi) |

## <a name="personalization"></a>Personalización

|Página de configuración| URI |
|-------------|-----|
| Background | ms-settings:personalization-background |
| Elegir las carpetas que aparecen en Inicio | ms-settings:personalization-start-places |
| Colores | ms-settings:personalization-colors<br/>MS-Settings: colores |
| Resumen | MS-Settings: vista de la personalización (**en desuso en Windows 10, versión 1809 y posteriores**) |
| Pantalla de bloqueo | ms-settings:lockscreen |
| Barra de navegación | MS-Settings: personalización-barra de navegación (**desusado en Windows 10, versión 1809 y posteriores**) |
| Personalización (categoría) | ms-settings:personalization |
| Inicio | ms-settings:personalization-start |
| Barra de tareas | ms-settings:taskbar |
| Temas | ms-settings:themes |

## <a name="phone"></a>Teléfono

|Página de configuración| URI |
|-------------|-----|
| Tu teléfono | ms-settings:mobile-devices<br/>MS-Settings: Mobile-Devices-addphone<br/>MS-Settings: Mobile-Devices-addphone-Direct (abre la aplicación de **teléfono** ) |

## <a name="privacy"></a>Privacidad

|Página de configuración| URI |
|-------------|-----|
| Aplicaciones para accesorios | MS-Settings: Privacy-accessoryapps (**en desuso en Windows 10, versión 1809 y posteriores**) |
| Información de cuenta | ms-settings:privacy-accountinfo |
| Historial de actividades | ms-settings:privacy-activityhistory |
| Id. de publicidad | MS-Settings: Privacy-advertisingid (**en desuso en Windows 10, versión 1809 y posteriores**) |
| Diagnósticos de aplicaciones | ms-settings:privacy-appdiagnostics |
| Descargas automáticas de archivos | ms-settings:privacy-automaticfiledownloads |
| Aplicaciones en segundo plano | ms-settings:privacy-backgroundapps |
| Calendario | ms-settings:privacy-calendar |
| Historial de llamadas | ms-settings:privacy-callhistory |
| Cámara | ms-settings:privacy-webcam |
| Contactos | ms-settings:privacy-contacts |
| Documentos | ms-settings:privacy-documents |
| Correo electrónico | ms-settings:privacy-email |
| Seguidor de ojos | ms-settings:privacy-eyetracker (requiere hardware de seguidor de ojos) |
| Comentarios y diagnósticos | ms-settings:privacy-feedback |
| Sistema de archivos | ms-settings:privacy-broadfilesystemaccess |
| General, | ms-settings:privacy-general |
| Ubicación | ms-settings:privacy-location |
| Mensajes | ms-settings:privacy-messaging |
| Micrófono | ms-settings:privacy-microphone |
| Movimiento | ms-settings:privacy-motion |
| Notificaciones | ms-settings:privacy-notifications |
| Otros dispositivos | ms-settings:privacy-customdevices |
| Imágenes | ms-settings:privacy-pictures |
| Llamadas de teléfono | MS-Settings: Privacy-phonecalls |
| Señales de radio | ms-settings:privacy-radios |
| Voz, entrada manuscrita y escritura |ms-settings:privacy-speechtyping |
| Tareas | ms-settings:privacy-tasks |
| Vídeos | ms-settings:privacy-videos |
| Activación de voz | MS-Settings: Privacy-voiceactivation |

## <a name="surface-hub"></a>Surface Hub

|Página de configuración| URI |
|-------------|-----|
| Cuentas | ms-settings:surfacehub-accounts |
| Liberador de sesiones | ms-settings:surfacehub-sessioncleanup |
| Reuniones de equipo | ms-settings:surfacehub-calling |
| Administración de dispositivos de equipo | ms-settings:surfacehub-devicemanagenent |
| Pantalla de inicio de sesión | ms-settings:surfacehub-welcome |

## <a name="system"></a>Sistema

|Página de configuración| URI |
|-------------|-----|
| Acerca de | ms-settings:about |
| Configuración avanzada de pantalla | ms-settings:display-advanced (solo está disponible en dispositivos que admiten opciones avanzadas de pantalla) |
| Preferencias de dispositivo y volumen de la aplicación | MS-Settings: apps-Volume (**agregado en Windows 10, versión 1903**)|
| Ahorro de batería | ms-settings:batterysaver (solo está disponible en dispositivos que tengan batería, como una tableta) |
| Configuración de ahorro de batería | ms-settings:batterysaver-settings (solo está disponible en dispositivos que tengan batería, como una tableta) |
| Uso de la batería | ms-settings:batterysaver-usagedetails (solo está disponible en dispositivos que tengan batería, como una tableta) |
| Portapapeles | MS-Settings: Portapapeles |
| Pantalla | ms-settings:display |
| Ubicaciones de guardado predeterminadas | ms-settings:savelocations |
| Pantalla | ms-settings:screenrotation |
| Duplicar mi pantalla | ms-settings:quietmomentspresentation |
| Durante estas horas | ms-settings:quietmomentsscheduled |
| Cifrado | ms-settings:deviceencryption |
| Asistente de concentración | ms-settings:quiethours <br> ms-settings:quietmomentshome |
| Configuración de gráficos | ms-settings:display-advancedgraphics (solo está disponible en dispositivos que admiten opciones gráficas avanzadas) |
| Mensajes | ms-settings:messaging |
| Multitarea | ms-settings:multitasking |
| Ajustes de luz nocturna | ms-settings:nightlight |
| Teléfono | ms-settings:phone-defaultapps |
| Proyectar en este PC | ms-settings:project |
| Experiencias compartidas | ms-settings:crossdevice |
| Modo tableta | ms-settings:tabletmode |
| Barra de tareas | ms-settings:taskbar |
| Notificaciones y acciones | ms-settings:notifications |
| Escritorio remoto | ms-settings:remotedesktop |
| Teléfono | MS-Settings: Phone (**en desuso en Windows 10, versión 1809 y posteriores**) |
| Inicio/apagado y suspensión | ms-settings:powersleep |
| Sonido | MS-Settings: sonido |
| Almacenamiento | ms-settings:storagesense |
| Sensor de almacenamiento | ms-settings:storagepolicies |

## <a name="time-and-language"></a>Hora e idioma

|Página de configuración| URI |
|-------------|-----|
| Fecha y hora | ms-settings:dateandtime |
| Configuración de IME de Japón | ms-settings:regionlanguage-jpnime (disponible si está instalado el Editor de métodos de entrada de Japón de Microsoft) |
| Región | MS-Settings: regionformatting |
| Idioma | MS-Settings: teclado<br/>ms-settings:regionlanguage<br/>MS-Settings: regionlanguage-bpmfime<br/>MS-Settings: regionlanguage-cangjieime<br/>MS-Settings: regionlanguage-chsime-pinyin-domainlexicon<br/>MS-Settings: regionlanguage-chsime-pinyin-KeyConfig<br/>MS-Settings: regionlanguage-chsime-pinyin-UDP<br/>MS-Settings: regionlanguage-chsime-Wubi-UDP<br/>MS-Settings: regionlanguage-quickime |
| Configuración IME de Pinyin | ms-settings:regionlanguage-chsime-pinyin (disponible si está instalado el Editor de métodos de entrada de Pinyin de Microsoft) |
| Voz | ms-settings:speech |
| Configuración IME de Wubi  | ms-settings:regionlanguage-chsime-wubi (disponible si está instalado el Editor de métodos de entrada de Wubi de Microsoft) |

## <a name="update--security"></a>Actualización y seguridad

|Página de configuración| URI |
|-------------|-----|
| Activación | ms-settings:activation |
| Copia de seguridad | ms-settings:backup |
| Optimización de distribución | ms-settings:delivery-optimization |
| Encuentra mi dispositivo | ms-settings:findmydevice |
| Para desarrolladores | ms-settings:developers |
| Recuperación | ms-settings:recovery |
| Solucionar problemas | ms-settings:troubleshoot |
| Seguridad de Windows | ms-settings:windowsdefender |
| Programa Windows Insider | ms-settings:windowsinsider (solo está disponible si el usuario se inscribe en WIP)<br/>MS-Settings: windowsinsider-optin |
| Windows Update | ms-settings:windowsupdate<br>ms-settings:windowsupdate-action |
| Windows Update: opciones avanzadas | ms-settings:windowsupdate-options |
| Windows Update: opciones de reinicio | ms-settings:windowsupdate-restartoptions |
| Windows Update: ver el historial de actualizaciones | ms-settings:windowsupdate-history |

## <a name="user--accounts"></a>Cuentas de usuario

|Página de configuración| URI |
|-------------|-----|
| Aprovisionamiento | ms-settings:workplace-provisioning (solo disponible si la empresa ha implementado un paquete de aprovisionamiento) |
| Aprovisionamiento | ms-settings:provisioning (solo disponible en dispositivos móviles y si la empresa ha implementado un paquete de aprovisionamiento) |
| Windows Anywhere | ms-settings:windowsanywhere (el dispositivo debe admitir Windows Anywhere) |
