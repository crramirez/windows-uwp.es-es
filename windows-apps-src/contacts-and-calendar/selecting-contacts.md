---
author: normesta
description: Mediante el espacio de nombres Windows.ApplicationModel.Contacts, tienes varias opciones para seleccionar contactos.
title: Seleccionar contactos
ms.assetid: 35FEDEE6-2B0E-4391-84BA-5E9191D4E442
keywords: contactos, selección seleccionar un solo contacto seleccionar varios contactos contactos, selecciona varios seleccionar contacto datos de contacto, seleccionar datos específicos contacto, seleccionar campos específicos
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a721e618864155e4eec66d222e8eeafa2e0ca038
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6036865"
---
# <a name="select-contacts"></a>Seleccionar contactos



Mediante el espacio de nombres [**Windows.ApplicationModel.Contacts**](https://msdn.microsoft.com/library/windows/apps/BR225002), tienes varias opciones para seleccionar contactos. Aquí te mostraremos cómo seleccionar un único contacto o varios contactos, y aprenderás a configurar el selector de contactos para recuperar solamente la información de contacto que necesita tu aplicación.

## <a name="set-up-the-contact-picker"></a>Configuración del selector de contactos

Crea una instancia de [**Windows.ApplicationModel.Contacts.ContactPicker**](https://msdn.microsoft.com/library/windows/apps/BR224913) y asígnala a una variable.

```cs
var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
```

## <a name="set-the-selection-mode-optional"></a>Establecer el modo de selección (opcional)

De manera predeterminada, el selector de contactos recupera todos los datos disponibles para los contactos que selecciona el usuario. La propiedad [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.contacts.contactpicker.selectionmode) permite configurar el selector de contactos para recuperar únicamente los campos de datos que necesita la aplicación. Esta es una forma más eficaz de usar el selector de contactos si solo necesitas un subconjunto de los datos de contacto disponibles.

Primero, establece la propiedad [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.contacts.contactpicker.selectionmode) en **Fields**:

```cs
contactPicker.SelectionMode = Windows.ApplicationModel.Contacts.ContactSelectionMode.Fields;
```

Luego, usa la propiedad [**DesiredFieldsWithContactFieldType**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.contacts.contactpicker.desiredfieldswithcontactfieldtype) para especificar los campos que quieres que recupere el selector de contactos. En este ejemplo se configura el selector de contactos para que recupere direcciones de correo electrónico:

``` cs
contactPicker.DesiredFieldsWithContactFieldType.Add(Windows.ApplicationModel.Contacts.ContactFieldType.Email);
```

## <a name="launch-the-picker"></a>Inicio del selector

```cs
Contact contact = await contactPicker.PickContactAsync();
```

Usa [**PickContactsAsync**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.contacts.contactpicker.pickcontactsasync) si quieres que el usuario seleccione uno o varios contactos.

```cs
public IList<Contact> contacts;
contacts = await contactPicker.PickContactsAsync();
```

## <a name="process-the-contacts"></a>Procesar los contactos

Cuando vuelva el selector, comprueba si el usuario seleccionó algún contacto. Si es así, procesa la información de contacto.

En este ejemplo se muestra cómo procesar un solo contacto. Aquí se recupera el nombre del contacto y se copia en un control [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) denominado *OutputName*.

```cs
if (contact != null)
{
    OutputName.Text = contact.DisplayName;
}
else
{
    rootPage.NotifyUser("No contact was selected.", NotifyType.ErrorMessage);
}
```

En este ejemplo se muestra cómo procesar varios contactos.

```cs
if (contacts != null && contacts.Count > 0)
{
    foreach (Contact contact in contacts)
    {
        // Do something with the contact information.
    }
}
```

## <a name="complete-example-single-contact"></a>Ejemplo completo (contacto único)

En este ejemplo se usa el selector de contactos para recuperar el nombre, la dirección de correo, la ubicación o el número de teléfono de un solo contacto.

```cs
// ...
using Windows.ApplicationModel.Contacts;
// ...

private async void PickAContactButton-Click(object sender, RoutedEventArgs e)
{
    ContactPicker contactPicker = new ContactPicker();

    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.Email);
    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.Address);
    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.PhoneNumber);

    Contact contact = await contactPicker.PickContactAsync();

    if (contact != null)
    {
        OutputFields.Visibility = Visibility.Visible;
        OutputEmpty.Visibility = Visibility.Collapsed;

        OutputName.Text = contact.DisplayName;

        AppendContactFieldValues(OutputEmails, contact.Emails);
        AppendContactFieldValues(OutputPhoneNumbers, contact.Phones);
        AppendContactFieldValues(OutputAddresses, contact.Addresses);
    }
    else
    {
        OutputEmpty.Visibility = Visibility.Visible;
        OutputFields.Visibility = Visibility.Collapsed;
    }
}

private void AppendContactFieldValues<T>(TextBlock content, IList<T> fields)
{
    if (fields.Count > 0)
    {
        StringBuilder output = new StringBuilder();

        if (fields[0].GetType() == typeof(ContactEmail))
        {
            foreach (ContactEmail email in fields as IList<ContactEmail>)
            {
                output.AppendFormat("Email: {0} ({1})\n", email.Address, email.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactPhone))
        {
            foreach (ContactPhone phone in fields as IList<ContactPhone>)
            {
                output.AppendFormat("Phone: {0} ({1})\n", phone.Number, phone.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactAddress))
        {
            List<String> addressParts = null;
            string unstructuredAddress = "";

            foreach (ContactAddress address in fields as IList<ContactAddress>)
            {
                addressParts = (new List<string> { address.StreetAddress, address.Locality, address.Region, address.PostalCode });
                unstructuredAddress = string.Join(", ", addressParts.FindAll(s => !string.IsNullOrEmpty(s)));
                output.AppendFormat("Address: {0} ({1})\n", unstructuredAddress, address.Kind);
            }
        }

        content.Visibility = Visibility.Visible;
        content.Text = output.ToString();
    }
    else
    {
        content.Visibility = Visibility.Collapsed;
    }
}
```

## <a name="complete-example-multiple-contacts"></a>Ejemplo completo (varios contactos)

En este ejemplo se usa el selector de contactos para recuperar varios contactos y luego se agregan los contactos a un control [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) denominado `OutputContacts`.

```cs
MainPage rootPage = MainPage.Current;
public IList<Contact> contacts;

private async void PickContactsButton-Click(object sender, RoutedEventArgs e)
{
    var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
    contactPicker.CommitButtonText = "Select";
    contacts = await contactPicker.PickContactsAsync();

    // Clear the ListView.
    OutputContacts.Items.Clear();

    if (contacts != null && contacts.Count > 0)
    {
        OutputContacts.Visibility = Windows.UI.Xaml.Visibility.Visible;
        OutputEmpty.Visibility = Visibility.Collapsed;

        foreach (Contact contact in contacts)
        {
            // Add the contacts to the ListView.
            OutputContacts.Items.Add(new ContactItemAdapter(contact));
        }
    }
    else
    {
        OutputEmpty.Visibility = Visibility.Visible;
    }         
}
```

``` cs
public class ContactItemAdapter
{
    public string Name { get; private set; }
    public string SecondaryText { get; private set; }

    public ContactItemAdapter(Contact contact)
    {
        Name = contact.DisplayName;
        if (contact.Emails.Count > 0)
        {
            SecondaryText = contact.Emails[0].Address;
        }
        else if (contact.Phones.Count > 0)
        {
            SecondaryText = contact.Phones[0].Number;
        }
        else if (contact.Addresses.Count > 0)
        {
            List<string> addressParts = (new List<string> { contact.Addresses[0].StreetAddress,
              contact.Addresses[0].Locality, contact.Addresses[0].Region, contact.Addresses[0].PostalCode });
              string unstructuredAddress = string.Join(", ", addressParts.FindAll(s => !string.IsNullOrEmpty(s)));
            SecondaryText = unstructuredAddress;
        }
    }
}
```

## <a name="summary-and-next-steps"></a>Resumen y pasos siguientes

Con esto deberías tener nociones básicas sobre cómo usar el selector de contactos para recuperar información de contacto. Descarga las [muestras de aplicaciones universales de Windows](http://go.microsoft.com/fwlink/p/?linkid=619979) de GitHub para ver más ejemplos de cómo usar contactos y el selector de contactos.
