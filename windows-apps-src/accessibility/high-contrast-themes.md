---
Description: Describe los pasos necesarios para asegurarte que una aplicación para la Plataforma universal de Windows (UWP) pueda usarse cuando un tema de contraste alto esté activo.
title: Temas de contraste alto
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
label: High-contrast themes
template: detail.hbs
---

Temas de contraste alto
=============================================================================

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Describe los pasos necesarios para asegurarte de que una aplicación para la Plataforma de Windows universal (UWP) pueda usarse cuando un tema de contraste alto esté activo.

De manera predeterminada, una aplicación para UWP admite temas de contraste alto. Si un usuario elige que el sistema use un tema de contraste alto en la configuración del sistema o en las herramientas de accesibilidad, el marco usa automáticamente la configuración de colores y estilos que producen un diseño y una representación de contraste alto para los controles y los componentes de la interfaz de usuario.

Esta compatibilidad predeterminada se basa en el uso de temas y plantillas predeterminados. Estos temas y plantillas hacen referencia a colores del sistema como definiciones de recursos, y los orígenes de los recursos se modifican automáticamente cuando el sistema usa un modo de contraste alto. Sin embargo, si usas estilos, temas y plantillas personalizados para el control, ten cuidado de no deshabilitar la compatibilidad integrada para contraste alto. Si usas uno de los diseñadores XAML para Microsoft Visual Studio para la aplicación de estilos, el diseñador generará un tema de contraste alto independiente junto con el tema principal siempre que definas una plantilla que sea significativamente diferente a la predeterminada. Los diccionarios de temas independientes se incluyen en la colección [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/BR208807), una propiedad dedicada de un elemento [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794).

Para más información sobre temas y plantillas de control, consulta [Inicio rápido: plantillas de control](https://msdn.microsoft.com/library/windows/apps/xaml/Hh465374). Suele ser muy revelador consultar controles específicos en los temas y diccionarios de recursos XAML y ver cómo se construyen los temas y cómo hacen referencia a los recursos que son similares aunque diferentes para cada posible opción de configuración de contraste alto.

<span id="Detecting_when_a_high-contrast_theme_is_enabled"></span><span id="detecting_when_a_high-contrast_theme_is_enabled"></span><span id="DETECTING_WHEN_A_HIGH-CONTRAST_THEME_IS_ENABLED"></span>Detectar cuando está habilitado un tema de contraste alto
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Una aplicación para UWP puede usar miembros de la clase [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) para detectar la configuración actual de temas de contraste alto. La propiedad [**HighContrast**](https://msdn.microsoft.com/library/windows/apps/BR242237_highcontrast) determina si un tema de contraste alto está seleccionado actualmente. Si el valor **HighContrast** está establecido en **true**, el siguiente paso es comprobar el valor de la propiedad [**HighContrastScheme**](https://msdn.microsoft.com/library/windows/apps/BR242237_highcontrastscheme) para obtener el nombre del tema de contraste alto que se usa. Los temas "Blanco en contraste alto" y "Negro en contraste alto" normalmente son valores de **HighContrastScheme** a los que el código debería responder. Las claves de [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) definidas mediante XAML no pueden tener espacios, por lo que las claves para estos temas en un diccionario de recursos normalmente son "HighContrastWhite" y "HighContrastBlack", respectivamente. También deberías tener una lógica de reserva para un tema de contraste alto predeterminado en caso de que el valor sea alguna otra cadena. En el [ejemplo de contraste alto XAML](http://go.microsoft.com/fwlink/p/?linkid=254993) se muestra esta lógica.

**Nota:** Asegúrate de llamar al constructor [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) desde un ámbito cuando la aplicación esté inicializada y ya esté mostrando contenido.

 

Las aplicaciones pueden cambiar para usar valores de recursos de contraste alto mientras la aplicación se encuentra en ejecución. Esto funciona siempre que los recursos se soliciten con la [extensión de marcado {ThemeResource}](https://msdn.microsoft.com/library/windows/apps/Mt185591) en el código XAML del estilo o la plantilla. Todos los temas predeterminados (generic.xaml) usan esta técnica de extensión de marcado {ThemeResource}, por lo que obtendrás este comportamiento si usas los temas de control predeterminados. Los controles personalizados o los estilos de controles personalizados pueden hacerlo si también usaste esta técnica de recurso de extensión de marcado {ThemeResource} en tus plantillas y estilos personalizados.

<span id="related_topics"></span>Temas relacionados
-----------------------------------------------

* [Accesibilidad](accessibility.md)
* [Ejemplo de configuración y contraste de la interfaz de usuario](http://go.microsoft.com/fwlink/p/?linkid=231539)
* [Ejemplo de accesibilidad XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [Ejemplo de contraste alto XAML](http://go.microsoft.com/fwlink/p/?linkid=254993)
* [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)
 

 





<!--HONumber=Mar16_HO3-->


