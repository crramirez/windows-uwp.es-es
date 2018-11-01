---
author: stevewhims
title: Tareas iniciales con animación
ms.assetid: C1C3F5EA-B775-4700-9C45-695E78C16205
description: En este proyecto, moveremos un rectángulo, le aplicaremos un efecto de atenuación y después lo volveremos a mostrar.
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4822a436225bea92fdf1e981ad33378996adefe4
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "5920446"
---
# <a name="getting-started-animation"></a>Introducción: animación


## <a name="adding-animations"></a>Agregar animaciones

En iOS, los efectos de animación se crean casi siempre mediante programación. Por ejemplo, puedes usar las animaciones que proporcionan los métodos **animateWithDuration** basados en bloques de la clase **UIView** o los métodos anteriores no basados en bloques. Asimismo, también puedes usar explícitamente la clase **CALayer** para animar capas. Las animaciones de las aplicaciones de Windows se pueden crear mediante programación, pero también pueden definirse mediante declaración con lenguaje XAML. Puedes usar Microsoft Visual Studio para editar el código XAML directamente, pero Visual Studio también incluye una herramienta llamada **Blend**, que crea el código XAML por ti a medida que trabajas con animaciones en un diseñador. De hecho, Blend te permite abrir, diseñar, generar y ejecutar proyectos completos de Visual Studio gráficamente. El tutorial siguiente te permitirá probarlo.

Crea una nueva aplicación para la Plataforma universal de Windows (UWP) y asígnale un nombre parecido a "SimpleAnimation". En este proyecto, moveremos un rectángulo, le aplicaremos un efecto de atenuación y después lo volveremos a mostrar. Las animaciones en XAML se basan en el concepto de *guiones gráficos* (no deben confundirse con los guiones gráficos de iOS). Los guiones gráficos usan *fotogramas clave* para animar los cambios en las propiedades.

Con el proyecto abierto, en el **Explorador de soluciones**, haz clic con el botón derecho en el nombre del proyecto y, a continuación, selecciona **Abrir en Blend** o **Diseñar en Blend**, tal como se muestra en la ilustración siguiente. Visual Studio continúa ejecutándose en segundo plano.

![comando de menú Abrir en Blend](images/ios-to-uwp/vs-open-in-blend.png)

Después de iniciar Blend, deberías ver algo parecido a lo que muestra la ilustración siguiente.

![entorno de desarrollo de Blend](images/ios-to-uwp/blend-1.png)

Haga doble clic en **MainPage.xaml** en el **Explorador de soluciones** situado en el lado izquierdo. A continuación, en la tira vertical de herramientas situada en el borde de la **vista Diseño** central, selecciona la herramienta **Rectángulo** y, a continuación, dibuja un rectángulo en la **vista Diseño**, tal como se muestra en la siguiente ilustración.

![agregar un rectángulo en la vista Diseño](images/ios-to-uwp/blend-2.png)

Para que el rectángulo sea de color verde, busca la ventana **Propiedades** y, en el área **Pincel**, haz clic en el botón **Pincel de color sólido** y luego en el icono **Cuentagotas de color**. Haz clic en alguna parte de la banda de tonos verdes.

Para empezar a animar el rectángulo, en la ventana **Objetos y escala de tiempo**, pulsa el botón del signo más (**Nuevo**) tal como se muestra en la ilustración siguiente y después pulsa **Aceptar**.

![agregar un guion gráfico](images/ios-to-uwp/blend-3.png)

Es entonces cuando aparece un guion gráfico en la ventana **Objetos y escala de tiempo** (puede que tengas que cambiar el tamaño de la vista para verla correctamente). La **vista Diseño** muestra cambios que indican que **la grabación de la escala de tiempo de Storyboard1 está activa**. Para capturar el estado actual del rectángulo, en la ventana **Objetos y línea de tiempo**, pulsa el botón **Registrar fotograma clave** que se encuentra justo encima de la flecha amarilla, tal como se muestra en la ilustración siguiente.

![registrar un fotograma clave](images/ios-to-uwp/blend-4.png)

A continuación, mueve el rectángulo y atenúalo. Para ello, arrastra la flecha naranja/amarilla a la posición de dos segundos y, a continuación, arrastra el rectángulo verde ligeramente hacia la derecha. Una vez hecho esto, en la ventana **Propiedades** del área **Apariencia**, cambia la propiedad **Opacidad** a **0**, tal como se muestra en la ilustración siguiente. Para obtener una vista previa de la animación, pulsa el botón **Reproducir** en el panel de guion gráfico.

![ventana Propiedades y botón Reproducir](images/ios-to-uwp/blend-5.png)

