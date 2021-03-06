---
ms.assetid: 0C69521B-47E0-421F-857B-851B0E9605F2
title: Enlazar datos jerárquicos y crear una vista de tipo maestro/detalles
description: Puedes hacer una vista de tipo maestro/detalles (también conocida como lista/detalles) de varios niveles de datos jerárquicos al enlazar controles de elementos a instancias de CollectionViewSource que están enlazadas juntas en una cadena.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: adcdb638a2ccfb9684676eb205592a25b24c8432
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169969"
---
# <a name="bind-hierarchical-data-and-create-a-masterdetails-view"></a>Enlazar datos jerárquicos y crear una vista de tipo maestro/detalles



> **Nota**  Consulta también [Muestra de maestro y detalles](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlMasterDetail).

Puedes hacer una vista de tipo maestro/detalles (también conocida como lista/detalles) de varios niveles de datos jerárquicos al enlazar controles de elementos a instancias de [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) que están enlazadas juntas en una cadena. En este tema se usa la [extensión de marcado {x:Bind}](../xaml-platform/x-bind-markup-extension.md) donde es posible y la extensión más flexible (pero menos eficaz) [de marcado {Binding}](../xaml-platform/binding-markup-extension.md) cuando es necesario.

Una estructura común de las aplicaciones de Plataforma de Windows universal (UWP) es navegar a distintas páginas de detalles cuando un usuario realiza una selección en una lista maestra. Esto es útil cuando quieres ofrecer una representación visual rica de cada elemento en cada nivel de una jerarquía. Otra opción es mostrar varios niveles de datos en una sola página. Esto es útil cuando quieres mostrar unas pocas listas sencillas que permitirán al usuario navegar rápidamente hasta el elemento que le interesa. En este tema se describe cómo implementar esta interacción. Las instancias de [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) hacen un seguimiento de la selección actual en cada nivel jerárquico.

Crearemos una vista de una jerarquía de un equipo deportivo organizada en listas de ligas, divisiones y equipos, e incluimos una vista de detalles de los equipos. Cuando seleccionas un elemento de cualquier lista, las vistas siguientes se actualizan automáticamente.

![vista maestro/detalles de una jerarquía deportiva](images/xaml-masterdetails.png)

## <a name="prerequisites"></a>Requisitos previos

