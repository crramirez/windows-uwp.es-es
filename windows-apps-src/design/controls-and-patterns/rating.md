---
author: QuinnRadich
description: Permite a los usuarios ver y establecer clasificaciones que reflejan el grado de satisfacción con el contenido y los servicios.
title: Control de clasificación
template: detail.hbs
ms.author: quradic
ms.date: 10/25/2017
ms.topic: article
keywords: windows10, uwp
pm-contact: abarlow
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 45cea3986f149714b1f0f14743bdecd83f8b6782
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6652895"
---
# <a name="rating-control"></a>Control de clasificación

El control de clasificación permite a los usuarios ver y establecer clasificaciones que reflejan el grado de satisfacción con el contenido y los servicios. Los usuarios pueden interactuar con el control de clasificación con función táctil, el lápiz, el mouse, el controlador para juegos y el teclado. Las siguientes instrucciones muestran cómo usar características del control de clasificación para proporcionar flexibilidad y personalización.

> **API importantes**: [Clase RatingControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.ratingcontrol)

![Ejemplo de control de clasificación](images/rating_rs2_doc_ratings_intro.png)

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/RatingControl">abrir la aplicación y ver RatingControl en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="editable-rating-with-placeholder-value"></a>Clasificación editable con el valor del marcador de posición

Quizás la manera más habitual de usar el control de clasificación es mostrar una clasificación promedio mientras se permite al usuario escribir su propio valor de clasificación. En este escenario, el control de clasificación se establece inicialmente para reflejar el promedio de clasificación de la satisfacción de todos los usuarios de un servicio concreto o el tipo de contenido (por ejemplo, música, vídeos, libros, etc.). Permanece en este estado hasta que un usuario interactúa con el control con el objetivo de clasificar un elemento de manera individual. Esta interacción cambia el estado del control de clasificaciones para reflejar la clasificación de satisfacción personal del usuario.

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

A veces es necesario mostrar clasificaciones del contenido secundario, por ejemplo, el que se muestra en contenido recomendado o al mostrar una lista de comentarios y sus clasificaciones correspondientes. En este caso, el usuario no debe poder editar la clasificación, por lo que puedes hacer que el control de solo lectura.
El modo de solo lectura también es la manera recomendada de utilizar el control de clasificación cuando se usa en listas virtualizadas de contenido muy grandes, tanto por motivos de diseño de la interfaz de usuario como de rendimiento.

![Lista larga de solo lectura](images/rating_rs2_doc_reviews.png)

Para ello, debería hacer lo siguiente:

```XAML
<RatingControl IsReadOnly="True"/>
```

## <a name="additional-functionality"></a>Funcionalidad adicional

El control de clasificación tiene muchas características adicionales que se pueden usar. En nuestra documentación de referencia de MSDN encontrará detalles para usar estas características.
Esta es una lista no completa de funcionalidad adicional:
-   Excelente rendimiento de lista larga
-   Tamaño compacto para escenarios de interfaz de usuario limitados
-   Clasificación y relleno de valor continuo
-   Personalización de espaciado
-   Deshabilitar animaciones de crecimiento
-   Personalización del número de estrellas

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.