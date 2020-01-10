---
title: Diseño de página para aplicaciones para UWP
description: Al diseñar la aplicación, lo primero que hay que tener en cuenta es la estructura de diseño. En este artículo se describe la estructura común de los diseños de página básicos, incluidos los elementos de interfaz de usuario que necesitará y dónde deben ir en una página. En las aplicaciones para UWP, cada página normalmente tiene elementos de navegación, comandos y contenido.
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.openlocfilehash: 7333cebc945715412e3ff1140ca26e1ed5368704
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684546"
---
# <a name="page-layout"></a>Diseño de página

En las aplicaciones para UWP, cada [**Página**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) normalmente tiene elementos de navegación, comandos y contenido. 

La aplicación puede tener varias páginas: cuando un usuario inicia una aplicación para UWP, el código de aplicación crea un [**marco**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) para colocarlo dentro de la [**ventana**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window)de la aplicación. A continuación, el marco puede [desplazarse](../basics/navigate-between-two-pages.md) entre las instancias de [**Página**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) de la aplicación. 

La mayoría de las páginas siguen una estructura de diseño común, y en este artículo se tratan los elementos de interfaz de usuario que necesitará y dónde deben ir en una página. 

![estructura de página](images/page-components.svg)

## <a name="navigation"></a>Navegación
El diseño de la aplicación comienza con el modelo de navegación que elija, que define el modo en que los usuarios navegan entre las páginas de la aplicación. En este artículo, trataremos dos patrones de navegación comunes: navegación izquierda y navegación superior. Para obtener instrucciones sobre cómo elegir otras opciones de navegación, consulte [conceptos básicos del diseño de navegación para aplicaciones para UWP](../basics/navigation-basics.md).

![patrones de navegación superior e izquierdo](images/top-left-nav.svg)

### <a name="left-nav"></a>Navegación izquierda
La navegación izquierda, o el patrón del [Panel de navegación](../controls-and-patterns/navigationview.md) , se reserva generalmente para la navegación en el nivel de la aplicación y existe en el nivel más alto dentro de la aplicación, lo que significa que siempre debe estar visible y disponible. Se recomienda la navegación izquierda cuando hay más de cinco elementos de navegación o más de cinco páginas en la aplicación. El patrón del panel de navegación contiene normalmente:
- Elementos de navegación
- Punto de entrada a la configuración de la aplicación
- Punto de entrada a la configuración de la cuenta

El control [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview) implementa el patrón de navegación izquierdo para UWP.

Cuando se selecciona un elemento de navegación, el marco debe desplazarse a la página del elemento seleccionado.

![panel de navegación expandido](images/navview-expanded.svg)

El botón de menú permite a los usuarios expandir y contraer el panel de navegación. Cuando el tamaño de la pantalla es superior a 640 PX, al hacer clic en el botón de menú se contrae el panel de navegación en una barra.

![cuadro de navegación compacto](images/navview-compact.svg)

Cuando el tamaño de la pantalla es inferior a 640 PX, el panel de navegación está totalmente contraído.

![panel de navegación mínimo](images/navview-minimal.svg)

### <a name="top-nav"></a>Navegación superior

La navegación superior también puede actuar como navegación de nivel superior. Mientras que la navegación izquierda es contraíble, la navegación superior siempre está visible. El control [NavigationView](../controls-and-patterns/navigationview.md) implementa el patrón de navegación y pestañas superior de UWP.

![navegación superior](images/pivot-large.svg)

## <a name="command-bar"></a>Barra de comandos

A continuación, es posible que desee proporcionar a los usuarios un acceso sencillo a las tareas más comunes de la aplicación. Una [barra de comandos](../controls-and-patterns/app-bars.md) puede proporcionar acceso a los comandos de nivel de aplicación o de nivel de página, y se puede usar con cualquier patrón de navegación.

![colocación de la barra de comandos en la parte superior ](images/app-bar-desktop.svg)

La barra de comandos se puede colocar en la parte superior o inferior de la página, lo que sea más adecuado para la aplicación.

![colocación de la barra de comandos en la parte inferior](images/app-bar-mobile.svg)

## <a name="content"></a>Contenido

Por último, el contenido varía mucho de una aplicación a otra, por lo que puede presentar contenido de muchas maneras diferentes. Aquí se describen algunos patrones de página comunes que podría querer usar en la aplicación. Muchas aplicaciones usan algunos o todos estos patrones de página comunes para mostrar diferentes tipos de contenido. Del mismo modo, no dude en mezclar y hacer coincidir estos patrones para optimizar la aplicación.

## <a name="landing"></a>Página de aterrizaje

![página de aterrizaje](images/hero-screen.svg)

Las páginas de aterrizaje, también conocida como pantallas prominentes, suele aparecer en el nivel superior de la experiencia de una aplicación. El gran área de superficie actúa como una fase para que las aplicaciones resalten el contenido que los usuarios puedan querer explorar y consumir.

## <a name="collections"></a>Colecciones

![galería](images/gridview.svg)

Las colecciones permiten a los usuarios examinar grupos de contenido o datos. La [vista de cuadrícula](../controls-and-patterns/item-templates-gridview.md) es una buena opción para las fotos o el contenido centrado en multimedia, y la [vista de lista](../controls-and-patterns/item-templates-listview.md) es una buena opción para el contenido con mucho texto o datos.

## <a name="masterdetail"></a>Maestro y detalles

![Maestro/detalles](images/master-detail.svg)

El modelo [maestro y detalles](../controls-and-patterns/master-details.md) consta de una vista de lista (maestra) y una vista de contenido (detalles). Ambos paneles son fijos y tienen un desplazamiento vertical. Cuando se selecciona un elemento en la vista de lista, la vista de contenido se actualiza de forma correspondiente. 

## <a name="forms"></a>Formularios
![formulario](images/form.svg)

Un [formulario](../controls-and-patterns/forms.md) es un grupo de controles que recopila y envía datos de los usuarios. La mayoría, si no todas las aplicaciones, usan algún tipo de formulario para las páginas de configuración, iniciar sesión en portales, los centros de comentarios, la creación de cuentas u otros fines. 

## <a name="sample-apps"></a>Aplicaciones de muestra
Para ver cómo se pueden implementar estos patrones, consulte nuestras [aplicaciones de ejemplo de UWP](https://developer.microsoft.com/windows/samples):
- [Reproductor de vídeo de BuildCast](https://github.com/Microsoft/BuildCast)
- [Programador de comidas](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Libro para colorear](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Base de datos de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database)