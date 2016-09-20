---
author: awkoren
Description: "En este tema se muestran ejemplos de las tareas de codificación necesarias para lograr algunos de los escenarios más habituales de Enterprise Data Protection (EDP) relacionados con la transferencia de datos."
MS-HAID: dev\_app\_to\_app.use\_edp\_to\_protect\_enterprise\_data\_transferred\_between\_apps
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Usar EDP para proteger los datos empresariales que se transfieren entre aplicaciones
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: 77533d4aca3cc84e0a021a0faac57f5afbbefdd7

---

# Usar EDP para proteger los datos empresariales que se transfieren entre aplicaciones


            __Nota__ La directiva de Protección de datos de empresa (EDP) no se puede aplicar en la versión 1511 de Windows 10 (compilación 10586) o en una versión anterior.

En este tema se muestran ejemplos de las tareas de codificación necesarias para lograr algunos de los escenarios más habituales de Enterprise Data Protection (EDP) relacionados con la transferencia de datos. Para obtener una perspectiva de desarrollador completa sobre cómo se relaciona EDP con los archivos, las secuencias, el portapapeles, las redes, las tareas en segundo plano y la protección de datos con la pantalla bloqueada, consulta [Enterprise Data Protection (EDP) (Protección de datos de empresa [EDP])](../enterprise/edp-hub.md).


            **Nota** La [muestra de Protección de datos de empresa (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409) abarca muchos de los escenarios que se muestran en este tema.

## Requisitos previos


-   **Prepararse para EDP**

    Consulta [Configurar el equipo para EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP).

-   **Confirmar la compilación de una aplicación habilitada para empresas**

    Una aplicación que garantiza de forma independiente que los datos de la empresa se mantienen bajo el control administrativo de la empresa se conoce como una aplicación habilitada para empresas. Una aplicación habilitada es más eficaz, inteligente, flexible y confiable que una que no lo está. La aplicación indica al sistema que está habilitada al declarar la funcionalidad **enterpriseDataPolicy** restringida. Aun así, además de configurar una funcionalidad, hay que realizar otras operaciones para habilitar la aplicación. Para obtener más información, consulta [Aplicaciones habilitadas para empresas](../enterprise/edp-hub.md#enterprise-enlightened-apps).

-   **Comprender la programación asincrónica de las aplicaciones de la Plataforma universal de Windows (UWP)**

    Para aprender a escribir aplicaciones asincrónicas en C\# o Visual Basic, consulta [Llamar a API asincrónicas en C\# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Asynchronous programming in C++ (Programación asincrónica en C++)](https://msdn.microsoft.com/library/windows/apps/mt187334).

## Origen del Portapapeles simple


En este escenario, la aplicación es de tipo Bloc de notas que procesa archivos personales y empresariales. Aquí, la aplicación no necesita cambiar su lógica de copiar y pegar. Solo debe llamar a [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791) siempre que el usuario abra y muestre contenido de un documento empresarial. Una vez que el contenido se muestre en la interfaz de usuario de la aplicación, el usuario puede copiarlo para luego pegarlo en otra aplicación, que es la razón por la que la configuración de la directiva de interfaz de usuario es importante. De este modo, el sistema operativo puede aplicar la directiva establecida para una operación de pegar que implique datos protegidos. Igualmente, es importante borrar la directiva de interfaz de usuario en cuanto ya no se necesite, de modo que el usuario es una vez más pueda copiar y pegar datos personales. 
            **TryApplyProcessUIPolicy** devuelve false si una directiva de empresa no administra su argumento de identidad.

```CSharp
using Windows.Security.EnterpriseData;

...

private void OnFileLoaded(FileProtectionInfo fileProtectionInfo, string contentsOfFile)
{
    if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy
            (fileProtectionInfo.Identity);
        if (isIdentityManaged)
        {
            // UI policy is now in effect for the file's identity.
        }
        else
        {
            // Enterprise policy is not in effect, because the file's identity
            // is not managed. In this case, we have a file protected to an
            // unmanaged identity, which is not a valid situation.
            // We still have to call ClearProcessUIPolicy if we want to clear the policy.
            ProtectionPolicyManager.ClearProcessUIPolicy();
        }
    }
    else
    {
        // In case we applied the policy for the previous file, now we need to clear it.
        ProtectionPolicyManager.ClearProcessUIPolicy();
    }
    // Code goes here to display "contentsOfFile" in your UI. Ready for copy-paste...
}
```

## Destino de Portapapeles simple


En este escenario, la aplicación es una aplicación de correo electrónico que controla las cuentas personales y corporativas. El usuario intenta pegar datos en una respuesta de correo electrónico con su cuenta personal. En este caso, antes de que la aplicación recupere el contenido, simplemente tiene que hacer una llamada a [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636). Si ya tenemos acceso, ese método devuelve una respuesta inmediatamente. Sin embargo, si no tenemos acceso, el método hace que aparezca un cuadro de diálogo en el que se pide el consentimiento del usuario y se "desbloquea" el paquete de datos si se da consentimiento. Esto es para dar al usuario la oportunidad de cancelar una operación de pegar realizada por error.

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private async void OnPasteSimple()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        // In case we don't already have acccess, request consent from the user
        // for us to access the clipboard data.
        await dataPackageView.RequestAccessAsync();

        try
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            // Code goes here to insert the text into the email...
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the clipboard.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

## El destino del Portapapeles es un documento neutro y vacío


En este escenario, la aplicación es una aplicación de procesamiento de texto. Después de crear un documento nuevo, siempre que el documento quede vacío, tu aplicación lo trata como neutro, ni empresarial ni personal. El usuario luego pega los datos empresariales en este documento neutro en cuanto al contexto. Dado que el documento es neutro en cuanto al contexto, podríamos cambiar el documento a un contexto de empresa y evitar llamar a [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636) por completo (dado que en dicho caso no es necesario mostrar el cuadro de diálogo). Por lo tanto, si los datos están protegidos, simplemente cambiamos a un contexto de empresa y pegamos los datos.

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private async void OnPasteWithApplyPolicy()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        // If the data has no enterprise identity, then we already have access.
        if (!string.IsNullOrEmpty(dataPackageView.Properties.EnterpriseId))
        {
            ProtectionPolicyEvaluationResult policyResult =
                await dataPackageView.RequestAccessAsync(dataPackageView.Properties.EnterpriseId);
            if (this.isNewEmptyDocument &&
                policyResult == ProtectionPolicyEvaluationResult.Allowed)
            {
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog.
                bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy(dataPackageView.Properties.EnterpriseId);
                // You can use "isIdentityManaged", but it's not necessary.
            }
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.
                await dataPackageView.RequestAccessAsync();
            }
        }

        try
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            // Code goes here to insert the text into the email...
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the clipboard.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

## Origen del Portapapeles con identidad de empresa explícita


En este escenario, la aplicación es una aplicación de procesamiento de texto. Usa un subproceso en segundo plano para confirmar operaciones de copia realizadas por el usuario. El usuario copia algunos datos de un archivo empresarial y cambia inmediatamente a un archivo personal, en cuyo momento, el contexto global de la aplicación se hace personal. En este punto, dado que el estado global ha cambiado y no se debe usar, el subproceso en segundo plano de la aplicación debe indicar explícitamente al Portapapeles que los datos entrantes son empresariales. Para ello, se establece la propiedad [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861).

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private void CopyToClipboard(string stringToCopy, string identity)
{
    // Copy the string to the clipboard.
    var dataPackage = new DataPackage();
    dataPackage.SetText(stringToCopy);
    dataPackage.Properties.EnterpriseId = identity;
    Clipboard.SetContent(dataPackage);
}
```

## Etiquetar una ventana específica con la identidad de la empresa


En este escenario, la aplicación es una aplicación de procesamiento de texto que controla varios documentos en diferentes ventanas, algunos de los cuales son empresariales y otras personales. La aplicación quiere garantizar que los datos que se pegan en la ventana de un documento personal se examinan correctamente (es decir, se deniegan o permiten si se trata de datos empresariales) y que los datos salientes de la ventana de un documento empresarial se marcan correctamente. Para hacer esto, se establece la propiedad [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785).

```CSharp
using Windows.Security.EnterpriseData;

...

private void TagCurrentViewWithEnterpriseId(string identity)
{
    ProtectionPolicyManager.GetForCurrentView().Identity = identity;
}
```

## Etiquetar un objeto de arrastre saliente con la identidad de la empresa


La aplicación tiene una ventana personal abierta con algún contenido empresarial que se puede arrastrar. El usuario comienza a arrastrar parte de este contenido y la aplicación debe asegurar que este se marque como empresarial. Este escenario no implica nuevas API. Para este escenario, la aplicación establecerá la propiedad [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861) (consulta [Escenario de origen del Portapapeles con identidad explícita de la empresa](#clipboard_source_explicit_id) arriba).

## Consultas recibidas para arrastrar el objeto por su identidad de la empresa


La aplicación tiene abierto un documento nuevo y vacío, que se supone que es neutro siempre que esté vacío, y el usuario arrastra y suelta algo de contenido en el documento. Ahora, la aplicación debe determinar la identidad de la empresa del objeto para cambiar el estado del documento según corresponda. Para este escenario, la aplicación recibirá la propiedad **EnterpriseId** del paquete de datos mediante la lectura de [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861) (consulta arriba [Escenario de origen del Portapapeles con identidad explícita de la empresa](#clipboard_source_explicit_id).

## La aplicación tiene un origen de contrato para contenido compartido


Al admite el contrato para contenido compartido en la aplicación, para configurar un origen de contenido compartido, establece el contexto de identidad de la empresa el objeto [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/br205873), tal como se muestra en este ejemplo de código.


            **Nota** Este ejemplo de código depende de si ya configuraste la identidad del objeto de administrador de directivas de protección para la vista actual (consulta [Etiquetar una ventana específica con la identidad de la empresa](#tag_window_with_id)); de lo contrario, la propiedad [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) contendrá la cadena vacía.



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private void OnShareSourceOperation(object sender, RoutedEventArgs e)
{
    // Register the current page as a share source (or you could do this earlier in your app).
    DataTransferManager.GetForCurrentView().DataRequested += OnDataRequested;
    DataTransferManager.ShowShareUI();
}

private void OnDataRequested(DataTransferManager sender, DataRequestedEventArgs args)
{
    if (!string.IsNullOrEmpty(this.shareSourceContent))
    {
        var protectionPolicyManager = ProtectionPolicyManager.GetForCurrentView();
        DataPackage requestData = args.Request.Data;
        requestData.Properties.Title = this.shareSourceTitle;
        requestData.Properties.EnterpriseId = protectionPolicyManager.Identity;
        requestData.SetText(this.shareSourceContent);
    }
}
```

## Aplicación como destino de contrato para contenido compartido


Este ejemplo de código continúa nuestra directiva siempre que el archivo con el que estamos trabajando esté vacío. Entonces estamos libres de cambiar contextos según sea necesario para los datos entrantes y evitamos la aparición de mensajes de consentimiento en la medida de lo posible. Por lo tanto, cuando la aplicación reciba datos como un destino de contrato para contenido compartido, debe llamar a [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791) si no hay ningún contenido existente. De lo contrario, debe llamar a [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636). En el ejemplo de código se muestra cómo.

```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.ApplicationModel.DataTransfer.ShareTarget;

...

string identity = "microsoft.com";

protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    ShareOperation shareOperation = args.ShareOperation;
    if (shareOperation.Data.Contains(StandardDataFormats.Text))
    {
        // If the data has no enterprise identity, then we already have access.
        if (!string.IsNullOrEmpty(shareOperation.Data.Properties.EnterpriseId))
        {
            ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult =
                await ProtectionPolicyManager.RequestAccessAsync
                    (shareOperation.Data.Properties.EnterpriseId, identity);
            if (this.isNewEmptyDocument && protectionPolicyEvaluationResult ==
                ProtectionPolicyEvaluationResult.Allowed)
            {
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog.
                bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy
                    (shareOperation.Data.Properties.EnterpriseId);
                // You can use "isIdentityManaged", but it';s not necessary.
            }
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.
                protectionPolicyEvaluationResult = await shareOperation.Data.RequestAccessAsync();
            }
        }

        try
        {
            // Get the text from the share operation...
            App.shareTargetContent = await shareOperation.Data.GetTextAsync();
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the share operation.
        }
    }
    else
    {
        // The value in the share operation is not in the text format.
    }
}
```

## Consultar la directiva de copiar y pegar de manera pasiva


En este escenario, la aplicación habilita la interfaz de usuario de pegar solo cuando hay datos en el Portapapeles. Para esta función, puedes usar el método [**ProtectionPolicyManager.CheckAccess**](https://msdn.microsoft.com/library/windows/apps/dn705783), que permite una comprobación pasiva de la directiva.


            **Nota** Este ejemplo de código depende de si ya configuraste la identidad del objeto de administrador de directivas de protección para la vista actual (consulta [Etiquetar una ventana específica con la identidad de la empresa](#tag_window_with_id)); de lo contrario, la propiedad [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) contendrá la cadena vacía.



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private bool IsClipboardPeekAllowedAsync()
{
    ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = ProtectionPolicyEvaluationResult.Blocked;
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);
    }

    // Since this is a convenience feature to allow a peek of clipboard content,
    // if state is Blocked or ConsentRequired, do not show peek if this helper
    // method returns false.
    return (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed);
}
```

## Solicitar acceso para una operación de copiar y pegar


En este escenario se muestra cómo comprobar el acceso para una operación de pegar.


            **Nota** Este ejemplo de código depende de si ya configuraste la identidad del objeto de administrador de directivas de protección para la vista actual (consulta [Etiquetar una ventana específica con la identidad de la empresa](#tag_window_with_id)); de lo contrario, la propiedad [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) contendrá la cadena vacía.



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private async void OnPasteWithRequestAccess()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);

        if (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed)
        {
            try
            {
                string contentsOfClipboard = await dataPackageView.GetTextAsync();
                // Code goes here to insert the text into the app...
                this.fileContentsTextBox.Text = contentsOfClipboard;
            }
            catch (Exception)
            {
                // Code goes here to handle the exception retrieving text from the clipboard.
            }
        }
        else
        {
            // Paste from the enterprise context is not allowed by policy.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```


            **Nota** Este artículo está orientado a desarrolladores de Windows 10 que escriben aplicaciones para la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).



## Temas relacionados


[enterprise data protection (EDP) sample (Muestra de Protección de datos de empresa [EDP])](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Espacio de nombres Windows.Security.EnterpriseData**](https://msdn.microsoft.com/library/windows/apps/dn279153)








<!--HONumber=Jun16_HO4-->


