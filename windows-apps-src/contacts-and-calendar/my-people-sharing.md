---
title: Uso compartido de Mis allegados
description: Use el uso compartido de mis personas para que los usuarios puedan anclar contactos a su barra de tareas y mantenerse en contacto fácilmente desde cualquier lugar de Windows.
ms.date: 06/28/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8cbf82592e91b82e2d9d34d116d00aecf2ddd021
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339413"
---
# <a name="my-people-sharing"></a>Uso compartido de Mis allegados

La característica mis personas permite a los usuarios anclar contactos a su barra de tareas, permitiéndoles mantenerse en contacto con facilidad desde cualquier lugar de Windows, independientemente de la aplicación a la que estén conectados. Ahora los usuarios pueden compartir contenido con sus contactos anclados arrastrando los archivos desde el explorador de archivos hasta el PIN mis personas. También pueden compartirse con cualquier contacto de la tienda de contactos de Windows a través del acceso a recursos compartidos estándar. Siga leyendo para obtener información sobre cómo habilitar su aplicación como un destino de uso compartido de mis personas.

![Panel de uso compartido de mis personas](images/my-people-sharing.png)

## <a name="requirements"></a>Requisitos

+ Windows 10 y Microsoft Visual Studio 2019. Para obtener información detallada sobre la instalación, vea [configurar con Visual Studio](/windows/apps/get-started/get-set-up).
+ Conocimientos básicos de C# o algún lenguaje similar de programación orientado a objetos. Para empezar a trabajar con C#, vea [crear una aplicación "Hello, World"](../get-started/create-a-hello-world-app-xaml-universal.md).

## <a name="overview"></a>Información general

Hay tres pasos que debe seguir para habilitar la aplicación como el destino de uso compartido de mis personas:

1. [Declare la compatibilidad con el contrato de activación shareTarget en el manifiesto de aplicación.](#declaring-support-for-the-share-contract)
2. [Anote los contactos que los usuarios pueden compartir para usar la aplicación.](#annotating-contacts)
3. Admite varias instancias de la aplicación que se ejecutan al mismo tiempo.  Los usuarios deben poder interactuar con una versión completa de la aplicación al mismo tiempo que la usan para compartir con otros usuarios. Pueden utilizarla en varias ventanas de uso compartido a la vez. Para admitir esto, la aplicación debe ser capaz de ejecutar varias vistas simultáneamente. Para obtener información sobre cómo hacerlo, vea el artículo ["Mostrar varias vistas de una aplicación"](../design/layout/show-multiple-views.md).

Cuando lo haya hecho, la aplicación aparecerá como un destino de recurso compartido en la ventana mis recursos compartidos, que se puede iniciar de dos maneras:
1. Se elige un contacto mediante el acceso a recursos compartidos.
2. Los archivos se arrastran y se colocan en un contacto anclado a la barra de tareas.

## <a name="declaring-support-for-the-share-contract"></a>Declarar la compatibilidad con el contrato de recurso compartido

Para declarar la compatibilidad con la aplicación como destino de recurso compartido, abra primero la aplicación en Visual Studio. En el **Explorador de soluciones** , haga clic con el botón derecho en **Package. Appxmanifest** y seleccione **abrir con**. En el menú, seleccione **Editor XML (texto)** y haga clic en **Aceptar**. Después, realice los siguientes cambios en el manifiesto:


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

Este código agrega compatibilidad con todos los archivos y formatos de datos, pero puede elegir especificar qué tipos de archivos y formatos de datos se admiten (consulte la documentación de la [clase ShareTarget](/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget) para obtener más detalles).

## <a name="annotating-contacts"></a>Anotar contactos

Para permitir que la ventana mis recursos compartidos muestre la aplicación como un destino de recurso compartido para sus contactos, debe escribirlas en el almacén de contactos de Windows. Para obtener información sobre cómo escribir contactos, vea el ejemplo de integración de la [tarjeta de contacto](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration). 

Para que la aplicación aparezca como un destino de recurso compartido mis personas al compartir con un contacto, debe escribir una anotación en ese contacto. Las anotaciones son fragmentos de datos de la aplicación que están asociados a un contacto. La anotación debe contener la clase activable correspondiente a la vista deseada en su miembro **ProviderProperties** y declarar la compatibilidad con la operación de **uso compartido** .

Puede anotar contactos en cualquier momento mientras la aplicación se está ejecutando, pero generalmente debe anotar los contactos en cuanto se agreguen al almacén de contactos de Windows.

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

"AppId" es el nombre de familia del paquete, seguido de "!". y el identificador de clase activable. Para buscar el nombre de familia del paquete, Abra **Package. appxmanifest** con el editor predeterminado y mire en la pestaña "empaquetado". Aquí, "app" es la clase activable que corresponde a la vista de destino del recurso compartido.

## <a name="running-as-a-my-people-share-target"></a>Ejecutar como destino de recurso compartido mis personas

Por último, para ejecutar la aplicación, invalide el método [OnShareTargetActivated](/uwp/api/Windows.UI.Xaml.Application#Windows_UI_Xaml_Application_OnShareTargetActivated_Windows_ApplicationModel_Activation_ShareTargetActivatedEventArgs_) en la clase principal de la aplicación para controlar la activación del destino del recurso compartido. La propiedad [ShareTargetActivatedEventArgs. ShareOperation. contacts](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation#Properties) contendrá los contactos con los que se comparte, o estará vacío si se trata de una operación de recurso compartido estándar (no de un recurso compartido de mis personas).

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

## <a name="see-also"></a>Vea también
+ [Incorporación de compatibilidad con mis personas](my-people-support.md)
+ [Clase ShareTarget](/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget)
+ [Ejemplo de integración de tarjeta de contacto](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)