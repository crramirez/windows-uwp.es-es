---
author: mijacobs
Description: El color proporciona una forma intuitiva de buscar a través de los distintos niveles de información de una aplicación y sirve de herramienta crucial para reforzar el modelo de interacción.
title: Color
ms.assetid: 3ba7176f-ac47-498c-80ed-4448edade8ad

label: Color
template: detail.hbs
extraBodyClass: style-color
---

# Color

El color proporciona una forma intuitiva de buscar a través de los distintos niveles de información de una aplicación y sirve de herramienta crucial para reforzar el modelo de interacción.

En Windows, el color también es personal. Los usuarios pueden elegir un color y un tema claro u oscuro que se reflejen a lo largo de su experiencia.

## Color de énfasis

El usuario puede seleccionar un único color, llamado de énfasis, desde *Configuración > Personalización > Colores*. Tienen su selección de un conjunto ajustado de 48 muestras de color, excepto en Xbox, que tiene una paleta 21 colores compatibles con TV.

<!-- Alternate version for the dev center. Need to add hex values. -->
![Colores de énfasis predeterminados](images/accentcolorswatch.png) Colores de énfasis predeterminados

![Colores de énfasis de Xbox](images/accentcolorswatch_xbox.png) Colores de énfasis de Xbox



Cuando se elige un color de énfasis, este aparece como parte del tema del sistema. Las áreas en las que influye son Inicio, barra de tareas, la ventana de cromo, los estados de interacción seleccionados y los hipervínculos dentro de los [controles comunes](https://dev.windows.com/design/controls-patterns). Cada aplicación puede incorporar aún más el color de énfasis con su tipografía, fondos e interacciones, o bien reemplazarlo todo para conservar la imagen de marca.

## Color sobre color

Una vez que se ha seleccionado un color de énfasis, los tonos claros y oscuros del color de énfasis se crean basándose en los valores de HSB de luminosidad de color. Las aplicaciones pueden usar las variaciones de tono para crear una jerarquía visual y para proporcionar una indicación de interacción.

De manera predeterminada, los hipervínculos usarán el color de énfasis del usuario. Si el fondo de página es de un color similar, puedes asignar un tono más claro (o más oscuro) de énfasis para los hipervínculos a fin de mejorar el contraste.

<figure class="figure-img" >
    <img src="images/shades.png" alt="A single accent color with its 6 shades"  />
        <figcaption><p>Distintos tonos claros y oscuros del color de énfasis predeterminado.</p>
</figcaption>
</figure>

<figure class="figure-img" >
    <img src="images/action_center_redline_zoom.png" alt="Redlines for Colored Action Center"  />
        <figcaption><p>Un ejemplo de cómo se aplica la lógica de color a una especificación de diseño.</p>
</figcaption>
</figure>

<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            En XAML, el color de énfasis principal se expone como un [recurso de tema](https://msdn.microsoft.com/library/windows/apps/Mt187274.aspx) denominado `SystemAccentColor`. Los tonos están disponibles como `SystemAccentColorLight3`, `SystemAccentColorLight2`, `SystemAccentColorLight1`, `SystemAccentColorDark1`, `SystemAccentColorDark2` y `SystemAccentColorDark3`. También está disponible mediante programación a través de [UISettings.GetColorValue](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx) y la enumeración [UIColorType](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx).
    </div>
</aside>

## Temas de color

El usuario también puede elegir entre un tema claro u oscuro para el sistema. Algunas aplicaciones optan por cambiar su tema según las preferencias del usuario, mientras que otras no ofrecen esa opción.

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
![El tema oscuro alternativo](images/themes-dark-alt.png)
#### Lista
![El tema oscuro de lista](images/themes-dark-list.png)
#### Cromo
![El tema oscuro cromo](images/themes-dark-chrome.png)

<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            Cada color está disponible como XAML [recurso de tema](https://msdn.microsoft.com/library/windows/apps/Mt187274.aspx#the_xaml_color_ramp_and_theme-dependent_brushes) que sigue la convención de nomenclatura `System*Color` (ej.: `SystemChromeHighColor`). Puedes controlar el tema de la aplicación a través de [Application.RequestedTheme](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.requestedtheme.aspx) o [FrameworkElement.RequestedTheme](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.requestedtheme.aspx).
    </div>
</aside>

## Accesibilidad

La paleta está optimizada para el uso de la pantalla. Se recomienda mantener una relación de contraste para el texto de 4,5:1 respecto al fondo para mejorar la legibilidad. Hay muchas herramientas gratuitas para probar si los colores la cumplen, como [Contrast Ratio](http://leaverou.github.io/contrast-ratio/).


<!--HONumber=May16_HO2-->


