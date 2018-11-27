---
Description: Distribute a packaged desktop application (Desktop Bridge)
Search.Product: eADQiWindows 10XVcnh
title: Publicar la aplicación de escritorio empaquetada a la Microsoft Store o la instalación de prueba en uno o más dispositivos.
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.localizationpriority: medium
ms.openlocfilehash: 9fe216b70b7c10f254c0e3b7b18275a72d46215c
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "7717154"
---
# <a name="distribute-a-packaged-desktop-application"></a>Distribuir una aplicación de escritorio empaquetada

Publicar la aplicación de escritorio empaquetada a la Microsoft Store o la instalación de prueba en uno o más dispositivos.  

> [!NOTE]
> ¿Tienes un plan para la forma de transición a los usuarios a la aplicación empaquetada? Antes de distribuir la aplicación, consulta la sección [Realizar la transición de usuarios a la aplicación empaquetada](#transition-users) de esta guía para obtener algunas ideas.

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>Distribuir la aplicación publicándola en la Microsoft Store

[Microsoft Store](https://www.microsoft.com/store/apps) es la forma más cómoda para que los clientes obtengan la aplicación.

Publicar la aplicación en la Microsoft Store para llegar a un público. Asimismo, los clientes de empresas pueden comprar tu aplicación para distribuirla de manera interna en sus organizaciones mediante la [Tienda Microsoft para empresas](https://www.microsoft.com/business-store).

Si vas a publicar en Microsoft Store, se te hará una serie de preguntas adicionales como parte del proceso de envío. Eso es porque el manifiesto del paquete declara una funcionalidad restringida denominada **runFullTrust**, y necesitamos aprobar el uso de esa funcionalidad en la aplicación. Puedes leer más acerca de este requisito aquí: [Capacidades restringidas](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities).

No tienes que firmar la aplicación antes de enviarla a la tienda.

>[!IMPORTANT]
> Si vas a publicar la aplicación en la Microsoft Store, asegúrate de que la aplicación funciona correctamente en dispositivos que ejecutan Windows 10 S. Este es un requisito de Store. Consulta [Probar la aplicación de Windows en Windows 10 S](desktop-to-uwp-test-windows-s.md).

<a id="side-load" />

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>Distribuir la aplicación sin enviarla a Microsoft Store

Si prefieres distribuir la aplicación sin tener que usar la tienda, puedes manualmente distribuir aplicaciones a uno o más dispositivos.

Esta opción puede serte de ayuda si quieres controlar mejor la experiencia de distribución o si no quieres realizar el proceso de certificación de Microsoft Store.

Para distribuir la aplicación a otros dispositivos sin enviarla en la tienda, tienes que obtener un certificado, firmar la aplicación con ese certificado y, a continuación, transferir localmente la aplicación en esos dispositivos.

Puedes [crear un certificado](../packaging/create-certificate-package-signing.md) u obtenerlo de un proveedor habitual como [Verisign](https://www.verisign.com/).

Si vas a distribuir la aplicación en dispositivos que ejecutan Windows 10 S, la aplicación debe estar firmada por Microsoft Store, por lo que tendrás que pasar por el proceso de envío de la tienda antes de distribuir la aplicación en esos dispositivos.

Si decides crear un certificado, tienes que instalarlo en el almacén de certificados **Raíz de confianza** o **Personas de confianza** de cada dispositivo que ejecute la aplicación. Si obtienes un certificado de un proveedor habitual, no tendrás que instalar nada en otros sistemas, además de la aplicación.  

> [!IMPORTANT]
> Asegúrate de que el nombre del publicador del certificado coincide con el de la aplicación.

Para firmar la aplicación con un certificado, vea [firmar un paquete de aplicación con SignTool](../packaging/sign-app-package-using-signtool.md).

Para transferir localmente la aplicación a otros dispositivos, consulta [transferir localmente aplicaciones LOB en Windows 10](https://technet.microsoft.com/itpro/windows/deploy/sideload-apps-in-windows-10).

**Vídeos**

|Publicar la aplicación en Microsoft Store |Distribuir una aplicación de empresa  |
|---|---|
|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Windows-Store-Publication-3cWyG5WhD_5506218965"      width="426" height="472" allowFullScreen frameBorder="0"></iframe>|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Video-Distribution-for-Enterprise-Apps-XJ5Hd5WhD_1106218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>Traslado de los usuarios a la aplicación empaquetada

Antes de distribuir la aplicación, es buena idea agregar algunas extensiones al manifiesto de paquete para que los usuarios se acostumbren a usar la aplicación empaquetada. Aquí te mostramos algunas cosas que puedes hacer.

* Incluir los iconos de inicio y los botones de la barra de tareas existentes en la aplicación empaquetada.
* Asociar la aplicación empaquetada con un conjunto de tipos de archivo.
* Hacer que la aplicación empaquetada abra determinados tipos de archivos de manera predeterminada.

Para obtener la lista completa de las extensiones y las instrucciones que indican cómo usarlas, consulta [Transition users to your app (Realizar la transición de usuarios a la aplicación)](desktop-to-uwp-extensions.md#transition-users-to-your-app).

Asimismo, puedes agregar código a la aplicación empaquetada que te permitirá realizar estas tareas:

* Migrar los datos de usuario asociados con la aplicación de escritorio a las ubicaciones de la carpeta correspondiente de la aplicación empaquetada.
* Ofrecer a los usuarios la opción de desinstalar la versión de escritorio de la aplicación.

Hablemos un poco sobre estas tareas. Comenzaremos con la migración de datos de usuario.

### <a name="migrate-user-data"></a>Migrar datos de usuario

Si vas a agregar código que migrar los datos de usuario, es mejor para ese código se ejecute solamente cuando se inicie la aplicación. Antes de migrar los datos de los usuarios, puedes mostrar un cuadro de diálogo al usuario donde se explica lo que sucede, por qué se recomienda y lo que va a suceder con sus datos.

Aquí tienes un ejemplo que te muestra cómo puedes llevar a cabo esta acción en una aplicación empaquetada basada en .NET.

```csharp
private void MigrateUserData()
{
    String sourceDir = Environment.GetFolderPath
        (Environment.SpecialFolder.ApplicationData) + "\\AppName";

    if (sourceDir != null)
    {
        DialogResult migrateResult = MessageBox.Show
            ("Would you like to migrate your data from the previous version of this app?",
             "Data Migration", MessageBoxButtons.YesNo);

        if (migrateResult.Equals(DialogResult.Yes))
        {
            String destinationDir =
                Windows.Storage.ApplicationData.Current.LocalFolder.Path + "\\AppName";

            Process process = new Process();
            process.StartInfo.FileName = "robocopy.exe";
            process.StartInfo.Arguments = "%LOCALAPPDATA%\\AppName " + destinationDir + " /move";
            process.StartInfo.CreateNoWindow = true;
            process.Start();
            process.WaitForExit();

            if (process.ExitCode > 1)
            {
                //Migration was unsuccessful -- you can choose to block/retry/other action
            }
        }
    }
}
```

### <a name="uninstall-the-desktop-version-of-your-app"></a>Desinstalar la versión de escritorio de la aplicación

Es mejor no desinstalar la aplicación de escritorio de los usuarios sin antes solicitar su permiso. Muestra un cuadro de diálogo que le pida al usuario permiso para realizar la acción. Es posible que los usuarios decidan no desinstalar la versión de escritorio de la aplicación. Si esto sucede, tendrás que decidir si quieres bloquear el uso de la aplicación de escritorio o permitir el uso de side-by-side de ambas aplicaciones.

Aquí tienes un ejemplo que te muestra cómo puedes llevar a cabo esta acción en una aplicación empaquetada basada en .NET.

Para ver el contexto completo de este fragmento de código, consulta el archivo **MainWindow.cs** de este ejemplo [Visor de imágenes WPF con transición/migración/desinstalación](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition).

```csharp
private void RemoveDesktopApp()
{              
    //Typically, you can find your uninstall string at this location.
    String uninstallString = (String)Microsoft.Win32.Registry.GetValue
        (@"HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion" +
         @"\Uninstall\{7AD02FB8-B85E-44BC-8998-F4803BA5A0E3}\", "UninstallString", null);

    //Detect if the previous version of the Desktop application is installed.
    if (uninstallString != null)
    {
        DialogResult uninstallResult = MessageBox.Show
            ("To have the best experience, consider uninstalling the "
              + " previous version of this app. Would you like to do that now?",
              "Uninstall the previous version", MessageBoxButtons.YesNo);

        if (uninstallResult.Equals(DialogResult.Yes))
        {
                    string[] uninstallArgs = uninstallString.Split(' ');

            Process process = new Process();
            process.StartInfo.FileName = uninstallArgs[0];
            process.StartInfo.Arguments = uninstallArgs[1];
            process.StartInfo.CreateNoWindow = true;

            process.Start();
            process.WaitForExit();

            if (process.ExitCode != 0)
            {
                //Uninstallation was unsuccessful - You can choose to block the application here.
            }
        }
    }

}
```

### <a name="video"></a>Vídeo

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Transition-Taskbar-Pins-Start-Tiles-File-Type-Associations-and-Protocol-Handlers-MD5mv5WhD_2406218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="next-steps"></a>Pasos siguientes

**Encuentra respuestas a tus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

Si tienes problemas al publicar la aplicación en la Store, esta [entrada de blog](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/) contiene algunos consejos útiles.

**Enviar comentarios o realizar sugerencias acerca de las características**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
