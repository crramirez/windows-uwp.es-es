---
title: Notas de la versión de WinUI 2.1
description: Notas de la versión de WinUI 2.1, incluidas las nuevas características y correcciones de errores.
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: f362c4ae7654d6ef3b888b908c4779fce62af5fe
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493090"
---
# <a name="windows-ui-library-21"></a>Biblioteca de interfaz de usuario de Windows 2.1

La última versión oficial de la biblioteca de interfaz de usuario de Windows, WinUI 2.1, se publicó el 8 de abril de 2019. 

WinUI le ofrece muchas de las características más recientes de la plataforma de la experiencia del usuario de Windows, incluidos los controles y estilos actualizados de Fluent, disponibles para usar de inmediato y compatibles con la Actualización de aniversario de Windows 10 (14393). La [galería de controles XAML](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/#xaml-controls-gallery) proporciona ejemplos para explorar las nuevas e interesantes características que se han agregado a la biblioteca.

Descarga del [paquete WinUI 2.1 NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190405004)

Puede optar por usar paquetes de WinUI en la aplicación mediante el administrador de paquetes NuGet: consulte [Introducción a la biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/getting-started) para obtener más información.

WinUI es un proyecto de código abierto que se hospeda en GitHub. Agradecemos los informes de errores, las solicitudes de características y las contribuciones de código de la comunidad en el [repositorio de la biblioteca de interfaz de usuario de Windows](https://aka.ms/winui).

## <a name="whats-new-in-this-release"></a>Novedades de esta versión

### <a name="itemsrepeater"></a>ItemsRepeater

Use un control ItemsRepeater para crear experiencias de colección personalizadas con un sistema de diseño flexible, vistas personalizadas y virtualización.
A diferencia de ListView, ItemsRepeater no proporciona una experiencia de usuario final completa, ya que no tiene una interfaz de usuario predeterminada y no proporciona directivas para el foco, la selección o la interacción del usuario. En su lugar, es un bloque de creación que puede usar para crear sus propios controles personalizados y experiencias únicas basadas en la colección. Admite la creación de experiencias más satisfactorias y productivas.

![Ejemplo](../images/ItemsRepeater%20-%20MSN%20News.gif)

[Documentación](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/items-repeater)

### <a name="animatedvisualplayer"></a>AnimatedVisualPlayer

El control AnimatedVisualPlayer hospeda y controla la reproducción de elementos visuales animados, lo que le permite agregar gráficos de movimiento personalizados de alto rendimiento a su aplicación. Por ejemplo, el control AnimatedVisualPlayer se usa para mostrar y controlar animaciones de Lottie.

![Ejemplo](../images/AnimatedVisualPlayerUpdated.gif)

[Documentación](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie)

### <a name="teachingtip"></a>TeachingTip

El control TeachingTip proporciona una forma atractiva y fluida para que las aplicaciones guíen e informen a los usuarios con sugerencias no invasivas y ricas en contenido. TeachingTip puede enfocar características nuevas o importantes, enseñar a los usuarios a realizar tareas y mejorar el flujo de trabajo, para lo cual proporciona información contextualmente pertinente para la tarea que está realizando.

![Ejemplo](../images/TeachingTipUpdated.gif)

[Documentación](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/teaching-tip)

### <a name="radiomenuflyoutitem"></a>RadioMenuFlyoutItem

Incluye la posibilidad de tener las opciones de estilo de "Botón de radio" en un control MenuBar. Esto habilita grupos de opciones con viñetas vinculadas como un grupo de botones de radio. La lógica se controla para el desarrollador.

![Ejemplo](../images/RadioMenuFlyoutItem1.png)

[Documentación](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus#create-a-menu-flyout-or-a-context-menu)

### <a name="compactdensity"></a>CompactDensity

El modo compacto permite a los desarrolladores crear experiencias cómodas para cualquier número de escenarios. Con solo agregar un diccionario de recursos, la aplicación puede admitir un promedio aproximada de un 33 % más de interfaz de usuario.

![Ejemplo de densidad compacta](../images/CompactDensityUpdated.png)

[Documentación](https://docs.microsoft.com/windows/uwp/design/style/spacing )

### <a name="shadows"></a>Sombras

![Ejemplo](../images/shadow.gif)

La creación de una jerarquía visual de los elementos de la interfaz de usuario facilita el análisis de la interfaz de usuario y transmite en qué debe centrarse el usuario. La acción de adelantar determinados elementos de la interfaz de usuario, Elevación, a menudo se usa para lograr esta jerarquía en el software. 

Con la actualización de mayo de 2019 de Windows 10, muchos de nuestros controles comunes agregan elevación mediante la sombra y la profundidad de z de forma predeterminada. Los controles NavigationView y TeachingTip de WinUI 2.1 también tendrán sombras predeterminadas cuando se ejecuten en un sistema operativo con la actualización de mayo de 2019 de Windows 10. La lista completa de controles que tienen sombras predeterminadas y el procedimiento de uso de API adicionales estarán disponibles una vez que se publique la actualización de mayo de 2019 de Windows 10, y el vínculo se publicará aquí.

## <a name="examples"></a>Ejemplos

La aplicación de ejemplo XAML Controls Gallery incluye demostraciones interactivas y código de ejemplo para usar controles WinUI.

* Instalación de la aplicación XAML Controls Gallery desde [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt).

* La aplicación XAML Controls Gallery también está disponible en [código abierto en GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery).

## <a name="documentation"></a>Documentación

Se incluyen artículos sobre procedimientos para los controles de la biblioteca de interfaz de usuario de Windows con la [documentación sobre controles de la Plataforma universal de Windows](/windows/uwp/design/controls-and-patterns/).

Los documentos de referencia de API se encuentran aquí: [API de la biblioteca de interfaz de usuario de Windows](/uwp/api/overview/winui/).

## <a name="microsoftuixaml-21-version-history"></a>Historial de versiones de Microsoft.UI.Xaml 2.1

### <a name="microsoftuixaml-21-official-release"></a>Versión oficial de Microsoft.UI.Xaml 2.1

Abril de 2019

[Página de versiones de GitHub](https://github.com/Microsoft/microsoft-ui-xaml/releases)

[Descarga de paquetes NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190405004)

#### <a name="new-feature-not-included-in-earlier-pre-releases"></a>Nueva característica (no incluida en versiones preliminares anteriores)

* [CompactDensity](https://docs.microsoft.com/windows/uwp/design/style/spacing): el modo compacto permite a los desarrolladores crear experiencias cómodas para cualquier número de escenarios. Con solo agregar un diccionario de recursos, la aplicación puede admitir un promedio aproximada de un 33 % más de interfaz de usuario.

* Sombras: la creación de una jerarquía visual de los elementos de la interfaz de usuario facilita el análisis de la interfaz de usuario y transmite en qué debe centrarse el usuario. La acción de adelantar determinados elementos de la interfaz de usuario, Elevación, a menudo se usa para lograr esta jerarquía en el software. Muchos de nuestros controles comunes agregan elevación mediante la sombra y la profundidad de z de forma predeterminada.  

### <a name="microsoftuixaml-21190218001-prerelease"></a>Microsoft.UI.Xaml 2.1.190218001-prerelease

Febrero de 2019

[Página de versiones de GitHub](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.190219001-prerelease)

[Descarga de paquetes NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190218001-prerelease)

Nuevas características experimentales:

* [Control TeachingTip](https://github.com/Microsoft/microsoft-ui-xaml/issues/21)  
  Este nuevo control proporciona a la aplicación una manera de guiar e informar a los usuarios con una notificación no invasiva y rica en contenido. El control TeachingTip puede usarse para enfocar una característica nueva o importante, enseñar a los usuarios a realizar una tarea o mejorar el flujo de trabajo del usuario, para lo cual proporciona información contextualmente pertinente para la tarea que está realizando.

### <a name="microsoftuixaml-21190131001-prerelease"></a>Microsoft.UI.Xaml 2.1.190131001-prerelease

Febrero de 2019

[Página de versiones de GitHub](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.190131001-prerelease)

[Descarga de paquetes NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190131001-prerelease)

Nuevas características experimentales:

* [AnimatedVisualPlayer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer)  
  Este nuevo control permite reproducir animaciones vectoriales complejas de alto rendimiento, incluidas las animaciones de [Lottie](https://github.com/airbnb/lottie) creadas con [Lottie-Windows](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie).

### <a name="microsoftuixaml-21181217001-prerelease"></a>Microsoft.UI.Xaml 2.1.181217001-prerelease

Diciembre de 2018

[Página de versiones de GitHub](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.181217001-prerelease)

[Descarga de paquetes NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.181217001-prerelease)

Nuevas características experimentales:

* [ItemsRepeater](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)

* [RadioButtons](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiobuttons)

* [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem)
