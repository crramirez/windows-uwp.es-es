---
author: TylerMSFT
title: "Iniciar la aplicación Configuración de Windows"
description: "Aprende a iniciar la aplicación Configuración de Windows desde la aplicación. En este tema se describe el esquema de URI ms-settings. Usa este esquema de URI para iniciar la aplicación Configuración de Windows en las páginas de configuración específicas."
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.sourcegitcommit: 3cf9dd4ab83139a2b4b0f44a36c2e57a92900903
ms.openlocfilehash: e52a4245e8697a68bfc5c5605dc54e5ea510c662

---

# Iniciar la aplicación Configuración de Windows


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

Aprende a iniciar la aplicación Configuración de Windows desde la aplicación. En este tema se describe el esquema de URI **ms-settings:**. Usa este esquema de URI para iniciar la aplicación Configuración de Windows en las páginas de configuración específicas.

El inicio de la aplicación Configuración es una parte importante de la programación de una aplicación compatible con la privacidad. Si la aplicación no puede obtener acceso a un recurso con información confidencial, se recomienda proporcionar al usuario un vínculo a la configuración de privacidad de ese recurso. Para más información, consulta [Directrices para aplicaciones compatibles con la privacidad](https://msdn.microsoft.com/library/windows/apps/hh768223).

## Cómo iniciar la aplicación Configuración


Si la configuración de privacidad no te permite obtener acceso a un recurso con información confidencial, te recomendamos que proporciones un vínculo a la configuración de privacidad en la aplicación **Configuración**. Esto hará que para que los usuarios sea más fácil cambiar su configuración.

Para iniciar directamente la aplicación **Configuración**, usa el esquema de URI `ms-settings:`, como se muestra en los siguientes ejemplos.

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

Como alternativa, la aplicación puede llamar al método [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) para iniciar la aplicación **Configuración** desde el código.

```cs
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

Este ejemplo muestra cómo iniciar la página de configuración de privacidad de la cámara mediante el URI `ms-settings:privacy-webcam` .

![camera privacy settings.](images/privacyawarenesssettingsapp.png)

Para obtener más información sobre el inicio de URI, consulta [Iniciar la aplicación predeterminada de un URI](launch-default-app.md).

## Referencia del esquema de URI ms-settings:


Usa los siguientes URI para abrir varias páginas de la aplicación Configuración. Ten en cuenta que la columna SKU compatibles indica si la página de configuración existe en Windows 10 para las ediciones de escritorio (Home, Pro, Enterprise y Education), Windows 10 Mobile, o en ambos.

| Categoría           | Página de configuración                          | SKU compatibles | URI                                       |
|--------------------|----------------------------------------|----------------|-------------------------------------------|
| Página principal          | Página de aterrizaje de Configuración              | Ambos           | ms-settings:                              |
| Sistema             | Pantalla                                | Ambos           | ms-settings:screenrotation                |
|                    | Notificaciones y acciones                | Ambos           | ms-settings:notifications                 |
|                    | Teléfono                                  | Solo para móviles    | ms-settings:phone                         |
|                    | Mensajes                              | Solo para móviles    | ms-settings:messaging                     |
|                    | Ahorro de batería                          | Dispositivos móviles y de escritorio con batería, como una tableta    | ms-settings:batterysaver                  |
|                    | Ahorro de batería/Configuración del ahorro de batería | Dispositivos móviles y de escritorio con batería, como una tableta | ms-settings:batterysaver-settings         |
|                    | Ahorro de batería/Uso de batería            | Dispositivos móviles y de escritorio con batería, como una tableta    | ms-settings:batterysaver-usagedetails     |
|                    | Inicio/apagado y suspensión                          | Solo para dispositivos de escritorio   | ms-settings:powersleep                    |
|                    | Escritorio: Acerca de                         | Ambos           | ms-settings:deviceencryption              |
|                    |                                        |                |                                           |
|                    | Móvil: Cifrado de dispositivo              |                |                                           |
|                    | Mapas sin conexión                           | Ambos           | ms-settings:maps                          |
|                    | Acerca de                                  | Ambos           | ms-settings:about                         |
| Dispositivos            | Cámara predeterminada                         | Solo para móviles    | ms-settings:camera                        |
|                    | Bluetooth                              | Solo para el escritorio   | ms-settings:bluetooth                     |
|                    | Mouse y panel táctil                       | Ambos           | ms-settings:mousetouchpad                 |
|                    | NFC                                    | Ambos           | ms-settings:nfctransactions               |
| Conexión inalámbrica y de red | Wi-Fi                                  | Ambos           | ms-settings:network-wifi                  |
|                    | Modo avión                          | Ambos           | ms-settings:network-airplanemode          |
| Redes e Internet | Uso de datos                             | Ambos           | ms-settings:datausage                     |
|                    | Red móvil y SIM                         | Ambos           | ms-settings:network-cellular              |
|                    | Zona con cobertura inalámbrica móvil                         | Ambos           | ms-settings:network-mobilehotspot         |
|                    | Proxy                                  | Ambos           | ms-settings:network-proxy                 |
| Personalización    | Personalización (categoría)             | Ambos           | ms-settings:personalization               |
|                    | Segundo plano                             | Solo para el escritorio   | ms-settings:personalization-background    |
|                    | Colores                                 | Ambos           | ms-settings:personalization-colors        |
|                    | Sonidos                                 | Solo para móviles    | ms-settings:sounds                        |
|                    | Pantalla de bloqueo                            | Ambos           | ms-settings:lockscreen                    |
| Cuentas           | Correo electrónico y cuentas                | Ambos           | ms-settings:emailandaccounts              |
|                    | Acceso al trabajo                            | Ambos           | ms-settings:workplace                     |
|                    | Sincronizar tu configuración                     | Ambos           | ms-settings:sync                          |
| Hora e idioma  | Fecha y hora                            | Ambos           | ms-settings:dateandtime                   |
|                    | Región e idioma                      | Solo para el escritorio   | ms-settings:regionlanguage                |
| Accesibilidad     | Narrador                               | Ambos           | ms-settings:easeofaccess-narrator         |
|                    | Lupa                              | Ambos           | ms-settings:easeofaccess-magnifier        |
|                    | Contraste alto                          | Ambos           | ms-settings:easeofaccess-highcontrast     |
|                    | Subtítulos (CC)                        | Ambos           | ms-settings:easeofaccess-closedcaptioning |
|                    | Teclado                               | Ambos           | ms-settings:easeofaccess-keyboard         |
|                    | Mouse                                  | Ambos           | ms-settings:easeofaccess-mouse            |
|                    | Otras opciones                          | Ambos           | ms-settings:easeofaccess-otheroptions     |
| Privacidad            | Ubicación                               | Ambos           | ms-settings:privacy-location              |
|                    | Cámara                                 | Ambos           | ms-settings:privacy-webcam                |
|                    | Micrófono                             | Ambos           | ms-settings:privacy-microphone            |
|                    | Movimiento                                 | Ambos           | ms-settings:privacy-motion                |
|                    | Voz, entrada manuscrita y escritura                | Ambos           | ms-settings:privacy-speechtyping          |
|                    | Información de cuenta                           | Ambos           | ms-settings:privacy-accountinfo           |
|                    | Contactos                               | Ambos           | ms-settings:privacy-contacts              |
|                    | Calendario                               | Ambos           | ms-settings:privacy-calendar              |
|                    | Historial de llamadas                           | Ambos           | ms-settings:privacy-callhistory           |
|                    | Correo electrónico                                  | Ambos           | ms-settings:privacy-email                 |
|                    | Mensajes                              | Ambos           | ms-settings:privacy-messaging             |
|                    | Señales de radio                                 | Ambos           | ms-settings:privacy-radios                |
|                    | Aplicaciones en segundo plano                        | Ambos           | ms-settings:privacy-backgroundapps        |
|                    | Otros dispositivos                          | Ambos           | ms-settings:privacy-customdevices         |
|                    | Comentarios y diagnósticos                 | Ambos           | ms-settings:privacy-feedback              |
| Actualización y seguridad  | Para desarrolladores                         | Ambos           | ms-settings:developers                    |
 



<!--HONumber=Jun16_HO5-->


