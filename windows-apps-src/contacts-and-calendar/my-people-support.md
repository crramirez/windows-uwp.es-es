---
title: Agregar compatibilidad con Mis allegados a una aplicación
description: Explica cómo agregar compatibilidad con mis personas a una aplicación y cómo anclar y desanclar contactos.
ms.date: 06/28/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2eea2d228ddf5ad6dfaef227bfaeb0bafb071490
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170499"
---
# <a name="adding-my-people-support-to-an-application"></a>Agregar compatibilidad con Mis allegados a una aplicación

> [!Note]
> A partir de la actualización de Windows 10 de mayo de 2019 (versión 1903), las instalaciones nuevas de Windows 10 ya no mostrarán "personas en la barra de tareas" de forma predeterminada. Para habilitar la característica, los clientes pueden hacer clic con el botón derecho en la barra de tareas y presionar "Mostrar personas en la barra de tareas". No se recomienda que los desarrolladores agreguen soporte técnico a sus aplicaciones y visite el blog para [desarrolladores de Windows](https://blogs.windows.com/windowsdeveloper/) para obtener más información sobre la optimización de aplicaciones para Windows 10.

La característica mis personas permite a los usuarios anclar contactos de una aplicación directamente a su barra de tareas, lo que crea un nuevo objeto de contacto con el que pueden interactuar de varias maneras. En este artículo se muestra cómo puede Agregar compatibilidad con esta característica, lo que permite a los usuarios anclar contactos directamente desde su aplicación. Cuando se anclan los contactos, se ponen a su disposición nuevos tipos de interacción con el usuario, como [el uso compartido](my-people-sharing.md) y las [notificaciones](my-people-notifications.md)de mis personas.

![Chat mis personas](images/my-people-chat.png)

## <a name="requirements"></a>Requisitos

+ Windows 10 y Microsoft Visual Studio 2019. Para obtener información detallada sobre la instalación, vea [configurar con Visual Studio](../get-started/get-set-up.md).
+ Conocimientos básicos de C# o algún lenguaje similar de programación orientado a objetos. Para empezar a trabajar con C#, vea [crear una aplicación "Hello, World"](../get-started/create-a-hello-world-app-xaml-universal.md).

## <a name="overview"></a>Información general

Hay tres cosas que debe hacer para permitir que la aplicación use la característica mis personas:

1. [Declare la compatibilidad con el contrato de activación shareTarget en el manifiesto de aplicación.](./my-people-sharing.md#declaring-support-for-the-share-contract)
2. [Anote los contactos que los usuarios pueden compartir para usar la aplicación.](./my-people-sharing.md#annotating-contacts)
3.  Compatibilidad con varias instancias de la aplicación que se ejecutan al mismo tiempo. Los usuarios deben poder interactuar con una versión completa de la aplicación mientras la usan en un panel de contactos.  Incluso pueden usarlo en varios paneles de contacto a la vez.  Para admitir esto, la aplicación debe ser capaz de ejecutar varias vistas simultáneamente. Para obtener información sobre cómo hacerlo, vea el artículo ["Mostrar varias vistas de una aplicación"](../design/layout/show-multiple-views.md).

Cuando lo haya hecho, la aplicación aparecerá en el panel de contactos para los contactos anotados.

## <a name="declaring-support-for-the-contract"></a>Declarar la compatibilidad del contrato

Para declarar la compatibilidad con el contrato mis personas, abra la aplicación en Visual Studio. En el **Explorador de soluciones**, haga clic con el botón derecho en **Package. Appxmanifest** y seleccione **abrir con**. En el menú, seleccione **Editor XML (texto))** y haga clic en **Aceptar**. Realice los siguientes cambios en el manifiesto:

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

Con esta adición, la aplicación ahora se puede iniciar a través de **Windows. ** Contrato de ContactPanel, que permite interactuar con los paneles de contacto.

## <a name="annotating-contacts"></a>Anotar contactos

Para permitir que los contactos de la aplicación aparezcan en la barra de tareas mediante el panel mis personas, debe escribirlos en el almacén de contactos de Windows.  Para obtener información sobre cómo escribir contactos, vea el [ejemplo de tarjeta Contact](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration).

La aplicación también debe escribir una anotación para cada contacto. Las anotaciones son fragmentos de datos de la aplicación que están asociados a un contacto. La anotación debe contener la clase activable correspondiente a la vista deseada en su miembro **ProviderProperties** y declarar la compatibilidad con la operación **ContactProfile** .

Puede anotar contactos en cualquier momento mientras la aplicación se está ejecutando, pero generalmente debe anotar los contactos en cuanto se agreguen al almacén de contactos de Windows.

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and contact panel support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactPanelAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations.ContactProfile;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation));
}
```

"AppId" es el nombre de familia del paquete, seguido de "!". y el identificador de clase activable. Para buscar el nombre de familia del paquete, Abra **Package. appxmanifest** con el editor predeterminado y mire en la pestaña "empaquetado". Aquí, "app" es la clase activable correspondiente a la vista de inicio de la aplicación.

## <a name="allow-contacts-to-invite-new-potential-users"></a>Permitir a los contactos invitar a nuevos usuarios potenciales

De forma predeterminada, la aplicación solo aparecerá en el panel de contactos para los contactos que haya anotado específicamente.  Esto es para evitar la confusión con los contactos con los que no se puede interactuar a través de la aplicación.  Si desea que la aplicación aparezca para los contactos que la aplicación no conoce (para invitar a los usuarios a agregar ese contacto a su cuenta, por ejemplo), puede agregar lo siguiente al manifiesto:

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

Con este cambio, la aplicación aparecerá como una opción disponible en el panel de contacto para todos los contactos que el usuario haya anclado.  Cuando la aplicación se activa mediante el contrato del panel de contactos, debe comprobar si el contacto es uno de los que conoce su aplicación.  Si no es así, debe mostrar la nueva experiencia de usuario de la aplicación.

![Panel de contactos Mis allegados](images/my-people.png)

## <a name="support-for-email-apps"></a>Compatibilidad con aplicaciones de correo electrónico

Si está escribiendo una aplicación de correo electrónico, no es necesario anotar cada contacto manualmente.  Si declara la compatibilidad con el panel contacto y para el protocolo mailto:, la aplicación aparecerá automáticamente para los usuarios con una dirección de correo electrónico.

## <a name="running-in-the-contact-panel"></a>Ejecución en el panel de contactos

Ahora que la aplicación aparece en el panel de contactos de algunos o de todos los usuarios, debe controlar la activación con el contrato del panel de contactos.

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

Cuando la aplicación se active con este contrato, recibirá un [objeto ContactPanelActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.contactpanelactivatedeventargs).  Contiene el identificador del contacto con el que la aplicación está intentando interactuar en el inicio y un objeto [ContactPanel](/uwp/api/windows.applicationmodel.contacts.contactpanel) . Debe mantener una referencia a este objeto ContactPanel, que le permitirá interactuar con el panel.

El objeto ContactPanel tiene dos eventos para los que la aplicación debe escuchar:
+ El evento **LaunchFullAppRequested** se envía cuando el usuario ha invocado el elemento de la interfaz de usuario que solicita que se inicie la aplicación completa en su propia ventana.  La aplicación es responsable de iniciarse, pasando todo el contexto necesario.  Puede hacerlo de forma gratuita si lo desea (por ejemplo, mediante el inicio de protocolo).
+ El **evento de cierre** se envía cuando la aplicación está a punto de cerrarse, lo que le permite guardar cualquier contexto.

El objeto ContactPanel también le permite establecer el color de fondo del encabezado del panel de contacto (si no se establece, el valor predeterminado será el tema del sistema) y cerrar mediante programación el panel contacto.

## <a name="supporting-notification-badging"></a>Compatibilidad con distintivos de notificación

Si desea que los contactos anclados a la barra de tareas se contengan en el distintivo cuando lleguen nuevas notificaciones de la aplicación que están relacionadas con esa persona, debe incluir el parámetro **Hint-People** en las [notificaciones del sistema](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md) y las notificaciones expresivas de [mis personas](./my-people-notifications.md).

![Distintivos de notificación de personas](images/my-people-badging.png)

Para notificar a un contacto, el nodo notificador de nivel superior debe incluir el parámetro Hint-People para indicar el envío o el contacto relacionado. Este parámetro puede tener cualquiera de los siguientes valores:
+ **Dirección de correo electrónico** 
    + Por ejemplo, mailto:johndoe@mydomain.com
+ **Número de teléfono** 
    + Por ejemplo, Tel: 888-888-8888
+ **Id. remoto** 
    + Por ejemplo, remoteid: 1234

Este es un ejemplo de cómo identificar una notificación del sistema relacionada con una persona específica:
```XML
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastText01">
            <text>John Doe posted a comment.</text>
        </binding>
    </visual>