En este tema suponemos que sabes cómo crear una aplicación básica para UWP. Si quieres obtener instrucciones para crear tu primera aplicación UWP, consulta el tema sobre cómo [crear tu primera aplicación UWP con C# o Visual Basic](/previous-versions/windows/apps/hh974581(v=win.10)).

## <a name="create-the-project"></a>Crear el proyecto

Crea un nuevo proyecto de **Aplicación vacía (Windows Universal)** . Asígnale el nombre "MasterDetailsBinding".

## <a name="create-the-data-model"></a>Crear el modelo de datos

Agrega una nueva clase al proyecto, asígnale el nombre de ViewModel.cs y agrégale este código. Se trata de la clase de origen de enlace.

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace MasterDetailsBinding
{
    public class Team
    {
        public string Name { get; set; }
        public int Wins { get; set; }
        public int Losses { get; set; }
    }

    public class Division
    {
        public string Name { get; set; }
        public IEnumerable<Team> Teams { get; set; }
    }

    public class League
    {
        public string Name { get; set; }
        public IEnumerable<Division> Divisions { get; set; }
    }

    public class LeagueList : List<League>
    {
        public LeagueList()
        {
            this.AddRange(GetLeague().ToList());
        }

        public IEnumerable<League> GetLeague()
        {
            return from x in Enumerable.Range(1, 2)
                   select new League
                   {
                       Name = "League " + x,
                       Divisions = GetDivisions(x).ToList()
                   };
        }

        public IEnumerable<Division> GetDivisions(int x)
        {
            return from y in Enumerable.Range(1, 3)
                   select new Division
                   {
                       Name = String.Format("Division {0}-{1}", x, y),
                       Teams = GetTeams(x, y).ToList()
                   };
        }

        public IEnumerable<Team> GetTeams(int x, int y)
        {
            return from z in Enumerable.Range(1, 4)
                   select new Team
                   {
                       Name = String.Format("Team {0}-{1}-{2}", x, y, z),
                       Wins = 25 - (x * y * z),
                       Losses = x * y * z
                   };
        }
    }
}
```

## <a name="create-the-view"></a>Crear la vista

Después, expón la clase de origen de enlace desde la clase que representa la página de marcado. Para ello, agrega una propiedad de tipo **LeagueList** a **MainPage**.

```cs
namespace MasterDetailsBinding
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new LeagueList();
        }
        public LeagueList ViewModel { get; set; }
    }
}
```

Por último, reemplaza el contenido del archivo MainPage.xaml por el marcado siguiente, que declara tres instancias de [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) y las enlaza juntas en una cadena. A continuación, los controles subsiguientes se pueden enlazar al elemento **CollectionViewSource** apropiado, según su nivel en la jerarquía.

```xml
<Page
    x:Class="MasterDetailsBinding.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MasterDetailsBinding"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Page.Resources>
        <CollectionViewSource x:Name="Leagues"
            Source="{x:Bind ViewModel}"/>
        <CollectionViewSource x:Name="Divisions"
            Source="{Binding Divisions, Source={StaticResource Leagues}}"/>
        <CollectionViewSource x:Name="Teams"
            Source="{Binding Teams, Source={StaticResource Divisions}}"/>

        <Style TargetType="TextBlock">
            <Setter Property="FontSize" Value="15"/>
            <Setter Property="FontWeight" Value="Bold"/>
        </Style>

        <Style TargetType="ListBox">
            <Setter Property="FontSize" Value="15"/>
        </Style>

        <Style TargetType="ContentControl">
            <Setter Property="FontSize" Value="15"/>
        </Style>

    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <StackPanel Orientation="Horizontal">

            <!-- All Leagues view -->

            <StackPanel Margin="5">
                <TextBlock Text="All Leagues"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Leagues}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- League/Divisions view -->

            <StackPanel Margin="5">
                <TextBlock Text="{Binding Name, Source={StaticResource Leagues}}"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Divisions}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- Division/Teams view -->

            <StackPanel Margin="5">
                <TextBlock Text="{Binding Name, Source={StaticResource Divisions}}"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Teams}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- Team view -->

            <ContentControl Content="{Binding Source={StaticResource Teams}}">
                <ContentControl.ContentTemplate>
                    <DataTemplate>
                        <StackPanel Margin="5">
                            <TextBlock Text="{Binding Name}" 
                                FontSize="15" FontWeight="Bold"/>
                            <StackPanel Orientation="Horizontal" Margin="10,10">
                                <TextBlock Text="Wins:" Margin="0,0,5,0"/>
                                <TextBlock Text="{Binding Wins}"/>
                            </StackPanel>
                            <StackPanel Orientation="Horizontal" Margin="10,0">
                                <TextBlock Text="Losses:" Margin="0,0,5,0"/>
                                <TextBlock Text="{Binding Losses}"/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ContentControl.ContentTemplate>
            </ContentControl>

        </StackPanel>

    </Grid>
</Page>
```

Ten en cuenta que mediante un enlace directo al elemento [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource), estás dando por sentado que quieres enlazar al elemento actual en los enlaces en los que la ruta de acceso no se encuentre en la propia colección. No es necesario especificar la propiedad **CurrentItem** como la ruta de acceso del enlace, aunque sí puedes hacerlo si encuentras alguna ambigüedad. Por ejemplo, el elemento [**ContentControl**](/uwp/api/Windows.UI.Xaml.Controls.ContentControl), que representa la vista del equipo, tiene la propiedad [**Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content) enlazada al elemento **CollectionViewSource** de `Teams`. No obstante, los controles del elemento [**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate) se enlazan a las propiedades de la clase `Team`, porque el elemento **CollectionViewSource** facilita de forma automática el equipo seleccionado actualmente de la lista de equipos cuando es necesario.

 

 