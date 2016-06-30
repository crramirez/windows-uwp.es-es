---
title: Compartir certificados entre aplicaciones
description: "Las aplicaciones para la Plataforma universal de Windows (UWP) que necesitan una autenticación segura que vaya más allá de una combinación de id. de usuario y contraseña pueden usar certificados para la autenticación."
ms.assetid: 159BA284-9FD4-441A-BB45-A00E36A386F9
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: 2bb1b601e1ab35115c88692f6c36dccc70836541

---

# Compartir certificados entre aplicaciones


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Las aplicaciones para la Plataforma universal de Windows (UWP) que necesitan una autenticación segura que vaya más allá de una combinación de id. de usuario y contraseña pueden usar certificados para la autenticación. La autenticación de certificado ofrece un alto grado de confianza al autenticar a un usuario. En algunos casos, un grupo de servicios querrá autenticar un usuario en varias aplicaciones. En este artículo se muestra cómo puedes autenticar varias aplicaciones usando el mismo certificado, y cómo facilitar un práctico código para que un usuario importe un certificado ofrecido para obtener acceso a servicios web protegidos.

Las aplicaciones se pueden autenticar en un servicio web usando un certificado, y varias aplicaciones pueden usar un único certificado del almacén de certificados para autenticar el mismo usuario. Si un certificado no existe en el almacén, puedes agregar código a tu aplicación para importar un certificado desde un archivo PFX.

## Habilitar Microsoft Internet Information Services (IIS) y la asignación de certificados de cliente


En este artículo se usa Microsoft Internet Information Services (IIS) como ejemplo. IIS no está habilitado de forma predeterminada. Puedes habilitar IIS mediante el Panel de control.

1.  Abre el Panel de control y selecciona **Programas**.
2.  Selecciona **Activar o desactivar características de Windows**.
3.  Expande **Internet Information Services** y, después, expande **Servicios World Wide Web**. Expande **Características de desarrollo de aplicaciones** y selecciona **ASP.NET 3.5** y **ASP.NET 4.5**. Al seleccionar estas opciones automáticamente se habilita **Internet Information Services**.
4.  Haz clic en **Aceptar** para aplicar los cambios.

## Crear y publicar un servicio web protegido


1.  Inicia Microsoft Visual Studio como administrador y selecciona **Nuevo proyecto** en la página de inicio. Se necesita acceso de administrador para publicar un servicio web en un servidor IIS. En el cuadro de diálogo Nuevo proyecto, cambia el marco de trabajo a **.NET Framework 3.5**. Selecciona **Visual C#** -&gt;**Web** -&gt;**Visual Studio** -&gt;**Aplicación de servicio web de ASP.NET**. Asigna a la aplicación el nombre "FirstContosoBank". Haz clic en **Aceptar** para crear el proyecto.
2.  En el archivo **Service1.asmx.cs**, cambia el método web predeterminado **HelloWorld** por el siguiente método "Login".
    ```cs
            [WebMethod]
            public string Login()
            {
                // Verify certificate with CA
                var cert = new System.Security.Cryptography.X509Certificates.X509Certificate2(
                    this.Context.Request.ClientCertificate.Certificate);
                bool test = cert.Verify();
                return test.ToString();
            }
    ```

3.  Guarda el archivo **Service1.asmx.cs**.
4.  En el **Explorador de soluciones**, haz clic con el botón secundario en la aplicación "FirstContosoBank" y selecciona **Publicar**.
5.  En el cuadro de diálogo **Publicar web**, crea un perfil nuevo y asígnale el nombre "ContosoProfile". Haz clic en **Siguiente**.
6.  En la página siguiente, escribe el nombre de tu servidor IIS y especifica el nombre del "Sitio web predeterminado/FirstContosoBank". Haz clic en **Publicar** para publicar tu servicio web.

## Configurar el servicio web para que use la autenticación de certificados de cliente


