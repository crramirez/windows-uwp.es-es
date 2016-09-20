---
author: mijacobs
Description: "En este artículo se describen los procedimientos recomendados para crear y mostrar la configuración de aplicaciones."
title: "Directrices para la configuración de una aplicación"
ms.assetid: 2D765E90-3FA0-42F5-A5CB-BEDC14C3F60A
label: Guidelines
template: detail.hbs
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: aeccd755c5fe5df8f2ff5549950ce2d6cb74e8e4

---


# Directrices para la configuración de una aplicación





La configuración es la parte de la aplicación que el usuario puede personalizar y se encuentra en la página de configuración de la aplicación. Por ejemplo, la configuración de la aplicación en una aplicación del lector de noticias puede permitir al usuario especificar qué fuentes de noticias mostrar o cuántas columnas mostrar en la pantalla, mientras que la configuración de la aplicación meteorológica podría permitir al usuario elegir entre Celsius y Fahrenheit como unidad predeterminada de medida. En este artículo se describen los procedimientos recomendados para crear y mostrar la configuración de aplicaciones.

![ejemplo de un panel de configuración](images/app-settings.png)

## <span id="Should_I_include_a_settings_page_in_my_app_"></span><span id="should_i_include_a_settings_page_in_my_app_"></span><span id="SHOULD_I_INCLUDE_A_SETTINGS_PAGE_IN_MY_APP_"></span>¿Debo incluir una página de configuración en mi aplicación?


        Estos son ejemplos de opciones de la aplicación que pertenecen a una página de configuración de la aplicación: 

-   Las opciones de configuración que afectan al comportamiento de la aplicación y que no se ajustan con frecuencia, como cuando eliges entre Celsius o Fahrenheit como unidades de temperatura predeterminadas en una aplicación del tiempo, cuando cambias la configuración de una cuenta para una aplicación de correo, la configuración de las notificaciones o las opciones de accesibilidad.
-   Opciones que dependen de las preferencias del usuario, como música, efectos de sonido o temas de colores.
-   La información sobre la aplicación a la que no se tiene acceso muy a menudo, como la política de privacidad, la ayuda, la versión de la aplicación o la información de copyright.

