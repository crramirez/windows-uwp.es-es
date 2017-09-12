---
title: "Agregar compatibilidad con Mis allegados a una aplicación"
description: "Explica cómo agregar compatibilidad con Mis allegados a una aplicación y cómo anclar y Desanclar contactos"
author: mukin
ms.author: mukin
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 61f73fd2fc0d14d0d1d478d67763c9b0e252cd36
ms.sourcegitcommit: ec18e10f750f3f59fbca2f6a41bf1892072c3692
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2017
---
# <a name="adding-my-people-support-to-an-application"></a>Agregar compatibilidad con Mis allegados a una aplicación

> [!IMPORTANT]
> **VERSIÓN PREVIA | Requiere la actualización Fall Creators Update**: Debes elegir el [SDK 16225 de Insider](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK) y estar ejecutando la [Compilación 16226 de Insider](https://blogs.windows.com/windowsexperience/2017/06/21/announcing-windows-10-insider-preview-build-16226-pc/) o superior para poder usar las API de Mis allegados.

La función Mis allegados permite a los usuarios a anclar contactos desde una aplicación directamente a la barra de tareas, creando un nuevo objeto de contacto con el que pueden interactuar de varias maneras. Este artículo muestra cómo se puede agregar compatibilidad para esta función, permitiendo a los usuarios anclar contactos directamente desde tu aplicación. Cuando se anclan contactos se dispone de nuevos tipos de interacción, como [Compartir con Mis allegados](my-people-sharing.md) y [notificaciones](my-people-notifications.md).

![Chat Mis allegados](images/my-people-chat.png)

## <a name="requirements"></a>Requisitos

+ Windows 10 y Microsoft Visual Studio 2017. Para obtener detalles sobre la instalación, consulta [Prepararse para Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up).
+ Conocimientos básicos de C# o algún lenguaje de programación orientado a objetos similar. Para comenzar con C#, consulta [Crear una aplicación "Hello, world"](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).

## <a name="overview"></a>Introducción

Hay tres cosas que debes hacer para habilitar la aplicación para usar la función Mis allegados:

1. [Declarar la compatibilidad con el contrato de activación shareTarget en el manifiesto de la aplicación.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#declaring-support-for-the-share-contract)
2. [Anotar los contactos con los que los usuarios pueden compartir al usar tu aplicación.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#annotating-contacts)
3.  Admitir varias instancias de tu aplicación en ejecución al mismo tiempo. Los usuarios deben poder interactuar con una versión completa de la aplicación mientras la usan en un panel de contactos.  Pueden incluso usarla en varios paneles de contactos a la vez.  Para admitir esto, la aplicación debe poder ejecutar varias vistas al mismo tiempo. Para obtener información sobre cómo hacerlo, consulta el artículo ["Mostrar varias vistas en una aplicación"](https://docs.microsoft.com/en-us/windows/uwp/layout/show-multiple-views).

Cuando lo hayas hecho, la aplicación aparecerá en el panel de contactos de contactos marcados.

## <a name="declaring-support-for-the-contract"></a>Declaración de compatibilidad para el contrato

Para declarar la compatibilidad para el contrato de Mis allegados, abre tu aplicación en Visual Studio. En el **Explorador de soluciones**, haz clic con el botón derecho en **Package.appxmanifest** y selecciona **Abrir con**. En el menú, selecciona **Editor XML (texto)** y haz clic en **Aceptar**. Realiza los siguientes cambios en el manifiesto:

**Antes**

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10">

    <Applications>
        <Application Id="MyApp"
          Executable="$targetnametoken$.exe"
          EntryPoint="My.App">
        </Application>
    </Applications>

```

**Después**

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4">

    <Applications>
        <Application Id="MyApp"
          Executable="$targetnametoken$.exe"
          EntryPoint="My.App">
          <Extensions>
            <uap4:Extension Category="windows.contactPanel" />
          </Extensions>
        </Application>
    </Applications>

```

Con esta adición, ahora se puede iniciar la aplicación a través del contrato **windows. ContactPanel**, que te permite interactuar con los paneles de contacto.

## <a name="annotating-contacts"></a>Anotación de contactos

Para permitir que los contactos de tu aplicación aparezcan en la barra de tareas mediante el panel Mis allegados, debes escribirlos en el almacén de contactos de Windows.  Para obtener información sobre cómo escribir los contactos, consulta la [Muestra de tarjeta de contacto](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration).

La aplicación también debe escribir una anotación en cada contacto. Las anotaciones son elementos de datos de la aplicación que se asocian con un contacto. La anotación debe contener la clase activable correspondiente a la vista deseada en su miembro **ProviderProperties** y declarar la compatibilidad con la operación **ContactProfile**.

Puedes anotar contactos en cualquier momento mientras se ejecuta la aplicación pero, por lo general, debes anotar los contactos tan pronto como se agreguen al almacén de contactos de Windows.

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and contact panel support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactPanelAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations::ContactProfile;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation));
}
```

El "appId" es el nombre de familia de paquete, seguido por '!' y el identificador de clase activable. Para buscar el nombre de familia de paquete, abre **Package.appxmanifest** mediante el editor predeterminado y busca en la pestaña "Empaquetado". Aquí, "Aplicación" es la clase activable correspondiente a la vista de inicio de la aplicación.

## <a name="allow-contacts-to-invite-new-potential-users"></a>Permitir a los contactos invitar a nuevos usuarios potenciales

De forma predeterminada, la aplicación solo aparecerá en el panel de contactos para los contactos que hayas anotado específicamente.  Esto es para evitar confusiones con los contactos con los que no se puede interactuar a través de la aplicación.  Si quieres que la aplicación se muestre para contactos que la aplicación no conoce (para invitar a los usuarios a agregar determinado contacto con su cuenta, por ejemplo), puedes agregar lo siguiente al manifiesto:

**Antes**

```Csharp
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
      <Extensions>
        <uap4:Extension Category="windows.contactPanel" />
      </Extensions>
    </Application>
</Applications>
```

**Después**

```Csharp
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
      <Extensions>
        <uap4:Extension Category="windows.contactPanel">
            <uap4:ContactPanel SupportsUnknownContacts="true" />
        </uap4:Extension>
      </Extensions>
    </Application>
</Applications>
```

Con este cambio, la aplicación aparecerá como una opción disponible en el panel de contactos para todos los contactos que el usuario haya anclado.  Cuando la aplicación se activa mediante el contrato del panel de contactos, debes comprobar para ver si el contacto es uno que conoce la aplicación.  De lo contrario, deberías mostrar la experiencia de nuevo usuario de la aplicación.

![Panel de contactos Mis allegados](images/my-people.png)

## <a name="support-for-email-apps"></a>Compatibilidad con aplicaciones de correo electrónico

Si estás escribiendo una aplicación de correo electrónico, no necesitas anotar cada contacto manualmente.  Si declaras compatibilidad para el panel de contactos y para el protocolo mailto:, la aplicación aparecerá automáticamente para los usuarios con dirección de correo electrónico.

## <a name="running-in-the-contact-panel"></a>Ejecución en el panel de contactos

Ahora que la aplicación aparece en el panel de contactos para algunos o todos los usuarios, necesitas controlar la activación con el contrato del panel de contactos.

```Csharp
override protected void OnActivated(IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.ContactPanel)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        var rootFrame = new Frame();

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;

        // Navigate to the page that shows the Contact UI.
        rootFrame.Navigate(typeof(ContactPage), e);

        // Ensure the current window is active
        Window.Current.Activate();
    }
}
```

Cuando la aplicación se activa con este contrato, recibirá un [objeto ContactPanelActivatedEventArgs](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.activation.contactpanelactivatedeventargs).  Este objeto contiene el identificador del contacto con el que está intentando interactuar la aplicación al inicio y un objeto [ContactPanel](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactpanel). Debes mantener una referencia a este objeto ContactPanel, que te permitirá interactuar con el panel.

El objeto ContactPanel tiene dos eventos a los que la aplicación debe escuchar:
+ El evento **LaunchFullAppRequested** se envía cuando el usuario ha invocado el elemento de interfaz de usuario que solicita que tu aplicación completa se inicie en su propia ventana.  La aplicación es responsable de iniciarse por sí misma, pasando por todo el contexto necesario.  Eres libre de hacer esto, aunque te gustaría (por ejemplo, a través del inicio de protocolo).
+ El **evento de cierre** se envía cuando la aplicación va a cerrarse, lo que te permite guardar cualquier contexto.

El objeto ContactPanel también te permite establecer el color de fondo del encabezado del panel de contacto (si no se establece, tomará como predeterminado el tema del sistema) y cerrar mediante programación el panel de contactos.



## <a name="the-pinnedcontactmanager-class"></a>Clase PinnedContactManager

La clase [PinnedContactManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager) se usa para administrar los contactos que están anclados a la barra de tareas. Esta clase te permite anclar y desanclar contactos, determinar si un contacto está anclado y determinar si el sistema en el que se ejecuta la aplicación admite el anclaje en una superficie determinada.

Puedes recuperar el objeto PinnedContactManager mediante el método **GetDefault**:

```Csharp
PinnedContactManager pinnedContactManager = PinnedContactManager.GetDefault();
```

## <a name="pinning-and-unpinning-contacts"></a>Anclado y desanclado de contactos
Ahora puedes anclar y desanclar contactos mediante la clase PinnedContactManager que acabas de crear. Los métodos **RequestPinContactAsync** y **RequestUnpinContactAsync** proporcionan al usuario diálogos de confirmación, por lo que deben llamarse desde proceso del contenedor de subproceso único de aplicaciones (ASTA o subproceso de usuario).

```Csharp
async void PinContact (Contact contact)
{
    await pinnedContactManager.RequestPinContactAsync(contact,
                                                      PinnedContactSurface.Taskbar);
}

async void UnpinContact (Contact contact)
{
    await pinnedContactManager.RequestUnpinContactAsync(contact,
                                                        PinnedContactSurface.Taskbar);
}
```

También puedes anclar varios contactos a la vez:

```Csharp
async Task PinMultipleContacts(Contact[] contacts)
{
    await pinnedContactManager.RequestPinContactsAsync(
        contacts, PinnedContactSurface.Taskbar);
}
```

> [!Note]
> Actualmente no hay ninguna operación por lotes para desanclar contactos.

**Nota:** 

## <a name="see-also"></a>Consulta también
+ [Uso compartido de Mis allegados](my-people-sharing.md)
+ [Notificaciones de Mis allegados](my-people-notifications.md)
+ [Muestra de tarjeta de contacto](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)
+ [Documentación de la clase PinnedContactManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager)
