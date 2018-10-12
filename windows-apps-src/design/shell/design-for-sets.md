---
author: jwmsft
description: Aprende a optimizar tu aplicación para proporcionar la mejor experiencia en la interfaz de usuario de Conjuntos.
title: Diseños para Conjuntos
template: detail.hbs
ms.author: jimwalk
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, barra de título
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 7c3e0e6ec7331e860c9153e2a2e29a51fb5848bd
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "4536326"
---
# <a name="designing-for-sets"></a>Diseños para Conjuntos

> [!IMPORTANT]
> En este artículo se describe una funcionalidad que no se ha lanzado aún y que puede sufrir importantes modificaciones antes de que se lance la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.

A partir de Windows Insider Preview, la funcionalidad Conjuntos está disponible para los usuarios de la aplicación. Con Conjuntos, la aplicación se dibuja en una ventana que puede compartirse con otras aplicaciones y cada aplicación obtiene su propia pestaña en la barra de título.

En este artículo, trataremos las áreas donde puede que quieras optimizar tu aplicación para proporcionar la mejor experiencia en la interfaz de usuario de Conjuntos.

> [!TIP]
> Para obtener más información acerca de Conjuntos, consulta la entrada de blog [Introducción sobre Conjuntos](https://insider.windows.com/en-us/articles/introducing-sets/) y la charla [Desarrollo de Conjuntos](https://developer.microsoft.com/events/build/content/developing-for-sets-on-windows-10) de la compilación de Microsoft de 2018.

## <a name="customizing-tab-visuals"></a>Personalización de elementos visuales de pestañas

De manera predeterminada, el sistema intenta seleccionar colores de iconos y texto adecuados para la pestaña de la aplicación cuando esté activa. Esto garantiza que la pestaña de la aplicación tendrá un buen aspecto con el mínimo esfuerzo por tu parte, o incluso si no haces ninguna optimización para Conjuntos.

Sin embargo, puede que haya casos en los que sea mejor personalizar el color de la pestaña de la aplicación. En esta sección, explicamos los comportamientos predeterminados de la pestaña y analizamos cuándo se deben o no modificar para tu aplicación.

### <a name="tab-states"></a>Estados de la pestaña

Cuando la aplicación está en un Conjunto, su pestaña puede estar en uno de estos tres estados: seleccionado y activo, seleccionado e inactivo o no seleccionado o inactivo.

- **Seleccionado-Activo**: una pestaña que está seleccionada desde dentro de un conjunto de ventanas agrupadas y está en la ventana activa en primer plano.

    (En este documento, la descripción de modificar la pestaña _activa_ significa que la pestaña está Seleccionada-Activa.)
- **Seleccionada-Inactiva**: una pestaña que está seleccionada desde dentro de un conjunto de ventanas agrupadas pero que no está en la ventana activa en primer plano.
- **No seleccionada-Inactiva**: una pestaña que no está seleccionada desde dentro de un conjunto de ventanas agrupadas.

El color de las pestañas inactivas la actualiza y mantiene el sistema en función del tema del sistema. No hay ninguna forma de influir en ella desde tu aplicación.

De manera predeterminada, la pestaña seleccionada y activa respeta el color del tema del sistema especificado por el usuario en la Configuración de Windows. Puede personalizar el color de la pestaña para tu aplicación solo cuando la pestaña esté activa.

![Estados de la pestaña en Conjuntos](images/sets-tab-states.jpg)

### <a name="coloring-of-active-tabs"></a>Colores de las pestañas activas

El color de la pestaña activa viene determinado por valores definidos en la aplicación o por la configuración del sistema. El color de pestaña usado cuando la aplicación está activa se determina de la siguiente manera:

- Si especificas un color de la pestaña en la aplicación, tiene la prioridad más alta. El color de pestaña que especifiques en la aplicación se usa cuando la aplicación está activa, independientemente de la configuración del sistema.
- De lo contrario, si el usuario selecciona la opción en Configuración de Windows para mostrar el color de énfasis en las barras de título, se usa el color de énfasis del sistema.
  - Esta configuración se encuentra en la aplicación Configuración de Windows, en Personalización > Colores > Mostrar color de énfasis en las siguiente superficies: Barras de título.
- Por último, si no se aplica ninguna configuración de aplicación o usuario, la pestaña usa el color del tema del sistema actual.

### <a name="considerations-when-you-modify-tab-colors"></a>Consideraciones al modificar los colores de las pestañas

Aquí describimos situaciones donde quizás quieras modificar el color de la pestaña de la aplicación y cosas que debes tener en cuenta en esos casos. También abordaremos algunas situaciones en las que te recomendamos que no modifiques el color de la pestaña, pero permites que el sistema lo gestione.

#### <a name="match-your-brand-colors"></a>Coincidir con los colores de marca

Por lo general, el factor más importante que determina si modificar el color de la pestaña es el deseo de que coincida con el color de marca. Cuando modifiques la pestaña de la aplicación para que coincida con el color de marca, debes probar su aspecto teniendo en cuenta las otras consideraciones descritos en esta sección, como el diseño de la aplicación o los diferentes colores de temas del sistema.

#### <a name="horizontal-layout"></a>Diseño horizontal

Si el diseño de la aplicación incluye una banda sólida de color (que no sea de Acrílico) que discurre horizontalmente por la parte superior, normalmente funciona bien conectar tu aplicación con su pestaña mediante el color coincidente. Elige un color sólido para tu pestaña que dé la sensación de conexión con tu aplicación, proporcionando de forma ideal cierta continuidad al color usado en la parte superior de la aplicación.

#### <a name="horizontal-layout-with-acrylic"></a>Diseño horizontal con Acrílico

Si la aplicación usa una banda de material Acrílico que discurre horizontalmente por la parte superior, te recomendamos que dejes que el sistema determine el color de la pestaña.

También te recomendamos usar aquí Acrílico en la aplicación, en lugar de usar Acrílico en segundo plano para permitir que el segundo plano de la aplicación fluya a esta área en lugar de crear un efecto de bandas que se muestren a través de la aplicación o el escritorio detrás de esta banda.

Ten en cuenta que el usuario es capaz de establecer estas pestañas para que usen el color de énfasis para que puedan aparecer con un tema claro/oscuro o color de énfasis.

#### <a name="vertical-layout"></a>Diseño vertical

Si el diseño de la aplicación incluye un panel vertical de color sólido dispuesto verticalmente, te recomendamos que no personalices los colores de las pestañas. La posición de la pestaña por encima de la aplicación la puede cambiar en cualquier momento el usuario, por lo que no puedes disponer de continuidad de color entre la parte superior de la aplicación y la pestaña. El sistema usa otras indicaciones visuales, como sombras, para conectar la pestaña con la aplicación.

### <a name="how-to-modify-tab-colors"></a>Cómo modificar los colores de la pestaña

El color de la pestaña activa utiliza las API de personalización de barra de título existentes. Si ya has personalizado el color de la barra de título de la aplicación, el cambio también se aplica a la pestaña de la aplicación cuando la aplicación está en un Conjunto.

Para modificar los colores de la pestaña, establece las propiedades de [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) para especificar:

- Un color de segundo plano sólido para la pestaña.
- Un color de primer plano sólido para el texto.

En este ejemplo se muestra cómo obtener una instancia de ApplicationViewTitleBar y cómo establecer sus propiedades de color.

```csharp
// using Windows.UI.ViewManagement;
var titleBar = ApplicationView.GetForCurrentView().TitleBar;

// Set active window colors
titleBar.ForegroundColor = Windows.UI.Colors.White;
titleBar.BackgroundColor = Windows.UI.Colors.Green;
```

Para obtener más información, consulta la sección _Personalización de colores simples_ del artículo [Personalización de la barra de título](title-bar.md#simple-color-customization) y el [Ejemplo de personalización de la barra de título](http://go.microsoft.com/fwlink/p/?LinkId=620613).

### <a name="considerations-for-full-title-bar-customization"></a>Consideraciones para la personalización de la barra de título completa.

También puedes personalizar totalmente la barra de título de la aplicación, como se describe en la sección _Personalización completa_ del artículo [Personalización de la barra de título](title-bar.md#full-customization). Esto suele realizarse para [extender Acrílico en la barra de título](../style/acrylic.md#extend-acrylic-into-the-title-bar) o para poner el contenido personalizado en la barra de título. Si haces esto, asegúrate de seguir las directrices para el modo de pantalla completa y el modo de tableta y para mostrar solo el contenido personalizado de la barra de título cuando [CoreApplicationViewTitleBar.IsVisible](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.isvisible) es **true**.

Cuando la aplicación se ejecuta en un Conjunto, CoreApplicationViewTitleBar.IsVisible es **false** y no debe mostrarse el contenido de la barra de título. Sin embargo, si no sigues esta directriz para ocultar el contenido de la barra de título personalizado, se muestra debajo de la pestaña de la aplicación y no en el área de la barra de título.

Si has colocado contenido o funcionalidad en la interfaz de usuario de la barra de título personalizada, considera la posibilidad de ponerlo a disposición en otra superficie de interfaz de usuario en la aplicación.

### <a name="how-to-modify-the-tab-icon"></a>Cómo modificar el icono de la pestaña

Para asegurarte de que el icono de la aplicación tiene el mejor aspecto en un Conjunto, debes proporcionar un icono alternativo sin placa para tu aplicación. (El icono de la aplicación usado en tu pestaña de la aplicación es el mismo icono usado en la barra de tareas). El propósito del icono alternativo es tener un buen aspecto frente a cualquier color de fondo. Si está disponible, se usará el icono alternativo.

En el manifiesto de la aplicación, especifica un icono sin placa de forma alternativa además del icono normal. Para obtener más información, consulta [logotipos e iconos de la aplicación](/windows/uwp/design/style/app-icons-and-logos). El icono para especificar se documenta como "activos de lista de tamaño de destino sin placa" en la sección [más información acerca de los activos de icono de la aplicación](/windows/uwp/design/style/app-icons-and-logos#more-about-app-icon-assets) del artículo.

Si no especificas un icono alternativo en el manifiesto de la aplicación, el sistema asignará una nueva placa al icono de la ventana con el color de la pestaña y usará esa.

![Iconos utilizados en Conjuntos](images/sets-icons.png)

> El mismo icono se usa en la barra de tareas y en la pestaña de la aplicación.

## <a name="restore-previous-sets-with-user-activities"></a>Restaurar Conjuntos anteriores con actividades del usuario

Una ventaja de Conjuntos es que permite a los usuarios restaurar pestañas anteriormente abiertas para aplicaciones y contenido web cuando inician una aplicación o abren un documento. (Consulta el vídeo en la entrada de blog [Introducción sobre Conjuntos](https://insider.windows.com/en-us/articles/introducing-sets/) para obtener más información.) Esto se habilita a través de _actividades del usuario_.

De manera predeterminada, el sistema crea actividades del usuario en nombre de la aplicación, lo cual permite la restauración de tu aplicación en una pestaña cuando un usuario inicia una aplicación o abre un documento. Sin embargo, las actividades predeterminadas del usuario creadas por el sistema solo pueden iniciar la aplicación en su estado predeterminado. No puede restaurar la aplicación al estado que tenía anteriormente como parte del Conjunto.

Puedes optimizar tu aplicación para Conjuntos proporcionando actividades personalizadas del usuario. La actividad del usuario que proporciones crea vínculos profundos en la aplicación para restaurarla al estado que tenía la última vez como parte del Conjunto que se va a restaurar.

Para proporcionar una actividad de usuario personalizada:

- Responde a un sistema operativo iniciado [UserActivityRequest](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityrequest) con una [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity) apropiada.
- La UserActivity contiene un URI de vínculo profundo de activación que el sistema usa para iniciar la aplicación con un contexto específico.

Para obtener más información, consulta [Evento UserActivityRequestManager.UserActivityRequested](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityrequestmanager.useractivityrequested), [Administración de la activación de URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation) y [Continuar la actividad del usuario, incluso en diferentes dispositivos](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities).

## <a name="enable-multi-instance-for-uwp-apps"></a>Habilitar varias instancias para aplicaciones para UWP

A partir de Windows 10, versión 1803, las aplicaciones para UWP admiten instancias múltiples. UWP es aún instancia única de manera predeterminada, y tienes que indicar explícitamente cada aplicación que quieres que tenga instancias múltiples.

Si conviertes tu aplicación en instancia múltiple, tiene la ventaja de que permite a los usuarios ejecutar tu aplicación en más de un Conjunto al mismo momento. Una aplicación de una instancia única solo se puede ejecutar en un Conjunto a la vez.

Para obtener más información sobre cómo habilitar instancias múltiples en tu aplicación para UWP, consulta [Crear una aplicación universal de Windows de instancias múltiples](https://docs.microsoft.com/windows/uwp/launch-resume/multi-instance-uwp).

## <a name="use-an-in-app-back-button"></a>Usar un botón Atrás en la aplicación

Para implementar la navegación hacia atrás en la aplicación, recomendamos que coloques un botón Atrás en la interfaz de usuario de la aplicación siguiendo las [instrucciones de botón Atrás](../basics/navigation-history-and-backwards-navigation.md). Si la aplicación usa el control NavigationView, debes usar el botón Atrás integrado de NavigationView.

Si la aplicación usa el botón Atrás del sistema, debes reemplazarlo por el botón Atrás en la aplicación en su lugar. Esto garantiza que los usuarios tengan siempre una experiencia de botón Atrás coherente si la aplicación se ejecuta o no en un Conjunto y también garantiza que los elementos visuales del botón Atrás sigan siendo coherentes en las aplicaciones.

Para obtener información detallada sobre la de un botón Atrás en la aplicación, consulta [Historial de navegación y navegación hacia atrás](../basics/navigation-history-and-backwards-navigation.md).

### <a name="support-for-the-system-back-button-in-sets"></a>Compatibilidad con el botón Atrás del sistema en Conjuntos

Si la aplicación usa el botón Atrás del sistema en lugar de un botón en la aplicación, la interfaz de usuario del sistema seguirá representando el botón Atrás del sistema para garantizar compatibilidad con versiones anteriores

- Si la aplicación no está en un Conjunto, el botón Atrás se representa dentro de la barra de título. La experiencia visual y las interacciones del usuario para el botón Atrás no han cambiado.
- Si la aplicación está en un Conjunto, el botón Atrás se representa dentro de la barra Atrás del sistema.

La barra Atrás del sistema es una "banda" que se inserta entre la banda de la pestaña y el área de contenido de la aplicación. La banda recorre el ancho de la aplicación, con el botón Atrás en el borde izquierdo. La banda tiene una altura vertical lo suficientemente grande como para garantizar el tamaño adecuado de la función táctil para el botón Atrás.

![La barra Atrás del sistema en Conjuntos](images/sets-system-back-bar.png)

> La barra Atrás del sistema que se muestra en una aplicación.

La barra Atrás del sistema se muestra dinámicamente, en función de la visibilidad del botón Atrás. Cuando el botón Atrás está visible, se inserta la barra Atrás del sistema, desplazando por debajo de la banda de la pestaña el contenido de la aplicación. Cuando se oculta el botón Atrás, la barra Atrás del sistema se quita dinámicamente, desplazando el contenido de la aplicación hacia arriba para que coincida con la banda de la pestaña.

Para evitar tener que subir y bajar la interfaz de usuario de la aplicación, te recomendamos que uses un botón Atrás en la aplicación en lugar del botón Atrás del sistema.

Se realizan personalizaciones de la barra de título en la pestaña de la aplicación y en la barra Atrás del sistema. Si usas las API ApplicationViewTitleBar para especificar las propiedades de color de fondo y de primer plano, los colores se aplican a la pestaña y a la barra Atrás del sistema.

## <a name="related-articles"></a>Artículos relacionados

- [Personalización de la barra de título](title-bar.md)
- [Historial de navegación y navegación hacia atrás](../basics/navigation-history-and-backwards-navigation.md)
- [Color](../style/color.md)
