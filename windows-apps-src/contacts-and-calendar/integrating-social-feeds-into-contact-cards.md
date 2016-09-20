---
author: normesta
description: "Muestra cómo integrar fuentes sociales en la aplicación Contactos"
MSHAttr: PreferredLib:/library/windows/apps
title: "Proporcionar fuentes sociales a la aplicación Contactos"
translationtype: Human Translation
ms.sourcegitcommit: 767acdc847e1897cc17918ce7f49f9807681f4a3
ms.openlocfilehash: c5b9666d8654a4065bc0e4e400d3e47de4773b8b

---

# Proporcionar fuentes sociales a la aplicación Contactos

Integrar los datos de fuentes sociales de la base de datos en la aplicación Contactos.

Los datos de fuentes aparecerán en las páginas **Novedades** de la aplicación Contactos o en la página **Perfil** de un contacto.

Los usuarios pueden pulsar un elemento de la fuente para abrir la aplicación.

![Fuentes sociales en la aplicación Contactos](images/social-feeds.png)

Para empezar, crea una aplicación en primer plano que etiquete contactos de fuentes sociales y un agente en segundo plano que envíe datos de fuentes a la aplicación Contactos.

Para obtener un ejemplo más completo, consulta la [muestra de información social](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp).

## Crear una aplicación en primer plano

En primer lugar, crea un proyecto de la Plataforma universal de Windows (UWP) y, después, agrégale **Extensiones de Windows Mobile para UWP**.

![Extensiones móviles](images/mobile-extensions.png)

### Buscar o crear contactos

Para encontrar contactos, puedes usar un nombre, una dirección de correo electrónico o un número de teléfono.

```cs
ContactStore contactStore = await ContactManager.RequestStoreAsync();

IReadOnlyList<Contact> contacts = null;

contacts = await contactStore.FindContactsAsync(emailAddress);

Contact contact = contacts[0];
```
También puedes crear contactos y agregarlos después a una lista de contactos.

```cs
Contact contact = new Contact();
contact.FirstName = "TestContact";

ContactEmail email = new ContactEmail();
email.Address = "TestContact@contoso.com";
email.Kind = ContactEmailKind.Other;
contact.Emails.Add(email);

ContactPhone phone = new ContactPhone();
phone.Number = "4255550101";
phone.Kind = ContactPhoneKind.Mobile;
contact.Phones.Add(phone);

ContactStore store = await
    ContactManager.RequestStoreAsync(ContactStoreAccessType.AppContactsReadWrite);

ContactList contactList;

IReadOnlyList<ContactList> contactLists = await store.FindContactListsAsync();

if (0 == contactLists.Count)
    contactList = await store.CreateContactListAsync("TestContactList");
else
    contactList = contactLists[0];

await contactList.SaveContactAsync(contact);
```

### Etiquetar cada contacto con una anotación

Esta *anotación* hace que la aplicación Contactos solicite datos de fuentes del contacto a tu agente en segundo plano.

Como parte de la anotación, asocia el identificador del contacto a un identificador que la aplicación use internamente para identificar al contacto.

```cs
ContactAnnotationStore annotationStore = await
   ContactManager.RequestAnnotationStoreAsync(ContactAnnotationStoreAccessType.AppAnnotationsReadWrite);

ContactAnnotationList annotationList;

IReadOnlyList<ContactAnnotationList> annotationLists = await annotationStore.FindAnnotationListsAsync();
if (0 == annotationLists.Count)
    annotationList = await annotationStore.CreateAnnotationListAsync();
else
    annotationList = annotationLists[0];

ContactAnnotation annotation = new ContactAnnotation();
annotation.ContactId = contact.Id;
annotation.RemoteId = "user22";

annotation.SupportedOperations = ContactAnnotationOperations.SocialFeeds;

await annotationList.TrySaveAnnotationAsync(annotation);

```
### Aprovisionar al agente en segundo plano

