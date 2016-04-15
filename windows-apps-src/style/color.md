---
Description: El color proporciona una forma intuitiva de buscar a través de los distintos niveles de información de una aplicación y sirve de herramienta crucial para reforzar el modelo de interacción.
title: Color
ms.assetid: 3ba7176f-ac47-498c-80ed-4448edade8ad
label: Color
template: detail.hbs
extraBodyClass: style-color
brief: Color provides intuitive wayfinding through an app's various levels of information and serves as a crucial tool for reinforcing the interaction model.<br /><br />In Windows, color is also personal. Users can choose a color and a light or dark theme to be reflected throughout their experience.
---

# Color de las aplicaciones para UWP
El color proporciona una forma intuitiva de buscar a través de los distintos niveles de información de una aplicación y sirve de herramienta crucial para reforzar el modelo de interacción.

## Color de énfasis

El usuario puede seleccionar un único color denominado énfasis. Se puede elegir d'entre un conjunto de muestras de 48 colores protegido.


<!-- Alternate version for the dev center. Need to add hex values. -->
<figure>
![Accent colors](images/accentcolorswatch.png)
<figcaption>Como regla general, cuando uses el color de énfasis original como fondo, coloca siempre el texto de encima en blanco.</figcaption>
</figure>

Cuando se elige un color de énfasis, este aparece como parte del tema del sistema. Las áreas en las que influye son Inicio, barra de tareas, la ventana de cromo, los estados de interacción seleccionados y los hipervínculos dentro de los [controles comunes](https://dev.windows.com/design/controls-patterns). Cada aplicación puede incorporar aún más el color de énfasis con su tipografía, fondos e interacciones, o bien reemplazarlo todo para conservar la imagen de marca.

## Selección de colores

Una vez que se ha seleccionado un color de énfasis, los tonos claros y oscuros del color de énfasis se crean basándose en los valores del HCL de luminosidad de color. Las aplicaciones pueden usar las variaciones de tono para crear una jerarquía visual y para proporcionar una indicación de interacción.

![Un solo color de énfasis con sus 6 tonos](images/shades.png)

<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            In XAML, the accent color is exposed as a [theme resource](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx) named `SystemAccentColor`. It's also available programmatically from [UISettings.GetColorValue](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx). You can programmatically access the different shades from [UISettings.GetColorValue](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx), see the [UIColorType](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx) enum.
    </div>
</aside>

## Temas de color

El usuario también puede elegir entre un tema claro u oscuro para el sistema (opción para el teléfono, ya que aún no se dispone de esa opción en tableta y escritorio; estos ofrecen configuraciones desde la aplicación). Algunas aplicaciones optan por cambiar su tema según las preferencias del usuario, mientras que otras no ofrecen esa opción.

Las aplicaciones que usan un tema claro son para escenarios relacionados con las aplicaciones de productividad. Un ejemplo sería el conjunto de aplicaciones disponibles con Microsoft Office. Los temas claros facilitan la lectura de textos largos en combinación con períodos prolongados de tiempo dedicado a largas tareas.

El tema oscuro ofrece un contraste de contenido más visible para las aplicaciones que están basadas en multimedia o para escenarios donde los usuarios están expuestos a una gran cantidad de vídeos o imágenes. En estos escenarios, la lectura no es necesariamente la tarea principal, aunque una experiencia multimedia puede mostrarse bajo condiciones de ambiente poco luminoso.

Si la aplicación no se ajusta mucho a cualquiera de estas descripciones, considera seguir el tema del sistema para permitir al usuario decidir cuál es el mejor.

Para facilitar el diseño de temas, Windows proporciona una paleta de colores adicional que se adapta automáticamente al tema.


<!-- OP version -->
### Tema claro
#### Base
![El tema claro Base](images/themes-light-base.png)
#### Alt
![El tema claro Alt](images/themes-light-alt.png)
#### List
![El tema claro List](images/themes-light-list.png)
#### Chrome
![El tema claro Chrome](images/themes-light-chrome.png)
### Tema oscuro
#### Base
![El tema oscuro Base](images/themes-dark-base.png)
#### Alt
![El tema oscuro Alt](images/themes-dark-alt.png)
#### List
![El tema oscuro List](images/themes-dark-list.png)
#### Chrome
![El tema oscuro Chrome](images/themes-dark-chrome.png)


<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            Each color is available as a XAML [theme resource](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx#the_xaml_color_ramp_and_theme-dependent_brushes) that follows the `System*Color` naming convention (ex: `SystemChromeHighColor`). You can control your app's theme through either [Application.RequestedTheme](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.requestedtheme.aspx) or [FrameworkElement.RequestedTheme](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.requestedtheme.aspx).
    </div>
</aside>

## Accesibilidad

La paleta está optimizada para el uso de la pantalla. Se recomienda mantener una relación de contraste mínimo para el texto de 4,5:1 para mejorar una legibilidad óptima.


<!--HONumber=Mar16_HO5-->


