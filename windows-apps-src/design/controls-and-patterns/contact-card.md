---
author: mijacobs
Description: A button gives the user a way to trigger an immediate action.
title: Tarjeta de contacto
ms.author: mijacobs
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: kele
design-contact: tbd
dev-contact: tbd
doc-status: not-published
ms.localizationpriority: medium
ms.openlocfilehash: 9ed520c8ad71203a2f2f9888f775d7ca51d0089f
ms.sourcegitcommit: dc3389ef2e2c94b324872a086877314d6f963358
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/11/2018
ms.locfileid: "1874343"
---
# <a name="contact-card"></a>Tarjeta de contacto

La tarjeta de contacto muestra información de contacto, como el nombre, el número de teléfono y la dirección, para un [Contacto](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) (el mecanismo que usa UWP para representar personas y empresas).  La tarjeta de contacto también permite al usuario editar información de contacto. Puedes elegir entre mostrar una tarjeta de contacto compacta o una tarjeta de contacto completa que contenga información adicional.

> **API importantes**: [método ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowFullContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_Foundation_Rect_), [método ShowFullContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_ApplicationModel_Contacts_FullContactCardOptions_), [método IsShowContactCardSupported](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported), [clase Contact](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact)  

Hay dos formas de mostrar la tarjeta de contacto:  
* Como una tarjeta de contacto estándar que aparece en un control flotante descartable por cambio de foco: la tarjeta de contacto desaparece cuando el usuario hace clic fuera de ella. 
* Como una tarjeta de contacto completa que ocupa más espacio y no es descartable por cambio de foco: el usuario debe hacer clic en **cerrar** para cerrarla. 


<figure>
    <img src="images/contact-card/contact-card-standard.png" alt="The full contact card">
    <figcaption>La tarjeta de contacto estándar</figcaption>
</figure>

<figure>
    <img src="images/contact-card/contact-card-full.png" alt="The full contact card">
    <figcaption>La tarjeta de contacto completa</figcaption>
</figure>


## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa la tarjeta de contacto cuando quieras mostrar la información de contacto de un contacto. Si solo quieres mostrar el nombre del contacto y la imagen, usa el [control de imagen de la persona](person-picture.md). 


<!-- TODO: Add examples back when the contact card has been added. -->

<!-- ## Examples

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>If you have the <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> app installed, click here to <a href="xamlcontrolsgallery:/item/Button">open the app and see the Button in action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Get the source code (GitHub)</a></li>
    </ul>
</td>
</tr>
</table> -->

## <a name="show-a-standard-contact-card"></a>Muestra una tarjeta de contacto estándar

1. Por lo general, se muestra una tarjeta de contacto porque el usuario hizo clic en algo: un botón o quizás el [control de imagen de la persona](person-picture.md). No queremos ocultar el elemento. Para evitar ocultarla, necesitamos crear una [Rect](/uwp/api/windows.foundation.rect) que describa la ubicación y el tamaño del elemento. 

    Vamos a crear una función de utilidad que lo haga por nosotros: la usaremos más adelante.
    ```csharp
    // Gets the rectangle of the element 
    public static Rect GetElementRectHelper(FrameworkElement element) 
    { 
        // Passing "null" means set to root element. 
        GeneralTransform elementTransform = element.TransformToVisual(null); 
        Rect rect = elementTransform.TransformBounds(new Rect(0, 0, element.ActualWidth, element.ActualHeight)); 
        return rect; 
    } 

    ```

2. Determina si puedes mostrar la tarjeta de contacto mediante una llamada al método [ContactManager.IsShowContactCardSupported](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported). Si no es compatible, muestra un mensaje de error. (En este ejemplo se supone que se va a mostrar la tarjeta de contacto en respuesta a un evento de clic.)
    ```csharp
    // Contact and Contact Managers are existing classes 
    private void OnUserClickShowContactCard(object sender, RoutedEventArgs e) 
    { 
        if (ContactManager.IsShowContactCardSupported()) 
        { 

    ```

3. Usa la función de utilidad que creaste en el paso 1 para obtener los límites del control que provocó el evento (para no taparlo con la tarjeta de contacto).

    ```csharp
            Rect selectionRect = GetElementRect((FrameworkElement)sender); 
    ```

4. Obtén el objeto [Contact](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) que quieres mostrar. Este ejemplo solo crea un contacto simple, pero el código debe recuperar un contacto real. 

    ```csharp
                // Retrieve the contact to display
                var contact = new Contact(); 
                var email = new ContactEmail(); 
                email.Address = "jsmith@contoso.com"; 
                contact.Emails.Add(email); 
    ```
5. Muestra la tarjeta de contacto mediante una llamada al método [ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowFullContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_Foundation_Rect_). 

    ```csharp
            ContactManager.ShowFullContactCard(
                contact, selectionRect, Placement.Default); 
        } 
    } 
    ```

Este es el ejemplo de código completo:

```csharp
// Gets the rectangle of the element 
public static Rect GetElementRect(FrameworkElement element) 
{ 
    // Passing "null" means set to root element. 
    GeneralTransform elementTransform = element.TransformToVisual(null); 
    Rect rect = elementTransform.TransformBounds(new Rect(0, 0, element.ActualWidth, element.ActualHeight)); 
    return rect; 
} 
 
// Display a contact in response to an event
private void OnUserClickShowContactCard(object sender, RoutedEventArgs e) 
{ 
    if (ContactManager.IsShowContactCardSupported()) 
    { 
        Rect selectionRect = GetElementRect((FrameworkElement)sender);

        // Retrieve the contact to display
        var contact = new Contact(); 
        var email = new ContactEmail(); 
        email.Address = "jsmith@contoso.com"; 
        contact.Emails.Add(email); 
    
        ContactManager.ShowContactCard(
            contact, selectionRect, Placement.Default); 
    } 
} 

```

## <a name="show-a-full-contact-card"></a>Muestra una tarjeta de contacto completa

Para mostrar la tarjeta de contacto completa, llama al método [ShowFullContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_ApplicationModel_Contacts_FullContactCardOptions_) en lugar de [ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowFullContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_Foundation_Rect_).

```csharp
private void onUserClickShowContactCard() 
{ 
   
    Contact contact = new Contact(); 
    ContactEmail email = new ContactEmail(); 
    email.Address = "jsmith@hotmail.com"; 
    contact.Emails.Add(email); 
 
 
    // Setting up contact options.     
    FullContactCardOptions fullContactCardOptions = new FullContactCardOptions(); 
 
    // Display full contact card on mouse click.   
    // Launch the People’s App with full contact card  
    fullContactCardOptions.DesiredRemainingView = ViewSizePreference.UseLess; 
     
 
    // Shows the full contact card by launching the People App. 
    ContactManager.ShowFullContactCard(contact, fullContactCardOptions); 
} 

```

## <a name="retrieving-real-contacts"></a>Recuperación de contactos "reales"

Los ejemplos de este artículo crean un contacto simple. En una aplicación real, seguramente quieras recuperar un contacto existente. Para obtener instrucciones, consulta el [Artículo de contactos y calendario](/windows/uwp/contacts-and-calendar/).




## <a name="related-articles"></a>Artículos relacionados
- [Contactos y calendario](/windows/uwp/contacts-and-calendar/)
- [Muestra de tarjetas de contacto](http://go.microsoft.com/fwlink/p/?LinkId=624040)
- [Control de imagen de personas](/windows/uwp/controls-and-patterns/person-picture/)
