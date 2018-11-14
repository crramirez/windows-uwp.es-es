---
author: mijacobs
Description: This article describes best practices for creating and displaying app settings.
title: Directrices para la configuración de una aplicación
ms.assetid: 2D765E90-3FA0-42F5-A5CB-BEDC14C3F60A
label: Guidelines
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 56a952950fa9f2d9d57d5beaed397dd72f64ea54
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "6449528"
---
# <a name="guidelines-for-app-settings"></a>Directrices para la configuración de una aplicación



La configuración es la parte de la aplicación que el usuario puede personalizar y se encuentra en la página de configuración de la aplicación. Por ejemplo, la configuración de la aplicación en una aplicación del lector de noticias puede permitir al usuario especificar qué fuentes de noticias mostrar o cuántas columnas mostrar en la pantalla, mientras que la configuración de la aplicación meteorológica podría permitir al usuario elegir entre Celsius y Fahrenheit como unidad predeterminada de medida. En este artículo se describen los procedimientos recomendados para crear y mostrar la configuración de aplicaciones.


## <a name="should-i-include-a-settings-page-in-my-app"></a>¿Debo incluir una página de configuración en mi aplicación?

Estos son ejemplos de opciones de la aplicación que pertenecen a una página de configuración de la aplicación:

-   Las opciones de configuración que afectan al comportamiento de la aplicación y que no se ajustan con frecuencia, como cuando eliges entre Celsius o Fahrenheit como unidades de temperatura predeterminadas en una aplicación del tiempo, cuando cambias la configuración de una cuenta para una aplicación de correo, la configuración de las notificaciones o las opciones de accesibilidad.
-   Opciones que dependen de las preferencias del usuario, como música, efectos de sonido o temas de colores.
-   La información sobre la aplicación a la que no se tiene acceso muy a menudo, como la política de privacidad, la ayuda, la versión de la aplicación o la información de copyright.

