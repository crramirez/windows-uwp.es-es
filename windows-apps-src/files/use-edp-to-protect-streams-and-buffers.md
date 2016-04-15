---
Description: 'En este tema se muestran ejemplos de las tareas de codificación necesarias para lograr algunos de los escenarios más habituales de Enterprise Data Protection (EDP) relacionados con los flujos y búferes.'
MS-HAID: 'dev\_files.use\_edp\_to\_protect\_streams\_and\_buffers'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: 'Usar Enterprise Data Protection (EDP) para proteger secuencias y búferes'
---

# Usar Enterprise Data Protection (EDP) para proteger secuencias y búferes

__Nota__ La directiva de Protección de datos de empresa (EDP) no se puede aplicar en la versión 1511 de Windows 10 (compilación 10586) o en una versión anterior.

En este tema se muestran ejemplos de las tareas de codificación necesarias para lograr algunos de los escenarios más habituales de Enterprise Data Protection (EDP) relacionados con los flujos y búferes. Para obtener una perspectiva de desarrollador completa sobre cómo se relaciona EDP con los archivos, las secuencias, el portapapeles, las redes, las tareas en segundo plano y la protección de datos con la pantalla bloqueada, consulta [Enterprise Data Protection (EDP) (Protección de datos de empresa [EDP])](../enterprise/edp-hub.md).

