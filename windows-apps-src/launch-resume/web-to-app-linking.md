---
title: Habilitación de aplicaciones para sitios web mediante controladores de URI de aplicación
description: Impulse la interacción del usuario con la aplicación al admitir la característica aplicaciones para sitios Web.
keywords: Vinculación en profundidad de Windows
ms.date: 08/25/2017
ms.topic: article
ms.assetid: 260cf387-88be-4a3d-93bc-7e4560f90abc
ms.localizationpriority: medium
ms.openlocfilehash: 5807cdc19e4b38c8cc8fa4ca45c4ef47e79b7742
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282249"
---
# <a name="enable-apps-for-websites-using-app-uri-handlers"></a>Habilitación de aplicaciones para sitios web mediante controladores de URI de aplicación

La característica Aplicaciones para sitios web asocian la aplicación con un sitio web, así que cuando alguien abre un vínculo al sitio web, la aplicación se inicia en lugar de abrir el navegador. Si la aplicación no está instalada, el sitio web se abrirá en el navegador como siempre. Los usuarios pueden confiar en esta experiencia porque solo los propietarios con contenido comprobado pueden registrarse para obtener un vínculo. Para poder comprobar todos sus vínculos entre un sitio web y la aplicación registrados, los usuarios irán a Configuración > Aplicaciones > Aplicaciones para sitios web.

Para habilitar la vinculación de web a aplicación, necesitarás:
- Identificar en el archivo de manifiesto los URI que la aplicación controlará.
- Un archivo JSON que defina la asociación entre la aplicación y el sitio web. con el Nombre de familia de paquete de la aplicación en la misma raíz de host que la declaración de manifiesto de la aplicación.
- Administrar la activación en la aplicación.

> [!Note]
> A partir de Windows 10 Creators Update, los vínculos admitidos en Microsoft Edge iniciarán la aplicación correspondiente. Los vínculos admitidos en otros exploradores (por ejemplo, Internet Explorer, etc.) le mantendrán en la experiencia de exploración.

## <a name="register-to-handle-http-and-https-links-in-the-app-manifest"></a>Registro para controlar los vínculos http y https en el manifiesto de la aplicación.

La aplicación debe identificar los URI de los sitios web que controlará. Para ello, agrega el registro de extensiones **Windows.appUriHandler** al archivo de manifiesto de la aplicación **Package.appxmanifest**.

Por ejemplo, si la dirección del sitio web es "msn.com", realizarías la siguiente entrada en el manifiesto de la aplicación:

```xml
<Applications>
  <Application ... >
      ...
      <Extensions>
         <uap3:Extension Category="windows.appUriHandler">
          <uap3:AppUriHandler>
            <uap3:Host Name="msn.com" />
          </uap3:AppUriHandler>
        </uap3:Extension>
      </Extensions>
  </Application>
</Applications>
```

La declaración anterior registra la aplicación para que controle los vínculos del host especificado. Si el sitio web tiene varias direcciones (por ejemplo: m.example.com, www\.example.com y example.com), agregue una entrada de `<uap3:Host Name=... />` independiente dentro del `<uap3:AppUriHandler>` de cada dirección.

## <a name="associate-your-app-and-website-with-a-json-file"></a>Asociar la aplicación y el sitio web con un archivo JSON

Para garantizar que solo la aplicación pueda abrir contenido de tu sitio web, incluye el nombre de familia de paquete de dicha aplicación en un archivo JSON ubicado en la raíz del servidor web o en el directorio conocido del dominio. Esto significa que tu sitio web da su consentimiento para que las aplicaciones listadas abran contenido del sitio. Puedes encontrar el nombre de familia de paquete en la sección Paquetes del diseñador de manifiestos de aplicaciones.

>[!Important]
> El archivo JSON no debe tener un sufijo de archivo .json.

Crea un archivo JSON (sin la extensión de archivo .json) denominado **windows-app-web-link** y proporciona el nombre de familia de paquete de la aplicación. Por ejemplo:

``` JSON
[{
  "packageFamilyName": "Your app's package family name, e.g MyApp_9jmtgj1pbbz6e",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 }]
```