1.  Ejecuta el **Administrador de Internet Information Services (IIS)**.
2.  Expande los sitios de tu servidor IIS. En **Sitio web predeterminado**, selecciona el nuevo servicio web "FirstContosoBank". En la sección **Acciones**, selecciona **Configuración avanzada...**.
3.  Establece **Grupo de aplicaciones** en **.NET v2.0** y haz clic en **Aceptar**.
4.  En el **Administrador de Internet Information Services (IIS)**, selecciona tu servidor IIS y haz doble clic en **Certificados de servidor**. En la sección **Acciones**, selecciona **Crear certificado autofirmado...**. Escribe "ContosoBank" como nombre descriptivo para el certificado y haz clic en **Aceptar**. Esto creará un nuevo certificado para que lo use el servidor IIS, con el formato "&lt;nombre_de_servidor&gt;.&lt;nombre_de_dominio&gt;".
5.  En el **Administrador de Internet Information Services (IIS)**, selecciona el sitio web predeterminado. En la sección **Acciones**, selecciona **Enlace** y haz clic en **Agregar...**. Selecciona "https" como tipo, establece el puerto en "443" y especifica el nombre de host completo del servidor IIS ("&lt;nombre_de_servidor&gt;.&lt;nombre_de_dominio&gt;"). Establece el certificado SSL en "ContosoBank". Haz clic en **Aceptar**. Haz clic en **Cerrar** en la ventana **Enlaces de sitios**.
6.  En el **Administrador de Internet Information Services (IIS)**, selecciona el servicio web "FirstContosoBank". Haz doble clic en **Configuración de SSL**. Activa **Requerir SSL**. En **Certificados de cliente**, selecciona **Requerir**. En la sección **Acciones**, haz clic en **Aplicar**.
7.  Para comprobar que el servicio web está correctamente configurado, abre el explorador web y escribe la siguiente dirección web: "https://&lt;nombre_de_servidor&gt;.&lt;nombre_de_dominio&gt;/FirstContosoBank/Service1.asmx". Por ejemplo, "https://myserver.example.com/FirstContosoBank/Service1.asmx". Si tu servicio web está correctamente configurado, se te pedirá que selecciones un certificado de cliente para poder acceder al servicio web.

Puedes repetir los pasos anteriores para crear varios servicios web a los que se pueda acceder con el mismo certificado de cliente.

## Crear una aplicación de la Tienda Windows que use autenticación de certificado


Ahora que tienes uno o varios servicios web protegidos, tus aplicaciones pueden usar certificados para autenticar en esos servicios web. Cuando se realiza una solicitud a un servicio web autenticado usando el objeto [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639), la solicitud inicial no contendrá un certificado de cliente. El servicio web autenticado responderá con una solicitud para autenticar el cliente. Cuando esto ocurre, el cliente de Windows consultará automáticamente los certificados de cliente disponibles en el almacén de certificados. El usuario puede seleccionar uno de estos certificados para autenticarse en el servicio web. Algunos certificados están protegidos por contraseña, por lo que necesitarás proporcionar al usuario una manera de especificar la contraseña del certificado.

Si no hay certificados de cliente disponibles, el usuario tendrá que agregar un certificado al almacén de certificados. En tu aplicación puedes incluir código que permita al usuario seleccionar un archivo PFX que contenga un certificado de cliente y, después, importar ese certificado al almacén de certificados de cliente.

