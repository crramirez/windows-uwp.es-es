---
title: Uso compartido de Mis allegados
description: Explica cómo agregar compatibilidad para el uso compartido de Mis allegados
author: muhsinking
ms.author: mukin
ms.date: 06/28/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7084c4dde7bdf2d59842a04fe9fd52bc029c264a
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7434492"
---
# <a name="my-people-sharing"></a>Uso compartido de Mis allegados

La función Mis allegados permite a los usuarios anclar contactos a su barra de tareas, lo que les permite mantenerse en contacto fácilmente desde cualquier lugar de Windows, sin importar mediante qué aplicación están conectados. Los usuarios pueden ahora compartir contenido con sus contactos anclados, arrastrando archivos desde el Explorador de archivos a su ancla Mis allegados. También pueden compartir con todos los contactos del almacén de contactos de Windows, mediante el acceso a compartir estándar. Sigue leyendo para obtener información sobre cómo habilitar la aplicación como destino de uso compartido de Mis allegados.

![Panel de uso compartido de Mis allegados](images/my-people-sharing.png)

## <a name="requirements"></a>Requisitos

+ Windows 10 y Microsoft Visual Studio 2017. Para obtener detalles sobre la instalación, consulta [Prepararse para Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up).
+ Conocimientos básicos de C# o algún lenguaje de programación orientado a objetos similar. Para comenzar con C#, consulta [Crear una aplicación "Hello, world"](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).

## <a name="overview"></a>Introducción

Hay tres pasos que debes seguir para habilitar la aplicación destino de uso compartido de Mis allegados:

1. [Declarar la compatibilidad con el contrato de activación shareTarget en el manifiesto de la aplicación.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#declaring-support-for-the-share-contract)
2. [Anotar los contactos con los que los usuarios pueden compartir al usar tu aplicación.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#annotating-contacts)
3. Admitir varias instancias de la aplicación en ejecución al mismo tiempo.  Los usuarios deben poder interactuar con una versión completa de la aplicación mientras también la usan para compartir con otros. Puede que la usen a la vez en varias ventanas de uso compartido. Para admitir esto, la aplicación debe poder ejecutar varias vistas al mismo tiempo. Para obtener información sobre cómo hacerlo, consulta el artículo ["Mostrar varias vistas en una aplicación"](https://docs.microsoft.com/en-us/windows/uwp/layout/show-multiple-views).

Cuando hayas hecho esto, la aplicación aparecerá como un destino de uso compartido en la ventana de compartir Mis allegados, que se puede iniciar de dos maneras:
1. Se elige un contacto a través del acceso al uso compartido.
2. Los archivos se arrastran y se sueltan sobre un contacto anclado a la barra de tareas.

## <a name="declaring-support-for-the-share-contract"></a>Declaración de compatibilidad para el contrato para contenido compartido

Para declarar la compatibilidad de la aplicación como destino de contenido compartido, primero abre la aplicación en Visual Studio. En el **Explorador de soluciones**, haz clic con el botón derecho en **Package.appxmanifest** y selecciona **Abrir con**. En el menú, selecciona **Editor XML (texto)** y haz clic en **Aceptar**. Después, realiza los siguientes cambios en el manifiesto:


**Antes**
```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
    </Application>
</Applications>
```

**Después**

```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
        <Extensions>
            <uap:Extension Category="windows.shareTarget">
                <uap:ShareTarget Description="Share with MyApp">
                    <uap:SupportedFileTypes>
                        <uap:SupportsAnyFileType/>
                    </uap:SupportedFileTypes>
                    <uap:DataFormat>Text</uap:DataFormat>
                    <uap:DataFormat>Bitmap</uap:DataFormat>
                    <uap:DataFormat>Html</uap:DataFormat>
                    <uap:DataFormat>StorageItems</uap:DataFormat>
                    <uap:DataFormat>URI</uap:DataFormat>
                </uap:ShareTarget>
            </uap:Extension>
         </Extensions>
    </Application>
</Applications>
```

Este código agrega compatibilidad para todos los formatos de datos y archivos, pero puedes especificar qué tipos de archivo y formatos de datos son compatibles (consulta [documentación de la clase ShareTarget](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget) para obtener más detalles).

## <a name="annotating-contacts"></a>Anotación de contactos

Para permitir que la ventana de uso compartido de Mis allegados muestre tu aplicación como destino de contenido compartido para tus contactos, debes escribirlos en el almacén de contactos de Windows. Para obtener información sobre cómo escribir los contactos, consulta la [Muestra de integración de tarjeta de contacto](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration). 

Para que la aplicación aparezca como destino de contenido compartido de Mis allegados al compartir con un contacto, debe escribir una anotación en ese contacto. Las anotaciones son elementos de datos de la aplicación que se asocian con un contacto. La anotación debe contener la clase activable correspondiente a la vista deseada en su miembro **ProviderProperties** y declarar la compatibilidad con la operación **Compartir**.

Puedes anotar contactos en cualquier momento mientras se ejecuta la aplicación pero, por lo general, debes anotar los contactos tan pronto como se agreguen al almacén de contactos de Windows.

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and Share support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactShareAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations::Share;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation);
}
```

El "appId" es el nombre de familia de paquete, seguido por '!' y el identificador de clase activable. Para buscar el nombre de familia de paquete, abre **Package.appxmanifest** mediante el editor predeterminado y busca en la pestaña "Empaquetado". Aquí, "Aplicación" es la clase activable correspondiente a la vista de destino de contenido compartido.

## <a name="running-as-a-my-people-share-target"></a>Ejecución como destino de contenido compartido de Mis allegados

Por último, para ejecutar la aplicación, invalida el método [OnShareTargetActivated](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Application#Windows_UI_Xaml_Application_OnShareTargetActivated_Windows_ApplicationModel_Activation_ShareTargetActivatedEventArgs_) en la clase principal de tu aplicación para controlar la activación de destino de contenido compartido. La propiedad [ShareTargetActivatedEventArgs.ShareOperation.Contacts](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation#Properties) contendrá los contactos con los que se está compartiendo o estará vacía si se trata de una operación de compartir estándar (no un uso compartido de Mis allegados).

```Csharp
protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    bool isPeopleShare = false;
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
    {
        // Make sure the current OS version includes the My People feature before
        // accessing the ShareOperation.Contacts property
        isPeopleShare = (args.ShareOperation.Contacts.Count > 0);
    }

    if (isPeopleShare)
    {
        // Show share UI for MyPeople contact(s)
    }
    else
    {
        // Show standard share UI for unpinned contacts
    }
}
```

## <a name="see-also"></a>Consulta también
+ [Agregar compatibilidad con Mis allegados](my-people-support.md)
+ [Clase ShareTarget](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget)
+ [Ejemplo de integración de la tarjeta de contacto](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)
