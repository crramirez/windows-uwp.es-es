---
ms.assetid: 0C69521B-47E0-421F-857B-851B0E9605F2
title: Enlazar datos jerárquicos y crear una vista de tipo maestro-detalles
description: Puede hacer una vista de tipo maestro-detalles (también conocida como list-details) de varios niveles de datos jerárquicos al enlazar controles de elementos a instancias de CollectionViewSource que estén enlazadas juntas en una cadena.
---
# Enlazar datos jerárquicos y crear una vista de tipo maestro-detalles

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


> **Nota:** Consulta también [Master/detail sample](http://go.microsoft.com/fwlink/p/?linkid=619991).

Puedes hacer una vista de tipo maestro-detalles (también conocida como list-details) de varios niveles de datos jerárquicos al enlazar controles de elementos a instancias de [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) que están enlazadas juntas en una cadena. En este tema se usa la [extensión de marcado {x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) donde es posible y la extensión más flexible (pero menos eficaz) [de marcado {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) cuando es necesario.

Una estructura común de las aplicaciones de Plataforma de Windows universal (UWP) es navegar a distintas páginas de detalles cuando un usuario realiza una selección en una lista maestra. Esto es útil cuando quieres ofrecer una representación visual rica de cada elemento en cada nivel de una jerarquía. Otra opción es mostrar varios niveles de datos en una sola página. Esto es útil cuando quieres mostrar unas pocas listas sencillas que permitirán al usuario navegar rápidamente hasta el elemento que le interesa. En este tema se describe cómo implementar esta interacción. Las instancias de [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) hacen un seguimiento de la selección actual en cada nivel jerárquico.

Crearemos una vista de una jerarquía de un equipo deportivo organizada en listas de ligas, divisiones y equipos, e incluimos una vista de detalles de los equipos. Cuando seleccionas un elemento de cualquier lista, las vistas siguientes se actualizan automáticamente.

![vista maestro/detalles de una jerarquía deportiva](images/xaml-masterdetails.png)

## Requisitos previos

En este tema suponemos que sabes cómo crear una aplicación básica UWP. Si quieres obtener instrucciones para crear tu primera aplicación UWP, consulta el tema sobre cómo [crear tu primera aplicación UWP con C# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/Hh974581).

## Crear el proyecto

Crea un nuevo proyecto de **Aplicación vacía (Windows Universal)**. Asígnale el nombre "MasterDetailsBinding".

## Crear el modelo de datos

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
        public IEnumerable&lt;Team&gt; Teams { get; set; }
    }

    public class League
    {
        public string Name { get; set; }
        public IEnumerable&lt;Division&gt; Divisions { get; set; }
    }

    public class LeagueList : List&lt;League&gt;
    {
        public LeagueList()
        {
            this.AddRange(GetLeague().ToList());
        }

        public IEnumerable&lt;League&gt; GetLeague()
        {
            return from x in Enumerable.Range(1, 2)
                   select new League
                   {
                       Name = &quot;League &quot; + x,
                       Divisions = GetDivisions(x).ToList()
                   };
        }

        public IEnumerable&lt;Division&gt; GetDivisions(int x)
        {
            return from y in Enumerable.Range(1, 3)
                   select new Division
                   {
                       Name = String.Format(&quot;Division {0}-{1}&quot;, x, y),
                       Teams = GetTeams(x, y).ToList()
                   };
        }

        public IEnumerable&lt;Team&gt; GetTeams(int x, int y)
        {
            return from z in Enumerable.Range(1, 4)
                   select new Team
                   {
                       Name = String.Format(&quot;Team {0}-{1}-{2}&quot;, x, y, z),
                       Wins = 25 - (x * y * z),
                       Losses = x * y * z
                   };
        }
    }
}
```

## Crear la vista

Después, expón la clase del origen de enlace desde la clase que representa la página de marcado. Para ello, agrega una propiedad de tipo **LeagueList** a **MainPage**.

```cs
namespace MasterDetailsBinding
{
    /// &lt;summary&gt;
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// &lt;/summary&gt;
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

Por último, reemplaza el contenido del archivo MainPage.xaml por el marcado siguiente, que declara tres instancias de [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) y las enlaza juntas en una cadena. A continuación, los controles subsiguientes se pueden enlazar al elemento **CollectionViewSource** apropiado, según su nivel en la jerarquía.

```xaml
<Page
    x:Class=&quot;MasterDetailsBinding.MainPage&quot;
    xmlns=&quot;http://schemas.microsoft.com/winfx/2006/xaml/presentation&quot;
    xmlns:x=&quot;http://schemas.microsoft.com/winfx/2006/xaml&quot;
    xmlns:local=&quot;using:MasterDetailsBinding&quot;
    xmlns:d=&quot;http://schemas.microsoft.com/expression/blend/2008&quot;
    xmlns:mc=&quot;http://schemas.openxmlformats.org/markup-compatibility/2006&quot;
    mc:Ignorable=&quot;d&quot;>

    <Page.Resources>
        <CollectionViewSource x:Name=&quot;Leagues&quot;
            Source=&quot;{x:Bind ViewModel}&quot;/>
        <CollectionViewSource x:Name=&quot;Divisions&quot;
            Source=&quot;{Binding Divisions, Source={StaticResource Leagues}}&quot;/>
        <CollectionViewSource x:Name=&quot;Teams&quot;
            Source=&quot;{Binding Teams, Source={StaticResource Divisions}}&quot;/>

        <Style TargetType=&quot;TextBlock&quot;>
            <Setter Property=&quot;FontSize&quot; Value=&quot;15&quot;/>
            <Setter Property=&quot;FontWeight&quot; Value=&quot;Bold&quot;/>
        </Style>

        <Style TargetType=&quot;ListBox&quot;>
            <Setter Property=&quot;FontSize&quot; Value=&quot;15&quot;/>
        </Style>

        <Style TargetType=&quot;ContentControl&quot;>
            <Setter Property=&quot;FontSize&quot; Value=&quot;15&quot;/>
        </Style>

    </Page.Resources>

    <Grid Background=&quot;{ThemeResource ApplicationPageBackgroundThemeBrush}&quot;>

        <StackPanel Orientation=&quot;Horizontal&quot;>

            <!-- All Leagues view -->

            <StackPanel Margin=&quot;5&quot;>
                <TextBlock Text=&quot;All Leagues&quot;/>
                <ListBox ItemsSource=&quot;{Binding Source={StaticResource Leagues}}&quot; 
                    DisplayMemberPath=&quot;Name&quot;/>
            </StackPanel>

            <!-- League/Divisions view -->

            <StackPanel Margin=&quot;5&quot;>
                <TextBlock Text=&quot;{Binding Name, Source={StaticResource Leagues}}&quot;/>
                <ListBox ItemsSource=&quot;{Binding Source={StaticResource Divisions}}&quot; 
                    DisplayMemberPath=&quot;Name&quot;/>
            </StackPanel>

            <!-- Division/Teams view -->

            <StackPanel Margin=&quot;5&quot;>
                <TextBlock Text=&quot;{Binding Name, Source={StaticResource Divisions}}&quot;/>
                <ListBox ItemsSource=&quot;{Binding Source={StaticResource Teams}}&quot; 
                    DisplayMemberPath=&quot;Name&quot;/>
            </StackPanel>

            <!-- Team view -->

            <ContentControl Content=&quot;{Binding Source={StaticResource Teams}}&quot;>
                <ContentControl.ContentTemplate>
                    <DataTemplate>
                        <StackPanel Margin=&quot;5&quot;>
                            <TextBlock Text=&quot;{Binding Name}&quot; 
                                FontSize=&quot;15&quot; FontWeight=&quot;Bold&quot;/>
                            <StackPanel Orientation=&quot;Horizontal&quot; Margin=&quot;10,10&quot;>
                                <TextBlock Text=&quot;Wins:&quot; Margin=&quot;0,0,5,0&quot;/>
                                <TextBlock Text=&quot;{Binding Wins}&quot;/>
                            </StackPanel>
                            <StackPanel Orientation=&quot;Horizontal&quot; Margin=&quot;10,0&quot;>
                                <TextBlock Text=&quot;Losses:&quot; Margin=&quot;0,0,5,0&quot;/>
                                <TextBlock Text=&quot;{Binding Losses}&quot;/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ContentControl.ContentTemplate>
            </ContentControl>

        </StackPanel>

    </Grid>
</Page>
```

Ten en cuenta que mediante un enlace directo al elemento [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833), estás dando por sentado que quieres enlazar al elemento actual en los enlaces en los que la ruta de acceso no se encuentre en la propia colección. No es necesario especificar la propiedad **CurrentItem** como la ruta de acceso del enlace, aunque sí puedes hacerlo si encuentras alguna ambigüedad. Por ejemplo, el elemento [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365), que representa la vista del equipo, tiene la propiedad [**Content**](https://msdn.microsoft.com/library/windows/apps/BR209365-content) enlazada al elemento **CollectionViewSource** `Teams`. No obstante, los controles del elemento [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) se enlazan a las propiedades de la clase `Team`, porque el elemento **CollectionViewSource** facilita de forma automática el equipo seleccionado actualmente de la lista de equipos cuando es necesario.

 

 



<!--HONumber=Mar16_HO1-->


