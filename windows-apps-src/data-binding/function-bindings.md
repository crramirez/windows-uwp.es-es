---
description: Obtenga información sobre cómo usar las funciones como el paso hoja de la ruta de acceso de enlace de datos en la extensión de marcado xBind.
title: Funciones de x:Bind
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp, xBind
ms.localizationpriority: medium
ms.openlocfilehash: 4d677767f7eb73bf46784b3f256b511e54013548
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170049"
---
# <a name="functions-in-xbind"></a>Funciones de x:Bind

> [!NOTE]
> Para obtener información general sobre el uso del enlace de datos en la aplicación con **{x:Bind}** (y para realizar una comparación total entre **{x:Bind}** y **{Binding}** ), consulta el tema [Enlace de datos en profundidad](data-binding-in-depth.md) y [Extensión de marcado {x:Bind}](../xaml-platform/x-bind-markup-extension.md).

A partir de la versión 1607 de Windows 10, **{x: Bind}** admite el uso de una función como el paso hoja de la ruta de acceso de enlace. Esto permite lo siguiente:

- Lograr la conversión de valores de una forma más sencilla
- Obtener una manera de que los enlaces dependan de más de un parámetro

> [!NOTE]
> Para usar las funciones con **{x: Bind}** , la versión del SDK de destino mínima de la aplicación debe ser la 14393 o posterior. No puedes usar las funciones si la aplicación está destinada a versiones anteriores de Windows 10. Para obtener más información sobre las versiones de destino, consulta [Version adaptive code (Código adaptativo para versiones)](../debug-test-perf/version-adaptive-code.md).

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

```xaml
<object property="{x:Bind pathToFunction.FunctionName(functionParameter1, functionParameter2, ...), bindingProperties}" ... />
```

## <a name="path-to-the-function"></a>Ruta de acceso a la función

La [ruta de acceso a la función](../xaml-platform/x-bind-markup-extension.md#property-path) se especifica como otras tantas rutas de acceso de propiedades y puede incluir [puntos](../xaml-platform/x-bind-markup-extension.md#property-path-resolution) (.), [indexadores](../xaml-platform/x-bind-markup-extension.md#collections) o [conversiones](../xaml-platform/x-bind-markup-extension.md#casting) para localizar la función.

Las funciones estáticas pueden especificarse mediante la sintaxis XMLNamespace:ClassName.MethodName. Por ejemplo, usa la sintaxis siguiente para enlazar a funciones estáticas en el código subyacente.

```xaml
<Page 
     xmlns:local="using:MyNamespace">
     ...
    <StackPanel>
        <TextBlock x:Name="BigTextBlock" FontSize="20" Text="Big text" />
        <TextBlock FontSize="{x:Bind local:MyHelpers.Half(BigTextBlock.FontSize)}" 
                   Text="Small text" />
    </StackPanel>
</Page>
```

```csharp
namespace MyNamespace
{
    static public class MyHelpers
    {
        public static double Half(double value) => value / 2.0;
    }
}
```

También puedes usar funciones del sistema directamente en el marcado para crear escenarios sencillos, como el formato de fecha, el formato de texto, las concatenaciones de texto, etc.. Por ejemplo:

```xaml
<Page 
     xmlns:sys="using:System"
     xmlns:local="using:MyNamespace">
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

El motor de enlace reacciona a las notificaciones de cambio de propiedad que se activan con el nombre de la función y vuelve a evaluar los enlaces según sea necesario. Por ejemplo:

```xaml
<DataTemplate x:DataType="local:Person">
   <StackPanel>
      <TextBlock Text="{x:Bind FullName}" />
      <Image Source="{x:Bind IconToBitmap(Icon, CancellationToken), Mode=OneWay}" />
   </StackPanel>
</DataTemplate>
```

```csharp
public class Person : INotifyPropertyChanged
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
> Puedes usar las funciones de x:Bind para lograr los mismos escenarios que se consiguen con convertidores y enlace múltiple en WPF.

## <a name="function-arguments"></a>Argumentos de función

Se pueden especificar varios argumentos de función separados por comas (,)

- Ruta de acceso: debe tener la misma sintaxis que al enlazar directamente con el objeto.
  - Si el modo es OneWay/TwoWay, se realizará la detección de cambios y se volverá a evaluar el enlace en cuanto cambien los objetos.
- La cadena de la constante debe estar entre comillas: es necesario usar las comillas para designarla como una cadena. Asimismo, puedes usar el acento circunflejo (^) para evitar las comillas de las cadenas.
- Número de constante: por ejemplo, -123.456.
- Elemento booleano: especificado como "x:True" o "x:False".

### <a name="two-way-function-bindings"></a>Enlaces de funciones bidireccionales

En un escenario con un enlace bidireccional, es necesario especificar una segunda función para la dirección inversa del enlace. Esto se hace mediante la propiedad de enlace **BindBack**. En el ejemplo siguiente, la función debe tomar un argumento que sea el valor que se debe devolver al modelo.

```xaml
<TextBlock Text="{x:Bind a.MyFunc(b), BindBack=a.MyFunc2, Mode=TwoWay}" />
```

## <a name="see-also"></a>Consulte también
* [Extensión de marcado {x:Bind}](../xaml-platform/x-bind-markup-extension.md)