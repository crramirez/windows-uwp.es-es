---
Description: Procedimientos de prueba que debes seguir para asegurarte de que la aplicación para la Plataforma universal de Windows (UWP) sea accesible.
ms.assetid: 272D9C9E-B179-4F5A-8493-926D007A0225
title: Pruebas de accesibilidad
label: Accessibility testing
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8af03b32453bcdacb3da95678cf23a988c375f1b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359640"
---
# <a name="accessibility-testing"></a>Pruebas de accesibilidad  

Procedimientos de prueba que debes seguir para asegurarte de que la aplicación para la Plataforma universal de Windows (UWP) sea accesible.

<span id="run_accessibility_testing_tools"/>
<span id="RUN_ACCESSIBILITY_TESTING_TOOLS"/>

## <a name="run-accessibility-testing-tools"></a>Ejecutar herramientas de prueba de accesibilidad  
El Kit de desarrollo de software (SDK) de Windows incluye varias herramientas de prueba de accesibilidad, como [**AccScope**](https://docs.microsoft.com/windows/desktop/WinAuto/accscope), [**Inspect**](https://docs.microsoft.com/windows/desktop/WinAuto/inspect-objects) y [**UI Accessibility Checker**](https://docs.microsoft.com/windows/desktop/WinAuto/ui-accessibility-checker). Estas herramientas pueden ayudarte a comprobar la accesibilidad de tu aplicación. Asegúrate de comprobar todos los escenarios de aplicación y los elementos de la interfaz de usuario.

Puedes iniciar las herramientas de prueba de accesibilidad desde un símbolo del sistema de Microsoft Visual Studio o desde la carpeta de herramientas de Windows SDK (el subdirectorio "bin" donde se ha instalado Windows SDK en la máquina de desarrollo).
  
> [!VIDEO https://www.youtube.com/embed/ce0hKQfY9B8]
  
<span id="AccScope"/>
<span id="accscope"/>
<span id="ACCSCOPE"/>

### <a name="accscope"></a>**AccScope**  

La herramienta [**AccScope**](https://docs.microsoft.com/windows/desktop/WinAuto/accscope) permite que los desarrolladores y los evaluadores evalúan la accesibilidad de su aplicación durante la fase de desarrollo y diseño de la aplicación, posiblemente en fases prototipo anteriores, en lugar de hacerlo en las fases de prueba más avanzadas del ciclo de desarrollo de la aplicación. Está diseñada especialmente para probar los escenarios de accesibilidad de la característica Narrador con la aplicación.

<span id="inspect"/>
<span id="INSPECT"/>

### <a name="inspect"></a>**Inspeccionar**  

[**Inspeccionar** ](https://docs.microsoft.com/windows/desktop/WinAuto/inspect-objects) le permite seleccionar cualquier elemento de interfaz de usuario y ver sus datos de accesibilidad. Puedes ver los modelos de control y las propiedades de Automatización de la interfaz de usuario de Microsoft y probar la estructura de navegación de los elementos de automatización en el árbol de automatización de la interfaz de usuario. Usa **Inspect** mientras desarrollas la interfaz de usuario para comprobar cómo los atributos de accesibilidad se exponen en la automatización de la interfaz de usuario. En algunos casos, los atributos provienen de la compatibilidad para la automatización de la interfaz de usuario que ya viene implementada en los controles XAML predeterminados. En otros casos, los atributos provienen de valores específicos que definiste en el marcado XAML, como propiedades adjuntas [**AutomationProperties**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties).

En la siguiente imagen se muestra la herramienta [**Inspect**](https://docs.microsoft.com/windows/desktop/WinAuto/inspect-objects) consultando las propiedades de Automatización de la interfaz de usuario del elemento de menú **Editar** del Bloc de notas.

![Captura de pantalla de la herramienta Inspeccionar.](./images/inspect.png)

<span id="ui_accessibility_checker"/>
<span id="UI_ACCESSIBILITY_CHECKER"/>

### <a name="ui-accessibility-checker"></a>**UI Accessibility Checker**  
**UI Accessibility Checker (AccChecker)** te ayuda a detectar problemas de accesibilidad en tiempo de ejecución. Cuando la interfaz de usuario esté completa y en funcionamiento, usa **AccChecker** para probar diferentes escenarios, comprobar si la información de accesibilidad en tiempo de ejecución es correcta y detectar problemas en tiempo de ejecución. Puedes ejecutar **AccChecker** en el modo de línea de comandos o de interfaz de usuario. Para ejecutar la herramienta en modo de interfaz de usuario, abre el directorio de **AccChecker** en el directorio "bin" de Windows SDK, ejecuta acccheckui.exe y haz clic en el menú **Ayuda**.

<span id="ui_automation_verify"/>
<span id="UI_AUTOMATION_VERIFY"/>

### <a name="ui-automation-verify"></a>**UI Automation Verify**  
**UI Automation Verify (UIA Verify)** es un marco automatizado de pruebas y verificación para las implementaciones de la Automatización de la interfaz de usuario. **UIA Verify** puede integrarse en el código de prueba y realizar pruebas automatizadas periódicas o comprobaciones puntuales de los escenarios de Automatización de la interfaz de usuario. Para ejecutar **UIA Verify**, ejecuta VisualUIAVerifyNative.exe desde el subdirectorio UIAVerify.

<span id="accessible_event_watcher"/>
<span id="ACCESSIBLE_EVENT_WATCHER"/>

### <a name="accessible-event-watcher"></a>**Accessible Event Watcher**  
[**Monitor de eventos accesibles (AccEvent)** ](https://docs.microsoft.com/windows/desktop/WinAuto/accessible-event-watcher) comprueba si los elementos de interfaz de usuario de una aplicación activan eventos de UI Automation y Microsoft Active Accessibility en adecuado cuando se producen cambios de la interfaz de usuario. Se pueden producir cambios en la interfaz de usuario cuando cambia el foco, cuando se invoca o se selecciona un elemento de la interfaz de usuario o cambia uno de sus estados o propiedades.

> [!NOTE]
> La mayoría de las herramientas de prueba de accesibilidad mencionadas en la documentación se ejecutan en un equipo, no en un teléfono. Puedes ejecutar algunas herramientas mientras desarrollas y usas un emulador, aunque la mayoría de estas herramientas no pueden exponer el árbol de automatización de la interfaz de usuario en el emulador.

<span id="test_keyboard_accessibility"/>
<span id="TEST_KEYBOARD_ACCESSIBILITY"/>

## <a name="test-keyboard-accessibility"></a>Probar la accesibilidad de teclado  
La mejor manera de probar tu accesibilidad de teclado es desconectar el mouse o usar el teclado en pantalla si estás usando un dispositivo de tableta. Prueba la navegación de accesibilidad de teclado usando la tecla de _tabulador_. Deberías poder recorrer todos los elementos interactivos de la interfaz de usuario usando la tecla de _tabulador_. Para los elementos compuestos de la interfaz de usuario, comprueba si puedes navegar por las diferentes partes de los elementos usando las teclas de dirección. Por ejemplo, deberías poder navegar por listas de elementos con las teclas de dirección. Finalmente, asegúrate de que puedas invocar a todos los elementos interactivos de la interfaz de usuario con el teclado una vez que esos elementos tengan el foco, normalmente usando la tecla Entrar o Barra espaciadora.

<span id="verify_the_contrast_ratio_of_visible_text"/>
<span id="VERIFY_THE_CONTRAST_RATIO_OF_VISIBLE_TEXT"/>

## <a name="verify-the-contrast-ratio-of-visible-text"></a>Comprobar la relación de contraste del texto visible  
Usa herramientas de contraste de color para comprobar que la relación de contraste del texto visible sea aceptable. Algunas excepciones son los elementos de interfaz de usuario inactivos y los logotipos o el texto decorativo que no transmiten ninguna información y pueden reordenarse sin cambiar el significado. Consulta el tema sobre [requisitos de texto accesible](accessible-text-requirements.md) para obtener más información sobre excepciones y relación de contraste. Consulta [Técnicas de WCAG 2.0 G18 (Sección recursos)](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources) para ver las herramientas que pueden probar las relaciones de contraste.

> [!NOTE]
> Algunas de las herramientas indicadas en el tema sobre las técnicas de WCAG 2.0 G18 no se pueden usar de manera interactiva con una aplicación para UWP. Es posible que debas escribir manualmente en la herramienta los valores de color de primer plano y fondo, realizar capturas de pantalla de la interfaz de usuario de la aplicación y luego ejecutar la herramienta de relación de contraste sobre la imagen de la captura de pantalla, o ejecutar la herramienta a la vez que abres los archivos de mapa de bits en un programa de edición de imágenes en lugar de hacerlo al cargar la imagen en la aplicación.

<span id="verify_your_app_in_high_contrast"/>
<span id="VERIFY_YOUR_APP_IN_HIGH_CONTRAST"/>

## <a name="verify-your-app-in-high-contrast"></a>Comprobar tu aplicación en contraste alto  
Usa tu aplicación mientras esté activo un tema de contraste alto para comprobar que todos los elementos de la interfaz de usuario se muestren correctamente. Todo el texto debe poder leerse y todas las imágenes deben verse con claridad. Ajusta las plantillas de control o los recursos XAML del diccionario de temas para corregir cualquier problema en los temas que provenga de los controles. En los casos en que los problemas de contraste alto destacados no provengan de temas o controles (por ejemplo, archivos de imagen), proporciona versiones diferentes para usar cuando esté activo un tema de contraste alto.

<span id="verify_your_app_with_make_everything_on_your_screen_bigger"/>
<span id="VERIFY_YOUR_APP_WITH_MAKE_EVERYTHING_ON_YOUR_SCREEN_BIGGER"/>

## <a name="verify-your-app-with-display-settings"></a>Comprobar la aplicación con configuración de pantalla  

Usa las opciones de pantalla del sistema para ajustar el valor de puntos por pulgada (ppp) de la pantalla y asegúrate de que la interfaz de usuario de la aplicación se escala correctamente cuando cambie el valor de ppp. (Algunos usuarios cambian valores de PPP como una opción de accesibilidad, está disponible en **de accesibilidad** , así como mostrar las propiedades.) Si encuentra algún problema, siga el [directrices para el diseño escalado](https://developer.microsoft.com/windows/design) y proporcionar recursos adicionales para diferentes factores de escala.

<span id="verify_main_app_scenarios_by_using_narrator"/>
<span id="VERIFY_MAIN_APP_SCENARIOS_BY_USING_NARRATOR"/>

## <a name="verify-main-app-scenarios-by-using-narrator"></a>Comprobar los escenarios de aplicaciones principales mediante el uso del Narrador  
Utiliza el Narrador para probar la experiencia de lectura de pantalla de la aplicación.

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode/player]

**Para probar la aplicación con Narrador con un mouse y teclado, siga estos pasos:**
1.  Inicia el Narrador presionando la _tecla del logotipo de Windows, Ctrl y Entrar_. En versiones anteriores a Windows 10, versión 1607, usa _tecla del logotipo de Windows y Entrar_ para iniciar el Narrador.
2.  Navega por la aplicación con el teclado utilizando la tecla de _tabulador_, las teclas de dirección y la combinación _tecla Bloq Mayús + teclas de dirección_.
3.  Conforme estás navegando por la aplicación, escucha cómo lee Narrador los elementos de la interfaz de usuario y comprueba lo siguiente:
    * En cada control, asegúrate de que el Narrador lee todo el contenido visible. Comprueba también que el Narrador lee el nombre de cada control, los estados aplicables (comprobado, seleccionado, etc.) y los tipos de control (botón, casilla, elemento de lista, etc.).
    * Si el elemento es interactivo, comprueba que puedes usar el Narrador para invocar su acción presionando las teclas _Bloq Mayús + Entrar_.
    * En cada tabla, asegúrate de que Narrador lea correctamente el nombre de la tabla, su descripción (si está disponible) y los encabezados de fila y columna.

4.  Presiona _Bloq Mayús + Mayús+ Entrar_ para buscar la aplicación y comprobar que todos los controles aparecen en la lista de búsqueda y que los nombres de los controles están localizados y se pueden leer.
5.  Apaga el monitor e intenta lograr los escenarios utilizando solamente el teclado y el Narrador. Para obtener la lista completa de los comandos y accesos directos del Narrador, presiona _Bloq Mayús + F1_.

A partir de Windows 10, versión 1607, presentamos un nuevo modo de desarrollador en Narrador. Activa el modo de desarrollador cuando Narrador ya esté en ejecución presionando _Bloq Mayús + Mayús + F12_. Cuando el modo de desarrollador esté habilitado, la pantalla estará enmascarada y destacará solamente los objetos accesibles y el texto asociado que se expone mediante programación a Narrador. Esto te ofrece un una buena representación visual de la información que se expone a Narrador.

**Siga estos pasos para probar la aplicación mediante el modo de interacción del Narrador:**

> [!NOTE]
> El Narrador introduce automáticamente el modo táctil en los dispositivos que admiten contactos 4+. Narrador no admite escenarios de varios monitores o digitalizadores multitáctiles en la pantalla principal.

1.  Familiarízate con la interfaz de usuario y analiza el diseño.

    * **Navegar por la interfaz de usuario mediante el uso de gestos de pasar el dedo un solo dedo.** Usa los deslizamientos rápidos hacia la derecha o hacia la izquierda para moverte entre los elementos y hacia arriba o hacia abajo para cambiar la categoría de los elementos por los que navegas. Las categorías incluyen todos los elementos, los vínculos, las tablas, los encabezados, etc. La navegación con gestos de deslizar rápidamente con un solo dedo es similar a navegar con _Bloq Mayús + tecla de dirección_.
    * **Usar gestos de la pestaña para desplazarse por los elementos pueden recibir el foco.** Un deslizamiento con tres dedos hacia la derecha o hacia la izquierda es lo mismo que navegar con el _tabulador_ y _Mayús + Tabulador_ en un teclado.
    * **Espacialmente investigar la interfaz de usuario con un solo dedo.** Arrastra un dedo hacia arriba y abajo o hacia la derecha y la izquierda, para que Narrador lea los elementos bajo el dedo. Puedes usar el mouse como alternativa puesto que usa la misma lógica de posicionamiento que se utiliza al arrastrar un dedo.
    * **Lee toda la ventana y todo su contenido deslizando tres dedos hacia arriba**. Esto equivale a usar _Bloq Mayús + W_.

    Si hay partes importantes de la interfaz de usuario a las que no puedes acceder, es posible que tengas un problema de accesibilidad.

2.  Interactúa con un control para probar sus acciones principales y secundarias, así como su comportamiento de desplazamiento.

    Entre las acciones principales se incluyen cosas como activar un botón, colocar un símbolo de intercalación de texto y establecer el foco al control. Entre las acciones secundarias se incluyen algunas como seleccionar un elemento de la lista o expandir un botón que ofrece varias opciones.

    * Para probar una acción principal: Pulsar doble, o presione con un solo dedo y puntee en Sí.
    * Para probar una acción secundaria: Puntee en el triple, o presione con un solo dedo y pulsar dos veces con otro.
    * Para probar el comportamiento de desplazamiento: Use los deslizamientos con dos dedos para desplazarse en la dirección deseada.

    Algunos controles proporcionan acciones adicionales. Para mostrar la lista completa, realiza una pulsación con cuatro dedos.

    Si un control responde al mouse o al teclado, pero no responde a una interacción táctil principal o secundaria, es posible que el control necesite implementar patrones de control de [Automatización de la interfaz de usuario](https://docs.microsoft.com/windows/desktop/WinAuto/entry-uiauto-win32).

También debes considerar la posibilidad de usar la herramienta [**AccScope**](https://docs.microsoft.com/windows/desktop/WinAuto/accscope) para probar los escenarios de accesibilidad de Narrador con tu aplicación. El tema sobre la herramienta [**AccScope**](https://docs.microsoft.com/windows/desktop/WinAuto/accscope) describe cómo configurar **AccScope** para probar los escenarios de Narrador.

<span id="Examine_the_UI_Automation_representation_for_your_app"/>
<span id="examine_the_ui_automation_representation_for_your_app"/>
<span id="EXAMINE_THE_UI_AUTOMATION_REPRESENTATION_FOR_YOUR_APP"/>

## <a name="examine-the-ui-automation-representation-for-your-app"></a>Examina la representación de la Automatización de la interfaz de usuario para la aplicación  
Algunas de las herramientas de prueba de Automatización de la interfaz de usuario antes mencionadas permiten ver la aplicación sin tener en cuenta deliberadamente su aspecto, sino que la representa como una estructura de elementos de Automatización de la interfaz de usuario. Así es como los clientes de Automatización de la interfaz de usuario, en especial las tecnologías de asistencia, interactuarán con tu aplicación en escenarios de accesibilidad.

La herramienta [**AccScope**](https://docs.microsoft.com/windows/desktop/WinAuto/accscope) proporciona un punto de vista interesante sobre la aplicación porque permite ver los elementos de Automatización de la interfaz de usuario como una representación visual o como una lista. Si usas la visualización, puedes examinar con detalle las partes de forma que puedas relacionarlas con el aspecto visual de la interfaz de usuario de la aplicación. Incluso puedes probar la accesibilidad de los primeros prototipos de la interfaz de usuario antes de asignar toda la lógica a la interfaz de usuario, para asegurarte de que tanto la interacción visual como la navegación en escenarios de accesibilidad para la aplicación estén equilibradas.

Un aspecto que puedes probar es si en la vista de elementos de Automatización de la interfaz de usuario aparecen elementos que no quieres que aparezcan allí. Si encuentras elementos que quieres omitir de la vista o, por el contrario, si faltan elementos, puedes usar la propiedad adjunta XAML [**AutomationProperties.AccessibilityView**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.accessibilityview) para ajustar cómo aparecerán los controles XAML en las vistas de accesibilidad. Después de revisar las vistas de accesibilidad básicas, es un buen momento para volver a comprobar las secuencias de tabulación o la navegación espacial habilitadas por las teclas de dirección para asegurarte de que los usuarios llegan a todos los elementos interactivos y que se exponen en la vista control.

<span id="related_topics"/>

## <a name="related-topics"></a>Temas relacionados  
* [Accesibilidad](accessibility.md)
* [Prácticas que se deben evitar](practices-to-avoid.md)
* [Automatización de la interfaz de usuario](https://docs.microsoft.com/windows/desktop/WinAuto/entry-uiauto-win32)
* [Accesibilidad en Windows](https://go.microsoft.com/fwlink/p/?LinkId=320802)
* [Empezar a trabajar con Narrador](https://support.microsoft.com/help/22798/windows-10-narrator-get-started)
