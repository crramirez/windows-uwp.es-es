---
title: Cómo iniciar la aplicación Configuración de Windows
description: Obtenga información sobre cómo iniciar la aplicación de configuración de Windows desde la aplicación mediante el esquema de URI de MS-Settings.
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.date: 11/18/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: contperfq2
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 309568b80d51cc8bd6cd2394317ef3bb8266a212
ms.sourcegitcommit: 2a23972e9a0807256954d6da5cf21d0bbe7afb0a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2020
ms.locfileid: "94941821"
---
# <a name="launch-the-windows-settings-app"></a>Cómo iniciar la aplicación Configuración de Windows

**API importantes**

-   [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync)
-   [**PreferredApplicationPackageFamilyName**](/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname)
-   [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview)

Aprende a iniciar la aplicación Configuración de Windows. En este tema se describe el esquema de **MS-Settings:** URI. Usa este esquema de URI para iniciar la aplicación Configuración de Windows en páginas específicas de configuración.

El inicio de la aplicación Configuración es una parte importante de la programación de una aplicación compatible con la privacidad. Si la aplicación no puede obtener acceso a un recurso con información confidencial, se recomienda proporcionar al usuario un vínculo a la configuración de privacidad de ese recurso. Para obtener más información, consulta [Directrices para aplicaciones compatibles con la privacidad](../security/index.md).

## <a name="how-to-launch-the-settings-app"></a>Cómo iniciar la aplicación Configuración

Para iniciar la aplicación **Configuración**, usa el esquema de URI `ms-settings:`, como se muestra en los siguientes ejemplos.

### <a name="xaml-hyperlink-control"></a>Control de hipervínculo XAML

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

### <a name="calling-launchuriasync"></a>Llamar a LaunchUriAsync

Como alternativa, la aplicación puede llamar al método [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) para iniciar la aplicación de **configuración** . Este ejemplo muestra cómo iniciar la página de configuración de privacidad de la cámara mediante el URI `ms-settings:privacy-webcam` .

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

```cppwinrt
bool result = co_await Windows::System::Launcher::LaunchUriAsync(Windows::Foundation::Uri(L"ms-settings:privacy-webcam"));
```

El código anterior inicia la página de configuración de privacidad de la cámara:

:::image type="content" source="images/privacyawarenesssettingsapp.png" alt-text="Configuración de privacidad de la cámara.":::

Para obtener más información sobre el inicio de URI, consulta [Iniciar la aplicación predeterminada de un URI](launch-default-app.md).

## <a name="ms-settings-uri-scheme-reference"></a>Referencia del esquema de URI ms-settings:

En las secciones siguientes se describen las distintas categorías de URI de configuración de MS que se usan para abrir varias páginas de la aplicación de configuración:

