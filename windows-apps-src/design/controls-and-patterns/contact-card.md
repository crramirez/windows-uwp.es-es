---
description: Obtenga información sobre cómo usar las tarjetas de contacto para que los usuarios puedan mostrar y editar información de los contactos, como el nombre, el número de teléfono y la dirección.
title: Tarjeta de contacto
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kele
design-contact: tbd
dev-contact: tbd
doc-status: not-published
ms.localizationpriority: medium
ms.openlocfilehash: d31c1a22260b8d98e767fba6b6d6e0db20fdbbb8
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829593"
---
# <a name="contact-card"></a>Tarjeta de contacto

La tarjeta de contacto muestra información de contacto, como el nombre, el número de teléfono y la dirección, de un [Contacto](/uwp/api/Windows.ApplicationModel.Contacts.Contact) (el mecanismo que usa Windows para representar personas y empresas).  La tarjeta de contacto también permite al usuario editar información de contacto. Puedes elegir entre mostrar una tarjeta de contacto compacta o una tarjeta de contacto completa que contenga información adicional.

> **API importantes**: [método ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showcontactcard), [método ShowFullContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showfullcontactcard), [método IsShowContactCardSupported](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported), [clase Contact](/uwp/api/Windows.ApplicationModel.Contacts.Contact)  

Hay dos formas de mostrar la tarjeta de contacto:  
* Como una tarjeta de contacto estándar que aparece en un control flotante con cierre del elemento por cambio de foco: la tarjeta de contacto desaparece cuando el usuario hace clic fuera de ella. 
* Como una tarjeta de contacto completa que ocupa más espacio y no tiene cierre del elemento por cambio de foco: el usuario debe hacer clic en **cerrar** para cerrarla. 


<figure>
    <img src="images/contact-card/contact-card-standard.png" alt="Screenshot showing a standard contact card.">
    <figcaption>La tarjeta de contacto estándar</figcaption>
</figure>

<figure>
    <img src="images/contact-card/contact-card-full.png" alt="Screenshot showing a full contact card.">
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
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Get the source code (GitHub)</a></li>
    </ul>
</td>
</tr>
</table> -->

## <a name="show-a-standard-contact-card"></a>Muestra de una tarjeta de contacto estándar

1. Por lo general, se muestra una tarjeta de contacto porque el usuario ha hecho clic en algo: un botón o quizás el [control de imagen de la persona](person-picture.md). No queremos ocultar el elemento. Para evitar ocultarla, necesitamos crear un elemento [Rect](/uwp/api/windows.foundation.rect) que describa la ubicación y el tamaño del elemento. 

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

2. Determina si puedes mostrar la tarjeta de contacto mediante una llamada al método [ContactManager.IsShowContactCardSupported](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported). Si no es compatible, muestra un mensaje de error. (En este ejemplo se supone que se va a mostrar la tarjeta de contacto en respuesta a un evento de clic).
    ```csharp
    // Contact and Contact Managers are existing classes 
    private void OnUserClickShowContactCard(object sender, RoutedEventArgs e) 
    { 
        if (ContactManager.IsShowContactCardSupported()) 
        { 

    ```

3. Usa la función de utilidad que has creado en el paso 1 para obtener los límites del control que ha provocado el evento (para no taparlo con la tarjeta de contacto).

    ```csharp
            Rect selectionRect = GetElementRect((FrameworkElement)sender); 
    ```

4. Obtén el objeto [Contact](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) que quieres mostrar. Este ejemplo solo crea un contacto sencillo, pero el código debe recuperar un contacto real. 

    ```csharp
                // Retrieve the contact to display
                var contact = new Contact(); 
                var email = new ContactEmail(); 
                email.Address = "jsmith@contoso.com"; 
                contact.Emails.Add(email); 
    ```
5. Muestra la tarjeta de contacto mediante una llamada al método [ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showcontactcard). 

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

## <a name="show-a-full-contact-card"></a>Muestra de una tarjeta de contacto completa

Para mostrar la tarjeta de contacto completa, llama al método [ShowFullContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showfullcontactcard) en lugar de [ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showcontactcard).

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

En los ejemplos de este artículo se crea un contacto sencillo. En una aplicación real, seguramente quieras recuperar un contacto existente. Para obtener instrucciones, consulta el [Artículo sobre contactos y calendario](../../contacts-and-calendar/index.md).




## <a name="related-articles"></a>Artículos relacionados
- [Contactos y calendario](../../contacts-and-calendar/index.md)
- [Ejemplo de tarjetas de contacto](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCards)
- [Control de imagen de personas](/windows/uwp/controls-and-patterns/person-picture/)
