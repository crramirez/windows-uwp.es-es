---
Description: Build UWP apps and custom/templated controls that support platform text scaling.
title: Ajuste de escala de texto
label: Text scaling
template: detail.hbs
keywords: Mostrar UWP, texto, ajuste de escala, accesibilidad, "facilidad de acceso,", "Conversión de texto más grande", interacción del usuario, entrada
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 22ad7a1ac6160fd8b1cfb70c69f299c5d89192d3
ms.sourcegitcommit: 17896441726714fa66b5ca4f9df2cdb2259f360e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/26/2018
ms.locfileid: "8988244"
---
# <a name="text-scaling"></a>Ajuste de escala de texto

![Ejemplo de 100 a 225% de escala de texto](images/coretext/text-scaling-news-hero-small.png)  
*Ejemplo de texto escalado en Windows 10 (100% a 225%)*

## <a name="overview"></a>Introducción

Lectura de texto en una pantalla de equipo (de dispositivo móvil en un portátil al monitor de escritorio a la pantalla de un dispositivo Surface Hub giant) puede ser un reto para muchas personas. Por el contrario, algunos usuarios encuentran los tamaños de fuente que se usan en las aplicaciones y sitios web para que sea más grande que es necesario.

Para garantizar que el texto sea legible como sea posible para la gama más amplia de usuarios, Windows proporciona la capacidad para que los usuarios cambiar el tamaño de fuente relativa en el sistema operativo y en aplicaciones individuales. En lugar de con una aplicación de lupa (que por lo general, solo se amplía todo el contenido dentro de un área de la pantalla y presenta sus propios problemas de facilidad de uso), cambiar la resolución de pantalla o depender de escalado de PPP (que cambia el tamaño de todo el contenido en función de la pantalla y visualización típico distancia), un usuario puede acceder rápidamente a una opción de configuración para cambiar el tamaño de solo texto, que van desde el 100% (el tamaño predeterminado) hasta 225%.

## <a name="support"></a>Compatibilidad

Aplicaciones universales de Windows (ambas estándar y PWA), compatibilidad con texto de escala de manera predeterminada.

Si la aplicación para UWP incluye controles personalizados, superficies de texto personalizado, alturas de control codificados de forma rígida, marcos anteriores o los marcos de terceros 3, es probable que deba realizar algunas actualizaciones para garantizar una experiencia coherente y útil para los usuarios.  

DirectWrite, GDI y SwapChainPanels de XAML no admiten de forma nativa escala de texto, mientras que la compatibilidad de Win32 está limitada a los menús, los iconos y las barras de herramientas.  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>Experiencia del usuario

Los usuarios pueden ajustar la escala de texto con el texto de la marca -> control deslizante más grande en la configuración de accesibilidad -> pantalla de visión o la pantalla.

![Ejemplo de 100 a 225% de escala de texto](images/coretext/text-scaling-settings-100-small.png)  
*Escala de texto desde la configuración -> Accesibilidad -> pantalla de visión o la pantalla*

## <a name="ux-guidance"></a>Directrices sobre la experiencia de usuario

Cuando se cambia el tamaño de texto, controles y contenedores deben también el tamaño y redistribuyen para acomodar el texto y su nuevo diseño. Como se mencionó anteriormente, dependiendo de la aplicación, el marco de trabajo y la plataforma, gran parte de este trabajo se realiza automáticamente. Las siguientes instrucciones de experiencia de usuario cubren los casos donde no es.

### <a name="use-the-platform-controls"></a>Usar los controles de plataforma

¿Dijimos esto ya? Vale la pena repetir: cuando sea posible, usa siempre los controles integrados que se proporcionan con los distintos marcos de aplicación de Windows para obtener la experiencia de usuario más amplia posible de la menor cantidad de esfuerzo.

Por ejemplo, todos los controles de texto UWP admiten el escalado experiencia sin necesidad de personalización o plantillas de texto completo.

Este es un fragmento de una aplicación para UWP básica que incluye un par de controles de texto estándar:

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

![Texto animado ajuste de escala del 100% a 225%](images/coretext/text-scaling.gif)  
*Ajuste de escala de texto animado*

### <a name="use-auto-sizing"></a>Usar la variación de tamaño automática

No se especifica absolutos tamaños para los controles. Siempre que sea posible, permitir que la plataforma de cambiar el tamaño de los controles automáticamente en función de la configuración de usuarios y dispositivos.  

En este fragmento de código del ejemplo anterior, se usa el `Auto` y `*` los valores de ancho de un conjunto de columnas de cuadrícula y permite que la plataforma ajustan el diseño de la aplicación en función del tamaño de los elementos contenidos en la cuadrícula.

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>Usar el ajuste de texto

Para garantizar que el diseño de la aplicación sea lo más flexible y adaptable como sea posible, habilita el ajuste de texto en cualquier control que contiene texto (muchos controles no admiten el ajuste de texto de manera predeterminada).

Si no se especifica el ajuste de texto, la plataforma usará otros métodos para ajustar el diseño, incluido el recorte (consulta el ejemplo anterior).

Este ejemplo, usamos el `AcceptsReturn` y `TextWrapping` las propiedades del cuadro de texto para garantizar que nuestro diseño es flexible como sea posible.

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![Animar el texto de escala del 100% a 225% con ajuste de texto](images/coretext/text-scaling-textwrap.gif)  
*Texto animado escalado con ajuste de texto*

### <a name="specify-text-trimming-behavior"></a>Especificar el comportamiento de recorte de texto

Si el ajuste de texto no es el comportamiento preferido, la mayoría de los controles de texto permitir que el texto de los recortes o especificar puntos suspensivos para el comportamiento de recorte de texto. Recorte es preferible al puntos suspensivos como puntos suspensivos ocupan de espacio a sí mismos.

> [!NOTE]
> Si es necesario recortar texto, imágenes al final de la cadena, y no al principio.

En este ejemplo, te mostramos cómo recortar texto en un TextBlock mediante la propiedad [TextTrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) .

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![Texto de ajuste de escala del 100% al 225% con el recorte de texto](images/coretext/text-scaling-clipping-small.png)  
*Texto de escala con el recorte de texto*

### <a name="use-a-tooltip"></a>Usar una información sobre herramientas

Si recortar texto, usa una información sobre herramientas para proporcionar el texto completo a los usuarios.

Esto, agregamos una información sobre herramientas en un bloque de texto que no es compatible con ajuste de texto:

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>No se escalan iconos basados en fuentes o símbolos

Al usar iconos basados en fuentes para enfatizar o detalle en, deshabilitar el escalado en estos caracteres.

Establecer la propiedad [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) en `false` para la mayoría de XAML controla.

### <a name="support-text-scaling-natively"></a>Escalado de forma nativa de texto de soporte técnico

Controlar el evento de sistema de [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) UISettings en tu entorno personalizados y controles. Este evento se genera cada vez que el usuario establece el factor de escala de texto de su sistema.

## <a name="summary"></a>Resumen

En este tema proporciona una visión general de soporte técnico de Windows de ajuste de escala de texto e incluye instrucciones de experiencia del usuario y de desarrollador sobre cómo personalizar la experiencia del usuario.

## <a name="related-articles"></a>Artículos relacionados

### <a name="api-reference"></a>Referencia de las API

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