Windows realizará una conexión https a tu sitio web y buscará el archivo JSON correspondiente en tu servidor web.

### <a name="wildcards"></a>Caracteres comodín

El ejemplo de archivo JSON anterior muestra el uso de caracteres comodín. Los caracteres comodín permiten admitir una gran variedad de vínculos con menos líneas de código. La vinculación de web a aplicación admite dos tipos de caracteres comodín en el archivo JSON:

| **Wildcard** | **Descripción**               |
|--------------|-------------------------------|
| **\***       | Representa cualquier subcadena      |
| **?**        | Representa un carácter único |

Por ejemplo, dado `"excludePaths" : [ "/news/*", "/blog/*" ]` en el ejemplo anterior, la aplicación admitirá todas las rutas de acceso que comienzan por la dirección del sitio web (por ejemplo, msn.com), **excepto** las de `/news/` y `/blog/`. se admitirá **MSN.com/Weather.html** , pero no **MSN.com/news/TopNews.html**.

### <a name="multiple-apps"></a>Varias aplicaciones

Si tienes dos aplicaciones que quieres vincular a tu sitio web, lista ambos nombres de familia de paquete de las aplicaciones en tu archivo JSON **windows-app-web-link**. Pueden admitirse ambas aplicaciones. Al usuario se le dará a elegir cuál será el vínculo predeterminado si ambas están instaladas. Si desean cambiar el vínculo predeterminado más adelante, pueden hacerlo en **Configuración > Aplicaciones para sitios web**. Los desarrolladores también pueden cambiar el archivo JSON en cualquier momento y ver los cambios ya el mismo día, pero no más tarde de ocho días después de la actualización.

``` JSON
[{
  "packageFamilyName": "Your apps's package family name, e.g MyApp_9jmtgj1pbbz6e",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 },
 {
  "packageFamilyName": "Your second app's package family name, for example, MyApp2_8jmtgj2pbbz6e",
  "paths": [ "/example/*", "/links/*" ]
 }]
```

Para proporcionar la mejor experiencia para los usuarios, usa excluir rutas para asegurarte de que el contenido solo en línea se excluye de las rutas de acceso admitidas en el archivo JSON.

Las rutas excluidas se comprueban en primer lugar y, si hay una coincidencia, se abrirá la página correspondiente con el navegador en lugar de con la aplicación designada. En el ejemplo anterior, '/News/\*' incluye todas las páginas bajo esa ruta de acceso mientras que '/News\*' (no hay ninguna barra diagonal hacia delante) incluye las rutas de acceso en ' News\*', como ' NewsLocal/', ' newsinternational/', etc.

## <a name="handle-links-on-activation-to-link-to-content"></a>Administrar vínculos en la activación para vincular con contenido

Dirígete a **App.xaml.cs** en la solución de Visual Studio de la aplicación y en **OnActivated()** agrega el control del contenido vinculado. En el siguiente ejemplo, la página que se abre en la aplicación depende de la ruta de acceso del URI:

``` CS
protected override void OnActivated(IActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
    {
        ...
    }

    // Check ActivationKind, Parse URI, and Navigate user to content
    Type deepLinkPageType = typeof(MainPage);
    if (e.Kind == ActivationKind.Protocol)
    {
        var protocolArgs = (ProtocolActivatedEventArgs)e;        
        switch (protocolArgs.Uri.AbsolutePath)
        {
            case "/":
                break;
            case "/index.html":
                break;
            case "/sports.html":
                deepLinkPageType = typeof(SportsPage);
                break;
            case "/technology.html":
                deepLinkPageType = typeof(TechnologyPage);
                break;
            case "/business.html":
                deepLinkPageType = typeof(BusinessPage);
                break;
            case "/science.html":
                deepLinkPageType = typeof(SciencePage);
                break;
        }
    }

    if (rootFrame.Content == null)
    {
        // Default navigation
        rootFrame.Navigate(deepLinkPageType, e);
    }

    // Ensure the current window is active
    Window.Current.Activate();
}
```