**Nota**  La [muestra de Protección de datos de empresa (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409) abarca muchos de los escenarios que se muestran en este tema.

## Requisitos previos


-   **Prepararse para EDP**

    Consulta [Configurar el equipo para EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP).

-   **Confirmar la compilación de una aplicación habilitada para empresas**

    Una aplicación que garantiza de forma independiente que los datos de la empresa se mantienen bajo el control administrativo de la empresa se conoce como una aplicación habilitada para empresas. Una aplicación habilitada es más eficaz, inteligente, flexible y confiable que una que no lo está. La aplicación indica al sistema que está habilitada al declarar la funcionalidad **enterpriseDataPolicy** restringida. Aun así, además de configurar una funcionalidad, hay que realizar otras operaciones para habilitar la aplicación. Para obtener más información, consulta [Aplicaciones habilitadas para empresas](../enterprise/edp-hub.md#enterprise-enlightened-apps).

-   **Comprender la programación asincrónica de las aplicaciones de la Plataforma universal de Windows (UWP)**

    Para aprender a escribir aplicaciones asincrónicas en C\# o Visual Basic, consulta [Llamar a API asincrónicas en C\# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Asynchronous programming in C++ (Programación asincrónica en C++)](https://msdn.microsoft.com/library/windows/apps/mt187334).

## Proteger una secuencia de datos a una identidad de empresa


**Nota**  Cuando se protege una secuencia o un búfer, es muy recomendable suscribirse al evento [**ProtectionPolicyManager.PolicyChanged**](https://msdn.microsoft.com/library/windows/apps/mt608411) para que tu aplicación sepa cuándo se deshabilita EDP en el dispositivo. Cuando esto sucede, debes desproteger las secuencias y los búferes. Las secuencias o búferes que dejes protegidos se pueden revocar si el usuario anula la inscripción del dispositivo de Mobile Device Management (MDM). Además, si EDP se deshabilitó en el momento de creación del recurso, esa revocación no es apropiada. La desprotección de los recursos cuando EDP está deshabilitada impide que esto suceda.



En este escenario, la aplicación tiene acceso a una secuencia desprotegida que contiene datos empresariales. Para garantizar que esta secuencia se proteja cuando se transfiere dentro y fuera del dispositivo, la aplicación protege los datos a la identidad de la empresa a la que pertenecen. Esto permite a la empresa borrar los datos, cuando sea necesario. Para determinar más adelante si se debe o no llamar a un método "unprotect" en una secuencia, la aplicación debe mantener el estado que indica si la secuencia estaba protegida, motivo por el cual la función de este código de ejemplo devuelve ese estado. Si la identidad pasada no se administra o si no se permite la aplicación para la identidad, la secuencia no estará protegida y se devolverá un estado "Desprotegido" desde la llamada a [**DataProtectionManager.ProtectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706021).

```CSharp
using Windows.Storage.Streams;
using Windows.Security.EnterpriseData;

private async System.Threading.Tasks.Task<bool> ProtectAStream
    (InMemoryRandomAccessStream unprotectedInMemoryRandomAccessStream, string identity,
    InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    IInputStream unprotectedStream = unprotectedInMemoryRandomAccessStream.GetInputStreamAt(0);
    IOutputStream protectedStream = protectedInMemoryRandomAccessStream.GetOutputStreamAt(0);

    // Protect "inputStream".
    DataProtectionInfo info = 
        await DataProtectionManager.ProtectStreamAsync(unprotectedStream, identity, protectedStream);

    // Indicate to the caller whether the stream was protected successfully. It will only be protected if
    // the identity is managed AND this app is allowed for this identity. Similar to buffers, this status
    // must be stored by the app. UnprotectStreamAsync must only be called if the stream was protected
    // successfully earlier.

    return (info.Status == DataProtectionStatus.Protected);
}
```

Para mostrar cómo usar un método como el del código de ejemplo anterior, este es un método auxiliar que lo usa para convertir una cadena en una secuencia desprotegida y luego en una secuencia protegida.

```CSharp
using Windows.Storage.Streams;

private async System.Threading.Tasks.Task<bool> ProtectStringAsStreamAsync
    (string unprotectedEnterpriseData, string identity, 
    InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    using (var unprotectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream())
    {
        using (var dataWriter = new DataWriter(unprotectedInMemoryRandomAccessStream))
        {
            dataWriter.WriteString(unprotectedEnterpriseData);
            await dataWriter.StoreAsync();
            await dataWriter.FlushAsync();
            return await this.ProtectAStream(unprotectedInMemoryRandomAccessStream,
                identity, protectedInMemoryRandomAccessStream);
        }
    }
}
```

## Recuperar el estado de una secuencia


En este escenario, la aplicación protegió anteriormente una secuencia a la que debes impedir el acceso no autorizado. Para recuperar el contenido de la secuencia cuando sea necesario, la aplicación puede comprobar el estado de la secuencia.

Ten en cuenta que el estado de una secuencia también se devuelve del objeto [**DataProtectionManager.UnprotectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706023). Esta API nunca devolverá un estado "Desprotegido", ya que requiere que el recurso de entrada esté protegido (no es posible comprobar de manera confiable que un recurso es desprotegido). Ten en cuenta si tienes código que compara el resultado con "Protegido", esto sugiere la presencia de un error de diseño. Es una indicación de que el código ha perdido la pista de si la secuencia está protegida.

```CSharp
using Windows.Storage.Streams;
using Windows.Security.EnterpriseData;

private async void CheckProtectedStreamStatus(IInputStream protectedStream)
{
    DataProtectionInfo dataProtectionInfo = 
        await DataProtectionManager.GetStreamProtectionInfoAsync(protectedStream);

    if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
    {
        // Code goes here to handle this situation. Perhaps, show UI
        // saying that the user's data has been revoked.
    }
    else if (dataProtectionInfo.Status != DataProtectionStatus.Protected)
    {
        // Code goes here to handle the unexpected protection status.
    }
}
```

## Desproteger una secuencia de datos


En este escenario, la aplicación desea desproteger una secuencia que protegía anteriormente. En este código de ejemplo, se toma una secuencia protegida (ten en cuenta que para que este proceso funcione, la secuencia debe estar protegida) y la desprotege con una llamada a [**DataProtectionManager.UnprotectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706023). Luego, lee una cadena de la secuencia y la devuelve.

```CSharp
using Windows.Storage.Streams;

private async System.Threading.Tasks.Task<string> UnprotectStreamIntoStringAsync
    (InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    using (var unprotectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream())
    {
        DataProtectionInfo dataProtectionInfo = 
            await DataProtectionManager.UnprotectStreamAsync(protectedInMemoryRandomAccessStream, 
            unprotectedInMemoryRandomAccessStream);

        using (var inputStream = unprotectedInMemoryRandomAccessStream.GetInputStreamAt(0))
        {
            using (var dataReader = new DataReader(inputStream))
            {
                await dataReader.LoadAsync((uint)unprotectedInMemoryRandomAccessStream.Size);
                return dataReader.ReadString((uint)unprotectedInMemoryRandomAccessStream.Size);
            }
        }
    }
}
```

Para mostrar cómo podrías usar los métodos auxiliares proporcionados hasta ahora, este es un controlador de eventos que toma una cadena de un cuadro de texto, escribe la cadena en una secuencia, protege la secuencia, desprotege la secuencia (si estaba protegida correctamente) y, por último, vuelve a leer cadena de la secuencia desprotegida y la muestra en la interfaz de usuario.

```CSharp
using Windows.Storage.Streams;

private async void ProtectAndThenUnprotectStream_Click(object sender, RoutedEventArgs e)
{
    var protectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream();
    bool isStreamProtected = await this.ProtectStringAsStreamAsync
        (this.enterpriseDataTextBox.Text, MainPage.IDENTITY, protectedInMemoryRandomAccessStream);

    // Only unprotect the stream if we're sure that the stream actually was protected.
    if (isStreamProtected)
    {
        this.resultDataTextBlock.Text = 
            await this.UnprotectStreamIntoStringAsync(protectedInMemoryRandomAccessStream);
    }
}
```

## Recuperar el estado de un búfer de datos estáticos


En este escenario, la aplicación protegió anteriormente un búfer al que debes impedir el acceso no autorizado. Para recuperar el contenido del búfer cuando sea necesario, la aplicación puede comprobar el estado del búfer.

Ten en cuenta que el estado de un búfer también se devuelve del objeto [**DataProtectionManager.UnprotectAsync**](https://msdn.microsoft.com/library/windows/apps/dn706022). Esta API nunca devolverá un estado "Desprotegido", ya que requiere que el recurso de entrada esté protegido.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

private async void CheckProtectedBufferStatus(IBuffer protectedData)
{
    DataProtectionInfo dataProtectionInfo = 
        await DataProtectionManager.GetProtectionInfoAsync(protectedData);

    if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
    {
        // Code goes here to handle this situation, perhaps show UI
        // saying that the user's data has been revoked.
    }
    else if (dataProtectionInfo.Status != DataProtectionStatus.Protected)
    {
        // Code goes here to handle the unexpected protection status.
    }
}
```

## Proteger datos estáticos que se recuperan de un recurso de empresa


En este escenario se trata lo mismo que en los códigos de ejemplo de secuencias, excepto que funciona con un búfer de datos. De nuevo, debes mantener el estado que indica si el búfer estaba protegido, tal como se muestra. Si la identidad pasada no se administra o si no se permite la aplicación para la identidad, el búfer no estará protegido y se devolverá un estado "Desprotegido" desde la llamada a [**DataProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn706020).

```CSharp
using Windows.Security.Cryptography;
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

...

private IBuffer data = null;
private bool isProtected = false;

void StoreBuffer(IBuffer data, bool isProtected)
{
    this.data = data;
    this.isProtected = isProtected;
}

IBuffer GetStoredBuffer(out bool isProtected)
{
    isProtected = this.isProtected;
    return this.data;
}

private string identity = "contoso.com";

private async void ProtectAndThenUnprotectBuffer_Click(object sender, RoutedEventArgs e)
{
    BinaryStringEncoding encoding = BinaryStringEncoding.Utf8;
    IBuffer inputData = CryptographicBuffer.ConvertStringToBinary
        (this.enterpriseDataTextBox.Text, encoding);
    BufferProtectUnprotectResult result =
        await DataProtectionManager.ProtectAsync(inputData, this.identity);

    // Record whether the buffer was protected successfully. It will only be protected if
    // the identity is managed AND this app is allowed for this identity. This status
    // must be stored by the app. UnprotectAsync must only be called if the buffer was
    // protected successfully earlier.
    bool isBufferProtected = 
        (result.ProtectionInfo.Status == DataProtectionStatus.Protected);
    IBuffer outputData = result.Buffer;

    // Store the data away...
    this.StoreBuffer(outputData, isBufferProtected);

    // ...and then later retrieve it.
    outputData = this.GetStoredBuffer(out isBufferProtected);

    string storedString = string.Empty;

    if (isBufferProtected)
    {
        result = await DataProtectionManager.UnprotectAsync(outputData);

        if (result.ProtectionInfo.Status != DataProtectionStatus.Unprotected)
        {
            // Code goes here to handle the situation where the buffer
            // is no longer accessible.
            return;
        }
        outputData = result.Buffer;
    }
    this.resultDataTextBlock.Text = CryptographicBuffer.ConvertBinaryToString(encoding, outputData);
}
```

## Habilitar la aplicación de directivas de interfaz de usuario según la identidad protegida de una secuencia o búfer


Cuando la aplicación está por mostrar el contenido de una secuencia o búfer protegido en su interfaz de usuario, debe habilitar la aplicación de directivas de interfaz de usuario en función de la identidad en la que se protege el recurso. Debes consultar la información de protección del recurso y habilitar la aplicación de directivas de la interfaz de usuario del sistema de la identidad recuperada.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

...

private async void EnableUIPolicyFromProtectedBuffer(IBuffer buffer)
{
    DataProtectionInfo protectionInfo = 
        await DataProtectionManager.GetProtectionInfoAsync(buffer);

    if (protectionInfo.Status != DataProtectionStatus.Protected)
    {
        // In this case, the app has lost access to the buffer
        // (ProtectedToOtherIdentity, Revoked). This must be handled.
        // &#39;Unprotected&#39; is never returned for GetProtectionInfoAsync().
        return;
    }

    ProtectionPolicyManager.TryApplyProcessUIPolicy(protectionInfo.Identity);
}

```

**Nota:**  Este artículo está orientado a desarrolladores de Windows 10 que programan aplicaciones para la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Temas relacionados


[enterprise data protection (EDP) sample (Muestra de Protección de datos de empresa [EDP])](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Espacio de nombres Windows.Security.EnterpriseData**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 





<!--HONumber=Mar16_HO5-->