</toast>
```

> [!NOTE]
> Si la aplicación usa las [API de ContactStore](/uwp/api/windows.applicationmodel.contacts.contactstore) y usa la propiedad [StoredContact. RemoteId](/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId) para vincular contactos almacenados en el equipo con contactos almacenados de forma remota, es fundamental que el valor de la propiedad RemoteId sea estable y único. Esto significa que el identificador remoto debe identificar de forma coherente una sola cuenta de usuario y debe contener una etiqueta única para garantizar que no entra en conflicto con los identificadores remotos de otros contactos en el equipo, incluidos los contactos que son propiedad de otras aplicaciones.
> Si no se garantiza que los identificadores remotos usados por la aplicación sean stable y Unique, puede usar la clase RemoteIdHelper que se muestra más adelante en este tema para agregar una etiqueta única a todos los identificadores remotos antes de agregarlos al sistema. O bien, puede optar por no usar la propiedad RemoteId en absoluto y, en su lugar, crear una propiedad extendida personalizada en la que almacenar los identificadores remotos para sus contactos.

## <a name="the-pinnedcontactmanager-class"></a>La clase PinnedContactManager

El [PinnedContactManager](/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager) se usa para administrar los contactos que se anclan a la barra de tareas. Esta clase le permite anclar y desanclar contactos, determinar si un contacto está anclado y determinar si el anclaje en una superficie determinada es compatible con el sistema en el que se está ejecutando actualmente la aplicación.

Puede recuperar el objeto PinnedContactManager mediante el método **GetDefault** :

```Csharp
PinnedContactManager pinnedContactManager = PinnedContactManager.GetDefault();
```

## <a name="pinning-and-unpinning-contacts"></a>Anclar y desanclar contactos
Ahora puede anclar y desanclar contactos con el PinnedContactManager que acaba de crear. Los métodos **RequestPinContactAsync** y **RequestUnpinContactAsync** proporcionan al usuario cuadros de diálogo de confirmación, por lo que deben llamarse desde el subproceso de la aplicación de contenedor uniproceso (asta o IU).

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

También puede anclar varios contactos a la vez:

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

## <a name="see-also"></a>Vea también
+ [Uso compartido de Mis allegados](my-people-sharing.md)
+ [Mis personas notificatons](my-people-notifications.md)
+ [Vídeo de Channel 9 sobre cómo agregar soporte técnico para mis usuarios a una aplicación](https://channel9.msdn.com/Events/Build/2017/P4056)
+ [Ejemplo de integración de mis personas](https://github.com/tonyPendolino/MyPeopleBuild2017)
+ [Ejemplo de tarjeta de contacto](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)
+ [Documentación de la clase PinnedContactManager](/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager)
+ [Conectar la aplicación a acciones en una tarjeta de contacto](./integrating-with-contacts.md)