**Sugerencia** Puedes usar makecert.exe para crear un archivo PFX para usarlo con este inicio rápido. Para obtener información sobre cómo usar makecert.exe, consulta [MakeCert.](https://msdn.microsoft.com/library/windows/desktop/aa386968)

 

1.  Abre Visual Studio y crea un proyecto nuevo en la página de inicio. Asigna el nombre "FirstContosoBankApp" al nuevo proyecto. Haz clic en **Aceptar** para crear el nuevo proyecto.
2.  En el archivo MainPage.xaml, agrega el siguiente código XAML en el elemento **Cuadrícula** predeterminado. Este XAML incluye un botón para buscar el archivo PFX que se va a importar, un cuadro de texto para especificar una contraseña para un archivo PFX protegido por contraseña, un botón para importar un archivo PFX seleccionado, un botón para iniciar sesión en el servicio web protegido, y un bloque de texto para mostrar el estado de la acción actual.
    ```xml
    <Button x:Name="Import" Content="Import Certificate (PFX file)" HorizontalAlignment="Left" Margin="352,305,0,0" VerticalAlignment="Top" Height="77" Width="260" Click="Import_Click" FontSize="16"/>
    <Button x:Name="Login" Content="Login" HorizontalAlignment="Left" Margin="611,305,0,0" VerticalAlignment="Top" Height="75" Width="240" Click="Login_Click" FontSize="16"/>
    <TextBlock x:Name="Result" HorizontalAlignment="Left" Margin="355,398,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Height="153" Width="560"/>
    <PasswordBox x:Name="PfxPassword" HorizontalAlignment="Left" Margin="483,271,0,0" VerticalAlignment="Top" Width="229"/>
    <TextBlock HorizontalAlignment="Left" Margin="355,271,0,0" TextWrapping="Wrap" Text="PFX password" VerticalAlignment="Top" FontSize="18" Height="32" Width="123"/>
    <Button x:Name="Browse" Content="Browse for PFX file" HorizontalAlignment="Left" Margin="352,189,0,0" VerticalAlignment="Top" Click="Browse_Click" Width="499" Height="68" FontSize="16"/>
    <TextBlock HorizontalAlignment="Left" Margin="717,271,0,0" TextWrapping="Wrap" Text="(Optional)" VerticalAlignment="Top" Height="32" Width="83" FontSize="16"/>
    ```
    
3.  Guarda el archivo MainPage.xaml.
4.  En el archivo MainPage.xaml.cs, agrega las siguientes instrucciones using:
    ```cs
    using Windows.Web.Http;
    using System.Text;
    using Windows.Security.Cryptography.Certificates;
    using Windows.Storage.Pickers;
    using Windows.Storage;
    using Windows.Storage.Streams;
    ```

5.  En el archivo MainPage.xaml.cs, agrega las siguientes variables a la clase **MainPage**. Especifican la dirección del método "Login" protegido de tu servicio web "FirstContosoBank" y una variable global que guarda un certificado PFX para importar en el almacén de certificados. Actualiza el &lt;nombre_de_servidor&gt; al nombre completo de tu servidor Microsoft Internet Information Server (IIS).
    ```cs
    private Uri requestUri = new Uri("https://<server-name>/FirstContosoBank/Service1.asmx?op=Login");
    private string pfxCert = null;
    ```

6.  En el archivo MainPage.xaml.cs, agrega el siguiente controlador de clic para el botón de inicio de sesión y el método para obtener acceso al servicio web protegido.
    ```cs
    private void Login_Click(object sender, RoutedEventArgs e)
    {
        MakeHttpsCall();
    }

    private async void MakeHttpsCall()
    {

        StringBuilder result = new StringBuilder("Login ");
        HttpResponseMessage response;
        try
        {
            Windows.Web.Http.HttpClient httpClient = new Windows.Web.Http.HttpClient();
            response = await httpClient.GetAsync(requestUri);
            if (response.StatusCode == HttpStatusCode.Ok)
            {
                result.Append("successful");
            }
            else
            {
                result = result.Append("failed with ");
                result = result.Append(response.StatusCode);
            }
        }
        catch (Exception ex)
        {
            result = result.Append("failed with ");
            result = result.Append(ex.Message);
        }

        Result.Text = result.ToString();
    }
    ```

7.  En el archivo MainPage.xaml.cs, agrega los siguientes controladores de clic para el botón para buscar un archivo PFX y el botón para importar un archivo PFX seleccionado al almacén de certificados.
    ```cs
    private async void Import_Click(object sender, RoutedEventArgs e)
    {
        try
        {
            Result.Text = "Importing selected certificate into user certificate store....";
            await CertificateEnrollmentManager.UserCertificateEnrollmentManager.ImportPfxDataAsync(
                pfxCert,
                PfxPassword.Password,
                ExportOption.Exportable,
                KeyProtectionLevel.NoConsent,
                InstallOptions.DeleteExpired,
                "Import Pfx");

            Result.Text = "Certificate import succeded";
        }
        catch (Exception ex)
        {
            Result.Text = "Certificate import failed with " + ex.Message;
        }
    }

    private async void Browse_Click(object sender, RoutedEventArgs e)
    {

        StringBuilder result = new StringBuilder("Pfx file selection ");
        FileOpenPicker pfxFilePicker = new FileOpenPicker();
        pfxFilePicker.FileTypeFilter.Add(".pfx");
        pfxFilePicker.CommitButtonText = "Open";
        try
        {
            StorageFile pfxFile = await pfxFilePicker.PickSingleFileAsync();
            if (pfxFile != null)
            {
                IBuffer buffer = await FileIO.ReadBufferAsync(pfxFile);
                using (DataReader dataReader = DataReader.FromBuffer(buffer))
                {
                    byte[] bytes = new byte[buffer.Length];
                    dataReader.ReadBytes(bytes);
                    pfxCert = System.Convert.ToBase64String(bytes);
                    PfxPassword.Password = string.Empty;
                    result.Append("succeeded");
                }
            }
            else
            {
                result.Append("failed");
            }
        }
        catch (Exception ex)
        {
            result.Append("failed with ");
            result.Append(ex.Message); ;
        }

        Result.Text = result.ToString();
    }
    ```

8.  Ejecuta tu aplicación e inicia sesión en tu servicio web protegido, e importa además un archivo PFX al almacén de certificados local.

Puedes usar estos pasos para crear varias aplicaciones que usen el mismo certificado de usuario para obtener acceso a servicios web protegidos iguales o diferentes.


<!--HONumber=Jun16_HO4-->


