---
author: normesta
description: "Muestra cómo agregar la aplicación junto a las acciones en una tarjeta de contacto"
MSHAttr: PreferredLib:/library/windows/apps
title: "Conectar la aplicación a acciones en una tarjeta de contacto"
translationtype: Human Translation
ms.sourcegitcommit: 5c0f6ef1f1a346a66ca554a415d9f24c8a314ae1
ms.openlocfilehash: 034dc2b7be69763416192014abe24b9bf924c443

---

# Conectar la aplicación a acciones en una tarjeta de contacto

La aplicación puede aparecer junto a acciones en una tarjeta de contacto o una minitarjeta de contacto. Los usuarios pueden elegir tu aplicación para realizar una acción, como abrir una página de perfil, realizar una llamada o enviar un mensaje.

![Tarjeta de contacto y minitarjeta de contacto](images/all-contact-cards.png)

Para empezar, busca contactos existentes o crea contactos nuevos. A continuación, crea una *anotación* y algunas entradas del manifiesto del paquete para describir las acciones que la aplicación admite. A continuación, escribe código que realice las acciones.

Para obtener una muestra más completa, consulta la [muestra de integración de la tarjeta de contacto](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration).

## Buscar o crear un contacto

Si tu aplicación ayuda a los contactos a conectarse con otras personas, busca contactos en Windows y anótalos a continuación. Si tu aplicación administra contactos, puedes agregarlos a una lista de contactos de Windows y anotarlos a continuación.

### Buscar un contacto

Para buscar contactos, usa un nombre, una dirección de correo electrónico o un número de teléfono.

```cs
ContactStore contactStore = await ContactManager.RequestStoreAsync();

IReadOnlyList<Contact> contacts = null;

contacts = await contactStore.FindContactsAsync(emailAddress);

Contact contact = contacts[0];
```

### Crear un contacto

Si tu aplicación es más bien como una libreta de direcciones, crea contactos y agrégalos a una lista de contactos.

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

## Etiquetar cada contacto con una anotación

Etiqueta cada contacto con una lista de acciones (operaciones) que la aplicación pueda realizar (por ejemplo, videollamadas y mensajería).

A continuación, asocia el identificador de un contacto a un identificador que tu aplicación use internamente para identificar a ese usuario.

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

annotation.SupportedOperations = ContactAnnotationOperations.Message |
  ContactAnnotationOperations.AudioCall |
  ContactAnnotationOperations.VideoCall |
 ContactAnnotationOperations.ContactProfile;

await annotationList.TrySaveAnnotationAsync(annotation);
```

## Registrarse para cada operación

En el manifiesto del paquete, regístrate para cada operación que enumeraste en la anotación.

Para el registro, agrega controladores de protocolo al elemento ``Extensions`` del manifiesto.

```xml
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-contact-profile">
      <uap:DisplayName>TestProfileApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-ipmessaging">
      <uap:DisplayName>TestMsgApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-voip-video">
      <uap:DisplayName>TestVideoApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-voip-call">
      <uap:DisplayName>TestCallApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
</Extensions>
```
También puedes agregarlos en la pestaña **Declaraciones** del diseñador de manifiestos en Visual Studio.

![Pestaña declaraciones del diseñador de manifiestos](images/manifest-designer-protocols.png)

## Buscar la aplicación junto a las acciones en una tarjeta de contacto

Abre la aplicación Contactos. La aplicación aparece junto a cada acción (operación) que especificaste en la anotación y el manifiesto del paquete.

![Tarjeta de contacto](images/a-contact-card.png)

Si los usuarios eligen tu aplicación para una acción, esta aparecerá como la aplicación predeterminada para esa acción la próxima vez que los usuarios abran una tarjeta de contacto.

## Buscar la aplicación junto a las acciones en una minitarjeta de contacto

En las minitarjetas de contacto, la aplicación aparece en las pestañas que representan acciones.

![Minitarjeta de contacto](images/mini-contact-card.png)

Las aplicaciones como **Correo** abren minitarjetas de contacto. Tu aplicación también puede abrirlas. Este código te muestra cómo hacerlo.

```cs
public async void OpenContactCard(object sender, RoutedEventArgs e)
{
    // Get the selection rect of the button pressed to show contact card.
    FrameworkElement element = (FrameworkElement)sender;

    Windows.UI.Xaml.Media.GeneralTransform buttonTransform = element.TransformToVisual(null);
    Windows.Foundation.Point point = buttonTransform.TransformPoint(new Windows.Foundation.Point());
    Windows.Foundation.Rect rect =
        new Windows.Foundation.Rect(point, new Windows.Foundation.Size(element.ActualWidth, element.ActualHeight));

   // helper method to find a contact just for illustrative purposes.
    Contact contact = await findContact("contoso@contoso.com");

    ContactManager.ShowContactCard(contact, rect, Windows.UI.Popups.Placement.Default);

}
```

Para ver más ejemplos con minitarjetas de contacto, consulta la [muestra de tarjetas de contacto](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCards).

Igual que la tarjeta de contacto, cada pestaña recuerda la aplicación que el usuario usó por última vez, lo que permite volver fácilmente a la aplicación.

## Realizar operaciones cuando los usuarios seleccionan tu aplicación en una tarjeta de contacto

Invalida el método [Application.OnActivated](https://msdn.microsoft.com/library/windows/apps/br242330) en el archivo **App.cs** y navega por los usuarios a una página de la aplicación. La [muestra de integración de la tarjeta de contacto](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration) muestra una manera de hacerlo.

En el archivo de código subyacente de la página, invalida el método [Page.OnNavigatedTo](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.page.onnavigatedto.aspx). La tarjeta de contacto pasa este método, el nombre de la operación y el identificador del usuario.

Para iniciar una llamada de audio o vídeo, consulta la [muestra de VoIP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/VoIP). Encontrarás la API completa en el espacio de nombres [WIndows.ApplicationModel.Calls](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.calls.aspx).

Para facilitar la mensajería, consulta el espacio de nombres [Windows.ApplicationModel.Chat](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.chat.aspx).

También puedes iniciar otra aplicación. Eso es lo que hace este código.

```cs
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    var args = e.Parameter as ProtocolActivatedEventArgs;
    // Display the result of the protocol activation if we got here as a result of being activated for a protocol.

    if (args != null)
    {
        var options = new Windows.System.LauncherOptions();
        options.DisplayApplicationPicker = true;

        options.TargetApplicationPackageFamilyName = “ContosoApp”;

        string launchString = args.uri.Scheme + ":" + args.uri.Query;
        var launchUri = new Uri(launchString);
        await Windows.System.Launcher.LaunchUriAsync(launchUri, options);
    }
}
```

La propiedad ```args.uri.scheme``` contiene el nombre de la operación y la propiedad ```args.uri.Query``` contiene el identificador del usuario.



<!--HONumber=Aug16_HO3-->


