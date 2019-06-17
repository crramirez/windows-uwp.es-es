---
Description: Distribuir una aplicación de escritorio empaquetada (puente de escritorio)
title: Publicar la aplicación de escritorio empaquetada en la Microsoft Store o transferir localmente en uno o varios dispositivos.
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 45d298aca60155915900f494654dce8e89fb1ee0
ms.sourcegitcommit: b9e2cd5232ad98f4ef367881b92000a3ae610844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67131903"
---
# <a name="distribute-your-packaged-desktop-app"></a>Distribuir la aplicación de escritorio empaquetada

Si decide [empaquetar la aplicación de escritorio en un paquete MSIX](/windows/msix/desktop/desktop-to-uwp-root), puede publicar la aplicación empaquetada a la Microsoft Store o transferir localmente en uno o varios dispositivos.

> [!NOTE]
> ¿Tiene un plan para cómo podría realizar la transición a los usuarios a la aplicación empaquetada? Antes de distribuir la aplicación, consulta la sección [Realizar la transición de usuarios a la aplicación empaquetada](#transition-users) de esta guía para obtener algunas ideas.

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>Distribuir la aplicación mediante la publicación en la Microsoft Store

[Microsoft Store](https://www.microsoft.com/store/apps) es la forma más cómoda para que los clientes obtengan la aplicación.

Publicar la aplicación en la Microsoft Store para llegar a la audiencia más amplia. Además, los clientes de la organización pueden adquirir la aplicación para distribuir internamente a sus organizaciones a través de la [Microsoft Store para empresas](https://www.microsoft.com/business-store).

Si vas a publicar en Microsoft Store, se te hará una serie de preguntas adicionales como parte del proceso de envío. Eso es porque el manifiesto del paquete declara una funcionalidad restringida denominada **runFullTrust**, y necesitamos aprobar el uso de esa funcionalidad en la aplicación. Puede leer más sobre este requisito aquí: [Capacidades restringidas](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities).

No se debe firmar la aplicación antes de enviarla a la Store.

>[!IMPORTANT]
> Si va a publicar la aplicación en la Microsoft Store, asegúrese de que la aplicación funciona correctamente en los dispositivos que ejecutan Windows 10 S. Este es un requisito de Store. Consulta [Probar la aplicación de Windows en Windows 10 S](/windows/msix/desktop/desktop-to-uwp-test-windows-s).

<a id="side-load" />

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>Distribuir la aplicación sin colocarlo en la Microsoft Store

Si en su lugar, podría distribuir la aplicación sin usar el Store, puede distribuir aplicaciones a uno o más dispositivos manualmente.

Esta opción puede serte de ayuda si quieres controlar mejor la experiencia de distribución o si no quieres realizar el proceso de certificación de Microsoft Store.

Para distribuir la aplicación a otros dispositivos sin colocarlo en el Store, tendrá que obtener un certificado, firmar la aplicación mediante el uso de ese certificado y, a continuación, transferir localmente su aplicación en esos dispositivos.

Puedes [crear un certificado](/windows/uwp/packaging/create-certificate-package-signing) u obtenerlo de un proveedor habitual como [Verisign](https://www.verisign.com/).

Si va a distribuir la aplicación en dispositivos que ejecutan Windows 10 S, la aplicación debe estar firmado por la Microsoft Store, por lo que tendrá que pasar por el proceso de envío Store para poder distribuir su aplicación en esos dispositivos.

Si decides crear un certificado, tienes que instalarlo en el almacén de certificados **Raíz de confianza** o **Personas de confianza** de cada dispositivo que ejecute la aplicación. Si obtienes un certificado de un proveedor habitual, no tendrás que instalar nada en otros sistemas, además de la aplicación.  

> [!IMPORTANT]
> Asegúrate de que el nombre del publicador del certificado coincide con el de la aplicación.

Para firmar la aplicación mediante un certificado, consulte [firmar un paquete de aplicación mediante SignTool](/windows/uwp/packaging/sign-app-package-using-signtool).

Para transferir localmente su aplicación en otros dispositivos, consulte [LOB transferir localmente aplicaciones de Windows 10](/windows/application-management/sideload-apps-in-windows-10).

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>Traslado de los usuarios a la aplicación empaquetada

Antes de distribuir la aplicación, es buena idea agregar algunas extensiones al manifiesto de paquete para que los usuarios se acostumbren a usar la aplicación empaquetada. Aquí te mostramos algunas cosas que puedes hacer.

* Incluir los iconos de inicio y los botones de la barra de tareas existentes en la aplicación empaquetada.
* Asociar la aplicación empaquetada con un conjunto de tipos de archivo.
* Asegúrese de abrir determinados tipos de archivos de forma predeterminada la aplicación empaquetada.

Para obtener la lista completa de las extensiones y las instrucciones que indican cómo usarlas, consulta [Transition users to your app (Realizar la transición de usuarios a la aplicación)](desktop-to-uwp-extensions.md#transition-users-to-your-app).

Además, considere la posibilidad de agregar código a la aplicación empaquetada que lleva a cabo estas tareas:

* Migra los datos de usuario asociados con su aplicación de escritorio a las ubicaciones de carpeta correspondiente de la aplicación empaquetada.
* Ofrecer a los usuarios la opción de desinstalar la versión de escritorio de la aplicación.

Hablemos un poco sobre estas tareas. Comenzaremos con la migración de datos de usuario.

### <a name="migrate-user-data"></a>Migrar datos de usuario

Si va a agregar código que migra los datos de usuario, es mejor ejecutar ese código únicamente cuando la aplicación se inicia por primera vez. Antes de migrar los datos de los usuarios, puedes mostrar un cuadro de diálogo al usuario donde se explica lo que sucede, por qué se recomienda y lo que va a suceder con sus datos.

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

Es mejor no desinstala la aplicación de escritorio de los usuarios sin solicitar su permiso. Muestra un cuadro de diálogo que le pida al usuario permiso para realizar la acción. Es posible que los usuarios decidan no desinstalar la versión de escritorio de la aplicación. Si esto sucede, tendrá que decidir si desea bloquear el uso de la aplicación de escritorio o admite el uso en paralelo de ambas aplicaciones.

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

**Proporcionar comentarios o hacer sugerencias**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
