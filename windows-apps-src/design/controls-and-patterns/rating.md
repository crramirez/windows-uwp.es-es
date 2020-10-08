---
description: Permite a los usuarios ver y establecer clasificaciones que reflejan el grado de satisfacción con el contenido y los servicios.
title: Control de clasificación
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5835993984bd0e080a1a049d20f19c96020f80f9
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91749671"
---
# <a name="rating-control"></a>Control de clasificación

El control de clasificación permite a los usuarios ver y establecer clasificaciones que reflejan el grado de satisfacción con el contenido y los servicios. Los usuarios pueden interactuar con el control de clasificación mediante la función táctil, el lápiz, el mouse, el controlador para juegos y el teclado. Las siguientes instrucciones muestran cómo usar las características del control de clasificación para proporcionar flexibilidad y personalización.

![Ejemplo de control de clasificación](images/rating_rs2_doc_ratings_intro.png)

**Obtención de la biblioteca de la interfaz de usuario de Windows**

:::row:::
   :::column:::
      ![Logotipo de WinUI](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      El control **RatingControl** se incluye como parte de la biblioteca de interfaz de usuario de Windows, un paquete NuGet que contiene nuevos controles y características de interfaz de usuario destinados a aplicaciones de Windows. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library](/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **API de la biblioteca de interfaz de usuario de Windows:** [Clase RatingControl](/uwp/api/microsoft.ui.xaml.controls.ratingcontrol)
>
> **API de plataforma:** [Clase RatingControl](/uwp/api/windows.ui.xaml.controls.ratingcontrol)

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/RatingControl">abrir la aplicación y ver RatingControl en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="editable-rating-with-placeholder-value"></a>Clasificación editable con el valor del marcador de posición

Quizás la manera más habitual de usar el control de clasificación es mostrar una clasificación promedio mientras se permite al usuario escribir su propio valor de clasificación. En este escenario, el control de clasificación se establece inicialmente para reflejar el promedio de clasificación de la satisfacción de todos los usuarios de un servicio concreto o el tipo de contenido (por ejemplo, música, vídeos, libros, etc.). Permanece en este estado hasta que un usuario interactúa con el control con el objetivo de clasificar un elemento de manera individual. Esta interacción cambia el estado del control de clasificación para reflejar la clasificación de satisfacción personal del usuario.

#### <a name="initial-average-rating-state"></a>Estado de la clasificación promedio inicial
![Estado de la clasificación promedio inicial](images/rating_rs2_doc_movie_aggregate.png)

#### <a name="representation-of-user-rating-once-set"></a>Representación de la clasificación de usuario una vez establecida

![Representación de la clasificación de usuario una vez establecida](images/rating_rs2_doc_movie_user.png)

```XAML
<RatingControl x:Name="MyRating" ValueChanged="RatingChanged"/>
```

```csharp
private void RatingChanged(RatingControl sender, object args)
{
    if (sender.Value == null)
    {
        MyRating.Caption = "(" + SomeWebService.HowManyPreviousRatings() + ")";
    }

    else
    {
        MyRating.Caption = "Your rating";
    }
}
```

### <a name="read-only-rating-mode"></a>Modo de clasificación de solo lectura

A veces es necesario mostrar clasificaciones del contenido secundario, por ejemplo, el que se muestra en contenido recomendado o al mostrar una lista de comentarios y sus clasificaciones correspondientes. En este caso, el usuario no debe poder editar la clasificación, por lo que puedes hacer que el control sea de solo lectura.
El modo de solo lectura también es la manera recomendada de usar el control de clasificación cuando se usa en listas virtualizadas de contenido muy grandes, tanto por motivos de diseño de la interfaz de usuario como de rendimiento.

![Lista larga de solo lectura](images/rating_rs2_doc_reviews.png)

Para ello, deberías hacer lo siguiente:

```XAML
<RatingControl IsReadOnly="True"/>
```

## <a name="additional-functionality"></a>Funcionalidades adicionales

El control de clasificación tiene muchas características adicionales que se pueden usar. En nuestra documentación de referencia encontrarás los detalles necesarios para usar estas características.
Esta es una lista no completa de funcionalidades adicionales:
-   Excelente rendimiento de lista larga
-   Tamaño compacto para escenarios de interfaz de usuario limitados
-   Clasificación y relleno de valor continuo
-   Personalización de espaciado
-   Deshabilitación de animaciones de crecimiento
-   Personalización del número de estrellas

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.
