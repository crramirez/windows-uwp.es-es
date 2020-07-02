---
Description: Distribución de una aplicación empaquetada con Puente de dispositivo de escritorio
title: Publica una aplicación de escritorio empaquetada en Microsoft Store o transferirla localmente a uno o más dispositivos.
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 7eb57e8cea83a4d45087be4c4685ada8d108fa7a
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334494"
---
# <a name="distribute-your-packaged-desktop-app"></a>Distribución de la aplicación de escritorio empaquetada

Si decides [empaquetar la aplicación de escritorio en un paquete MSIX](/windows/msix/desktop/desktop-to-uwp-root), puedes publicar la aplicación empaquetada en Microsoft Store o transferirla localmente a uno o más dispositivos.

> [!NOTE]
> ¿Tienes un plan que te permita realizar la transición de usuarios a la aplicación empaquetada? Antes de distribuir la aplicación, consulta la sección [Traslado de los usuarios a la aplicación empaquetada](#transition-users) de esta guía para obtener algunas ideas.

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>Distribución de la aplicación publicándola en Microsoft Store

[Microsoft Store](https://www.microsoft.com/store/apps) es la forma más cómoda para que los clientes obtengan la aplicación.

Publica la aplicación en Microsoft Store para llegar a un público más amplio. Asimismo, los clientes de empresas pueden comprar la aplicación para distribuirla de manera interna en sus organizaciones a través de [Microsoft Store para Empresas](https://businessstore.microsoft.com/store).

Si vas a publicar en Microsoft Store, se te hará una serie de preguntas adicionales como parte del proceso de envío. Eso es porque el manifiesto del paquete declara una funcionalidad restringida denominada **runFullTrust**, cuyo uso necesitamos aprobar en la aplicación. Puedes obtener más información al respecto aquí: [Funcionalidades restringidas](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities).

No tienes que firmar la aplicación antes de enviarla a Store.

>[!IMPORTANT]
> Si tienes previsto publicar la aplicación en Microsoft Store, debes asegurarte de que funciona correctamente en dispositivos que ejecutan Windows 10 S. Este es un requisito de Store. Consulta [Probar la aplicación de Windows en Windows 10 S](/windows/msix/desktop/desktop-to-uwp-test-windows-s).

<a id="side-load"></a>

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>Distribución de la aplicación sin enviarla a Microsoft Store

Si prefieres distribuir la aplicación sin usar Store, puedes distribuir aplicaciones en uno o más dispositivos de forma manual.

Esta opción puede serte de ayuda si quieres controlar mejor la experiencia de distribución o si no quieres realizar el proceso de certificación de Microsoft Store.

Para distribuir la aplicación a otros dispositivos sin enviarla a Store, tienes que obtener un certificado, firmar la aplicación con ese certificado y, a continuación, transferir localmente la aplicación a esos dispositivos.

Puedes [crear un certificado](/windows/msix/package/create-certificate-package-signing) u obtenerlo de un proveedor habitual como [Verisign](https://www.verisign.com/).

Microsoft Store debe firmar la aplicación si vas a distribuirla en dispositivos que ejecutan Windows 10 S. De esta manera, tendrás que realizar el proceso de envío de Store para poder distribuir la aplicación en esos dispositivos.

Si decides crear un certificado, tienes que instalarlo en el almacén de certificados **Raíz de confianza** o **Personas de confianza** de cada dispositivo que ejecute la aplicación. Si obtienes un certificado de un proveedor habitual, no tendrás que instalar nada en otros sistemas, aparte de la aplicación.  

> [!IMPORTANT]
> Asegúrate de que el nombre del editor del certificado coincide con el de la aplicación.

Para firmar la aplicación con un certificado, consulta [Firmar un paquete de la aplicación con SignTool](/windows/msix/package/sign-app-package-using-signtool).

Para transferir localmente la aplicación a otros dispositivos, consulta [Transferir localmente aplicaciones de LOB en Windows 10](/windows/application-management/sideload-apps-in-windows-10).

<a id="transition-users"></a>

## <a name="transition-users-to-your-packaged-app"></a>Traslado de los usuarios a la aplicación empaquetada

Antes de distribuir la aplicación, es buena idea agregar algunas extensiones al manifiesto de paquete para que los usuarios se acostumbren a usar la aplicación empaquetada. Aquí te mostramos algunas cosas que puedes hacer.

* Incluir los iconos de inicio y los botones de la barra de tareas existentes en la aplicación empaquetada.
* Asociar la aplicación empaquetada con un conjunto de tipos de archivo.
* Hacer que la aplicación empaquetada abra determinados tipos de archivos de manera predeterminada.

Para obtener la lista completa de las extensiones y las instrucciones que indican cómo usarlas, consulta [Proceso de transición de usuarios a la aplicación](desktop-to-uwp-extensions.md#transition-users-to-your-app).

Asimismo, puedes agregar código a la aplicación empaquetada que realiza estas tareas:

* Migrar los datos de usuario asociados a la aplicación de escritorio a las ubicaciones de carpeta correspondientes de la aplicación empaquetada.
* Ofrecer a los usuarios la opción de desinstalar la versión de escritorio de la aplicación.

Hablemos un poco sobre estas tareas. Comenzaremos con la migración de datos de usuario.

### <a name="migrate-user-data"></a>Migración de datos de usuario

Si vas a agregar código para migrar los datos de usuario, lo mejor es que ese código se ejecute solamente cuando se inicie la aplicación por primera vez. Antes de migrar los datos de los usuarios, puedes mostrar un cuadro de diálogo al usuario donde se explique lo que sucede, por qué se recomienda y lo que va a suceder con sus datos.

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

### <a name="uninstall-the-desktop-version-of-your-app"></a>Desinstalación de la versión de escritorio de la aplicación

Es mejor no desinstalar la aplicación de escritorio de los usuarios sin antes solicitar su permiso. Muestra un cuadro de diálogo que solicite al usuario permiso para realizar la acción. Es posible que los usuarios decidan no desinstalar la versión de escritorio de la aplicación. Si es así, tendrás que decidir si quieres bloquear el uso de la aplicación de escritorio o permitir el uso en paralelo de ambas aplicaciones.

Aquí tienes un ejemplo que te muestra cómo puedes llevar a cabo esta acción en una aplicación empaquetada basada en .NET.

Para ver el contexto completo de este fragmento de código, consulta el archivo **MainWindow.cs** de este [Visor de imágenes WPF con transición/migración/desinstalación](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition) de ejemplo.

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

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

Si tienes problemas al publicar la aplicación en Store, esta [entrada de blog](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/) contiene algunos consejos útiles.
