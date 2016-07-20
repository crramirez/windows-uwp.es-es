---
author: mcleblanc
Description: "En este tema del centro se describe un panorama completo de desarrollador sobre cómo Protección de datos de empresa (EDP) se relaciona con los archivos, los búferes, el Portapapeles, las redes, las tareas en segundo plano y la protección de datos con la pantalla bloqueada."
MS-HAID: dev\_enterprise.edp\_hub
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Enterprise Data Protection (EDP)
translationtype: Human Translation
ms.sourcegitcommit: 235a0d96c0cf86fdb16a0a6b933fc0f2bbed99f0
ms.openlocfilehash: 2cae64ff234a4fb85fd6a3e50ade3b91480b36c8

---

# Enterprise Data Protection (EDP)

> [!NOTE]
> La directiva de Enterprise Data Protection (EDP) no se puede aplicar en Windows 10, versión 1511 (compilación 10586), ni en versiones anteriores.

En este tema del centro se describe un panorama completo de desarrollador sobre cómo Protección de datos de empresa (EDP) se relaciona con los archivos, los búferes, el Portapapeles, las redes, las tareas en segundo plano y la protección de datos con la pantalla bloqueada.

Para obtener más información sobre EDP desde el punto de vista de los usuarios finales y administradores, consulta [Información general de Protección de datos de empresa (EDP)](https://technet.microsoft.com/library/dn985838(v=vs.85).aspx).

## ¿Qué es EDP?

EDP es un conjunto de características en equipos de escritorio, portátiles, tabletas y teléfonos para administración de dispositivos móviles (MDM). EDP ofrece un mayor control empresarial sobre cómo se controlan sus datos (archivos de empresa y BLOB de datos) en los dispositivos que administra la empresa.

-   Los datos de empresa se etiquetan con el cifrado. Estos se denominan "datos protegidos por la empresa" o simplemente "datos protegidos".
-   Solo las aplicaciones que permite la empresa administrativa explícitamente mediante la directiva de EDP pueden acceder a los datos protegidos por la empresa, por ejemplo, en archivos.
-   Solo las aplicaciones que se permitan explícitamente mediante la directiva de EDP pueden tener acceso a la red privada virtual (VPN) de la empresa.
-   La directiva de restricción de aplicaciones también determina cómo deben administrar las aplicaciones permitidas los datos empresariales.
-   Las restricciones basadas en directivas también se aplican en contenido empresarial intercambiado mediante el Portapapeles de Windows o mediante el contrato para contenido compartido.
-   A petición, la empresa administrativa puede revocar el acceso del dispositivo a contenido protegido, esencialmente borrando el dispositivo de los datos de la empresa y dejando a su vez los datos personales intactos.
-   Una aplicación de canal es una aplicación que descarga datos protegidos. Algunos ejemplos son aplicaciones de sincronización de archivos y el correo electrónico.

EDP mejora el [Sistema de cifrado de archivos (EFS)](http://technet.microsoft.com/library/cc700811.aspx) y [Windows Selective Wipe (Eliminación selectiva de Windows)](https://technet.microsoft.com/library/dn486874.aspx) para proporcionar más opciones de seguridad y flexibilidad. Las API de EDP nuevas te permiten crear aplicaciones que protegen y revocan el acceso al contenido empresarial, trabajan con propiedades de archivo protegidas y acceden a datos cifrados sin procesar. Además de incorporar nuevas API para proteger los archivos y las carpetas, incorpora varias API para proteger los búferes y los flujos. Además, presenta un conjunto de API que permite que las aplicaciones identifiquen la empresa y le indiquen que debe aplicar la directiva de protección de datos.

Con el fin de que la empresa administrativa pueda controlar el acceso a sus datos protegidos, la directiva de restricción de aplicaciones define una lista de aplicaciones y restricciones en esas aplicaciones. De manera predeterminada, una aplicación no puede acceder independientemente a los datos protegidos. Para obtener acceso, la aplicación debe agregarse a una lista denominada lista de aplicaciones permitidas y las aplicaciones de la lista de aplicaciones permitidas se denominan aplicaciones permitidas. En un dispositivo administrado, Windows puede restringir o auditar el acceso a los datos protegidos en el Portapapeles o mediante el contrato para contenido compartido, para que el acceso de una aplicación que no esté en la lista de aplicaciones permitidas se audite o requiera el consentimiento del usuario y, de lo contrario, se bloquee.

La empresa administrativa proporciona la directiva para EDP a un dispositivo (esto convierte el dispositivo en un "dispositivo administrado"). El aprovisionamiento de directivas puede realizarse a través de una inscripción del usuario en la administración de dispositivos móviles (MDM), si el departamento de TI lo configura manualmente o a través de otro mecanismo de distribución de directivas o de administración como, por ejemplo, System Center Configuration Manager (SCCM).

La protección de archivos EDP aprovecha las claves de Rights Management Service (RMS), si se aprovisionan, ya que estas claves pueden utilizar un perfil móvil entre dispositivos y, por lo tanto, permitir que los datos protegidos utilicen un perfil móvil. Ante la ausencia de las claves RMS, estas API recurrirán a las claves de eliminación selectiva locales y limitarán la funcionalidad de la itinerancia. Los datos que utilizan un perfil móvil con cifrado serán accesibles en Windows de nivel inferior y en dispositivos de terceros a través de aplicaciones de RMS específicas de la plataforma que proporcione Microsoft, así como con aplicaciones de terceros habilitadas para RMS.

En resumen, la empresa puede administrar los datos protegidos con las API de EDP, por lo que puedes crear la aplicación de forma que ayude a la empresa a proteger y administrar sus datos. En otras palabras, puedes crear una aplicación preparada para la empresa. Así pues, de ahora en adelante, esta guía se centrará en ayudarte a realizar todo esto.

## Configurar el equipo para EDP


Para poder desarrollar la aplicación de forma adecuada y probar cómo se comportará en la empresa, te recomendamos que configures el equipo o dispositivo con las condiciones adecuadas. Esto implica realizar unas cuantas tareas de forma habitual en el dominio de los administradores de TI.

-   Inscribe tu máquina de desarrollo en administración de dispositivos móviles (MDM).
-   Agrega tu aplicación a la lista de aplicaciones permitidas a través del [proveedor de servicios de configuración de AppLocker](https://msdn.microsoft.com/library/windows/hardware/dn920019).

La siguiente tarea consiste en crear una aplicación que tenga en cuenta (y pueda responder de forma dinámica a) la identidad administrada de la empresa en la que se ejecuta, así como la directiva de protección en vigor. Esto significa que debes "habilitar" la aplicación. Las aplicaciones que se habilitan para seguir la directiva tienen muchas más probabilidades de estar en la lista de aplicaciones permitidas para acceder a datos empresariales.

## Aplicaciones habilitadas para empresas


Una vez que la aplicación está en la lista de aplicaciones permitidas, podrá leer los datos protegidos. Asimismo, de manera predeterminada, el sistema protegerá automáticamente cualquier salida de datos de la aplicación. La protección automática es necesaria porque la empresa administrativa debe, de un modo u otro, garantizar que los datos de la empresa permanecen bajo su propio control. Sin embargo, mantener este control estricto en la aplicación es solo la manera predeterminada de lograr esto. Otra opción mejor consiste en pedir al sistema que confíe en ti lo suficiente como para que te ofrezca mayor flexibilidad y poder. Para ello, debes aumentar la inteligencia de la aplicación. Esto significa que debes ir un paso más allá de la lista de aplicaciones permitidas; significa que debes habilitar la aplicación para empresas y poder declarar que lo está.

La aplicación está habilitada si usa las técnicas que describiremos para mantener de forma autónoma los datos empresariales protegidos, independientemente de si están en reposo, en uso o en desarrollo. Una aplicación habilitada reconoce orígenes de datos empresariales y datos empresariales y los protege cuando estos llegan a la aplicación. Si está habilitada, también significa que tiene en cuenta, y cumple, la directiva EDP siempre que los datos empresariales salen de tu aplicación. Por ejemplo, no permite que el contenido vaya a un punto de conexión de red no empresarial, encapsula los datos en un formulario cifrado portátil antes de permitir que utilicen un perfil móvil y, posiblemente (según la configuración de directiva), pide confirmación al usuario antes de pegar datos empresariales en una aplicación que no está en la lista de aplicaciones permitidas. Una vez hayas habilitado la aplicación, esta anuncia al sistema que está habilitada declarando la funcionalidad restringida **enterpriseDataPolicy**. Para obtener más información sobre cómo trabajar con funcionalidades restringidas, consulta [Funcionalidades especiales y restringidas](https://msdn.microsoft.com/library/windows/apps/mt270968#special-and-restricted-capabilities).

Idealmente, todos los datos empresariales son datos protegidos que están tanto en reposo como en desarrollo. No obstante, inevitablemente, debe haber un período breve entre el que los datos empresariales se generan inicialmente y se protegen. Además, a veces, los datos empresariales pueden existir en un punto de conexión de red empresarial sin estar cifrados. Una aplicación habilitada es capaz de proteger de forma independiente dichos datos; en el caso de las aplicaciones permitidas, pero que no están habilitadas, el sistema debe imponerles la protección.

Esto se debe a que una aplicación que no está habilitada siempre se ejecuta en modo de empresa. El sistema se asegura de ello. Sin embargo, una aplicación habilitada puede moverse libremente entre el modo personal y empresarial, y según corresponda para el tipo de datos con los que trabaje en cualquier momento determinado. También es importante que una aplicación habilitada respete los datos personales y que no etiquete datos personales como datos empresariales. Es posible que una aplicación administre simultáneamente datos empresariales y personales, siempre que mantenga estos requisitos. En la sección siguiente, se muestra cómo cambiar los modos en el código.

<span id="confirming-an-identity-is-managed"/>
## Confirmar la administración de una entidad y determinar el nivel de aplicación de directivas de protección

Por lo general, la aplicación obtiene una identidad empresarial desde un recurso externo, como una dirección de correo electrónico del buzón de correo, un dominio administrado o un nombre de host de URI. Puedes llamar a [**ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync**](https://msdn.microsoft.com/library/windows/apps/dn706027) para obtener la identidad administrada, si existe, de un nombre de host del punto de conexión de red.

En este ejemplo de código, se ilustra la estructura general del comportamiento habilitado, así como la manera de determinar si realmente se administra una identidad empresarial, y la directiva que está actualmente en vigor.

```CSharp
using Windows.Security.EnterpriseData;

...

string identity = "contoso.com";

if (ProtectionPolicyManager.IsIdentityManaged(identity))
{
    EnforcementLevel enforcementLevel = ProtectionPolicyManager.GetEnforcementLevel();

    // During UI activities or network access, call ProtectionPolicyManager APIs
    // (taking the enforcement level into account) to ensure that the system
    // tags data with the identity as appropriate.

    ProtectionPolicyManager.GetForCurrentView().Identity = identity;
    // The app is now in enterprise mode.

    ProtectionPolicyManager.GetForCurrentView().Identity = string.Empty;
    // The app is back in personal mode.
}
else
{
    // No policy enforcement is done on this identity.
}
```

Como se muestra, primero debes determinar si la directiva de EDP está establecida para la identidad de la empresa. El término "administrada" es una abreviación de "administrada por una directiva de EDP". Cuando se establece la directiva de EDP para una identidad específica, [**ProtectionPolicyManager.IsIdentityManaged**](https://msdn.microsoft.com/library/windows/apps/dn705171) devuelve True para dicha identidad. Esta es la indicación para usar las API de EDP con esa identidad. Aunque las API de búfer y de archivo son un poco excepcionales, dado que funcionarán según lo previsto incluso para una entidad no administrada, ese escenario no tiene sentido. Si un dispositivo se administra, este se administra para una identidad de empresa. Si una entidad no se administra, no tiene sentido proteger los datos en esa entidad.

El siguiente paso consiste en determinar e implementar el nivel de aplicación de la directiva. Para determinar el nivel de aplicación de la directiva, llama al método [**GetEnforcementLevel**](https://msdn.microsoft.com/library/windows/apps/mt608406). Cuando se aplica una directiva de empresa en la identidad, la aplicación habilitada debe ayudar al sistema con la aplicación de directivas. Para ello, debe llamar a las API [**ProtectionPolicyManager** ](https://msdn.microsoft.com/library/windows/apps/dn705170) durante las actividades de la interfaz de usuario o los accesos a la red para asegurarse de que el sistema etiqueta las transferencias de datos con esta identidad cuando es necesario. En esta guía, se incluye más información sobre cómo interpretar el nivel de aplicación y ponerlo en práctica. El ejemplo de código también muestra cómo especificar el modo de empresa y regresar al modo personal. Para ello, se debe establecerse el valor [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) en la identidad de empresa o una cadena vacía, respectivamente. De nuevo, ten en cuenta que entrar y salir del modo de empresa solo tiene sentido con una identidad administrada.

## Características de EDP de un vistazo


**Protección de archivos y búfer**

-   La aplicación puede proteger, contener y borrar los datos asociados con una identidad de empresa.
-   Windows controla la administración de claves. Windows usa las claves RMS de la empresa cuando están disponibles para el dispositivo; de lo contrario, Windows recurre a la protección de eliminación selectiva local.

**Administración de directivas de dispositivo**

-   La aplicación puede consultar la identidad (empresa u organización) que administra el dispositivo.
-   La aplicación puede proteger a los usuarios de la divulgación involuntaria de datos mediante la asociación de una identidad con los datos en cuestión.
-   La aplicación puede proteger los recursos empresariales en la red al comprobar las conexiones de punto de conexión de red de la empresa (servidores, intervalos IP) y al asociar los datos a una entidad administrada (es decir, inscrita a MDM).
-   Las API de EDP solo funcionan con identidades administradas que tienen una directiva de EDP definida en el dispositivo. Si una identidad no se administra, las API lo indican a la aplicación, cuando es necesario.

Esta es una lista de vínculos a temas que profundizan en las API de EDP y escenarios específicos para estas áreas de características en concreto.

## Archivos

Consulta [Usar EDP para proteger archivos](../files/protect-your-enterprise-data-with-edp.md).

## Búferes y flujos

Consulta [Usar EDP para proteger flujos y búferes](../files/use-edp-to-protect-streams-and-buffers.md).

## Copiar al Portapapeles, compartir e intercambiar datos entre aplicaciones

Consulta [Usar EDP para proteger los datos empresariales que se transfieren entre aplicaciones](../app-to-app/use-edp-to-protect-enterprise-data-transferred-between-apps.md).

## Redes

Consulta [Etiquetado de conexiones de red con identidad EDP](../networking/tagging_network_connections_with_edp_identity.md).

## Protección de datos con la pantalla bloqueada (DPL) y tareas en segundo plano

Una organización puede optar por administrar una directiva de "protección de datos con la pantalla bloqueada (DPL)" segura, en la que las claves de cifrado que se necesiten para acceder a los recursos protegidos se quiten temporalmente de la memoria del dispositivo cuando este último esté bloqueado. Para preparar la aplicación para esta posibilidad, consulta la sección [Controlar los eventos de bloqueo de dispositivo y evitar dejar contenido no protegido en la memoria](#handle-lock-events) en este tema. Asimismo, si la aplicación tiene una tarea en segundo plano que debe proteger archivos, consulta [Protect enterprise data in a new file (for a background task) (Proteger datos empresariales en un nuevo archivo [para una tarea en segundo plano])](../files/protect-your-enterprise-data-with-edp.md#protect-data-new-file-bg).

## Aplicación de directivas de la interfaz de usuario


En esta sección, tomaremos el ejemplo de una aplicación de correo habilitada, que actualmente muestra un buzón empresarial entre un conjunto de buzones que incluyen buzones personales y empresariales que pertenecen al usuario. Para garantizar que no haya pérdidas de los datos empresariales en el buzón empresarial, la aplicación llama a [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791) para asegurarse de que el sistema operativo conozca el contexto actual de la aplicación, tanto si es empresarial como personal. La API devuelve False si una directiva de empresa no administra la identidad.

```CSharp
using Windows.Security.EnterpriseData;

...

public class Mailbox
{
    public bool HasEnterpriseMail { get { /* implementation */ } }
    public string Identity { get { /* implementation */ } }
}

private void SwitchMailbox(Mailbox targetMailbox)
{
    // Code goes here to perform setup for "targetMailbox".

    if (targetMailbox.HasEnterpriseMail)
    {
        bool result = 
            ProtectionPolicyManager.TryApplyProcessUIPolicy(targetMailbox.Identity);

        // Code goes here to process "result", which indicates whether
        // or not policy enforcement is in place for the identity.
    }
    else
    {
        // For personal mailboxes, we clear policy enforcement (in case
        // it is still set from when we processed an enterprise mailbox).
        ProtectionPolicyManager.ClearProcessUIPolicy();
    }
}
```
<span id="handle-lock-events"/>
## Controlar los eventos de bloqueo del dispositivo y evitar dejar contenido no protegido en la memoria


En este escenario, tomaremos el ejemplo de una aplicación de correo habilitada diseñada para administrar el correo personal y empresarial. Cuando la aplicación se ejecuta en una organización que ha optado por administrar una directiva de "protección de datos con la pantalla bloqueada" (DPL) segura, la aplicación debe asegurarse de que quita todos los datos confidenciales de la memoria mientras el dispositivo está bloqueado. Para ello, se registra en los eventos [**ProtectionPolicyManager.ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787) y [**ProtectionPolicyManager.ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786) para recibir una notificación siempre que el dispositivo se bloquee o desbloquee (en caso de que la DPL esté activada).


            [
              **ProtectedAccessSuspending**
            ](https://msdn.microsoft.com/library/windows/apps/dn705787) se genera antes de que se quiten temporalmente las claves de protección de datos aprovisionadas en el dispositivo. Estas claves se quitan cuando el dispositivo se bloquea con el fin de garantizar que no puede haber ningún acceso no autorizado a los datos cifrados mientras el dispositivo está bloqueado y posiblemente no lo posee su propietario. 
            [
              **ProtectedAccessResumed**
            ](https://msdn.microsoft.com/library/windows/apps/dn705786) se genera una vez que las claves vuelven a estar disponibles en el momento del desbloqueo del dispositivo.

Al administrar estos dos eventos, la aplicación puede garantizar que protege cualquier contenido confidencial en la memoria con [**DataProtectionManager**](https://msdn.microsoft.com/library/windows/apps/dn706017). También debe cerrar cualquier flujo de archivo abierto en sus archivos protegidos para garantizar que el sistema no almacene en caché ningún dato confidencial de la memoria. Puedes hacerlo de maneras distintas. Para cerrar un flujo de archivos devuelto desde un método Open de **StorageFile**, puedes llamar al método **Dispose** en el flujo. Puedes definir el ámbito del uso del flujo con una instrucción using (C\# o VB). O bien, puedes encapsular un objeto **DataReader** o **DataWriter** alrededor del flujo y usar el método **Dispose** o usar una instrucción con ese objeto.

**Nota**  
En un entorno sin una directiva DPL configurada, se genera el evento [**ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786), pero no el evento [**ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787). Ten esto en cuenta en tu código y no des por sentado que los eventos siempre se produzcan en parejas en cada sistema, ni tampoco que siempre puedas usar los eventos para determinar el estado bloqueado o desbloqueado del dispositivo. En este ejemplo de código puedes observar que no suponemos nada sobre el estado protegido del correo electrónico que se muestra, ni sobre el estado abierto del flujo de archivos de la base de datos.

Además, ten en cuenta que cuando un dispositivo se reanuda después del bloqueo sin una directiva DPL configurada, [**ProtectedAccessResumedEventArgs.Identities**](https://msdn.microsoft.com/library/windows/apps/dn705772) es una colección vacía.

Con fines de brevedad, este ejemplo de código se ha simplificado ligeramente y se centra en el correo empresarial. En una aplicación real, los correos personales se escribirían en un archivo de base de datos de correo diferente y desprotegido y, normalmente, no protegerías los correos electrónicos personales con la pantalla bloqueada.

```CSharp
using Windows.Security.Cryptography;
using Windows.Security.EnterpriseData;
using Windows.Storage;
using Windows.Storage.Streams;

...

public class DisplayedMail
{
    public string Body { get; set; }
    public IBuffer ProtectedBody { get; set; }
    public bool IsProtected { get; set; }
}

private IOutputStream mailDatabaseStream = null;
private string currentlyDisplayedMailIdentity = "contoso.com";
private DisplayedMail currentlyDisplayedMail = new DisplayedMail()
    { Body = "Lorem ipsum dolor...", ProtectedBody = null, IsProtected = false };

// Gets the app's protected mail database file, then opens and stores a stream on it.
private async void OpenMailDatabase()
{
    // Only attempt to open the database file stream if we know it's closed.
    if (this.mailDatabaseStream == null)
    {
        StorageFolder appDataStorageFolder = ApplicationData.Current.LocalFolder;
        StorageFile storageFile = await appDataStorageFolder.GetFileAsync("maildb.dat");
        using (IRandomAccessStream randomAccessStream =
            await storageFile.OpenAsync(FileAccessMode.ReadWrite))
        {
            this.mailDatabaseStream = randomAccessStream.GetOutputStreamAt(0);
        }
    }
}

// Called once by the app when starting up.
private void AppSetup()
{
    ProtectionPolicyManager.ProtectedAccessSuspending +=
        this.ProtectionPolicyManager_ProtectedAccessSuspending;
    ProtectionPolicyManager.ProtectedAccessResumed +=
        this.ProtectionPolicyManager_ProtectedAccessResumed;
    this.OpenMailDatabase();
}

// Background work called when the app receives an email.
private async void AppMailReceived(string fauxEmail)
{
    // Only attempt to write to the database file stream if we know it's open.
    if (this.mailDatabaseStream != null)
    {
        IBuffer emailAsBuffer = CryptographicBuffer.ConvertStringToBinary
            (fauxEmail, BinaryStringEncoding.Utf8);
        await this.mailDatabaseStream.WriteAsync(emailAsBuffer);
        await this.mailDatabaseStream.FlushAsync();
    }
    else
    {
        // Code goes here to handle the case where the device is
        // locked and we can't access the protected mail database.
    }
}

// Called by ProtectionPolicyManager when the device is locked if under DPL.
private async void ProtectionPolicyManager_ProtectedAccessSuspending
    (object sender, ProtectedAccessSuspendingEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.currentlyDisplayedMailIdentity))
    {
        // This event is not for our identity. Another will be sent for our identity.
        return;
    }

    // Get suspension deferral.
    Windows.Foundation.Deferral deferral = e.GetDeferral();

    // Protect the displayed mail content.
    if (!this.currentlyDisplayedMail.IsProtected)
    {
        IBuffer mailBodyBuffer = CryptographicBuffer.ConvertStringToBinary
            (this.currentlyDisplayedMail.Body, BinaryStringEncoding.Utf8);
        BufferProtectUnprotectResult result = await DataProtectionManager.ProtectAsync
            (mailBodyBuffer, this.currentlyDisplayedMailIdentity);
        if (result.ProtectionInfo.Status == DataProtectionStatus.Protected)
        {
            // Save the protected version and clear the unprotected version.
            this.currentlyDisplayedMail.ProtectedBody = result.Buffer;
            this.currentlyDisplayedMail.Body = null;
        }
    }

    // Close the mail database file stream to make sure that we have no unprotected
    // content in memory.
    this.mailDatabaseStream.Dispose();
    this.mailDatabaseStream = null;

    // Optionally, code goes here to use e.Deadline to determine whether we have more
    // than 15 seconds left before the suspension deadline. If we do then process any
    // messages queued up for sending while we are still able to access them.

    // All done. Complete deferral.
    deferral.Complete();
}

// Called by ProtectionPolicyManager when the device is unlocked.
private async void ProtectionPolicyManager_ProtectedAccessResumed
    (object sender, ProtectedAccessResumedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.currentlyDisplayedMailIdentity))
    {
        // This event is not for our identity. Another will be sent for our identity.
        return;
    }

    // Unprotect the displayed mail content.
    if (this.currentlyDisplayedMail.IsProtected)
    {
        BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync
            (this.currentlyDisplayedMail.ProtectedBody);
        if (result.ProtectionInfo.Status == DataProtectionStatus.Unprotected)
        {
            // Restore the unprotected version.
            this.currentlyDisplayedMail.Body = CryptographicBuffer.ConvertBinaryToString
                (BinaryStringEncoding.Utf8, result.Buffer);
            this.currentlyDisplayedMail.ProtectedBody = null;
        }
    }

    // Reopen the closed mail database.
    this.OpenMailDatabase();
}
```

## Registrarse para recibir notificaciones cuando se revoca el contenido protegido


Imagina una situación en la que una aplicación de correo configura un buzón empresarial en un dispositivo de un usuario. En algún momento, y por alguno de varios motivos posibles, la empresa quiere revocar el acceso a los correos electrónicos protegidos por la empresa y a otros recursos de ese dispositivo. Hay varios motivos posibles para la revocación. Es más probable que improbable que la directiva de empresa concreta desencadene una revocación en el momento de la cancelación de la inscripción, de modo que un escenario puede ser que el usuario tenga el dispositivo de la empresa sin inscribir (quizá porque quiere vender o regalar el dispositivo, simplemente quiere usar otro, cambia de trabajo o se jubila). Otro escenario es que el administrador de TI envíe una notificación de cancelación de la inscripción de forma remota a través de administración de dispositivos móviles (MDM), por ejemplo, si se notifica como perdido un dispositivo.

Independientemente de los detalles del caso, la empresa envía una solicitud para borrar todo el correo del dispositivo del usuario, ya que ya no se necesita. Un cliente de administración remota de un dispositivo recibe la solicitud del servidor de administración remota de la empresa y llama a [**ProtectionPolicyManager.RevokeContent**](https://msdn.microsoft.com/library/windows/apps/dn705790) para revocar las claves necesarias para acceder al contenido protegido de ese dispositivo con esa identidad de empresa.

Si la aplicación necesita recibir una notificación cuando se produce una revocación, puedes registrarte al evento [**ProtectionPolicyManager.ProtectedContentRevoked**](https://msdn.microsoft.com/library/windows/apps/dn705788). Cuando la aplicación recibe el evento, puede eliminar los metadatos asociados con el buzón de la empresa, que ya no se necesitarán.

```CSharp
using Windows.Security.EnterpriseData;

...

private string mailIdentity = "contoso.com";

void MailAppSetup()
{
    ProtectionPolicyManager.ProtectedContentRevoked += ProtectionPolicyManager_ProtectedContentRevoked;
    // Code goes here to set up mailbox for 'mailIdentity'.
}

private void ProtectionPolicyManager_ProtectedContentRevoked(object sender, ProtectedContentRevokedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.mailIdentity))
    {
        // This event is not for our identity.
        return;
    }

    // Code goes here to delete any metadata associated with 'mailIdentity'.
}
```

## El cliente de administración remota inicia una eliminación de los datos protegidos por la empresa


Un usuario tiene varios archivos de empresa en su dispositivo personal que están protegidos con la identidad de la empresa. El usuario pierde su dispositivo. Cuando la empresa recibe la notificación de que el usuario ha perdido su dispositivo, la empresa envía una solicitud para borrar todos los datos confidenciales del dispositivo del usuario para evitar que se pierdan. El cliente de administración remota del dispositivo recibe esta solicitud del servidor de administración remota de la empresa y llama a [**ProtectionPolicyManager.RevokeContent**](https://msdn.microsoft.com/library/windows/apps/dn705790) para revocar las claves necesarias para acceder al contenido protegido con esa identidad de empresa.

```CSharp
Windows.Security.EnterpriseData.ProtectionPolicyManager.RevokeContent("contoso.com");
```

 

 






<!--HONumber=Jul16_HO1-->


