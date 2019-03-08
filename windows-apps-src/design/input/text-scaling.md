---
Description: Cree aplicaciones para UWP y personalizados o controles con plantilla que admiten el escalado de texto de plataforma.
title: Ajuste de escala de texto
label: Text scaling
template: detail.hbs
keywords: Mostrar UWP, texto, el escalado, accesibilidad, "facilidad de acceso,", "Conversión de texto más grande", la interacción del usuario, la entrada
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 22ad7a1ac6160fd8b1cfb70c69f299c5d89192d3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600820"
---
# <a name="text-scaling"></a>Ajuste de escala de texto

![Ejemplo de texto escalado 100% y % de 225](images/coretext/text-scaling-news-hero-small.png)  
*Ejemplo de texto escalado en Windows 10 (100% a % de 225)*

## <a name="overview"></a>Introducción

Leer texto en una pantalla del equipo (desde el dispositivo móvil al equipo portátil al monitor de escritorio a la pantalla de un dispositivo Surface Hub gigante) puede resultar complicado para muchas personas. Por el contrario, en algunos usuarios se encuentran los tamaños de fuente utilizados en las aplicaciones y sitios web sean mayores de lo necesario.

Para asegurarse de que el texto es legible como sea posible para una amplia gama de usuarios, Windows proporciona la capacidad de los usuarios cambiar el tamaño de fuente relativo en el sistema operativo y aplicaciones individuales. En lugar de mediante una aplicación Ampliador (que normalmente solo aumenta el tamaño de todo el contenido dentro de un área de la pantalla y presenta sus propios problemas de facilidad de uso), cambiar la resolución de pantalla o confiar en la escala de PPP (que cambia el tamaño de todo el contenido basándose en la presentación y visualización típico distancia), un usuario puede acceder rápidamente a una configuración para cambiar el tamaño de sólo texto, comprendido entre 100% (el tamaño predeterminado) 225%.

## <a name="support"></a>Soporte

Aplicaciones universales de Windows (tanto estándar como y PWA), admiten texto escalado de forma predeterminada.

Si la aplicación de UWP incluye controles personalizados, superficies de texto personalizado, alto de control codificada de forma rígida, marcos de trabajo anteriores o 3rd marcos de terceros, es probable que tenga que realizar algunas actualizaciones para garantizar una experiencia coherente y útil para los usuarios.  

DirectWrite, GDI y SwapChainPanels XAML no admiten de forma nativa texto escalado, mientras que Win32 compatibilidad se limita a las barras de herramientas, iconos y menús.  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>Experiencia del usuario

Los usuarios pueden ajustar la escala de texto con la conversión de texto -> mayor control deslizante en la configuración de accesibilidad -> Visión/presentación en pantalla.

![Ejemplo de texto escalado 100% y % de 225](images/coretext/text-scaling-settings-100-small.png)  
*Configuración de la configuración de escalado de texto -> facilidad de acceso -> Visión/presentación en pantalla*

## <a name="ux-guidance"></a>Directrices sobre la experiencia de usuario

Cambiar el tamaño de texto, controles y contenedores también deben cambiar el tamaño y reflujo para alojar el texto y su nuevo diseño. Como se mencionó anteriormente, dependiendo de la aplicación, marco y plataforma, gran parte de este trabajo se realiza automáticamente. Siguiendo las instrucciones de la experiencia del usuario tratan los casos donde no es.

### <a name="use-the-platform-controls"></a>Use los controles de plataforma

¿Dijimos esto ya? Merece la pena repetirlo: Cuando sea posible, use siempre los controles integrados proporcionados con los distintos marcos de aplicación de Windows para obtener la experiencia del usuario más completa posible para la menor cantidad de esfuerzo.

Por ejemplo, todos los controles de texto UWP admiten el escalado experiencia sin ninguna personalización o la creación de plantillas de texto completo.

Este es un fragmento de código desde una aplicación UWP básica que incluye un par de controles de texto estándar:

``` xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <TextBlock Grid.Row="0" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test" 
                HorizontalTextAlignment="Center" />
    <Grid Grid.Row="1">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <Image Grid.Column="0" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
        <StackPanel Grid.Column="1" 
                    HorizontalAlignment="Center">
            <TextBlock TextWrapping="WrapWholeWords">
                The quick brown fox jumped over the lazy dog.
            </TextBlock>
            <TextBox PlaceholderText="Type something here" />
        </StackPanel>
        <Image Grid.Column="2" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
    </Grid>
    <TextBlock Grid.Row="2" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test footer" 
                HorizontalTextAlignment="Center" />
</Grid>
```

![Texto animado escalado 100% y % de 225](images/coretext/text-scaling.gif)  
*Escalar texto animado*

### <a name="use-auto-sizing"></a>Utilice el ajuste automático de tamaño

No especifique tamaños absolutos para sus controles. Siempre que sea posible, permita que la plataforma de cambiar el tamaño de los controles automáticamente según la configuración de usuario y dispositivo.  

En este fragmento de código del ejemplo anterior, usamos el `Auto` y `*` los valores de ancho de un conjunto de columnas de la cuadrícula y permita que la plataforma de ajustar el diseño de la aplicación en función del tamaño de los elementos contenidos dentro de la cuadrícula.

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>Utilice el ajuste de texto

Para asegurarse de que el diseño de la aplicación es tan flexible y adaptables como sea posible, habilitar el ajuste de texto en cualquier control que contiene texto (muchos controles no admiten el ajuste de texto de forma predeterminada).

Si no se especifica el ajuste de texto, la plataforma usa otros métodos para ajustar el diseño, incluyendo el recorte (vea el ejemplo anterior).

En este caso, usamos el `AcceptsReturn` y `TextWrapping` para asegurarse de nuestro diseño de las propiedades de cuadro de texto es tan flexible como sea posible.

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![Texto escalado 100% y % de 225 con ajuste de texto animado](images/coretext/text-scaling-textwrap.gif)  
*Texto animado escalado con ajuste de texto*

### <a name="specify-text-trimming-behavior"></a>Especificar el comportamiento de recorte de texto

Si el ajuste de texto no es el comportamiento preferido, la mayoría de los controles de texto permiten recortar el texto o especificar los puntos suspensivos para el comportamiento de recorte de texto. Recorte es preferible a puntos suspensivos como elipses ocupan un espacio a sí mismos.

> [!NOTE]
> Si desea recortar el texto, recortar el final de la cadena, y no al principio.

En este ejemplo, se muestra cómo recortar texto en un TextBlock mediante la [TextTrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) propiedad.

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![Texto escalado 100% y % de 225 con recorte de texto](images/coretext/text-scaling-clipping-small.png)  
*Texto escalado con el recorte de texto*

### <a name="use-a-tooltip"></a>Utilizar un componente tooltip

Si recortar texto, use una información sobre herramientas para proporcionar el texto completo a los usuarios.

En este caso, una información sobre herramientas se agrega a un bloque de texto que no es compatible con ajuste de texto:

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>No se escalan los iconos basados en fuente o símbolos

Al utilizar iconos basados en fuente para la decoración o énfasis, deshabilite el escalado en estos caracteres.

Establecer el [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) propiedad `false` para la mayoría de XAML controla.

### <a name="support-text-scaling-natively"></a>Texto de la compatibilidad con escala de forma nativa

Controlar la [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) UISettings evento del sistema en el marco de trabajo personalizado y controles. Este evento se provoca cada vez que el usuario establece el factor de escala de texto en su sistema.

## <a name="summary"></a>Resumen

Este tema proporciona información general de texto de ajuste de escala de soporte técnico de Windows e incluye una guía de experiencia de usuario y para desarrolladores sobre cómo personalizar la experiencia del usuario.

## <a name="related-articles"></a>Artículos relacionados

### <a name="api-reference"></a>Referencia de la API

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
