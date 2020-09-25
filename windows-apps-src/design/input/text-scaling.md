---
Description: Cree aplicaciones de Windows y controles personalizados o con plantilla que admitan el escalado de texto de plataforma.
title: Ajuste de escala de texto
label: Text scaling
template: detail.hbs
keywords: UWP, texto, escalado, accesibilidad, "facilidad de acceso", mostrar, "agrandar el texto", interacción del usuario, entrada
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d8f7536da045514471c1af1c2f0cfac74af91a7a
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219948"
---
# <a name="text-scaling"></a>Ajuste de escala de texto

![Ejemplo de escalado de texto 100% a 225%](images/coretext/text-scaling-news-hero-small.png)  
*Ejemplo de escalado de texto en Windows 10 (100% a 225%)*

## <a name="overview"></a>Información general

La lectura de texto en la pantalla de un equipo (desde el dispositivo móvil hasta el monitor de escritorio hasta la pantalla de Giant de un Surface Hub) puede resultar desafiante para muchas personas. Por el contrario, algunos usuarios encuentran los tamaños de fuente que se usan en aplicaciones y sitios web para ser más grandes de lo necesario.

Para asegurarse de que el texto sea lo más legible posible para la gama más amplia de usuarios, Windows proporciona la posibilidad de que los usuarios cambien el tamaño de fuente relativo en el sistema operativo y en las aplicaciones individuales. En lugar de usar una aplicación de lupa (que normalmente solo aumenta el tamaño de todo el contenido dentro de un área de la pantalla y tiene sus propios problemas de facilidad de uso), cambiar la resolución de pantalla o confiar en el ajuste de PPP (que cambia el tamaño de todo el contenido según la pantalla y la distancia típica de visualización), un usuario puede acceder rápidamente a un ajuste para cambiar el tamaño del texto solamente, que varía entre 100 % (tamaño predeterminado) y 225 %.

## <a name="support"></a>Soporte técnico

Las aplicaciones universales de Windows (estándar y PWA) admiten el escalado de texto de forma predeterminada.

Si la aplicación Windows incluye controles personalizados, superficies de texto personalizado, altos de control codificados de forma rígida, marcos anteriores o marcos de terceros, es probable que tenga que realizar algunas actualizaciones para garantizar una experiencia coherente y útil para los usuarios.  

DirectWrite, GDI y XAML SwapChainPanels no admiten de forma nativa el escalado de texto, mientras que la compatibilidad con Win32 se limita a menús, iconos y barras de herramientas.  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>Experiencia del usuario

Los usuarios pueden ajustar la escala de texto con el control deslizante convertir texto en más grande en la pantalla Configuración-> facilidad de acceso > visión/visualización.

![Ejemplo de escalado de texto 100% a 225%](images/coretext/text-scaling-settings-100-small.png)  
*Configuración de la escala de texto en la pantalla de configuración-> facilidad de acceso > visión y visualización*

## <a name="ux-guidance"></a>Directrices sobre la experiencia de usuario

A medida que se cambia el tamaño del texto, los controles y contenedores también deben cambiar el tamaño y el flujo para alojar el texto y su nuevo diseño. Como se mencionó anteriormente, en función de la aplicación, el marco de trabajo y la plataforma, gran parte de este trabajo se realiza automáticamente. En las siguientes instrucciones de la experiencia del usuario se tratan los casos en los que no lo es.

### <a name="use-the-platform-controls"></a>Usar los controles de plataforma

¿Ya dijimos esto? Merece la pena repetirse: cuando sea posible, use siempre los controles integrados que se proporcionan con los distintos marcos de trabajo de aplicaciones de Windows para obtener la experiencia de usuario más completa posible por la menor cantidad de esfuerzo.

Por ejemplo, todos los controles de texto UWP admiten la experiencia de escalado de texto completo sin ninguna personalización o plantilla.

Este es un fragmento de código de una aplicación de UWP básica que incluye un par de controles de texto estándar:

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

![Ajuste de escala del texto animado 100% a 225%](images/coretext/text-scaling.gif)  
*Escalado de texto animado*

### <a name="use-auto-sizing"></a>Usar el ajuste automático de tamaño

No especifique tamaños absolutos para los controles. Siempre que sea posible, permita a la plataforma cambiar el tamaño de los controles automáticamente según la configuración del usuario y del dispositivo.  

En este fragmento de código del ejemplo anterior, usamos los `Auto` `*` valores de ancho y para un conjunto de columnas de la cuadrícula y permiten que la plataforma ajuste el diseño de la aplicación en función del tamaño de los elementos contenidos en la cuadrícula.

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>Usar ajuste de texto

Para asegurarse de que el diseño de la aplicación sea lo más flexible y adaptable posible, habilite el ajuste de texto en cualquier control que contenga texto (muchos controles no admiten el ajuste de texto de forma predeterminada).

Si no especifica el ajuste de texto, la plataforma usa otros métodos para ajustar el diseño, incluido el recorte (vea el ejemplo anterior).

Aquí, usamos las `AcceptsReturn` propiedades del `TextWrapping` cuadro de texto y para asegurarnos de que el diseño sea lo más flexible posible.

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![Tamaño de texto animado de 100% a 225% con ajuste de texto](images/coretext/text-scaling-textwrap.gif)  
*Escalado de texto animado con ajuste de texto*

### <a name="specify-text-trimming-behavior"></a>Especificar el comportamiento de recorte de texto

Si el ajuste de texto no es el comportamiento preferido, la mayoría de los controles de texto permiten recortar el texto o especificar puntos suspensivos para el comportamiento de recorte de texto. El recorte es preferible a los puntos suspensivos a medida que los puntos suspensivos ocupan espacio.

> [!NOTE]
> Si necesita recortar el texto, recorte el final de la cadena, no el principio.

En este ejemplo, se muestra cómo recortar el texto en un TextBlock mediante la propiedad [TextTrimming](/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) .

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![Ajuste de escala del texto 100% a 225% con recorte de texto](images/coretext/text-scaling-clipping-small.png)  
*Ajuste de escala de texto con recorte de texto*

### <a name="use-a-tooltip"></a>Usar una información sobre herramientas

Si recorta el texto, use una información sobre herramientas para proporcionar el texto completo a los usuarios.

Aquí, agregamos una información sobre herramientas a un TextBlock que no admite el ajuste de texto:

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>No escale iconos ni símbolos basados en fuentes

Al usar iconos basados en fuentes para énfasis o decoración, deshabilite el escalado de estos caracteres.

Establezca la propiedad [IsTextScaleFactorEnabled](/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) en `false` para la mayoría de los controles XAML.

### <a name="support-text-scaling-natively"></a>Compatibilidad de escala de texto de forma nativa

Controle el evento del sistema [TextScaleFactorChanged](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) UISettings en el marco de trabajo y los controles personalizados. Este evento se desencadena cada vez que el usuario establece el factor de escala de texto en su sistema.

## <a name="summary"></a>Resumen

En este tema se proporciona información general sobre la compatibilidad con el escalado de texto en Windows e incluye experiencia del desarrollador y experiencia del usuario sobre cómo personalizar la experiencia del usuario.

## <a name="related-articles"></a>Artículos relacionados

### <a name="api-reference"></a>Referencia de API

- [IsTextScaleFactorEnabled](/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