* [Cuentas](#accounts)
* [Aplicaciones](#apps)
* [Cortana](#cortana)
* [Dispositivos](#devices)
* [Accesibilidad](#ease-of-access)
* [Extras](#extras)
* [Juegos](#gaming)
* [Página de inicio](#home-page)
* [Realidad mixta](#mixed-reality)
* [Red e Internet](#network-and-internet)
* [Personalización](#personalization)
* [Número](#phone)
* [Privacidad](#privacy)
* [Surface Hub](#surface-hub)
* [Sistema](#system)
* [Hora e idioma](#time-and-language)
* [Actualización y seguridad](#update-and-security)
* [Cuentas de usuario](#user-accounts)



> [!NOTE]
> El hecho de que una página de configuración esté disponible varía según la SKU de Windows. No toda la página de configuración disponible en Windows 10 para escritorio está disponible en Windows 10 Mobile y viceversa. La columna notas también captura requisitos adicionales que se deben cumplir para que una página esté disponible.

<!-- TODO: 
* ms-settings:controlcenter
* ms-settings:holographic
* ms-settings:keyboard-advanced
* ms-settings:regionlanguage-adddisplaylanguage (crashed)
* ms-settings:regionlanguage-setdisplaylanguage (crashed)
* ms-settings:signinoptions-launchpinenrollment
* ms-settings:storagecleanup
* ms-settings:update-security -->

### <a name="accounts"></a>Cuentas

|Página de configuración| Identificador URI |
|-------------|-----|
| Obtener acceso a trabajo o escuela | ms-settings:workplace |
| Cuentas de correos y aplicaciones  | ms-settings:emailandaccounts |
| Familia y otras personas | ms-settings:otherusers |
| Configuración de un quiosco | MS-Settings: assignedaccess |
| Opciones de inicio de sesión | ms-settings:signinoptions<br>MS-Settings: signinoptions-dynamiclock |
| Sincronizar la configuración | ms-settings:sync |
| Configuración de Windows Hello | MS-Settings: signinoptions-launchfaceenrollment<br>MS-Settings: signinoptions-launchfingerprintenrollment |
| Tu información | ms-settings:yourinfo |

### <a name="apps"></a>Aplicaciones

|Página de configuración| Identificador URI |
|-------------|-----|
| Aplicaciones y características | MS-Settings: appsfeatures |
| Funciones de la aplicación | MS-Settings: appsfeatures-App (restablecer, administrar complemento & contenido descargable, etc.)|
| Aplicaciones para sitios web | MS-Settings: appsforwebsites |
| Aplicaciones predeterminadas | MS-Settings: defaultapps |
| Administrar características opcionales | MS-Settings: optionalfeatures |
| Mapas sin conexión | ms-settings:maps<br/>MS-Settings: Maps-downloadmaps (mapas de descarga) |
| Aplicaciones de inicio | MS-Settings: startupapps |
| Reproducción de vídeo | MS-Settings: videoreproducción |

### <a name="cortana"></a>Cortana

|Página de configuración| Identificador URI |
|-------------|-----|
| Cortana entre mis dispositivos | MS-Settings: Cortana-notifications |
| Más detalles | MS-Settings: Cortana-moredetails |
| Historial de & de permisos | MS-Settings: Cortana-permisos |
| Buscar ventanas | MS-Settings: Cortana-WindowsSearch |
| Hablar con Cortana | MS-Settings: Cortana-Language<br/>MS-Settings: Cortana<br/>MS-Settings: Cortana-talktocortana |

> [!NOTE] 
> Esta sección de configuración del escritorio se llamará búsqueda cuando el equipo esté establecido en regiones en las que Cortana no esté disponible actualmente o se haya deshabilitado Cortana. En este caso, no se mostrarán las páginas específicas de Cortana (Cortana en mis dispositivos y la comunicación con Cortana). 

### <a name="devices"></a>Dispositivos

|Página de configuración| Identificador URI |
|-------------|-----|
| Reproducción automática | MS-Settings: reproducción automática |
| Bluetooth | ms-settings:bluetooth |
| Dispositivos conectados | ms-settings:connecteddevices |
| Cámara predeterminada | MS-Settings: Camera (**en desuso en Windows 10, versión 1809 y posteriores**) |
| Mouse y panel táctil | MS-Settings: mousetouchpad (la configuración de Touchpad solo está disponible en los dispositivos que tienen un touchpad) |
| Lápiz & Windows Ink | MS-Settings: Pen |
| Impresoras & escáneres | MS-Settings: impresoras |
| Panel táctil | MS-Settings: dispositivos-Touchpad (solo disponible si el hardware de Touchpad está presente) |
| Escritura | MS-Settings: escribir |
| USB | MS-Settings: USB |
| Rueda | MS-Settings: rueda (solo disponible si se empareja el dial) |
| Tu teléfono | MS-Settings: dispositivos móviles  |

### <a name="ease-of-access"></a>Facilidad de acceso

|Página de configuración| Identificador URI |
|-------------|-----|
| Audio | MS-Settings: easeofaccess-audio |
| Subtítulos | ms-settings:easeofaccess-closedcaptioning |
| Filtros de color | MS-Settings: easeofaccess-ColorFilter |
| Tamaño de puntero y cursor | MS-Settings: easeofaccess-cursorandpointersize |
| Pantalla | MS-Settings: easeofaccess-display |
| Control ocular | MS-Settings: easeofaccess-eyecontrol |
| Fuentes | MS-Settings: fuentes |
| Contraste alto | ms-settings:easeofaccess-highcontrast |
| Teclado | ms-settings:easeofaccess-keyboard |
| Lupa | ms-settings:easeofaccess-magnifier |
| Mouse | ms-settings:easeofaccess-mouse |
| Narrador | ms-settings:easeofaccess-narrator |
| Otras opciones | MS-Settings: easeofaccess-otheroptions (**en desuso en Windows 10, versión 1809 y posteriores**) |
| Voz | MS-Settings: easeofaccess-speechrecognition |

### <a name="extras"></a>Extras

|Página de configuración| Identificador URI |
|-------------|-----|
| Extras | MS-Settings: extras (solo disponible si las "aplicaciones de configuración" están instaladas, por ejemplo, por un tercero) |

### <a name="gaming"></a>Juegos

|Página de configuración| Identificador URI |
|-------------|-----|
| Difusión | MS-Settings: juegos-difusión |
| Barra de juego | MS-Settings: Gaming-Gamebar |
| Game DVR | MS-Settings: Gaming-gamedvr |
| Modo Juego | MS-Settings: Gaming-gamemode |
| Reproducir una pantalla completa de juego | MS-Settings: quietmomentsgame |
| TruePlay | MS-Settings: Gaming-trueplay (**a partir de Windows 10, versión 1809 (10,0; Compilación 17763), esta característica se ha quitado de Windows**) |
| Redes Xbox | MS-Settings: Gaming-xboxnetworking |

### <a name="home-page"></a>Página de inicio

|Página de configuración| Identificador URI |
|-------------|-----|
| Página principal de configuración | ms-settings: |

### <a name="mixed-reality"></a>Realidad mixta

> [!NOTE]
> Estas opciones solo están disponibles si está instalada la aplicación del portal de realidad mixta.

| Página de configuración | Identificador URI |
|---------------|-----|
| Audio y voz | MS-Settings: Holographic-audio |
| Entorno | MS-Settings: Privacy-Holographic-Environment |
| Pantalla del casco | MS-Settings: Holographic-auriculares |
| Desinstalar | MS-Settings: Holographic-Management |

### <a name="network-and-internet"></a>Red e Internet

|Página de configuración| Identificador URI |
|-------------|-----|
| Modo avión | ms-settings:network-airplanemode<br/>ms-settings:proximity |
| Red móvil y SIM | ms-settings:network-cellular |
| Uso de datos | ms-settings:datausage |
| Acceso telefónico | ms-settings:network-dialup |
| DirectAccess | MS-Settings: Network-DirectAccess (solo disponible si DirectAccess está habilitado) |
| Ethernet | ms-settings:network-ethernet |
| Administrar redes conocidas | MS-Settings: Network-wifisettings |
| Zona con cobertura inalámbrica móvil | ms-settings:network-mobilehotspot |
| NFC | ms-settings:nfctransactions |
| Proxy | ms-settings:network-proxy |
| Status | ms-settings:network-status<br/>MS-Settings: red |
| VPN | MS-Settings: red-VPN |
| Wi-Fi | MS-Settings: red-WiFi (solo disponible si el dispositivo tiene un adaptador WiFi) |
| Llamada Wi-Fi | MS-Settings: Network-wificalling (solo disponible si la llamada a Wi-Fi está habilitada) |

### <a name="personalization"></a>Personalización

|Página de configuración| Identificador URI |
|-------------|-----|
| Información previa | ms-settings:personalization-background |
| Elegir las carpetas que aparecen en el inicio | MS-Settings: personalización-Inicio-lugares |
| Colores | ms-settings:personalization-colors<br/>MS-Settings: colores |
| Observe | MS-Settings: vista de la personalización (**en desuso en Windows 10, versión 1809 y posteriores**) |
| Pantalla de bloqueo | ms-settings:lockscreen |
| Barra de navegación | MS-Settings: personalización-barra de navegación (**desusado en Windows 10, versión 1809 y posteriores**) |
| Personalización (categoría) | ms-settings:personalization |
| Inicio | MS-Settings: personalización-Inicio |
| Barra de tareas | MS-Settings: barra de tareas |
| Temas | MS-Settings: temas |

### <a name="phone"></a>Teléfono

|Página de configuración| Identificador URI |
|-------------|-----|
| Tu teléfono | MS-Settings: dispositivos móviles<br/>MS-Settings: Mobile-Devices-addphone<br/>MS-Settings: Mobile-Devices-addphone-Direct (abre la aplicación de **teléfono** ) |

### <a name="privacy"></a>Privacidad

|Página de configuración| Identificador URI |
|-------------|-----|
| Aplicaciones para accesorios | MS-Settings: Privacy-accessoryapps (**en desuso en Windows 10, versión 1809 y posteriores**) |
| Información de cuenta | ms-settings:privacy-accountinfo |
| Historial de actividad | MS-Settings: Privacy-activityhistory |
| Id. de publicidad | MS-Settings: Privacy-advertisingid (**en desuso en Windows 10, versión 1809 y posteriores**) |
| Diagnósticos de aplicación | MS-Settings: Privacy-appdiagnostics |
| Descargas automáticas de archivos | MS-Settings: Privacy-automaticfiledownloads |
| Aplicaciones en segundo plano | ms-settings:privacy-backgroundapps |
| Calendario | ms-settings:privacy-calendar |
| Historial de llamadas | ms-settings:privacy-callhistory |
| Cámara | ms-settings:privacy-webcam |
| Contactos | ms-settings:privacy-contacts |
| Documentos | MS-Settings: privacidad-documentos |
| Correo electrónico | ms-settings:privacy-email |
| Seguidor de ojos | MS-Settings: Privacy-eyetracker (requiere hardware eyetracker) |
| Comentarios y diagnósticos | ms-settings:privacy-feedback |
| Sistema de archivos | MS-Settings: Privacy-broadfilesystemaccess |
| General | MS-Settings: privacidad o MS-Settings: privacidad-general |
| Escritura de & de entrada manuscrita |ms-settings:privacy-speechtyping |
| Location | ms-settings:privacy-location |
| Mensajería | ms-settings:privacy-messaging |
| Micrófono | ms-settings:privacy-microphone |
| Movimiento | ms-settings:privacy-motion |
| Notificaciones | MS-Settings: privacidad-notificaciones |
| Otros dispositivos | ms-settings:privacy-customdevices |
| Llamadas de teléfono | MS-Settings: Privacy-phonecalls |
| Imágenes | MS-Settings: privacidad-imágenes |
| Señales de radio | ms-settings:privacy-radios |
| Voz | MS-Settings: privacidad-voz |
| Tareas | MS-Settings: privacidad-tareas |
| Vídeos | MS-Settings: privacidad-vídeos |
| Activación de voz | MS-Settings: Privacy-voiceactivation |

### <a name="surface-hub"></a>Surface Hub

|Página de configuración| Identificador URI |
|-------------|-----|
| Cuentas | MS-Settings: surfacehub-accounts |
| Limpieza de sesión | MS-Settings: surfacehub-sessioncleanup |
| Conferencia de equipo | MS-Settings: surfacehub-Calling |
| Administración de dispositivos de equipo | MS-Settings: surfacehub-devicemanagenent |
| Pantalla principal | MS-Settings: surfacehub-Welcome |

### <a name="system"></a>Sistema

|Página de configuración| Identificador URI |
|-------------|-----|
| Acerca de | ms-settings:about |
| Configuración de pantalla avanzada | MS-Settings: display-Advanced (solo disponible en dispositivos que admiten opciones de presentación avanzadas) |
| Preferencias de dispositivo y volumen de la aplicación | MS-Settings: apps-Volume (**agregado en Windows 10, versión 1903**)|
| Ahorro de batería | MS-Settings: batterysaver (solo disponible en dispositivos con batería, como una tableta) |
| Configuración del ahorro de batería | MS-Settings: batterysaver-Settings (solo disponible en dispositivos con batería, como una tableta) |
| Uso de la batería | MS-Settings: batterysaver-UsageDetails (solo disponible en dispositivos con batería, como una tableta) |
| Portapapeles | MS-Settings: Portapapeles |
| Pantalla | ms-settings:display |
| Ubicaciones de almacenamiento predeterminadas | MS-Settings: savelocations |
| Pantalla | ms-settings:screenrotation |
| Duplicar la pantalla | MS-Settings: quietmomentspresentation |
| Durante estas horas | MS-Settings: quietmomentsscheduled |
| Cifrado | ms-settings:deviceencryption |
| Ayuda para el foco | MS-Settings: quiethours <br> MS-Settings: quietmomentshome |
| Configuración de gráficos | MS-Settings: display-advancedgraphics (solo disponible en dispositivos que admiten opciones de gráficos avanzadas) |
| Mensajería | ms-settings:messaging |
| Multitarea | MS-Settings: multitarea |
| Configuración de la luz nocturna | MS-Settings: lamparilla |
| Teléfono | MS-Settings: Phone-defaultapps |
| Proyección en este PC | MS-Settings: proyecto |
| Experiencias compartidas | MS-Settings: crossdevice |
| Modo tableta | MS-Settings: tabletmode |
| Barra de tareas | MS-Settings: barra de tareas |
| Notificaciones y acciones | ms-settings:notifications |
| Escritorio remoto | MS-Settings: remotedesktop |
| Teléfono | MS-Settings: Phone (**en desuso en Windows 10, versión 1809 y posteriores**) |
| Inicio/apagado y suspensión | ms-settings:powersleep |
| Sonido | MS-Settings: sonido |
| Storage | ms-settings:storagesense |
| Sensor de almacenamiento | MS-Settings: storagepolicies |

### <a name="time-and-language"></a>Hora e idioma

|Página de configuración| Identificador URI |
|-------------|-----|
| Fecha y hora | ms-settings:dateandtime |
| Configuración de IME de Japón | MS-Settings: regionlanguage-jpnime (disponible si está instalado el editor de métodos de entrada de Microsoft Japón) |
| Region | MS-Settings: regionformatting |
| Idioma | MS-Settings: teclado<br/>ms-settings:regionlanguage<br/>MS-Settings: regionlanguage-bpmfime<br/>MS-Settings: regionlanguage-cangjieime<br/>MS-Settings: regionlanguage-chsime-pinyin-domainlexicon<br/>MS-Settings: regionlanguage-chsime-pinyin-KeyConfig<br/>MS-Settings: regionlanguage-chsime-pinyin-UDP<br/>MS-Settings: regionlanguage-chsime-Wubi-UDP<br/>MS-Settings: regionlanguage-quickime |
| Configuración del IME de pinyin | MS-Settings: regionlanguage-chsime-pinyin (disponible si está instalado el editor de métodos de entrada de Microsoft pinyin) |
| Voz | ms-settings:speech |
| Configuración del IME de Wubi  | MS-Settings: regionlanguage-chsime-Wubi (disponible si está instalado el editor de métodos de entrada Wubi de Microsoft) |

### <a name="update-and-security"></a>Actualización y seguridad

|Página de configuración| Identificador URI |
|-------------|-----|
| Activación | MS-Settings: activación |
| Backup | MS-Settings: copia de seguridad |
| Optimización de entrega | MS-Settings: optimización de entrega |
| Encontrar mi dispositivo | MS-Settings: findmydevice |
| Para desarrolladores | ms-settings:developers |
| Recuperación | MS-Settings: recuperación |
| Solución de problemas | MS-Settings: solución de problemas |
| Seguridad de Windows | MS-Settings: windowsdefender |
| Programa Windows Insider | MS-Settings: windowsinsider (solo está presente si el usuario está inscrito en WIP)<br/>MS-Settings: windowsinsider-optin |
| Windows Update | ms-settings:windowsupdate<br>MS-Settings: windowsupdate-Action |
| Opciones de Update-Advanced de Windows | MS-Settings: windowsupdate-opciones |
| Opciones de Update-Restart de Windows | MS-Settings: windowsupdate-restartoptions |
| Historial de actualizaciones de Windows Update-View | MS-Settings: windowsupdate-historial |

### <a name="user-accounts"></a>Cuentas de usuario

|Página de configuración| Identificador URI |
|-------------|-----|
| Aprovisionamiento | MS-Settings: el aprovisionamiento del área de trabajo (solo disponible si la empresa ha implementado un paquete de aprovisionamiento) |
| Aprovisionamiento | MS-Settings: aprovisionamiento (solo disponible en dispositivos móviles y si la empresa ha implementado un paquete de aprovisionamiento) |
| Windows en cualquier lugar | MS-Settings: windowsanywhere (el dispositivo debe ser compatible con Windows Anywhere) |
