---
title: Diseño de página para aplicaciones UWP
description: Al diseñar la aplicación, lo primero que hay que tener en cuenta es la estructura de diseño. Este artículo describe la estructura común de los diseños de página básica, incluido qué elementos de interfaz de usuario necesita y dónde debe ir en una página. En aplicaciones UWP, cada página tiene generalmente la navegación, comandos y elementos de contenido.
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.openlocfilehash: edda9948e70b1757ddb46fae45a579bb2fdb8de1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633720"
---
# <a name="page-layout"></a>Diseño de página

En aplicaciones UWP, cada [ **página** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) generalmente tiene navegación, comandos y elementos de contenido. 

La aplicación puede tener varias páginas: cuando un usuario inicia una aplicación para UWP, el código de la aplicación crea un [ **marco** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) colocar dentro de la aplicación [ **ventana** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window). El marco puede, a continuación, [vaya](../basics/navigate-between-two-pages.md) entre la aplicación [ **página** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) instancias. 

Mayoría de las páginas sigue una estructura de diseño comunes y este artículo explica qué interfaz de usuario elementos necesitará y dónde debe ir en una página. 

![estructura de la página](images/page-components.svg)

## <a name="navigation"></a>Navegación
El diseño de la aplicación comienza con el modelo de navegación que elija, que define cómo los usuarios navegan entre páginas de la aplicación. En este artículo, analizaremos dos patrones de navegación comunes: deja el panel de navegación y panel de navegación superior. Para obtener instrucciones sobre cómo elegir otras opciones de navegación, consulte [conceptos básicos del diseño de navegación para aplicaciones UWP](../basics/navigation-basics.md).

![patrones de navegación superior e izquierdo](images/top-left-nav.svg)

### <a name="left-nav"></a>Barra de navegación izquierda
Barra de navegación, izquierda o la [panel de navegación](../controls-and-patterns/navigationview.md) de patrón, se reserva generalmente para la navegación de nivel de aplicación y existe en el nivel superior dentro de la aplicación, lo que significa que siempre debe estar visible y disponible. Se recomienda la barra de navegación izquierda cuando hay más de cinco elementos de navegación o más de cinco páginas de la aplicación. Normalmente, contiene el patrón del panel de navegación:
- Elementos de navegación
- Punto de entrada a la configuración de la aplicación
- Punto de entrada a la configuración de la cuenta

El [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview) control implementa el patrón de la barra de navegación izquierda para UWP.

Cuando se selecciona un elemento de navegación, el marco debe navegar hasta la página del elemento seleccionado.

![panel de navegación expandido](images/navview-expanded.svg)

El botón de menú permite a los usuarios expandir y contraer el panel de navegación. Cuando el tamaño de pantalla es mayor que 640 px, al hacer clic en el botón de menú contrae el panel de navegación en una barra.

![panel de navegación compact](images/navview-compact.svg)

Cuando el tamaño de pantalla es menor que 640 px, el panel de navegación se contrae por completo.

![panel de navegación mínima](images/navview-minimal.svg)

### <a name="top-nav"></a>Barra de navegación superior

Barra de navegación superior puede actuar también como navegación de nivel superior. Aunque es contraíble barra de navegación izquierda, superior nav siempre está visible. El [NavigationView](../controls-and-patterns/navigationview.md) control implementa el patrón de navegación y pestañas principales para UWP.

![navegación superior](images/pivot-large.svg)

## <a name="command-bar"></a>Barra de comandos

A continuación, es posible que desee proporcionar a los usuarios un acceso sencillo a las tareas más comunes de la aplicación. Un [barra de comandos](../controls-and-patterns/app-bars.md) puede proporcionar acceso a los comandos de nivel de página o aplicación, y se puede usar con cualquier modelo de navegación.

![colocación de la barra de comandos en la parte superior ](images/app-bar-desktop.svg)

La barra de comandos se puede colocar en la parte superior o inferior de la página, lo que sea más adecuado para su aplicación.

![colocación de la barra de comandos en la parte inferior](images/app-bar-mobile.svg)

## <a name="content"></a>Contenido

Por último, el contenido varía de un aplicación a otro, por lo que puede presentar el contenido de muchas maneras diferentes. En este caso, se describen algunos patrones comunes de página que desea usar en la aplicación. Muchas aplicaciones usan algunos o todos estos patrones de página comunes para mostrar diferentes tipos de contenido. Del mismo modo, no dude en mezclar y combinar estos patrones para optimizar la aplicación.

## <a name="landing"></a>Página de aterrizaje

![página de aterrizaje](images/hero-screen.svg)

Las páginas de aterrizaje, también conocida como pantallas prominentes, suele aparecer en el nivel superior de la experiencia de una aplicación. El gran área de superficie actúa como una fase para que las aplicaciones resalten el contenido que los usuarios puedan querer explorar y consumir.

## <a name="collections"></a>Colecciones

![galería](images/gridview.svg)

Las colecciones permiten a los usuarios examinar grupos de contenido o datos. La [vista de cuadrícula](../controls-and-patterns/item-templates-gridview.md) es una buena opción para las fotos o el contenido centrado en multimedia, y la [vista de lista](../controls-and-patterns/item-templates-listview.md) es una buena opción para el contenido con mucho texto o datos.

## <a name="masterdetail"></a>Maestro y detalles

![maestro y detalles](images/master-detail.svg)

El modelo [maestro y detalles](../controls-and-patterns/master-details.md) consta de una vista de lista (maestra) y una vista de contenido (detalles). Ambos paneles son fijos y tienen un desplazamiento vertical. Cuando se selecciona un elemento en la vista de lista, la vista de contenido se actualiza en consecuencia. 

## <a name="forms"></a>Formularios
![formulario](images/form.svg)

Un [formulario](../controls-and-patterns/forms.md) es un grupo de controles que recopila y envía datos de los usuarios. La mayoría, si no todas las aplicaciones, usan algún tipo de formulario para las páginas de configuración, iniciar sesión en portales, los centros de comentarios, la creación de cuentas u otros fines. 

## <a name="sample-apps"></a>Aplicaciones de muestra
Para ver cómo se pueden implementar estos patrones, consulte nuestra [aplicaciones de ejemplo UWP](https://developer.microsoft.com/en-us/windows/samples):
- [Reproductor de BuildCast vídeo](https://github.com/Microsoft/BuildCast)
- [Programador de comidas](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Libro para colorear](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Base de datos de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database)