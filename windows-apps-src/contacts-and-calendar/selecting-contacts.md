---
description: Mediante el espacio de nombres Windows.ApplicationModel.Contacts, tienes varias opciones para seleccionar contactos.
title: Seleccionar contactos
ms.assetid: 35FEDEE6-2B0E-4391-84BA-5E9191D4E442
keywords: contactos, seleccionar
keywords: seleccionar un solo contacto
keywords: seleccionar varios contactos
keywords: contactos, seleccionar varios
keywords: seleccionar datos específicos de contactos
keywords: contacto, selección de datos específicos
keywords: contacto, selección de campos específicos
---

# Seleccionar contactos

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Mediante el espacio de nombres [**Windows.ApplicationModel.Contacts**](https://msdn.microsoft.com/library/windows/apps/BR225002), tienes varias opciones para seleccionar contactos. Aquí te mostraremos cómo seleccionar un único contacto o varios contactos, y aprenderás a configurar el selector de contactos para recuperar solamente la información de contacto que necesita tu aplicación.

## Configuración del selector de contactos

Crea una instancia de [**Windows.ApplicationModel.Contacts.ContactPicker**](https://msdn.microsoft.com/library/windows/apps/BR224913) y asígnala a una variable.

```cs
var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
```

## Establecer el modo de selección (opcional)

De manera predeterminada, el selector de contactos recupera todos los datos disponibles para los contactos que selecciona el usuario. La propiedad [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/BR224913-selectionmode) permite configurar el selector de contactos para recuperar únicamente los campos de datos que necesita la aplicación. Esta es una forma más eficaz de usar el selector de contactos si solo necesitas un subconjunto de los datos de contacto disponibles.

Primero, establece la propiedad [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/BR224913-selectionmode) en **Fields**:

```cs
contactPicker.SelectionMode = Windows.ApplicationModel.Contacts.ContactSelectionMode.Fields;
```

Luego, usa la propiedad [**desiredFieldsWithContactFieldType**](https://msdn.microsoft.com/library/windows/apps/BR224913-desiredfieldswithcontactfieldtype) para especificar los campos que quieres que recupere el selector de contactos. En este ejemplo se configura el selector de contactos para que recupere direcciones de correo electrónico:

``` cs
contactPicker.DesiredFieldsWithContactFieldType.Add(Windows.ApplicationModel.Contacts.ContactFieldType.Email);
```

## Inicio del selector

```cs
Contact contact = await contactPicker.PickContactAsync();
```

Usa [**pickContactsAsync**](https://msdn.microsoft.com/library/windows/apps/BR224913-pickcontactsasync) si quieres que el usuario seleccione uno o varios contactos.

```cs
public IList&lt;Contact&gt; contacts;
contacts = await contactPicker.PickContactsAsync();
```

## Procesar los contactos

Cuando vuelva el selector, comprueba si el usuario seleccionó algún contacto. Si es así, procesa la información de contacto.

En este ejemplo se muestra cómo procesar un solo contacto. Aquí se recupera el nombre del contacto y se copia en un control [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) denominado *OutputName*.

```cs
if (contact != null)
{
    OutputName.Text = contact.DisplayName;
}
else
{
    rootPage.NotifyUser(&quot;No contact was selected.&quot;, NotifyType.ErrorMessage);
}
```

En este ejemplo se muestra cómo procesar varios contactos.

```cs
if (contacts != null &amp;&amp; contacts.Count &gt; 0)
{
    foreach (Contact contact in contacts)
    {
        // Do something with the contact information.
    }
}
```

## Ejemplo completo (contacto único)

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

private void AppendContactFieldValues&lt;T&gt;(TextBlock content, IList&lt;T&gt; fields)
{
    if (fields.Count &gt; 0)
    {
        StringBuilder output = new StringBuilder();

        if (fields[0].GetType() == typeof(ContactEmail))
        {
            foreach (ContactEmail email in fields as IList&lt;ContactEmail&gt;)
            {
                output.AppendFormat(&quot;Email: {0} ({1})\n&quot;, email.Address, email.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactPhone))
        {
            foreach (ContactPhone phone in fields as IList&lt;ContactPhone&gt;)
            {
                output.AppendFormat(&quot;Phone: {0} ({1})\n&quot;, phone.Number, phone.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactAddress))
        {
            List&lt;String&gt; addressParts = null;
            string unstructuredAddress = &quot;&quot;;

            foreach (ContactAddress address in fields as IList&lt;ContactAddress&gt;)
            {
                addressParts = (new List&lt;string&gt; { address.StreetAddress, address.Locality, address.Region, address.PostalCode });
                unstructuredAddress = string.Join(&quot;, &quot;, addressParts.FindAll(s =&gt; !string.IsNullOrEmpty(s)));
                output.AppendFormat(&quot;Address: {0} ({1})\n&quot;, unstructuredAddress, address.Kind);
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

## Ejemplo completo (varios contactos)

En este ejemplo se usa el selector de contactos para recuperar varios contactos y luego se agregan los contactos a un control [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) denominado `OutputContacts`.

```cs
MainPage rootPage = MainPage.Current;
public IList&lt;Contact&gt; contacts;

private async void PickContactsButton-Click(object sender, RoutedEventArgs e)
{
    var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
    contactPicker.CommitButtonText = &quot;Select&quot;;
    contacts = await contactPicker.PickContactsAsync();

    // Clear the ListView.
    OutputContacts.Items.Clear();

    if (contacts.Count &gt; 0)
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
        if (contact.Emails.Count &gt; 0)
        {
            SecondaryText = contact.Emails[0].Address;
        }
        else if (contact.Phones.Count &gt; 0)
        {
            SecondaryText = contact.Phones[0].Number;
        }
        else if (contact.Addresses.Count &gt; 0)
        {
            List&lt;string&gt; addressParts = (new List&lt;string&gt; { contact.Addresses[0].StreetAddress, 
              contact.Addresses[0].Locality, contact.Addresses[0].Region, contact.Addresses[0].PostalCode });
              string unstructuredAddress = string.Join(&quot;, &quot;, addressParts.FindAll(s =&gt; !string.IsNullOrEmpty(s)));
            SecondaryText = unstructuredAddress;
        }
    }
}
```

## Resumen y pasos siguientes

Con esto deberías tener nociones básicas sobre cómo usar el selector de contactos para recuperar información de contacto. Descarga las [muestras de aplicaciones universales de Windows](http://go.microsoft.com/fwlink/p/?linkid=619979) de GitHub para ver más ejemplos de cómo usar contactos y el selector de contactos.



<!--HONumber=Mar16_HO1-->