Asegúrate de que el contrato de API [SocialInfoContract](https://msdn.microsoft.com/library/windows/apps/dn706146.aspx) esté disponible en el dispositivo que ejecutará la aplicación.

Si está disponible, aprovisiona el agente en segundo plano.

```cs
if (Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
"Windows.ApplicationModel.SocialInfo.SocialInfoContract",
1))
{
    bool isProvisionSuccessful = await Windows.ApplicationModel.SocialInfo.Provider.SocialInfoProviderManager.ProvisionAsync();

    // Throw an exception if the app could not be provisioned
    if (!isProvisionSuccessful)
    {
        throw new Exception("Could not provision the app with the SocialInfoProviderManager");
    }
}
```
## Crear el agente en segundo plano

El agente en segundo plano es un componente de Windows Runtime que responde a las solicitudes de fuentes de la aplicación Contactos.

En el agente, podrás responder a esas solicitudes. Para ello, deberás proporcionar a la aplicación Contactos los datos de fuentes de la base de datos.

### Crear un componente de Windows Runtime

Agrega un proyecto **Componente de Windows Runtime (Windows Universal)** a la solución.

![Componente de Windows Runtime](images/windows-runtime-component.png)

### Registrar el agente en segundo plano como un servicio de aplicaciones

Para el registro, agrega controladores de protocolo al elemento ``Extensions`` del manifiesto.

```xml
<Extensions>
  <uap:Extension Category="windows.appService" EntryPoint="SocialFeeds.BackgroundAgent">
    <uap:AppService Name="com.microsoft.windows.social-feeds" />
  </uap:Extension>
</Extensions>
```
También puedes agregarlos en la pestaña **Declaraciones** del diseñador de manifiestos en Visual Studio.

![Servicio de aplicaciones en el diseñador de manifiestos](images/manifest-designer-app-service.png)

### Solicitar operaciones desde la aplicación Contactos

Pregunta a la aplicación Contactos qué tipo de datos quiere a continuación. La aplicación Contactos responderá a tu solicitud con un código que indica de qué fuente quiere datos.

Esta tabla describe cada fuente:

| Fuente | Descripción |
|-------|-------------|
| Página principal | Fuente que aparece en la página Novedades de la aplicación Contactos. |
| Contacto | Fuente que aparece en la página Novedades de un contacto. |
| Panel | Fuente que aparece en la tarjeta de contacto junto a la imagen de perfil. |
<br>
Las preguntas a la aplicación Contactos se realizan solicitando una *operación*. Implementa la interfaz [IBackgroundTask](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.aspx) e invalida el método [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx).

En el método [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx), envía dos pares clave-valor a la aplicación Contactos. Uno de ellos contiene la versión del protocolo y el otro contiene el tipo de operación.

A continuación, escucha la respuesta de la aplicación Contactos. Esa respuesta contendrá un código.

```cs
public sealed class BackgroundAgent : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {
        BackgroundTaskDeferral backgroundTaskDeferral = taskInstance.GetDeferral();

        AppServiceTriggerDetails triggerDetails = taskInstance.TriggerDetails as AppServiceTriggerDetails;

        AppServiceConnection appServiceConnection = triggerDetails.AppServiceConnection;

        bool continueProcessing = true;

        while (continueProcessing)
        {
            // Get next operation.
            ValueSet fields =
                new ValueSet();

            fields.Add("Version",
                (1 << 16) | 0);

            fields.Add(
                "Type", 0x10);

            AppServiceResponse response =
                await appServiceConnection.SendMessageAsync(fields);

            if (response == null || response.Status != AppServiceResponseStatus.Success)
            { /* throw exception */ }

            UInt32 type = (UInt32)response.Message["Type"];

            switch (type)
            {
                //Get Next Operation.
                case 0x10:
                    break;
                //Download Home Feed Operation.
                case 0x11:
                    break;
                //Download Contact Feed Operation.
                case 0x13:
                    break;
                //Download Dashboard Feed Operation.
                case 0x15:
                    break;
                //Operation Result.
                case 0x80:
                    break;
                //Good Bye.
                case 0xF1:
                    continueProcessing = false;
                    break;
            }
        }
        // Complete the task deferral
        backgroundTaskDeferral.Complete();

        backgroundTaskDeferral = null;
        appServiceConnection = null;
    }

}
```

Consulta el elemento ``Type`` de la propiedad [AppServiceResponse.Message](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceresponse.message.aspx) para obtener ese código. La siguiente es una lista completa de los códigos.

| Type| Descripción |
|-----|-------------|
| 0x10 | Solicitud a la aplicación Contactos de la siguiente operación. |
| 0x11 | Solicitud de la aplicación Contactos para que proporciones la fuente de página principal del usuario principal. |
| 0x13 | Solicitud de la aplicación Contactos para obtener la fuente de contactos del contacto seleccionado. |
| 0x15 | Solicitud de la aplicación Contactos para obtener el elemento de panel del contacto seleccionado. |
| 0x80 | Indica que la operación se completó. Notifica a la aplicación Contactos que los datos ya están disponibles. |
| 0xF1 | Mensaje de la aplicación Contactos que indica que no se requieren otras operaciones. El agente en segundo plano ya se puede apagar. |
<br>
La propiedad [AppServiceResponse.Message](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceresponse.message.aspx) también devuelve una colección de otros pares clave-valor que describen la respuesta. Esta es una lista de dichos valores.

| Clave | Type | Descripción |
|-----|------|-------------|
| Version | UINT32 | (Obligatorio) Identifica la versión del protocolo de mensajes. Los 16 bits superiores son la versión principal y los 16 bits inferiores son la versión secundaria. |
| Type | UINT32 | (Obligatorio) Tipo de operación que se va a llevar a cabo. En el ejemplo anterior se usa la clave Type para determinar qué operación solicita la aplicación Contactos.
| OperationId | UINT32 | Identificador de la operación. |
| OwnerRemoteId | Cadena | Identificador que la aplicación usa internamente para identificar ese contacto. |
| LastFeedItemTimeStamp | Cadena | Identificador del último elemento de la fuente que se recuperó. |
| LastFeedItemTimeStamp | DateTime | Marca de tiempo del último elemento de la fuente que se recuperó. |
| ItemCount | UINT32 | Número de elementos que solicita la aplicación Contactos. |
| IsFetchMore | BOOLEANO | Determina cuándo se actualiza la memoria caché interna. |
| ErrorCode | UINT32 | Código de error asociado a la operación del agente en segundo plano. |
<br>
### Proporcionar una fuente de datos a la aplicación Contactos

Un valor **Type** de ``0x11``, ``0x13`` o ``0x15`` es una solicitud de datos de fuentes de la aplicación Contactos.  

Los fragmentos de código siguientes muestran un enfoque para proporcionar esos datos a la aplicación Contactos.

> [!NOTE]
> Estos fragmentos de código provienen de la [muestra de información social](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp). Contienen referencias a interfaces, clases y miembros que se definen en otras partes de la muestra. Usa estos fragmentos de código junto con los demás ejemplos de este tema para comprender el flujo de las tareas y hacer referencia a la muestra si estás interesado en profundizar más en la pila de interfaces, clases y tipos.

**Obtener elementos de la fuente de contactos**

```cs
public override async Task DownloadFeedAsync()
{
    // Get the contact feeds
    IEnumerable<FeedItem> contactFeedItems =
        InMemorySocialCache.Instance.GetContactFeeds(OwnerRemoteId, ItemCount);

    // Check if the platform supports the SocialInfo APIs
    if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
         "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
    {

        return;
    }

    // Create the social feed updater.
    SocialFeedUpdater feedUpdater = await SocialInfoProviderManager.CreateSocialFeedUpdaterAsync(
        SocialFeedKind.ContactFeed,
        SocialFeedUpdateMode.Replace,
        OwnerRemoteId);

    // Translate the sample feed into Social info feed items.
    foreach (FeedItem fi in contactFeedItems)
    {
        SocialFeedItem item = new SocialFeedItem();

        item.Timestamp = fi.Timestamp;
        item.RemoteId = fi.Id;
        item.TargetUri = fi.TargetUri;
        item.Author.DisplayName = fi.AuthorDisplayName;
        item.Author.RemoteId = fi.AuthorId;
        item.PrimaryContent.Title = fi.Title;
        item.PrimaryContent.Message = fi.Message;

        if (fi.ImageUri != null)
        {
            item.Thumbnails.Add(new SocialItemThumbnail()
            {
                TargetUri = fi.TargetUri,
                ImageUri = fi.ImageUri
            });
        }

        feedUpdater.Items.Add(item);
    }

    await feedUpdater.CommitAsync();
}
```

**Obtener elementos del panel**

```cs
public override async Task DownloadFeedAsync()
{
    // Get the dashboard feed item from your database.
    FeedItem dashboardFeedItem = InMemorySocialCache.Instance.GetDashboardFeed(OwnerRemoteId);

    if (dashboardFeedItem != null)
    {
        // Check if the platform supports the social info APIs.
        if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
             "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
        {

            return;
        }

        SocialDashboardItemUpdater dashboard =
            await SocialInfoProviderManager.CreateDashboardItemUpdaterAsync(OwnerRemoteId);

        dashboard.Content.Message = dashboardFeedItem.Message;
        dashboard.Content.Title = dashboardFeedItem.Title;
        dashboard.Timestamp = dashboardFeedItem.Timestamp;

        // The TargetUri of the dashboard always has to be set.
        dashboard.TargetUri = dashboardFeedItem.TargetUri;

        // For a thumbnail, there must always be a target Uri.
        if ((dashboardFeedItem.ImageUri != null) && (dashboardFeedItem.TargetUri != null))
        {
            dashboard.Thumbnail = new SocialItemThumbnail()
            {
                ImageUri = dashboardFeedItem.ImageUri,
                TargetUri = dashboardFeedItem.TargetUri
            };
        }

        await dashboard.CommitAsync();
    }
}
```

**Obtener elementos de la fuente de página principal**

```cs
public override async Task DownloadFeedAsync()
{
    // Query the "database" for the home feeds.
    IEnumerable<FeedItem> homeFeedItems =
        InMemorySocialCache.Instance.GetHomeFeeds(OwnerRemoteId, ItemCount);

    // Check if the platform supports the social info APIs.
    if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
         "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
    {

        return;
    }

    // Create the social feed updater.
    SocialFeedUpdater feedUpdater = await SocialInfoProviderManager.CreateSocialFeedUpdaterAsync(
        SocialFeedKind.HomeFeed,
        SocialFeedUpdateMode.Replace,
        OwnerRemoteId);

    // Generate each of the feed items.
    foreach (FeedItem fi in homeFeedItems)
    {
        SocialFeedItem item = new SocialFeedItem();

        item.Timestamp = fi.Timestamp;
        item.RemoteId = fi.Id;
        item.TargetUri = fi.TargetUri;
        item.Author.DisplayName = fi.AuthorDisplayName;
        item.Author.RemoteId = fi.AuthorId;
        item.PrimaryContent.Title = fi.Title;
        item.PrimaryContent.Message = fi.Message;

        if (fi.ImageUri != null)
        {
            item.Thumbnails.Add(new SocialItemThumbnail()
            {
                TargetUri = fi.TargetUri,
                ImageUri = fi.ImageUri
            });
        }

        feedUpdater.Items.Add(item);
    }

    await feedUpdater.CommitAsync();
}
```

### Devolver la notificación de éxito o error a la aplicación Contactos

Encapsula tus llamadas en un bloque try catch y, a continuación, devuelve un mensaje de éxito o error a la aplicación Contactos después de haber proporcionado los datos de fuentes.

```cs
try
{
    // Calls to methods that fetch data and populate feed.
}
catch (Exception exception)
{
    errorCode = (UInt32)exception.HResult;
}

// Send status back to the people app.
ValueSet fields =
    new ValueSet();

fields.Add("ErrorCode", errorCode);

UInt32 operationID = (UInt32)response.Message["OperationId"];

fields.Add("OperationId", operationID);

await this.mAppServiceConnection.SendMessageAsync(fields);

```



<!--HONumber=Aug16_HO4-->


