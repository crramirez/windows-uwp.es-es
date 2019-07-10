---
description: Muestra la imagen de avatar de una persona, si está disponible; de lo contrario, muestra las iniciales de la persona o un glifo genérico.
title: Control de imagen de persona
template: detail.hbs
label: Parallax View
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: trestar
design-contact: kimsea
dev-contact: kefodero
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1897eded4d18a00a3c11cf1926adb1ebec6ae69a
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "63791718"
---
# <a name="person-picture-control"></a>Control de imagen de persona

El control de imagen de persona muestra la imagen de avatar de una persona, si está disponible; de lo contrario, muestra las iniciales de la persona o un glifo genérico. Puedes usar el control para mostrar un [objeto Contact](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact), un objeto que administra la información de contacto de una persona, o bien puedes proporcionar manualmente la información de contacto, como un nombre para mostrar y una imagen de perfil.  

> **API importantes**: [Clase PersonPicture](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.personpicture), [Clase Contact](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact) y [Clase ContactManager](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.ContactManager)

Esta ilustración muestra dos controles de imagen de persona acompañados de dos elementos de [bloque de texto](text-block.md) que muestran los nombres de los usuarios. 
![Control de imagen de persona](images/person-picture/person-picture_hero.png)


## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa la imagen de persona cuando quieras representar una persona y su información de contacto. Estos son algunos ejemplos de cuándo es posible usar el control:
* Para mostrar el usuario actual
* Para mostrar los contactos de una libreta de direcciones
* Para mostrar el remitente de un mensaje 
* Para mostrar un contacto de redes sociales

En la ilustración se muestra el control de imagen de persona en una lista de contactos: ![Control de imagen del usuario](images/person-picture/person-picture-control.png)

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/PersonPicture">abrir la aplicación y ver PersonPicture en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-the-person-picture-control"></a>Cómo usar el control de imagen de persona

Para crear una imagen de persona, usa la clase PersonPicture. En este ejemplo se crea un control PersonPicture y se proporciona manualmente el nombre para mostrar, la imagen de perfil y las iniciales de la persona:

```xaml
<Page
    x:Class="App2.ExamplePage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App2"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <PersonPicture
            DisplayName="Betsy Sherman"
            ProfilePicture="Assets\BetsyShermanProfile.png"
            Initials="BS" />
    </StackPanel>
</Page>
```

## <a name="using-the-person-picture-control-to-display-a-contact-object"></a>Usar el control de imagen de persona para mostrar un objeto Contact

Puedes usar el control de selector de usuarios para mostrar un objeto [Contact](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact): 

```xaml
<Page
    x:Class="SampleApp.PersonPictureContactExample"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SampleApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <PersonPicture
            Contact="{x:Bind CurrentContact, Mode=OneWay}" />
            
        <Button Click="LoadContactButton_Click">Load contact</Button>
    </StackPanel>
</Page>
```

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;
using Windows.ApplicationModel.Contacts;

namespace SampleApp
{
    public sealed partial class PersonPictureContactExample : Page, System.ComponentModel.INotifyPropertyChanged
    {
        public PersonPictureContactExample()
        {
            this.InitializeComponent();
        }

        private Windows.ApplicationModel.Contacts.Contact _currentContact; 
        public Windows.ApplicationModel.Contacts.Contact CurrentContact
        {
            get => _currentContact;
            set
            {
                _currentContact = value;
                PropertyChanged?.Invoke(this,
                    new System.ComponentModel.PropertyChangedEventArgs(nameof(CurrentContact)));
            }

        }
        public event System.ComponentModel.PropertyChangedEventHandler PropertyChanged;

        public static async System.Threading.Tasks.Task<Windows.ApplicationModel.Contacts.Contact> CreateContact()
        {

            var contact = new Windows.ApplicationModel.Contacts.Contact();
            contact.FirstName = "Betsy";
            contact.LastName = "Sherman";

            // Get the app folder where the images are stored.
            var appInstalledFolder = 
                Windows.ApplicationModel.Package.Current.InstalledLocation;
            var assets = await appInstalledFolder.GetFolderAsync("Assets");
            var imageFile = await assets.GetFileAsync("betsy.png");
            contact.SourceDisplayPicture = imageFile;

            return contact;
        }

        private async void LoadContactButton_Click(object sender, RoutedEventArgs e)
        {
            CurrentContact = await CreateContact();
        }
    }
}
```

> [!NOTE]
> Para simplificar el código, en este ejemplo se crea un nuevo objeto Contact. En una aplicación real, permitirías al usuario seleccionar un contacto o usarías un [ContactManager](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.ContactManager) para consultar una lista de contactos. Para obtener información sobre cómo recuperar y administrar contactos, consulta los [Artículos de contactos y calendario](../../contacts-and-calendar/index.md). 

## <a name="determining-which-info-to-display"></a>Determinar qué información se mostrará

Cuando proporcionas un objeto [Contact](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact), el control de imagen de persona lo evalúa para determinar qué información puede mostrar. 

Si hay una imagen disponible, el control muestra la primera imagen que encuentra, en este orden:

1. LargeDisplayPicture
1. SmallDisplayPicture
1. Thumbnail

Puedes cambiar la imagen que se elige estableciendo la propiedad PreferSmallImage en true; esto proporciona a SmallDisplayPicture una prioridad mayor que a LargeDisplayPicture.

Si no hay una imagen, el control muestra el nombre o las iniciales del contacto; si no hay ningún dato de nombre, el control muestra datos de contacto, como un número de teléfono o una dirección de correo electrónico. 

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplos de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): consulta todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

* [Contactos y calendario](../../contacts-and-calendar/index.md)
* Ejemplo de tarjetas de contacto