Los comandos que forman parte del flujo de trabajo habitual de la aplicación (por ejemplo, cambiar el tamaño del pincel en una aplicación de dibujo) no deben estar en una página de configuración. Para obtener información sobre la colocación de los comandos, consulta los [Conceptos básicos del diseño de comandos](https://msdn.microsoft.com/library/windows/apps/dn958433).

## <a name="general-recommendations"></a>Recomendaciones generales


-   Simplificar las páginas de configuración y hacer uso de los controles binarios (encendido/apagado). Un [modificador para alternar](../controls-and-patterns/toggles.md) suele ser el mejor control para una configuración binaria.
-   Para una configuración que permita a los usuarios elegir un elemento de un conjunto de hasta 5 opciones relacionadas que sean mutuamente excluyentes, usa [botones de radio](../controls-and-patterns/radio-button.md).
-   Crea un punto de entrada para todas las configuraciones de tu página de configuración de la aplicación.
-   Haz que tu configuración sea sencilla. Define valores predeterminados inteligentes y reduce el número de configuraciones tanto como sea posible.
-   Cuando un usuario cambia una opción de configuración, la aplicación debería reflejar el cambio inmediatamente.
-   No incluyas comandos que formen parte del flujo de trabajo común de la aplicación.

## <a name="entry-point"></a>Punto de entrada


La manera en la que los usuarios acceden a la página de configuración de la aplicación debe basarse en el diseño de la aplicación.

**Panel de navegación**

Para el diseño del panel de navegación, el elemento Configuración de la aplicación debe ser el último de la lista de navegación de opciones y estar anclado en la parte inferior:

![punto de entrada de la configuración de la aplicación para el panel de navegación](images/appsettings-entrypoint-navpane.png)

**Barra de aplicaciones**

Si estás usando la [barra de aplicaciones](../controls-and-patterns/app-bars.md) o la barra de herramientas, coloca el punto de entrada de configuración como el último elemento en el menú de desbordamiento "Más". Si es importante tener una mayor detectabilidad del punto de entrada de configuración de la aplicación, colócalo directamente en la barra de aplicaciones y no en el desbordamiento.

![punto de entrada de la configuración de la aplicación para la barra de aplicaciones](images/appsettings-entrypoint-tabs.png)

**Concentrador**

Si estás usando un diseño de navegación centralizada, el punto de entrada de la configuración de la aplicación debe colocarse en el menú de desbordamiento "Más" de la barra de aplicaciones.

**Pestañas y tablas dinámicas**

Para un diseño de pestañas o tablas dinámicas, no se recomienda colocar el punto de entrada de la configuración de la aplicación como uno de los elementos principales de la navegación. En su lugar, el punto de entrada de la configuración de la aplicación debe colocarse en el menú de desbordamiento "Más" de la barra de aplicaciones.

**Panel maestro y detalles**

En lugar de esconder el punto de entrada de la configuración de la aplicación en lo más profundo de un panel de detalles maestro, conviértelo en el último elemento anclado en el nivel superior del panel maestro.

## <a name="layout"></a>Diseño


Tanto en las plataformas móviles como de escritorio, la ventana de configuración de la aplicación debe abrirse en pantalla completa y llenar toda la ventana. Si el menú de configuración de la aplicación tiene hasta cuatro grupos de nivel superior, estos grupos deben estar en cascada descendente en una columna.

Escritorio:

![diseño de la página de configuración de la aplicación en el escritorio](images/appsettings-layout-navpane-desktop.png)

Móvil:

![diseño de la página de configuración de la aplicación en un teléfono](images/appsettings-layout-navpane-mobile.png)

## <a name="color-mode-settings"></a>Configuración de "Modo de color"


Si la aplicación permite a los usuarios elegir el modo de color de la aplicación, presenta estas opciones usando [botones de radio](../controls-and-patterns/radio-button.md) o un [cuadro combinado](../controls-and-patterns/lists.md#drop-down-lists) con el encabezado "Elegir un modo de aplicación". En las opciones debe haber lo siguiente:
- Claro
- Oscuro
- Versión predeterminada de Windows

También te recomendamos que agregues un hipervínculo a la página Colores de la aplicación de Configuración de Windows donde los usuarios puedan acceder al modo de aplicación predeterminado actual y modificarlo. Usa la cadena "Configuración de color de Windows" para el texto del hipervínculo.

![Sección "Elegir un modo"](images/appsettings_mode.png)

<!--
<div class="microsoft-internal-note">
Detailed redlines showing preferred text strings for the "Choose a mode" section are available on [UNI](http://uni/DesignDepot.FrontEnd/#/ProductNav/2543/0/dv/?t=Windows%7CControls%7CColorMode&f=RS2).
</div>
-->

## <a name="about-section-and-feedback-button"></a>Sección Acerca de y botón Comentarios


Se recomienda colocar la sección "Acerca de esta aplicación" en la aplicación como una página dedicada o en su propia sección. Si quieres usar un botón "Enviar comentarios", colócalo en la parte inferior de la página "Acerca de esta aplicación".

En un subtítulo "Condiciones de uso", coloca los "Términos de uso" y la "Declaración de privacidad" (debería ser [botones de hipervínculo](../controls-and-patterns/hyperlinks.md) con ajuste de texto), así como información legal adicional, como el copyright.

![Sección "Acerca de esta aplicación" con un botón "Enviar comentarios"](images/appsettings-about.png)


## <a name="recommended-page-content"></a>Contenido de la página recomendado


Cuando tengas una lista de los elementos que quieras incluir en la página de configuración de la aplicación, ten en cuenta las siguientes directrices:

-   Agrupar opciones relacionadas o similares en una etiqueta de configuración.
-   Intenta limitar el número total de opciones de configuración a un máximo de cuatro o cinco.
-   Muestra las mismas opciones de configuración sin importar el contexto de la aplicación. Si algunas opciones de configuración no son relevantes en un determinado contexto, deshabilítalas en el control flotante de la configuración de la aplicación.
-   Usa etiquetas descriptivas, de una sola palabra para la configuración. Por ejemplo, denomina a la configuración "Cuentas" en lugar de "Configuración de cuentas" en el caso de la configuración relacionada con la cuenta. Si solo quieres una opción para la configuración y las opciones no se prestan para una etiqueta descriptiva, usa "Opciones" o "Valores predeterminados".
-   Si una opción de configuración vincula directamente a la web en lugar de a un control flotante, házselo saber al usuario con una pista visual, como, "Ayuda (en línea)" o "Foros Web" con estilo de [hipervínculo](../controls-and-patterns/hyperlinks.md). Contempla agrupar varios vínculos de la Web en un control flotante con una sola opción de configuración. Por ejemplo, una opción de configuración "Acerca de" podría abrir un control flotante con vínculos a los términos de uso, la política de privacidad y el soporte técnico de la aplicación.
-   Combina las opciones menos usadas en una sola entrada para que las opciones más habituales puedan tener su propia entrada. Coloca el contenido o los vínculos que solo contienen información en la opción de configuración "Acerca de".
-   No dupliques la funcionalidad en el panel "Permisos". Windows proporciona este panel de forma predeterminada y no puedes modificarlo.

-   Agregar contenido de configuración a los controles flotantes Configuración
-   Presenta el contenido de arriba a abajo en una sola columna, desplazable si fuera necesario. Limita el desplazamiento a un máximo del doble del alto de pantalla.
-   Usa los controles siguientes para la configuración de la aplicación:

    -   [Modificadores para alternar](../controls-and-patterns/toggles.md): para permitir que los usuarios activen o desactiven valores.
    -   [Botones de radio](../controls-and-patterns/radio-button.md): para permitir a los usuarios elegir un elemento de un conjunto de hasta 5 opciones relacionadas que sean mutuamente excluyentes.
    -   [Cuadro de entrada de texto](../controls-and-patterns/text-block.md): para permitir que los usuarios escriban texto. Usa el tipo de cuadro de texto que corresponda al tipo de texto que obtienes del usuario, como correo electrónico o contraseña.
    -   [Hipervínculos](../controls-and-patterns/hyperlinks.md): para llevar a los usuarios a otra página dentro de la aplicación o a un sitio web externo. Cuando un usuario haga clic en un hipervínculo, el control flotante de configuración se descarta.
    -   [Botones](../controls-and-patterns/buttons.md): para permitir que los usuarios inicien una acción inmediata sin descartar el control flotante Configuración actual.
-   Agrega un mensaje descriptivo si se desactiva uno de los controles. Coloca este mensaje por encima del control deshabilitado.
-   Anima controles y contenido como un solo bloque después de que se hayan animado el control flotante de configuración y el encabezado. Anima el contenido mediante las animaciones [**enterPage**](https://msdn.microsoft.com/library/windows/apps/br212672) o [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br210288), con un desplazamiento izquierdo de 100 píxeles.
-   Usa encabezados de sección, párrafos y etiquetas para ayudar a organizar y aclarar el contenido, si fuera necesario.
-   Si necesitas repetir la configuración, usa un nivel adicional de interfaz de usuario o un modelo de expandir/contraer, pero evita las jerarquías que contienen más de dos niveles. Por ejemplo, una aplicación sobre el clima que proporciona una configuración por ciudad podría enumerar las ciudades y permitir que el usuario pulse sobre la ciudad para abrir un control flotante nuevo o expandirse para mostrar las opciones de configuración.
-   Si la carga de controles o de contenido web tarda, usa un control de progreso indeterminado para indicar al usuario que se está cargando la información. Para obtener más información, consulta [Directrices sobre controles de progreso](https://msdn.microsoft.com/library/windows/apps/hh465469).
-   No uses botones para la navegación o para confirmar cambios. Usa hipervínculos para ir a otras páginas y, en lugar de usar un botón para confirmar los cambios, guárdalos automáticamente en la configuración de la aplicación cuando el usuario descarte el control flotante de configuración.



## <a name="related-articles"></a>Artículos relacionados

* [Conceptos básicos del diseño de comandos](https://msdn.microsoft.com/library/windows/apps/dn958433)
* [Directrices sobre los controles de progreso](https://msdn.microsoft.com/library/windows/apps/hh465469)
* [Almacenar y recuperar datos de la aplicación](https://msdn.microsoft.com/library/windows/apps/mt299098)
* [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br210288)
