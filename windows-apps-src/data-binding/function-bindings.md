---
author: jwmsft
description: La extensión de marcado xBind permite a las funciones que se usará en el marcado.
title: 'Funciones de x: Bind'
ms.author: jimwalk
ms.date: 04/26/2018
ms.topic: article
keywords: Windows 10, uwp, xBind
ms.localizationpriority: medium
ms.openlocfilehash: 7e00762f389791fb3972b6f224759d35bf547e38
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5816448"
---
# <a name="functions-in-xbind"></a>Funciones de x: Bind

**Nota**para obtener información general sobre el uso de datos de enlace en la aplicación con **{X: Bind}** (y para realizar una comparación total entre **{X: Bind}** y **{Binding}**), consulta el [enlace de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946).

A partir de la versión 1607 de Windows 10, **{x: Bind}** admite el uso de una función como el paso hoja de la ruta de acceso de enlace. Esto permite:

- Lograr la conversión de valores de una forma más sencilla
- Obtener una manera de que los enlaces dependan de más de un parámetro

> [!NOTE]
> Para usar las funciones con **{x: Bind}**, la versión del SDK de destino mínima de la aplicación debe ser la 14393 o posterior. No puedes usar las funciones si la aplicación está destinada a versiones anteriores de Windows 10. Para obtener más información sobre las versiones de destino, consulta [Version adaptive code (Código adaptativo para versiones)](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

En el siguiente ejemplo, el primer y segundo planos del elemento están enlazados a las funciones dedicadas a realizar la conversión según el parámetro de color.

```xaml
<DataTemplate x:DataType="local:ColorEntry">
    <Grid Background="{x:Bind local:ColorEntry.Brushify(Color), Mode=OneWay}" Width="240">
        <TextBlock Text="{x:Bind ColorName}" Foreground="{x:Bind TextColor(Color)}" Margin="10,5" />
    </Grid>
</DataTemplate>
```

```csharp
class ColorEntry
{
    public string ColorName { get; set; }
    public Color Color { get; set; }

    public static SolidColorBrush Brushify(Color c)
    {
        return new SolidColorBrush(c);
    }

    public SolidColorBrush TextColor(Color c)
    {
        return new SolidColorBrush(((c.R * 0.299 + c.G * 0.587 + c.B * 0.114) > 150) ? Colors.Black : Colors.White);
    }
}
```

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object property="{x:Bind pathToFunction.FunctionName(functionParameter1, functionParameter2, ...), bindingProperties}" ... />
```

## <a name="path-to-the-function"></a>Ruta de acceso a la función

La ruta de acceso a la función se especifica como otras tantas rutas de acceso de propiedades y puede incluir puntos (.), indexadores o conversiones para localizar la función.

Las funciones estáticas pueden especificarse mediante la sintaxis XMLNamespace:ClassName.MethodName. Por ejemplo, usa la siguiente sintaxis para enlazar a las funciones estáticas en el código subyacente.

```xaml
<Page 
     xmlns:local="using:MyPage">
     ...
     <Grid x:Name="myGrid" Background="Black" >
        <TextBlock Foreground="{x:Bind local:GenerateAppropriateForeground(myGrid.Background)}" Text="Hello World!" />
    </Grid>
</Page>
```
```csharp
public class MyPage : Page
{
    public static GenerateAppropriateForeground(SolidColorBrush background)
    {
        //Implement static function
        ...
    }
}
```

También puedes usar las funciones del sistema directamente en el marcado para llevar a cabo escenarios sencillos como formato de fecha, el formato de texto, concatenaciones de texto, etc., por ejemplo:
```xaml
<Page 
     xmlns:sys="using:System"
     xmlns:local="using:MyPage">
     ...
     <CalendarDatePicker Date="{x:Bind sys:DateTime.Parse(TextBlock1.Text)}" />
     <TextBlock Text="{x:Bind sys:String.Format('{0} is now available in {1}', local:MyPage.personName, local:MyPage.location)}" />
</Page>
```

Si el modo es OneWay/TwoWay, se cambiará el proceso de detección realizado en la ruta de acceso de la función y se volverá a evaluar el enlace si se realizaron cambios en esos objetos.

La función a enlazar debe tener en cuenta lo siguiente:

- Debe ser accesible al código y a los metadatos, por lo que los métodos de trabajo interno o privado en C# (pero no en C++ o CX) deberán ser métodos públicos de WinRT.
- La sobrecarga debe basarse en el número de argumentos, no en el tipo; se intentará hacer coincidir la primera sobrecarga con el número de argumentos que haya.
- Los tipos de argumento deben coincidir con los datos que se pasan; no se realizan conversiones de restricción.
- El tipo de devolución de la función debe coincidir con el tipo de propiedad que está usando el enlace.

A partir de la siguiente actualización importante a Windows 10, el motor de enlace se reaccionar a las notificaciones de cambio de propiedad que se desencadena con el nombre de función y volver a evaluar los enlaces según sea necesario. Por ejemplo: 

```XAML
<DataTemplate x:DataType="local:Person">
   <StackPanel>
      <TextBlock Text="{x:Bind FullName}" />
      <Image Source="{x:Bind IconToBitmap(Icon, CancellationToken), Mode=OneWay}" />
   </StackPanel>
</DataTemplate>
```
```csharp
public class Person:INotifyPropertyChanged
{
    //Implementation for an Icon property and a CancellationToken property with PropertyChanged notifications
    ...

    //IconToBitmap function is essentially a multi binding converter between several options.
    public Uri IconToBitmap (Uri icon, Uri cancellationToken)
    {
        Uri foo = new Uri(...);        
        if (isCancelled)
        {
            foo = cancellationToken;
        }
        else 
        {
            if (this.fullName.Contains("Sr"))
            {
               //pass a different Uri back
               foo = new Uri(...);
            }
            else
            {
                foo = icon;
            }
        }
        return foo;
    }

    //Ensure FullName property handles change notification on itself as well as IconToBitmap since the function uses it
    public string FullName
    {
        get { return this.fullName; }
        set 
        {
            this.fullName = value;
            this.OnPropertyChanged ();
            this.OnPropertyChanged ("IconToBitmap"); 
            //this ensures Image.Source binding re-evaluates when FullName changes in addition to Icon and CancellationToken
        }
    }
}
```

> [!TIP]
> Puedes usar las funciones de x: Bind para lograr los mismos escenarios que lo que se ha admitido a través de convertidores y MultiBinding en WPF.

## <a name="function-arguments"></a>Argumentos de función

Se pueden especificar varios argumentos de función separados por comas (,)

- Ruta de acceso: debe tener la misma sintaxis que al enlazar directamente con el objeto.
  - Si el modo es OneWay/TwoWay, se realizará la detección de cambios y se volverá a evaluar el enlace en cuanto cambien los objetos.
- La cadena de la constante debe estar entre comillas: es necesario usar las comillas para designarla como una cadena. Asimismo, puedes usar el acento circunflejo (^) para evitar las comillas de las cadenas.
- Número de constante: por ejemplo, -123.456.
- Elemento booleano: especificado como "x:True" o "x:False".

### <a name="two-way-function-bindings"></a>Enlaces de funciones bidireccionales

En un escenario con un enlace bidireccional, es necesario especificar una segunda función para la dirección inversa del enlace. Esto se realiza mediante la propiedad de enlace de **restablecimiento de enlace** . En el ejemplo siguiente, la función debe tomar un argumento que es el valor que debe insertarse volver al modelo.
```xaml
<TextBlock Text="{x:Bind a.MyFunc(b), BindBack=a.MyFunc2, Mode=TwoWay}" />
```