A continuación, vuelve a mostrar el rectángulo. En la ventana **Objetos y escala de tiempo**, haz doble clic en **Storyboard1**. A continuación, en la ventana **Propiedades** del área **Común**, selecciona **AutoReverse**, tal como se muestra en la ilustración siguiente.

![seleccionar un guion gráfico](images/ios-to-uwp/blend-6.png)

Por último, haz clic en el botón **Reproducir** para ver qué sucede.

Puedes compilar y ejecutar el proyecto haciendo clic en el botón de ejecución verde en la parte superior de la ventana (también puedes presionar F5). Si lo haces, verás que el proyecto efectivamente se compila y ejecuta, pero el rectángulo verde no se mueve ni un ápice, como un bebé plantado en el pasillo de un supermercado junto al dulce que sus padres no quieren comprarle. Para iniciar la animación, tienes que agregar una línea de código al proyecto. Haz lo siguiente:

Guarda el proyecto abriendo el menú **Archivo** y seleccionando **Guardar MainPage.xaml**. Vuelve a Visual Studio. Si Visual Studio muestra un cuadro de diálogo que pregunta si deseas volver a cargar el archivo modificado, selecciona **Sí**. Haz doble clic en el archivo **MainPage.xaml.cs** (que está oculto en **MainPage.xaml**) para abrirlo, y agrega el siguiente código justo encima del método público MainPage():

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Add the following line of code.
    Storyboard1.Begin();
}
```

Vuelve a ejecutar el proyecto y verás que el rectángulo está animado. ¡Bien!

Si abres el archivo MainPage.xaml en la vista **XAML**, verás el código XAML que Blend ha agregado mientras trabajabas en el diseñador. En especial, fíjate en el código de los elementos `<Storyboard>` y `<Rectangle>`. El código siguiente muestra un ejemplo. Los puntos suspensivos indican código no relacionado que se omite por razones de brevedad; se han agregado saltos de línea para facilitar la lectura del código.

```xml
...
<Storyboard 
        x:Name="Storyboard1" 
        AutoReverse="True">
    <DoubleAnimationUsingKeyFrames 
            Storyboard.TargetProperty="(UIElement.RenderTransform).(CompositeTransform.TranslateX)"
            Storyboard.TargetName="rectangle">
        <EasingDoubleKeyFrame 
                KeyTime="0" 
                Value="0"/>
        <EasingDoubleKeyFrame 
                KeyTime="0:0:2" 
                Value="185.075"/>
    </DoubleAnimationUsingKeyFrames>
    <DoubleAnimationUsingKeyFrames 
            Storyboard.TargetProperty="(UIElement.RenderTransform).(CompositeTransform.TranslateY)" 
            Storyboard.TargetName="rectangle">
        <EasingDoubleKeyFrame 
                KeyTime="0" 
                Value="0"/>
        <EasingDoubleKeyFrame 
                KeyTime="0:0:2" 
                Value="2.985"/>
    </DoubleAnimationUsingKeyFrames>
    <DoubleAnimationUsingKeyFrames 
            Storyboard.TargetProperty="(UIElement.Opacity)" 
            Storyboard.TargetName="rectangle">
        <EasingDoubleKeyFrame 
                KeyTime="0" 
                Value="1"/>
        <EasingDoubleKeyFrame 
                KeyTime="0:0:2"
                Value="0"/>
    </DoubleAnimationUsingKeyFrames>
</Storyboard>
...
<Rectangle 
        x:Name="rectangle" 
        Fill="#FF00FF63" 
        HorizontalAlignment="Left" 
        Height="122" 
        Margin="151,312,0,0" 
        Stroke="Black" 
        VerticalAlignment="Top" 
        Width="239" 
        RenderTransformOrigin="0.5,0.5">
    <Rectangle.RenderTransform>
        <CompositeTransform/>
    </Rectangle.RenderTransform>
</Rectangle>
...
```

Puedes modificar este XAML manualmente o regresar a Blend y seguir trabajando desde allí. Con Blend es más divertido crear interfaces de usuario atractivas, y la capacidad para animarlas mediante una herramienta gráfica puede acelerar considerablemente el tiempo de desarrollo. Para obtener más información sobre las animaciones, consulta [Información general sobre animaciones](https://msdn.microsoft.com/library/windows/apps/mt187350).

**Nota**para obtener información sobre las animaciones de <span class="legacy-term">las aplicaciones</span>para UWP con JavaScript y HTML, consulta la [animación de la interfaz de usuario (HTML)](https://msdn.microsoft.com/library/windows/apps/hh465165).

### <a name="next-step"></a>Paso siguiente

[Introducción: A continuación](getting-started-what-next.md)