**Importante** Asegúrate de sustituir la lógica final `if (rootFrame.Content == null)` por `rootFrame.Navigate(deepLinkPageType, e);` como se muestra en el ejemplo anterior.

## <a name="test-it-out-local-validation-tool"></a>Prueba de la aplicación: Herramienta de validación local

Puedes probar la configuración de la aplicación y el sitio web mediante la ejecución de la herramienta de comprobador de registro de host de la aplicación, que está disponible en:

% WINDIR%\\system32\\**AppHostRegistrationVerifier. exe**

Prueba la configuración de la aplicación y el sitio web mediante la ejecución de esta herramienta con los siguientes parámetros:

**AppHostRegistrationVerifier. exe** *hostname packagefamilyname FilePath*

-   Hostname: su sitio web (por ejemplo, microsoft.com)
-   Nombre de familia de paquete (PFN): El PFN de la aplicación
-   Ruta de acceso de archivo: el archivo JSON para la validación local (por ejemplo, C:\\SomeFolder\\Windows-App-web-link)

Si la herramienta no devuelve nada, la validación funcionará en ese archivo cuando se carga. Si hay un código de error, no funcionará.

Puedes habilitar la siguiente clave de registro para forzar la coincidencia de ruta de acceso con aplicaciones de prueba como parte de la validación local:

`HKCU\Software\Classes\LocalSettings\Software\Microsoft\Windows\CurrentVersion\
AppModel\SystemAppData\YourApp\AppUriHandlers`

KeyName: `ForceValidation` valor: `1`

## <a name="test-it-web-validation"></a>Prueba de la aplicación: Validación web

Cierra la aplicación para comprobar que se activa al hacer clic en un vínculo. A continuación, copia la dirección de una de las rutas de acceso admitidas en tu sitio web. Por ejemplo, si la dirección del sitio web es "msn.com" y una de las rutas de acceso de soporte técnico es "ruta1", usaría `http://msn.com/path1`

Comprueba que la aplicación se cierra. Presiona **tecla Windows + R** para abrir el cuadro de diálogo **Ejecutar** y pega el vínculo en la ventana. Debe iniciarse la aplicación en lugar del navegador web.

Además, puedes probar tu aplicación si la inicias desde otra aplicación mediante la API [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync). Puedes usar esta API para probarla en teléfonos también.

Si quieres seguir la lógica de activación de protocolo, establece un punto de interrupción en el controlador de eventos **OnActivated**.

## <a name="appurihandlers-tips"></a>Sugerencias sobre AppUriHandlers:

- Asegúrate de especificar solo los vínculos que tu aplicación pueda controlar.
- Lista todos los hosts que admitirás.  Tenga en cuenta que www\.example.com y example.com son hosts diferentes.
- Los usuarios pueden elegir qué aplicación prefieren para controlar los sitios web en Configuración.
- El archivo JSON debe cargarse en un servidor https.
- Si necesitas cambiar las rutas de acceso que deseas admitir, puedes volver a publicar el archivo JSON sin tener que volver a publicar la aplicación. Los usuarios verán los cambios en un plazo de 1 a 8 días.
- Todas las aplicaciones transferidas localmente con AppUriHandlers contarán con vínculos validados para el host en su instalación. No es necesario cargar un archivo JSON para probar la característica.
- Esta característica funciona siempre que la aplicación sea una aplicación para UWP iniciada con [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) o una aplicación de escritorio de Windows que se inicie con [ShellExecuteEx](https://docs.microsoft.com/windows/desktop/api/shellapi/nf-shellapi-shellexecuteexa). Si la dirección URL se corresponde con un controlador de URI de aplicación registrado, se iniciará dicha aplicación en lugar del navegador.

## <a name="see-also"></a>Consulte también

[Proyecto de ejemplo de web a aplicación](https://github.com/project-rome/AppUriHandlers/tree/master/NarwhalFacts)
[windows.protocol registration](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-protocol)
[Controlar la activación de URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
[Ejemplo de inicio por asociación](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching) muestra cómo usar la API de LaunchUriAsync().
