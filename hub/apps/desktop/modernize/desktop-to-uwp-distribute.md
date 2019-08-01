---
Description: Distribuir una aplicación de escritorio empaquetada (puente de escritorio)
title: Publicar la aplicación de escritorio empaquetada en el Microsoft Store o transferirla localmente a uno o varios dispositivos.
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 597a283fd28b571ed968255312059c7049f3f700
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682562"
---
# <a name="distribute-your-packaged-desktop-app"></a>Distribuir la aplicación de escritorio empaquetada

Si decide empaquetar [la aplicación de escritorio en un paquete MSIX](/windows/msix/desktop/desktop-to-uwp-root), puede publicar la aplicación empaquetada en la Microsoft Store o transferirla localmente a uno o varios dispositivos.

> [!NOTE]
> ¿Tiene un plan sobre cómo podría realizar la transición de los usuarios a la aplicación empaquetada? Antes de distribuir la aplicación, consulta la sección [Realizar la transición de usuarios a la aplicación empaquetada](#transition-users) de esta guía para obtener algunas ideas.

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>Distribuya la aplicación publicándola en el Microsoft Store

[Microsoft Store](https://www.microsoft.com/store/apps) es la forma más cómoda para que los clientes obtengan la aplicación.

Publique la aplicación en el Microsoft Store para llegar a la audiencia más amplia. Además, los clientes de la organización pueden adquirir su aplicación para distribuirla internamente a sus organizaciones a través del [Microsoft Store para la empresa](https://businessstore.microsoft.com/store).

Si vas a publicar en Microsoft Store, se te hará una serie de preguntas adicionales como parte del proceso de envío. Eso es porque el manifiesto del paquete declara una funcionalidad restringida denominada **runFullTrust**, y necesitamos aprobar el uso de esa funcionalidad en la aplicación. Puede leer más sobre este requisito aquí: [Funcionalidades restringidas](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities).

No tiene que firmar la aplicación antes de enviarla a la tienda.

>[!IMPORTANT]
> Si tiene previsto publicar la aplicación en el Microsoft Store, asegúrese de que la aplicación funciona correctamente en los dispositivos que ejecutan Windows 10 S. Se trata de un requisito de almacén. Consulta [Probar la aplicación de Windows en Windows 10 S](/windows/msix/desktop/desktop-to-uwp-test-windows-s).

<a id="side-load" />

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>Distribuya la aplicación sin colocarla en el Microsoft Store

Si prefiere distribuir la aplicación sin usar la tienda, puede distribuir manualmente las aplicaciones a uno o varios dispositivos.

Esta opción puede serte de ayuda si quieres controlar mejor la experiencia de distribución o si no quieres realizar el proceso de certificación de Microsoft Store.

Para distribuir la aplicación a otros dispositivos sin colocarla en la tienda, tiene que obtener un certificado, firmar la aplicación mediante el certificado y, a continuación, transferir localmente la aplicación a esos dispositivos.

Puedes [crear un certificado](/windows/msix/package/create-certificate-package-signing) u obtenerlo de un proveedor habitual como [Verisign](https://www.verisign.com/).

Si planea distribuir la aplicación en dispositivos que ejecutan Windows 10, la aplicación debe estar firmada por el Microsoft Store por lo que tendrá que seguir el proceso de envío de la tienda antes de distribuir la aplicación en esos dispositivos.

Si decides crear un certificado, tienes que instalarlo en el almacén de certificados **Raíz de confianza** o **Personas de confianza** de cada dispositivo que ejecute la aplicación. Si obtienes un certificado de un proveedor habitual, no tendrás que instalar nada en otros sistemas, además de la aplicación.  

> [!IMPORTANT]
> Asegúrate de que el nombre del publicador del certificado coincide con el de la aplicación.

Para firmar la aplicación mediante un certificado, consulte [firmar un paquete de aplicación mediante SignTool](/windows/msix/package/sign-app-package-using-signtool).

Para transferir localmente la aplicación a otros dispositivos, consulte [transferir localmente aplicaciones de LOB en Windows 10](/windows/application-management/sideload-apps-in-windows-10).

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>Traslado de los usuarios a la aplicación empaquetada

Antes de distribuir la aplicación, es buena idea agregar algunas extensiones al manifiesto de paquete para que los usuarios se acostumbren a usar la aplicación empaquetada. Aquí te mostramos algunas cosas que puedes hacer.

* Incluir los iconos de inicio y los botones de la barra de tareas existentes en la aplicación empaquetada.
* Asocie la aplicación empaquetada a un conjunto de tipos de archivo.
* Haga que la aplicación empaquetada Abra determinados tipos de archivos de forma predeterminada.

Para obtener la lista completa de las extensiones y las instrucciones que indican cómo usarlas, consulta [Transition users to your app (Realizar la transición de usuarios a la aplicación)](desktop-to-uwp-extensions.md#transition-users-to-your-app).

Además, considere la posibilidad de agregar código a la aplicación empaquetada que realiza estas tareas:

* Migra los datos de usuario asociados a la aplicación de escritorio a las ubicaciones de carpeta adecuadas de la aplicación empaquetada.
* Ofrecer a los usuarios la opción de desinstalar la versión de escritorio de la aplicación.

Hablemos un poco sobre estas tareas. Comenzaremos con la migración de datos de usuario.

### <a name="migrate-user-data"></a>Migrar datos de usuario

Si va a agregar código que migra los datos de usuario, es mejor ejecutar ese código solo cuando se inicie la aplicación por primera vez. Antes de migrar los datos de los usuarios, puedes mostrar un cuadro de diálogo al usuario donde se explica lo que sucede, por qué se recomienda y lo que va a suceder con sus datos.

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

Es mejor no desinstalar la aplicación de escritorio de los usuarios sin pedirle primero permiso. Muestra un cuadro de diálogo que le pida al usuario permiso para realizar la acción. Es posible que los usuarios decidan no desinstalar la versión de escritorio de la aplicación. Si esto sucede, tendrá que decidir si desea bloquear el uso de la aplicación de escritorio o admitir el uso en paralelo de ambas aplicaciones.

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

## <a name="next-steps"></a>Pasos siguientes

**Encuentre respuestas a sus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

Si tienes problemas al publicar la aplicación en la Store, esta [entrada de blog](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/) contiene algunos consejos útiles.

**Enviar comentarios o realizar sugerencias de características**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