Los comandos que forman parte del flujo de trabajo habitual de la aplicación (por ejemplo, cambiar el tamaño del pincel en una aplicación de dibujo) no deben estar en una página de configuración. Para obtener información sobre la colocación de los comandos, consulta los [Conceptos básicos del diseño de comandos](https://msdn.microsoft.com/library/windows/apps/dn958433).

## <span id="general_principles"></span><span id="GENERAL_PRINCIPLES"></span>Recomendaciones generales


-   Simplificar las páginas de configuración y hacer uso de los controles binarios (encendido/apagado). Un [modificador para alternar](../controls-and-patterns/toggles.md) suele ser el mejor control para una configuración binaria.
-   Para una configuración que permita a los usuarios elegir un elemento de un conjunto de hasta 5 opciones relacionadas que sean mutuamente excluyentes, usa [botones de radio](../controls-and-patterns/radio-button.md).
-   Crea un punto de entrada para todas las configuraciones de tu página de configuración de la aplicación.
-   Haz que tu configuración sea sencilla. Define valores predeterminados inteligentes y reduce el número de configuraciones tanto como sea posible.
-   Cuando un usuario cambia una opción de configuración, la aplicación debería reflejar el cambio inmediatamente.
-   No incluyas comandos que formen parte del flujo de trabajo común de la aplicación.

## <span id="Entry_point"></span><span id="entry_point"></span><span id="ENTRY_POINT"></span>Punto de entrada


La manera en la que los usuarios acceden a la página de configuración de la aplicación debe basarse en el diseño de la aplicación.

**Panel de navegación**

Para el diseño del panel de navegación, el elemento Configuración de la aplicación debe ser el último de la lista de navegación de opciones y estar anclado en la parte inferior:

![punto de entrada de la configuración de la aplicación para el panel de navegación](images/appsettings-entrypoint-navpane.png)

**Barra de la aplicación**

Si usas una barra de la aplicación o una barra de herramientas, que normalmente forma parte de un diseño de navegación centralizada, con pestañas o tablas dinámicas, coloca el último elemento del punto de entrada en el menú del control flotante "Más". Si es importante tener una mayor detectabilidad del punto de entrada de configuración de la aplicación, coloca el punto de entrada directamente en la barra de la aplicación y no en el menú del control flotante "Más".

![punto de entrada de la configuración de la aplicación para la barra de la aplicación](images/appsettings-entrypoint-tabs.png)

**Navegación centralizada**

Si estás usando un diseño de navegación centralizada, el punto de entrada de la configuración de la aplicación debe colocarse en el menú del control flotante "Más" de la barra de la aplicación.

**Pestañas y tablas dinámicas**

Para un diseño de pestañas o tablas dinámicas, no se recomienda colocar el punto de entrada de la configuración de la aplicación como uno de los elementos principales de la navegación. En su lugar, el punto de entrada de la configuración de la aplicación debe colocarse en el menú del control flotante "Más" de la barra de la aplicación.

**Maestro-detalles**

En lugar de esconder el punto de entrada de la configuración de la aplicación en lo más profundo de un panel de detalles maestro, conviértelo en el último elemento anclado en el nivel superior del panel maestro.

## <span id="Layout"></span><span id="layout"></span><span id="LAYOUT"></span>Diseño


Tanto en las plataformas móviles como de escritorio, la ventana de configuración de la aplicación debe abrirse en pantalla completa y llenar toda la ventana. Si el menú de configuración de la aplicación tiene hasta un máximo de cuatro grupos de nivel superior, estos grupos deben estar en cascada hacia abajo en una columna.

Escritorio:

![diseño de la página de configuración de la aplicación en el escritorio](images/appsettings-layout-navpane-desktop.png)

Móvil:

![diseño de la página de configuración de la aplicación en el teléfono](images/appsettings-layout-navpane-mobile.png)

## <span id="_About__section_and__Give_feedback__button"></span><span id="_about__section_and__give_feedback__button"></span><span id="_ABOUT__SECTION_AND__GIVE_FEEDBACK__BUTTON"></span>Sección "Acerca de" y botón "Enviar comentarios"


Si necesitas una sección de "Acerca de esta aplicación" en la aplicación, crea una página de configuración de la aplicación específica. Si quieres usar un botón "Enviar comentarios", colócalo en la parte inferior de la página "Acerca de esta aplicación".

"Términos de uso" y "Declaración de privacidad" deben ser [botones de hipervínculo](../controls-and-patterns/hyperlinks.md) con el texto ajustado.

![Sección "Acerca de esta aplicación" con un botón "Enviar comentarios"](images/appsettings-about.png)

## <span id="dos_and_donts"></span><span id="DOS_AND_DONTS"></span>Recomendaciones


## <span id="add_entry_points"></span><span id="ADD_ENTRY_POINTS"></span>Contenido de la página de configuración de la aplicación


Cuando tengas una lista de elementos que quieras incluir en la página de configuración de la aplicación, ten en cuenta las siguientes directrices:

-   Agrupar opciones relacionadas o similares en una etiqueta de configuración.
-   Intenta limitar el número total de opciones de configuración a un máximo de cuatro o cinco.
-   Muestra las mismas opciones de configuración sin importar el contexto de la aplicación. Si algunas opciones de configuración no son relevantes en un determinado contexto, deshabilítalas en el control flotante de la configuración de la aplicación.
-   Usa etiquetas descriptivas, de una sola palabra para la configuración. Por ejemplo, denomina a la configuración "Cuentas" en lugar de "Configuración de cuentas" en el caso de la configuración relacionada con la cuenta. Si solo quieres una opción para la configuración y las opciones no se prestan para una etiqueta descriptiva, usa "Opciones" o "Valores predeterminados".
-   Si una opción de configuración vincula directamente a la web en lugar de a un control flotante, házselo saber al usuario con una pista visual, como, "Ayuda (en línea)" o "Foros Web" con estilo de [hipervínculo](../controls-and-patterns/hyperlinks.md). Contempla agrupar varios vínculos de la Web en un control flotante con una sola opción de configuración. Por ejemplo, una opción de configuración "Acerca de" podría abrir un control flotante con vínculos a los términos de uso, la política de privacidad y el soporte técnico de la aplicación.
-   Combina las opciones menos usadas en una sola entrada para que las opciones más habituales puedan tener su propia entrada. Coloca el contenido o los vínculos que solo contienen información en la opción de configuración "Acerca de".
-   No dupliques la funcionalidad en el panel "Permisos". Windows proporciona este panel de forma predeterminada y no puedes modificarlo.

## <span id="add_settings_to_flyouts"></span><span id="ADD_SETTINGS_TO_FLYOUTS"></span> Agregar contenido de configuración a los controles flotantes Configuración


-   Presenta el contenido de arriba a abajo en una sola columna, desplazable si fuera necesario. Limita el desplazamiento a un máximo del doble del alto de pantalla.
-   Usa los controles siguientes para la configuración de la aplicación:

    -   
            [Modificadores para alternar](../controls-and-patterns/toggles.md): para permitir que los usuarios activen o desactiven valores.
    -   
            [Botones de radio](../controls-and-patterns/radio-button.md): para permitir a los usuarios elegir un elemento de un conjunto de hasta 5 opciones relacionadas que sean mutuamente excluyentes.
    -   
            [Cuadro de entrada de texto](../controls-and-patterns/text-block.md): para permitir que los usuarios escriban texto. Usa el tipo de cuadro de texto que corresponda al tipo de texto que obtienes del usuario, como correo electrónico o contraseña.
    -   
            [Hipervínculos](../controls-and-patterns/hyperlinks.md): para llevar a los usuarios a otra página dentro de la aplicación o a un sitio web externo. Cuando un usuario haga clic en un hipervínculo, el control flotante de configuración se descarta.
    -   
            [Botones](../controls-and-patterns/buttons.md): para permitir que los usuarios inicien una acción inmediata sin descartar el control flotante Configuración actual.
-   Agrega un mensaje descriptivo si se desactiva uno de los controles. Coloca este mensaje por encima del control deshabilitado.
-   Anima controles y contenido como un solo bloque después de que se hayan animado el control flotante de configuración y el encabezado. Anima el contenido mediante las animaciones [**enterPage**](https://msdn.microsoft.com/library/windows/apps/br212672) o [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br210288), con un desplazamiento izquierdo de 100 píxeles.
-   Usa encabezados de sección, párrafos y etiquetas para ayudar a organizar y aclarar el contenido, si fuera necesario.
-   Si necesitas repetir la configuración, usa un nivel adicional de interfaz de usuario o un modelo de expandir/contraer, pero evita las jerarquías que contienen más de dos niveles. Por ejemplo, una aplicación sobre el clima que proporciona una configuración por ciudad podría enumerar las ciudades y permitir que el usuario pulse sobre la ciudad para abrir un control flotante nuevo o expandirse para mostrar las opciones de configuración.
-   Si la carga de controles o de contenido web tarda, usa un control de progreso indeterminado para indicar al usuario que se está cargando la información. Para obtener más información, consulta [Directrices sobre controles de progreso](https://msdn.microsoft.com/library/windows/apps/hh465469).
-   No uses botones para la navegación o para confirmar cambios. Usa hipervínculos para ir a otras páginas y, en lugar de usar un botón para confirmar los cambios, guárdalos automáticamente en la configuración de la aplicación cuando el usuario descarte el control flotante de configuración.

\[Este artículo contiene información específica para aplicaciones para la Plataforma universal de Windows (UWP) y Windows 10. Para obtener instrucciones sobre Windows 8.1, descarga el [PDF sobre las directrices para Windows 8.1](https://go.microsoft.com/fwlink/p/?linkid=258743)\].

## <span id="related_topics"></span>Temas relacionados

* [Conceptos básicos del diseño de comandos](https://msdn.microsoft.com/library/windows/apps/dn958433)
* 
            [Directrices para controles de progreso](https://msdn.microsoft.com/library/windows/apps/hh465469)
            
          
            **Para desarrolladores (XAML)**
          
* [Almacenar y recuperar datos de la aplicación](https://msdn.microsoft.com/library/windows/apps/mt299098)
* 
            [
              **EntranceThemeTransition**
            ](https://msdn.microsoft.com/library/windows/apps/br210288) �

�



<!--HONumber=Jun16_HO4-->


