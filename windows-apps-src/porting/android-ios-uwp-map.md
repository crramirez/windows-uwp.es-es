---
title: Compara las características de plataforma entre iOS, Android y Windows 10.
description: Vea una comparación detallada de los conceptos de desarrollo y las características de la plataforma entre iOS, Android y el Plataforma universal de Windows (UWP) en Windows 10.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 082736c8-2ac3-41b3-b246-e705edc23f34
ms.localizationpriority: medium
ms.openlocfilehash: 21cb4c105cc4c95c3a14c4c5bd0049265682f91c
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220588"
---
# <a name="windows-apps-concept-mapping-for-android-and-ios-developers"></a>Asignación del concepto de aplicaciones de Windows para desarrolladores de Android e iOS

Si eres un desarrollador con conocimientos o código de Android o iOS, y quieres hacer el cambio a Windows 10 y la Plataforma universal de Windows (UWP), este recurso tiene todo lo que necesitas para asignar características de la plataforma, y tus conocimientos, entre las tres plataformas.

Consulta también el contenido de migración en [Migrar de iOS a UWP](ios-to-uwp-root.md). Este documento también está disponible como [descarga](https://www.microsoft.com/download/details.aspx?id=52041).

## <a name="user-interface-ui"></a>Interfaz de usuario (UI)


<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concepto general</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP de Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Lenguaje de diseño.</strong><br><br>Un conjunto de convenciones que indica el aspecto y comportamiento de las aplicaciones en la plataforma.</td>
<td align="left">En las directrices <strong>Diseño de material de Android</strong> se proporciona un lenguaje visual que deben seguir los diseñadores y desarrolladores de Android.</td>
<td align="left">En <strong>Directrices de interfaz humana</strong> se ofrece asesoramiento para los desarrolladores y diseñadores de iOS.</td>
<td align="left"><a href="https://developer.microsoft.com/windows/apps/design"><strong>Diseño de aplicaciones de Windows para UWP</strong></a> muestra cómo crear una aplicación que tenga un aspecto fantástico en todos los dispositivos Windows 10. Encontrarás aspectos básicos sobre el diseño de la interfaz de usuario (UI), técnicas de diseño dinámico y una lista completa de directrices detalladas.<br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Lenguaje de marcado de la interfaz de usuario.</strong> <br><br>Un lenguaje de marcado que representa y describe una interfaz de usuario y sus componentes. En cada plataforma se proporciona un editor para la edición tanto visual y como de marcado.<br/></td>
<td align="left"><strong>Diseños XML</strong> que se editan mediante <strong>Studio Android</strong> o <strong>Eclipse</strong>.</td>
<td align="left"><strong>XIB</strong> y <strong>guiones gráficos</strong> que se editan mediante el <strong>configurador de interfaz de usuario</strong> en Xcode.</td>
<td align="left"><strong><a href="/windows/uwp/xaml-platform/xaml-overview">XAML</a></strong>que se edita mediante <strong><a href="https://visualstudio.microsoft.com/">Microsoft Visual Studio</a></strong> y <strong><a href="/visualstudio/designers/creating-a-ui-by-using-blend-for-visual-studio?view=vs-2015">Blend para Visual Studio</a></strong>.<br/><br/><a href="/windows/uwp/xaml-platform/index">Plataforma XAML</a><br/><br/><a href="/windows/uwp/design/basics/xaml-basics-ui">Crear una interfaz de usuario con XAML</a><br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">Definir diseños con XAML</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Controles de interfaz de usuario integrados.</strong> <br><br>Elementos de interfaz de usuario que proporciona la plataforma, como botones, controles de lista y controles de texto.</td>
<td align="left">Clases <strong>view</strong> y <strong>view group</strong> predefinidas conocidas como widgets, diseños, campos de texto, contenedores, controles de fecha y hora, y los controles expertos.</td>
<td align="left"><strong>Vistas</strong> y <strong>controles</strong> que se encuentran en la biblioteca de objetos de Xcode y que se enumeran en el catálogo de interfaz de usuario UIKit. Entre las vistas se incluyen vistas de imagen, vistas de selector y vistas de desplazamiento. Entre los controles se incluyen botones, selectores de fecha y campos de texto.</td>
<td align="left">En la plataforma XAML se proporciona un conjunto amplio de <strong>controles integrados</strong>, como botones, controles de lista, paneles, controles de texto, barras de comandos, selectores, contenido multimedia y la entrada manuscrita.<br/><br/><a href="/windows/uwp/controls-and-patterns/controls-and-events-intro">Agregar controles y controlar eventos</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Control de eventos de control.</strong> <br><br>Define la lógica que se ejecuta cuando se desencadenan eventos en los controles de interfaz de usuario.</td>
<td align="left">Los <strong>controladores de eventos</strong> y <strong>escuchas de eventos</strong> se agregan en XML o mediante programación.</td>
<td align="left">Los controles envían mensajes de <strong>acción</strong> a los <strong>destinos</strong>.</td>
<td align="left">Puedes definir métodos para controlar los eventos de un control XAML en un <strong>archivo de código subyacente</strong> adjunto a la página XAML. Los <strong>controladores de eventos</strong> siempre se escriben en el código. Sin embargo, puedes enlazar esos controladores a los eventos en el marcado XAML o en el código.<br/><br/><a href="/windows/uwp/controls-and-patterns/controls-and-events-intro">Agregar controles y controlar eventos</a><br/><br/><a href="/windows/uwp/xaml-platform/events-and-routed-events-overview">Introducción a eventos y eventos enrutados</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Enlace de datos.</strong> <br><br>Patrón de diseño de software que permite a la interfaz de usuario de la aplicación representar datos y, opcionalmente, mantenerse sincronizada con dichos datos.</td>
<td align="left">Se proporciona una <strong>biblioteca de enlaces de datos</strong>, aunque se encuentra en la versión beta.</td>
<td align="left">No existe ningún sistema de enlaces integrados en iOS. Se puede crear la <strong>observación de clave-valor</strong> para realizar el enlace de datos, ya sea con el uso de una biblioteca de terceros o escribiendo código adicional. Los controles usan un método de delegado o devolución de llamada para obtener los datos.</td>
<td align="left">La plataforma UWP controla el <strong>enlace de datos</strong> para ti. Use la extensión de marcado <strong><a href="/windows/uwp/xaml-platform/x-bind-markup-extension">{x:Bind}</a></strong> para aprovechar las ventajas del enlace de alto rendimiento o <strong><a href="/windows/uwp/xaml-platform/binding-markup-extension">{Binding}</a></strong> para aprovechar más características. Luego, solo tienes que configurar el enlace para elegir si la plataforma usará <strong>enlaces unidireccionales</strong> para mostrar los valores de un origen de datos en la interfaz de usuario o si también observará esos valores y actualizará la interfaz de usuario cuando cambien con <strong>enlaces bidireccionales</strong>.<br/><br/><a href="/windows/uwp/data-binding/index">Enlace de datos</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Automatización de la interfaz de usuario.</strong> <br><br>Acceso mediante programación a los elementos de la interfaz de usuario, lo que permite que las aplicaciones estén accesibles para los productos de tecnología de asistencia y permite el uso de scripts de prueba automatizados para su interacción con la interfaz de usuario.</td>
<td align="left">Los valores <strong>Text labels</strong>, <strong>contentDescription</strong> y <strong>hint</strong> ayudan a garantizar que los elementos de la interfaz de usuario se pueden encontrar mediante la automatización. Android Studio permite escribir pruebas de interfaz de usuario con los marcos de prueba <strong>UI Automator</strong> y <strong>Espresso</strong>.</td>
<td align="left">El <strong>instrumento de automatización</strong> permite escribir scripts de prueba de interfaz de usuario automatizados que identifican elementos mediante opciones de configuración de <strong>accesibilidad</strong> o la posición del elemento en la <strong>jerarquía de elementos</strong>.</td>
<td align="left">Con la configuración inicial, puedes acceder mediante programación a los elementos de interfaz de usuario integrados en UWP mediante la <strong><a href="/windows/desktop/WinAuto/uiauto-uiautomationoverview">automatización de la interfaz de usuario</a></strong>.<br/><strong>							La <a href="/windows/uwp/accessibility/custom-automation-peers">personalización de sistemas de automatización del mismo nivel</a></strong> te permite proporcionar compatibilidad para la automatización para tus propias clases de interfaz de usuario personalizadas. El <strong><a href="/visualstudio/test/use-ui-automation-to-test-your-code?view=vs-2015">proyecto de prueba de IU codificada</a></strong> en Visual Studio permite probar automáticamente toda la aplicación a través de la interfaz de usuario o probar la interfaz de usuario de forma aislada.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Cambiar la apariencia de un control.</strong> <br><br>Edita el tamaño, color y otros atributos.</td>
<td align="left">Los controles tienen <strong>propiedades</strong> que se pueden editar mediante la herramienta del diseñador, en el marcado XML o mediante programación.</td>
<td align="left">Los controles tienen <strong>atributos</strong> que se pueden editar con <strong>Attributes Inspector</strong> en el generador de interfaz de usuario o mediante programación.</td>
<td align="left">Puedes editar las <strong>propiedades</strong> de los controles en el marcado XAML o mediante programación, con Visual Studio y Blend para Visual Studio.<br/><br/><a href="/windows/uwp/controls-and-patterns/controls-and-events-intro">Agregar controles y controlar eventos</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Estilos visuales reutilizables.</strong> <br><br>Aplica cambios visuales en un número de controles en un formato reutilizable.</td>
<td align="left">Los <strong>estilos XML</strong> son conjuntos de propiedades que se aplican a uno o varios controles.</td>
<td align="left">En la configuración inicial, iOS no admite estilos visuales reutilizables, pero el protocolo UIAppearance permite que varios controles compartan atributos comunes.</td>
<td align="left">Puedes crear <strong><a href="/uwp/api/Windows.UI.Xaml.Style">estilos</a></strong> reutilizables, que se puede aplicar a varios controles y almacenar en un <strong><a href="/uwp/api/Windows.UI.Xaml.ResourceDictionary">ResourceDictionary</a></strong> para facilitar su reutilización.<br/><br/><a href="/previous-versions/windows/apps/hh465381(v=win.10)">Inicio rápido: aplicar estilos a los controles</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Editar la estructura visual de los controles.</strong> <br><br>Personalice la estructura visual de un control más allá simplemente modificando las propiedades o los atributos, por ejemplo, desplazando el texto de la casilla debajo de la casilla.</td>
<td align="left">En Android no existe ningún método simple para editar la estructura visual de los controles.</td>
<td align="left">En iOS no existe ningún método simple para editar la estructura visual de los controles.</td>
<td align="left">Para personalizar la estructura visual de un control, puedes copiar y editar su <strong><a href="/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate">plantilla de control</a></strong> en el marcado XAML.<br/><br/><a href="/previous-versions/windows/apps/hh465374(v=win.10)">Inicio rápido: Plantillas de control</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Gestos táctiles integrados.</strong> <br><br>Proporciona compatibilidad con la entrada táctil personalizada mediante el control de eventos de gestos resumidos de alto nivel, como pulsación y doble pulsación en las vistas y los controles.</td>
<td align="left">Los <strong>detectores de gestos</strong> detectan gestos táctiles comunes, como por ejemplo, desplazamiento, presión prolongada, pulsación, doble pulsación y deslizamiento.</td>
<td align="left">El marco UIKit proporciona <strong>reconocedores de gestos</strong> integrados que detectan gestos táctiles, como por ejemplo, pulsación, reducción, panorámica, deslizamiento, rotación y presión prolongada.</td>
<td align="left"><strong>Los elementos</strong> de la interfaz de usuario le permiten controlar <strong>los eventos de gestos estáticos</strong> , como puntear, pulsar doble, tocar y mantener pulsado el botón derecho, así como <strong>los eventos de gestos de manipulación</strong> , como deslizar, deslizar, girar, reducir y expandir. Los eventos de gestos son <strong>eventos enrutados</strong> y se pueden controlar mediante objetos primarios que contiene el elemento secundario UIElement.<br/><br/><a href="/windows/uwp/input-and-devices/touch-interactions">Interacciones táctiles</a><br/><br/><a href="/windows/uwp/design/layout/index">Interacciones del usuario personalizadas: gestos, manipulaciones e interacciones</a></td>
</tr>
</tbody>
</table>
<h2 id="navigation-and-app-structure">Navegación y estructura de aplicaciones</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concepto general</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP de Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Diseños.</strong> <br><br>El diseño define la estructura de la interfaz de usuario.</td>
<td align="left">El diseño está compuesto por <strong>grupos de vistas</strong>, tales como <strong>LinearLayout</strong> y <strong>RelativeLayout</strong>, que pueden anidar otras vistas o grupos de vistas.</td>
<td align="left">El diseño está compuesto por un objeto <strong>UIViewController</strong> que contiene objetos <strong>UIView</strong>, que se pueden anidar.</td>
<td align="left">XAML que proporciona un sistema de diseño flexible compuesto <strong>por clases del panel de diseño</strong> como <strong><a href="/uwp/api/windows.ui.xaml.controls.canvas">Canvas</a></strong>, <strong><a href="/uwp/api/windows.ui.xaml.controls.grid">Grid</a></strong>, <strong><a href="/uwp/api/windows.ui.xaml.controls.relativepanel">RelativePanel</a></strong> y <strong><a href="/uwp/api/windows.ui.xaml.controls.stackpanel">StackPanel</a></strong> para diseños estáticos y dinámicos. <strong><a href="/visualstudio/ide/reference/properties-window?view=vs-2015">Las propiedades</a></strong> se utilizan para controlar el tamaño y la posición de los elementos.<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">Definir diseños con XAML</a><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Navegación del mismo nivel.</strong> <br><br>Presentación al usuario de métodos de navegación entre páginas de la misma importancia jerárquica.</td>
<td align="left">Las <strong>pestañas</strong>, las <strong>vistas en dedo</strong> y los <strong>alimentadores de navegación</strong> proporcionan <strong>navegación lateral</strong>.</td>
<td align="left">Los controladores de <strong>barra de pestañas</strong>, <strong>los controladores de vista</strong> y los controladores de vista de <strong>Página</strong> permiten la navegación entre vistas de la misma jerarquía.</td>
<td align="left">Puedes mostrar una lista persistente de vínculos o pestañas situadas encima de contenido mediante <strong><a href="/windows/uwp/controls-and-patterns/tabs-pivot">pestañas o tablas dinámicas</a></strong>. El <strong><a href="/windows/uwp/controls-and-patterns/split-view">panel de navegación o vista en dos paneles</a></strong> permite mostrar una lista de vínculos junto con el contenido.<br/><br/><a href="/windows/uwp/layout/navigation-basics">Navegación</a><br/><br/><a href="/windows/uwp/layout/navigate-between-two-pages">Navegar entre dos páginas</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Navegación jerárquica.</strong> <br><br>Navegación entre las páginas principales y secundarias de una jerarquía.</td>
<td align="left"><strong>Las listas</strong>y <strong>las listas de cuadrículas</strong>, los <strong>botones</strong> y otros controles proporcionan <strong>navegación descendiente</strong> cuando se usan con <strong>intenciones</strong> para cargar otras <strong>actividades</strong>.</td>
<td align="left">Los <strong>controladores de navegación</strong> permiten a los usuarios navegar entre los niveles de una jerarquía.</td>
<td align="left"><strong>							Los <a href="/windows/uwp/controls-and-patterns/hub">controles de navegación centralizada</a></strong> permiten mostrar al usuario una vista previa del contenido que se puede seleccionar para navegar a páginas secundarias. <strong><a href="/windows/uwp/controls-and-patterns/master-details">Maestro/detalles</a></strong> permite que los usuarios elijan en una lista de resúmenes de elementos que se muestran junto a la sección de detalles correspondiente.<br/><br/><a href="/windows/uwp/layout/navigation-basics">Navegación</a><br/><br/><a href="/windows/uwp/layout/navigate-between-two-pages">Navegar entre dos páginas</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Navegación del botón atrás.</strong> <br><br>Navega hacia atrás por una aplicación.</td>
<td align="left">La <strong>Atrás</strong> y <strong>de</strong> botones dentro de la barra de acciones proporcionan <strong>ancestral</strong> y <strong>temporal</strong> de navegación usando la <strong>pila de retroceso</strong>.</td>
<td align="left">Al <strong>controlador de navegación</strong> se le puede agregar un botón Atrás.<br/></td>
<td align="left">Puedes controlar fácilmente las presiones del botón Atrás de software o hardware mediante la <strong><a href="/uwp/api/windows.ui.xaml.controls.frame.backstack">propiedad BackStack</a></strong>, que permite a los usuarios recorrer el <strong>historial de navegación</strong>.<br/><br/><a href="/windows/uwp/layout/navigation-history-and-backwards-navigation">Navegación con el botón Atrás</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Pantalla de presentación.</strong> <br><br>Muestra una imagen al iniciar la aplicación (se usa principalmente para la personalización de marca).</td>
<td align="left">Las pantallas de presentación no se proporcionan de manera predeterminada y se implementan al editar el <strong>fondo del tema</strong> de las actividades iniciales.</td>
<td align="left">Las aplicaciones deben tener una <strong>imagen de inicio estática</strong> o un <strong>archivo de inicio XIB o de guion gráfico</strong>.</td>
<td align="left">Puedes crear una pantalla de presentación con una <strong>imagen</strong> y un fondo de color. <a href="/windows/uwp/launch-resume/create-a-customized-splash-screen">El tiempo de la pantalla de presentación se puede ampliar</a>.<br/><br/><a href="/windows/uwp/launch-resume/add-a-splash-screen">Agregar una pantalla de presentación</a></td>
</tr>
</tbody>
</table>
<h2 id="custom-inputs">Entradas personalizadas</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concepto general</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP de Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Facturó.</strong> <br><br>Reconocimiento de voz para la entrada de voz y funcionalidades adicionales de voz.</td>
<td align="left">La entrada de voz puede la puede proporcionar cualquier aplicación que implemente un objeto <strong>RecognizerIntent</strong>, tal como <strong>Búsqueda por voz de Google</strong>. La clase <strong>SpeechRecognizer</strong> permite a las aplicaciones usar la API de reconocimiento de voz de Google.</td>
<td align="left">Las aplicaciones pueden usar la clase <strong>SFSpeechRecognizer</strong> para implementar la entrada de voz y el reconocimiento de voz.</td>
<td align="left">Puedes usar la API de <strong><a href="/windows/uwp/input-and-devices/speech-recognition">reconocimiento de voz</a></strong> para interactuar con la aplicación en primer plano. Puede usar <strong><a href="/windows/uwp/input-and-devices/cortana-interactions">interacciones de Cortana</a></strong> basadas en voz para iniciar aplicaciones en primer plano o en segundo plano y para interactuar con las aplicaciones en segundo plano.<br/><br/><a href="/windows/uwp/input-and-devices/speech-interactions">Interacciones de voz</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Entradas de usuario personalizadas.</strong> <br><br>Controla las entradas de teclado, mouse y lápiz, entre otras.</td>
<td align="left">La compatibilidad con interacciones incluye <strong>función táctil</strong>, <strong>panel táctil</strong>, <strong>lápiz</strong>, <strong>mouse</strong> y <strong>teclado</strong>. Los movimientos y las entradas se notifican de la misma manera que la función táctil, pero es posible detectar más información sobre el <strong>dispositivo de entrada</strong>.</td>
<td align="left">Se proporciona compatibilidad con <strong>función táctil</strong>, <strong>Apple Pencil</strong> y <strong>teclados</strong> de hardware.</td>
<td align="left">Encontrarás compatibilidad con una amplia variedad de interacciones, como por ejemplo, <strong><a href="/windows/uwp/input-and-devices/touch-interactions">función táctil</a></strong>, <strong><a href="/windows/uwp/input-and-devices/touchpad-interactions">panel táctil</a></strong>, <strong><a href="/windows/uwp/input-and-devices/pen-and-stylus-interactions">lápiz</a></strong> con entrada digital, <strong><a href="/windows/uwp/input-and-devices/mouse-interactions">mouse</a></strong> y <strong><a href="/windows/uwp/input-and-devices/keyboard-interactions">teclado</a></strong>. Las aplicaciones pueden controlar los datos sin tener que saber qué dispositivo de entrada se usó y, si fuera necesario, se pueden acceder a los datos de dispositivos de entrada sin procesar.<br/><br/><a href="/windows/uwp/input-and-devices/handle-pointer-input">Control de la entrada con puntero</a><br/><br/><a href="/windows/uwp/design/layout/index">Interacciones del usuario personalizadas</a></td>
</tr>
</tbody>
</table>
<h2 id="data">data</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concepto general</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP de Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Datos de la aplicación local.</strong> <br><br>Almacena localmente la configuración y los archivos relacionados con la aplicación.</td>
<td align="left">Los archivos locales se pueden guardar mediante <strong>openFileOutput</strong> y <strong>openFileInput</strong>. La configuración en un <strong>archivo de preferencias compartidas</strong> es accesible mediante <strong>getSharedPreferences</strong>.</td>
<td align="left">Los archivos locales se pueden almacenar en el directorio de <strong>compatibilidad con aplicaciones</strong>, al que se accede a través de la clase <strong>NSFileManager</strong>. La configuración en los archivos de <strong>preferencias</strong> es accesible mediante la clase <strong>NSUserDefaults</strong>.</td>
<td align="left">Las clases <strong><a href="/uwp/api/Windows.Storage">Windows.Storage</a></strong> controlan el almacenamiento de datos local de manera unificada. La configuración se almacena como un objeto <strong><a href="/uwp/api/Windows.Storage.ApplicationDataContainer">ApplicationDataContainer</a></strong> , al que se tiene acceso a través de la propiedad <strong><a href="/uwp/api/windows.storage.applicationdata.localsettings">ApplicationData. LocalSettings</a></strong> . Los archivos se almacenan en un objeto <strong><a href="/uwp/api/windows.storage.storagefolder">StorageFolder</a></strong>, al que se accede mediante la propiedad <strong><a href="/uwp/api/windows.storage.applicationdata.localfolder">ApplicationData.LocalFolder</a></strong>.<br/><br/><a href="/windows/uwp/app-settings/store-and-retrieve-app-data">Almacenar y recuperar la configuración y otros datos de aplicación</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Almacenamiento local de la base de datos.</strong> <br><br>Almacena los datos de la aplicación en una base de datos relacional, con asignadores relacionales de objetos (ORM), si corresponde.</td>
<td align="left">Se proporciona la base de datos <strong>SQLite</strong>. No hay ORM integrados. Las consultas SQL se ejecutan mediante la clase <strong>SQLiteDatabase</strong>.</td>
<td align="left">Se proporciona la base de datos <strong>SQLite</strong>. <strong>CoreData</strong> es el marco de trabajo de gráficos de objetos integrado que se puede usar con SQLite y proporcionar funcionalidad comparable con un ORM.</td>
<td align="left">Puedes almacenar datos mediante <strong>SQLite</strong>. <strong><a href="/windows/uwp/data-access/entity-framework-7-with-sqlite-for-csharp-apps">Entity Framework</a></strong> es un ORM integrado que elimina la necesidad de escribir grandes cantidades de código de acceso a datos y permite consultar fácilmente la base de datos sin necesidad de escribir código SQL. Puedes ejecutar consultas SQL directamente con la <a href="/windows/uwp/data-access/sqlite-databases">biblioteca SQLite</a>.<br/><br/><a href="/windows/uwp/data-access/index">Acceso a datos</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Bibliotecas HTTP para el acceso REST.</strong> <br><br>Bibliotecas integradas que te permiten comunicarte con servicios web y servidores web mediante HTTP(S).<br/></td>
<td align="left">Bibliotecas HTTP <strong>HttpURLConnection</strong> y <strong>Volley</strong>.</td>
<td align="left"><strong>NSURLSession</strong>, <strong>NSURLConnection</strong> y <strong>NSURLDownload</strong>.</td>
<td align="left">Puedes usar la API <strong><a href="/uwp/api/Windows.Web.Http.HttpClient">HttpClient</a></strong> integrada para acceder a la funcionalidad HTTP común, como por ejemplo, GET, DELETE, PUT, POST, patrones comunes de autenticación, SSL, cookies e información de progreso.</td>
</tr>
<tr class="even">
<td align="left"><strong>Servicios de copia de seguridad en la nube.</strong> <br><br>Servicios de copia de seguridad proporcionados por la plataforma para los datos de la aplicación.</td>
<td align="left">El <strong>administrador de copias de seguridad</strong> de Android controla la creación de copias de seguridad de datos de aplicación en <strong>Android Backup Service</strong> de Google.</td>
<td align="left">Un usuario puede configurar la <strong>copia de seguridad de iCloud</strong> para controlar sus copias de seguridad, como por ejemplo, los datos de aplicación. Aplicaciones que usan <strong>datos principales</strong> compatibles con iCloud, el <strong>almacén de pares clave-valor de iCloud</strong> y el <strong>almacenamiento de documentos de iCloud</strong>.</td>
<td align="left">Los datos de aplicaciones que se almacenan con las <strong><a href="/uwp/api/windows.storage.applicationdata">API de ApplicationData</a></strong> de itinerancia (incluidos <strong><a href="/uwp/api/windows.storage.applicationdata.roamingfolder">RoamingFolder</a></strong> y <a href="/uwp/api/windows.storage.applicationdata.roamingsettings"><strong>RoamingSettings</strong></a>) se sincronizarán automáticamente con la nube y también con los demás dispositivos del usuario. La sincronización se consigue mediante la cuenta de Microsoft del usuario.<br/><br/><a href="/windows/uwp/design/app-settings/store-and-retrieve-app-data">Directrices para datos de la aplicación de itinerancia</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Descargas de archivos HTTP.</strong> <br><br>Descarga archivos de pequeño y gran tamaño a través de HTTP.</td>
<td align="left"><strong>URLConnection</strong> y <strong>HTTPURLConnection</strong> se usan para descargar a través de http y FTP, también es posible hacer uso del administrador de <strong>descargas</strong> del sistema para descargar en segundo plano.</td>
<td align="left">Los objetos <strong>NSURLSession</strong> y <strong>NSURLConnection</strong> se pueden usar para descargar archivos a través de HTTP y FTP.</td>
<td align="left">Las <strong><a href="/uwp/api/windows.networking.backgroundtransfer">API de transferencia en segundo plano</a></strong> permiten transferir archivos de manera confiable a través de HTTP(S) y FTP, teniendo en cuenta la suspensión de la aplicación, la pérdida de conectividad y los ajustes según la conectividad y duración de la batería. También puede usar <strong><a href="/uwp/api/windows.web.http.httpclient">HttpClient</a></strong> , que es ideal para archivos más pequeños.<br/><br/><a href="/windows/uwp/networking/which-networking-technology">¿Qué tecnología de red?</a><br/><br/><a href="/windows/uwp/networking/background-transfers">Transferencias en segundo plano</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Sockets.</strong> <br><br>Crea sockets de datagramas UDP y TCP de bajo nivel para comunicarte con otros dispositivos a través de tu propio protocolo.</td>
<td align="left">La clase <strong>Socket</strong> proporciona sockets TCP y la clase <strong>DatagramSocket</strong> proporciona un socket UDP.</td>
<td align="left"><strong>NSStream</strong> y <strong>CFStream</strong> proporcionan Sockets TCP, <strong>CFSocket</strong> proporciona sockets UDP.</td>
<td align="left">Puedes usar la clase <strong><a href="/uwp/api/Windows.Networking.Sockets.DatagramSocket">DatagramSocket</a></strong> para comunicarte con un socket de datagramas UDP y la clase <strong><a href="/uwp/api/Windows.Networking.Sockets.StreamSocket">StreamSocket</a></strong> para comunicarte a través de TCP o RFCOMM de Bluetooth.<br/><br/><a href="/windows/uwp/networking/networking-basics">Conceptos básicos de redes</a><br/><br/><a href="/windows/uwp/networking/which-networking-technology">¿Qué tecnología de red?</a><br/><br/><a href="/windows/uwp/networking/sockets">Información general sobre los sockets</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>WebSockets.</strong> <br><br>Proporciona una comunicación bidireccional entre un cliente y servidor, lo que permite una transferencia de datos en tiempo real.</td>
<td align="left">En Android no existen bibliotecas WebSockets integradas.</td>
<td align="left">En iOS no existen bibliotecas WebSockets integradas.</td>
<td align="left">Las conexiones seguras a los servidores que admiten WebSockets se pueden realizar con la clase <strong><a href="/uwp/api/windows.networking.sockets.messagewebsocket">MessageWebSocket</a></strong> para mensajes más pequeños con notificaciones de recepción y <strong><a href="/uwp/api/windows.networking.sockets.streamwebsocket">StreamWebSocket</a></strong> para transferencias de archivos binarios de mayor tamaño que se pueden leer en las secciones.<br/><br/><a href="/windows/uwp/networking/networking-basics">Conceptos básicos de redes</a><br/><br/><a href="/windows/uwp/networking/which-networking-technology">¿Qué tecnología de red?</a><br/><br/><a href="/windows/uwp/networking/websockets">Introducción a WebSockets</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Bibliotecas de OAuth.</strong> <br><br>Bibliotecas OAuth que permiten acceder a proveedores OAuth de terceros y la administración de cuentas integrada en la plataforma.</td>
<td align="left">No se proporciona ninguna biblioteca OAuth genérica. La clase <strong>GoogleAuthUtil</strong> se proporciona para la autenticación OAuth con Google Play Services.<br/></td>
<td align="left">No se proporciona ninguna biblioteca OAuth genérica. El <strong>marco de cuentas</strong> proporciona acceso a las cuentas de usuario que ya están almacenadas en el dispositivo, como Facebook y Twitter.</td>
<td align="left">La biblioteca OAuth genérica <strong><a href="/windows/uwp/security/web-authentication-broker">Agente de autenticación web</a></strong> permite conectarte a servicios de proveedor de identidades de terceros. La <strong><a href="/windows/uwp/security/credential-locker">caja de seguridad de credenciales</a></strong> permite a los usuarios guardar su información de inicio de sesión y usarla en varios dispositivos. El espacio de nombres <strong><a href="/previous-versions/office/developer/onedrive-live-sdk-reference/dn896755(v=office.15)">Microsoft. Live</a></strong> le permite acceder fácilmente a OAuth de Live SDK para acceder a los servicios de Microsoft.<br/><br/><a href="/windows/uwp/security/authentication-and-user-identity">Autenticación e identidad de usuario</a><br/><br/><a href="/uwp/api/windows.security.authentication.web">Documentación de la API Windows.Security.Authentication.Web</a><br/><br/><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAuthenticationBroker">Ejemplo de código de WebAuthenticationBroker</a></td>
</tr>
</tbody>
</table>
<h2 id="tooling">Tooling</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concepto general</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP de Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>IDE.</strong> <br><br>Conjunto de herramientas que se usa para crear la aplicación.</td>
<td align="left"><strong>Android Studio</strong> y <strong>Eclipse</strong>, con Google empujando a los desarrolladores hacia el uso de Android Studio.</td>
<td align="left"><strong>Xcode</strong></td>
<td align="left"><strong><a href="https://visualstudio.microsoft.com/features/universal-windows-platform-vs">Visual Studio</a></strong> y <strong><a href="/visualstudio/designers/creating-a-ui-by-using-blend-for-visual-studio?view=vs-2015">Blend para Visual Studio</a></strong> tiene todas las herramientas necesarias para codificar, diseñar, conectar, depurar, analizar, optimizar y probar aplicaciones para UWP. Visual Studio también te proporciona <strong><a href="/windows/uwp/debug-test-perf/test-with-the-emulator">emuladores</a></strong> para dispositivos Windows 10 para que puedas probar la aplicación en una amplia variedad de dispositivos emulados.<br/><br/><a href="https://developer.microsoft.com/windows/downloads">Descargas y herramientas para UWP</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Organización del código.</strong> <br><br>Estructura de carpetas básica de una aplicación, que a menudo se crea a partir de una plantilla inicial.</td>
<td align="left">Archivo <strong>AndroidManifest</strong>, carpeta <strong>java</strong> que contiene los archivos de origen, carpeta <strong>res</strong> con recursos (por ejemplo, diseños y valores), scripts de compilación <strong>Gradle</strong> en Android Studio y scripts de compilación <strong>Ant</strong> en Eclipse.</td>
<td align="left">Archivos de origen y <strong>archivos de compatibilidad</strong>, archivo <strong>Info.plist</strong>, <strong>Main.storyboard</strong> y <strong>LaunchScreen.storyboard</strong>. Las imágenes se almacenan en <strong>bibliotecas de recursos</strong>.</td>
<td align="left">La aplicación para UWP contiene código XAML y archivos de código denominados Example.xaml y Example.xaml.cs, distintas imágenes en la <strong>carpeta Recursos</strong>, una página de inicio (por ejemplo, <strong>MainPage.xaml</strong> y <strong>MainPage.xaml.cs</strong>) y un manifiesto.<br/><br/><a href="/windows/uwp/get-started/create-a-hello-world-app-xaml-universal">Creación de una aplicación Hello World</a></td>
</tr>
</tbody>
</table>
<h2 id="app-lifecycle">Ciclo de vida de la aplicación</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concepto general</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP de Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Ciclo de vida de la aplicación.</strong> <br><br>Controla eventos cuando la aplicación se inicia, suspende, reanuda y cierra, y, de este modo, proporciona una oportunidad para guardar y restaurar el estado de la aplicación y ejecutar otras tareas.</td>
<td align="left">Cada actividad tiene su propio <strong>ciclo de vida de la actividad</strong> con estados como, por ejemplo, <strong>reanudada</strong>. Las <strong>devoluciones de llamada del ciclo de vida</strong> , como <strong>alreanudar</strong> , se implementan en las <strong>clases de actividad</strong>.</td>
<td align="left">El <strong>ciclo de vida de aplicación</strong> tiene estados como, por ejemplo, <strong>suspendida</strong>. Los métodos, como <strong>applicationDidEnterBackground:</strong>, se implementan en el <strong>objeto de aplicación de delegado</strong> para ejecutar código cuando se producen cambios de estado.</td>
<td align="left">La aplicación tiene los <strong>estados de ejecución de la aplicación</strong> NotRunning, Activated, Running, Suspending, Suspended y Resuming.<br/><br/>Puedes implementar los métodos de la <strong><a href="/uwp/api/windows.ui.xaml.application">clase Application</a></strong> OnLaunched, OnActivated, Suspending o Resuming en tu aplicación para ejecutar código cuando cambie el estado.<br/><br/><a href="/windows/uwp/launch-resume/app-lifecycle">Ciclo de vida de la aplicación</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Tareas en segundo plano.</strong> <br><br>Tareas que realizan operaciones en segundo plano y siguen ejecutándose cuando la aplicación ya no está en primer plano.</td>
<td align="left">Las aplicaciones pueden iniciar <strong>servicios</strong> que realizan operaciones en segundo plano cuando la aplicación ya no está en primer plano. Los servicios tienen su propio <strong>ciclo de vida</strong> y se registran en el manifiesto.</td>
<td align="left">La <strong>ejecución en segundo plano</strong> solo se permite para tipos de tarea específicos.<br/><br/>Las aplicaciones declaran <strong>tareas en segundo plano admitidas</strong> en el archivo Info.plist mediante el objeto <strong>UIBackgroundModes</strong>.<br/><br/>El sistema controla cuándo se ejecutan tareas en segundo plano y durante cuánto tiempo.</td>
<td align="left">Puedes crear una tarea en segundo plano mediante la implementación de la interfaz <strong><a href="/uwp/api/windows.applicationmodel.background.ibackgroundtask">IBackgroundTask</a></strong> y el registro de la tarea en el manifiesto de la aplicación. Puede configurar una tarea para que se desencadene con un <a href="/windows/uwp/launch-resume/run-a-background-task-on-a-timer-"><strong>temporizador</strong></a>, un <a href="/uwp/api/windows.applicationmodel.background.systemtriggertype"><strong>desencadenador del sistema</strong></a>y un <a href="/windows/uwp/launch-resume/use-a-maintenance-trigger"><strong>desencadenador de mantenimiento</strong></a>.<br/><br/><a href="/windows/uwp/launch-resume/support-your-app-with-background-tasks">Hacer que tu aplicación sea compatible con las tareas en segundo plano</a><br/><br/><a href="/windows/uwp/launch-resume/create-and-register-a-background-task">Crear y registrar una tarea en segundo plano</a><br/><br/><a href="/windows/uwp/launch-resume/guidelines-for-background-tasks">Directrices para tareas en segundo plano</a></td>
</tr>
</tbody>
</table>
<h2 id="performance">Rendimiento</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concepto general</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP de Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Prácticas recomendadas de rendimiento.</strong> <br><br>Directrices para crear aplicaciones que sean rápidas y dinámicas, y que tengan en cuenta la duración de la batería con un tiempo de inicio rápido.</td>
<td align="left">Android ofrece la guía de aprendizaje <strong>Best Practices for Performance</strong>.</td>
<td align="left">iOS proporciona el documento <strong>Performance Overview</strong>.</td>
<td align="left">Puede leer la guía de <strong><a href="/windows/uwp/debug-test-perf/performance-and-xaml-ui">rendimiento</a></strong> detallada con secciones que tratan temas como; establecer los objetivos de rendimiento, medir el rendimiento, la administración de memoria, las animaciones suaves, el acceso eficaz al sistema de archivos y las herramientas disponibles para la generación de perfiles y el rendimiento.</td>
</tr>
<tr class="even">
<td align="left"><strong>Permite ver la optimización de una interfaz de usuario con capacidad de respuesta.</strong> <br><br>Mejorar el rendimiento mediante la optimización de vistas.</td>
<td align="left">Optimizar las <strong>jerarquías de diseño</strong> mediante la herramienta Visor de jerarquías, <strong>reutilizar diseños</strong> y cargar <strong>vistas a petición</strong> son técnicas que ayudan a que la interfaz de usuario responda a los subprocesos y evite cuadros de diálogo del tipo &quot;La aplicación no responde&quot; (<strong>ANR</strong>).<br/></td>
<td align="left">Corregir los problemas en la interfaz de usuario relacionados con la <strong>representación fuera de la pantalla</strong>, las <strong>capas mezcladas</strong> y la <strong>rasterización</strong> con la herramienta <strong>Core Animation</strong> para ayudar a que la interfaz de usuario permanezca dinámica.</td>
<td align="left">Puedes <strong>optimizar</strong> fácilmente el <strong>marcado</strong> y los <strong>diseños</strong> XAML siguiendo unos sencillos pasos. Entre las técnicas se incluyen la reducción de la estructura de diseño, la minimización del número de elementos y la minimización del exceso de dibujo. <br/><br/><a href="/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive">Mantener la capacidad de respuesta del subproceso de la interfaz de usuario</a><br/><br/><a href="/windows/uwp/debug-test-perf/optimize-xaml-loading">Optimizar el marcado XAML</a><br/><br/><a href="/windows/uwp/debug-test-perf/optimize-your-xaml-layout">Optimiza tu diseño XAML</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Threading.</strong> <br><br>Usa los subprocesos para mantener una <strong>interfaz de usuario dinámica</strong> y ejecutar varias <strong>tareas en paralelo</strong>.</td>
<td align="left">Los subprocesos se logran mediante las clases <strong>Runnable</strong>, <strong>Handler</strong>, <strong>ThreadPoolExecutor</strong>, y el nivel superior <strong>AsyncTask</strong>.</td>
<td align="left">Los subprocesos se logran mediante los objetos <strong>NSThread</strong>, <strong>Grand Central Dispatch</strong> y <strong>NSOperation</strong> de mayor nivel.</td>
<td align="left">Puede trabajar con subprocesos enviando <strong>elementos de trabajo</strong> a <strong>ThreadPool</strong> con <strong><a href="/uwp/api/windows.system.threading.threadpool.runasync">RunAsync</a></strong>. Puede usar un temporizador para enviar un elemento de trabajo con <strong><a href="/uwp/api/windows.system.threading.threadpooltimer.createtimer">CreateTimer</a></strong> y crear un elemento de trabajo repetitivo con <strong><a href="/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer">CreatePeriodicTimer</a></strong>.<br/><br/><a href="/windows/uwp/threading-async/submit-a-work-item-to-the-thread-pool">Enviar un elemento de trabajo al grupo de subprocesos</a><br/><br/><a href="/windows/uwp/threading-async/use-a-timer-to-submit-a-work-item">Enviar un elemento de trabajo con un temporizador</a><br/><br/><a href="/windows/uwp/threading-async/create-a-periodic-work-item">Crear un elemento de trabajo periódico</a><br/><br/><a href="/windows/uwp/threading-async/best-practices-for-using-the-thread-pool">Procedimientos recomendados para usar el grupo de subprocesos</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Programación asincrónica.</strong> <br><br>Evita la complejidad de los subprocesos al sacar partido de modelos de programación asincrónica para mantener la capacidad de respuesta del subproceso de interfaz de usuario.</td>
<td align="left">El uso de <strong>subprocesos es necesario</strong> para crear clases asincrónicas propias. Algunas clases integradas son asincrónicas.</td>
<td align="left">El uso de <strong>subprocesos es necesario</strong> para crear clases asincrónicas propias. Algunas clases integradas son asincrónicas.</td>
<td align="left">Puede usar patrones asincrónicos para evitar el bloqueo del subproceso principal cuando cree sus propias API, por ejemplo, mediante <strong>Async</strong> y <strong>Await</strong> en C# y Visual Basic. Puedes usar las API asincrónicas integradas que terminan con la palabra <strong>Async</strong>.<br/><br/><a href="/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps">Programación asincrónica</a><br/><br/><a href="/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic">Llamar a API asincrónicas en C# o Visual Basic</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Optimización de la vista de lista.</strong> <br><br>Patrones integrados para ayudar en la optimización de las listas de datos, que con frecuencia tienen un rendimiento deficiente cuando se necesitan mostrar grandes cantidades de datos.</td>
<td align="left">El patrón de diseño <strong>ViewHolder</strong> se usa para evitar varias de búsquedas de vistas, lo que permite usar elementos de interfaz de usuario reutilizables.</td>
<td align="left">Se puede crear una variedad de optimizaciones para mejorar el rendimiento del objeto <strong>UITableView</strong>, no hay elementos integrados.</td>
<td align="left">Puedes usar los controles <a href="/uwp/api/windows.ui.xaml.controls.listview">ListView</a> y <a href="/uwp/api/windows.ui.xaml.controls.gridview">GridView</a>, que proporcionan <strong>virtualización de la interfaz de usuario</strong> en la configuración inicial. Esto ofrece una experiencia de movimiento panorámico y desplazamiento suaves, así como un tiempo de inicio más rápido. También puedes implementar los objetos <a href="/dotnet/api/system.collections.ilist">IList</a> e <a href="/dotnet/api/system.collections.specialized.inotifycollectionchanged">INotifyCollectionChanged</a> en el origen de datos, lo que proporciona la <strong>virtualización de datos</strong> y mejora aún más el rendimiento.<br/><br/><a href="/windows/uwp/debug-test-perf/optimize-gridview-and-listview">Optimización de la interfaz de usuario de ListView y GridView</a><br/><br/><a href="/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization">Virtualización de datos de ListView y GridView</a></td>
</tr>
</tbody>
</table>
<h2 id="monetization">Monetización</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concepto general</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP de Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Compras desde la aplicación.</strong> <br><br>Características de la plataforma que permiten al usuario realizar compras desde las aplicaciones.</td>
<td align="left">La <strong>facturación desde la aplicación</strong> la proporciona Google Services. Los productos se agregan a la <strong>Consola para Desarrolladores de Google Play</strong>. Las compras desde la aplicación se implementan con la <strong>Google Play Billing Library</strong>.</td>
<td align="left">Los productos se agregan a <strong>iTunes Connect</strong>. Las compras desde la aplicación se implementan mediante el marco <strong>StoreKit</strong>.<br/><br/>Los productos se compran mediante <strong>SKMutablePayment</strong> y <strong>SKPaymentQueue</strong>.</td>
<td align="left">Las compras de productos desde la aplicación de la aplicación se crean <a href="/windows/uwp/publish/iap-submissions">agregándolas a tu aplicación y enviándolas a la Tienda</a>. <br/><br/>Usas la <strong><a href="/uwp/api/windows.applicationmodel.store.currentapp">clase CurrentApp</a></strong> para definir las compras desde la aplicación. <br/><br/>Usas el objeto <strong><a href="/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync">CurrentApp.RequestProductPurchaseAsync</a></strong> para mostrar la interfaz de usuario que permite a los clientes a comprar el producto.<br/><br/><a href="/windows/uwp/monetize/enable-in-app-product-purchases">Habilitar compras de productos desde la aplicación</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Compras desde la aplicación que se usan.</strong> <br><br>Productos desde la aplicación que se pueden comprar, usar y luego volver a comprar.</td>
<td align="left">Las compras consumibles se habilitan al realizar una compra normal y luego consumir con el objeto <strong>consumePurchase</strong>, lo que permite comprar, usar y luego volver a comprar.</td>
<td align="left">Los productos consumibles se <strong>definen como productos consumibles</strong> en iTunes Connect.</td>
<td align="left">Puedes admitir consumibles al <a href="/windows/uwp/publish/enter-iap-properties">definir su tipo de producto como Consumible cuando los envíes</a> a la Tienda. Después, llame a <strong><a href="/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync">CurrentApp. ReportConsumableFulfillmentAsync</a></strong> una vez realizada una compra consumible para permitir que el cliente tenga acceso a ella.<br/><br/><a href="/windows/uwp/monetize/enable-consumable-in-app-product-purchases">Habilitar compras de productos consumibles desde la aplicación</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Prueba de compras desde la aplicación.</strong> <br><br>Se permite probar el código de compra desde la aplicación sin poner la aplicación en la Tienda.</td>
<td align="left">Para realizar pruebas, se usa el <strong>espacio aislado de facturación desde la aplicación</strong>.</td>
<td align="left">Para realizar pruebas, se usan <strong>cuentas de evaluador del espacio aislado</strong>.</td>
<td align="left">Puede probar las compras desde la aplicación simplemente mediante la clase <strong><a href="/uwp/api/windows.applicationmodel.store.currentappsimulator">CurrentAppSimulator</a></strong> en lugar de CurrentApp.<br/><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Clínicas.</strong> <br><br>Se permite limitar fácilmente el contenido o quitar la publicidad según una versión de prueba de una aplicación.</td>
<td align="left">Google Play <strong> no admite oficialmente las pruebas de aplicación</strong>. Las pruebas o la eliminación de publicidad se logran creando una compra desde la aplicación y tomando la ruta de acceso al código correspondiente al confirmar que la compra se ha realizado correctamente.</td>
<td align="left">La App Store<strong> no admite oficialmente las pruebas de aplicación</strong>. Las pruebas o la eliminación de publicidad se logran creando una compra desde la aplicación y tomando la ruta de acceso al código correspondiente al confirmar que la compra se ha realizado correctamente.</td>
<td align="left">Puedes ofrecer una versión de prueba gratuita de tu aplicación al usar la <strong><a href="/windows/uwp/publish/set-app-pricing-and-availability">opción 'Prueba gratuita'</a></strong> cuando envías la aplicación a la Tienda. Luego puedes usar el objeto <strong><a href="/uwp/api/windows.applicationmodel.store.licenseinformation.istrial">LicenseInformation.IsTrial</a></strong> para comprobar el estado de prueba de la aplicación y presentar distintas rutas de código, según corresponda. Puedes registrar el <a href="/uwp/api/windows.applicationmodel.store.licenseinformation.licensechanged">evento LicenseChanged</a> para que reciba notificación cuando el usuario cambia el estado de prueba mientras se ejecuta la aplicación.<br/><br/><a href="/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app">Excluir o limitar las características de una versión de prueba</a></td>
</tr>
</tbody>
</table>
<h2 id="adapting-to-multiple-platforms">Adaptarse a varias plataformas</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concepto general</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP de Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Interfaz de usuario adaptable: diseños flexibles.</strong> <br><br>Se admiten distintos tamaños de pantalla con un alto y ancho flexibles.</td>
<td align="left">Los diseños flexibles se pueden obtener mediante los valores <strong>wrap_content</strong> y <strong>match_parent</strong> en objetos LinearLayout o usando objetos RelativeLayout para la alineación.</td>
<td align="left">Los diseños flexibles se pueden obtener mediante el <strong>modelo adaptativo</strong> con guiones gráficos universales, lo que usa el <strong>diseño automático</strong> con <strong>restricciones</strong> y <strong>rasgos</strong>, como objetos horizontalSizeClass y displayScale que se aplican a los controladores de vista.</td>
<td align="left">Puedes crear un diseño fluido mediante <strong>propiedades de diseño</strong> y <strong>paneles</strong> con una combinación de tamaños fijos y dinámicos.<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">Definir diseños con XAML: paneles y propiedades de diseño</a><br/><br/><a href="/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">Diseño con capacidad de respuesta 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Interfaz de usuario adaptable: diseños personalizados.</strong> <br><br>Se admiten diferentes tamaños de pantalla con diseños de destino independientes.</td>
<td align="left">Se proporcionan archivos de diseño alternativos para distintas configuraciones de pantalla en el directorio de recursos mediante <strong>calificadores de configuración</strong>, como <strong>pequeño</strong>, <strong>grande</strong>, <strong>ldpi</strong> y <strong>hdpi</strong>, lo que permite dirigirse a diseños personalizados para pantallas de distintos tamaños y densidades.</td>
<td align="left">Define un <strong>guion gráfico de iPhone e iPad independiente</strong> para personalizar los diseños según las distintas familias de dispositivos en una aplicación universal.</td>
<td align="left">Puedes crear un diseño personalizado al definir <strong>archivos de marcado XAML diferentes</strong> por familia de dispositivos.<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">Definir diseños con XAML: diseños personalizados</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Interfaz de usuario adaptable: diseños con capacidad de respuesta.</strong> <br><br>Responde a los cambios de tamaño de pantalla, como la rotación, o a un cambio en el tamaño de una ventana.</td>
<td align="left">El uso de diseños flexibles con objetos <strong>LinearLayout</strong> y <strong>RelativeLayout</strong> o proporcionar archivos de diseño alternativos para distintas orientaciones permiten diseños dinámicos.</td>
<td align="left">Cuando el <strong>tamaño</strong> o los <strong>rasgos</strong> de una vista cambian, se aplican las <strong>restricciones</strong> especificadas en los guiones gráficos.</td>
<td align="left">Puedes redistribuir, cambiar la posición, cambiar el tamaño, mostrar o reemplazar fácilmente secciones de la interfaz de usuario en tiempo de ejecución en respuesta a los cambios del tamaño de ventana mediante los objetos <strong><a href="/uwp/api/windows.ui.xaml.visualstate">VisualState</a></strong>, <strong><a href="/uwp/api/windows.ui.xaml.visualstatemanager">VisualStateManager</a></strong> y <strong><a href="/uwp/api/windows.ui.xaml.adaptivetrigger">AdaptiveTrigger</a></strong>.<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">Definir diseños con XAML: estados visuales y desencadenadores de estado</a><br/><br/><a href="/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">Diseño con capacidad de respuesta 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Compatibilidad con diferentes funcionalidades del dispositivo.</strong> <br><br>Saca provecho de las características de hardware avanzadas y, al mismo tiempo, admite dispositivos que no disponen de ellas.</td>
<td align="left">Las pruebas de las características del dispositivo en tiempo de ejecución mediante el objeto <strong>PackageManager.hasSystemFeature</strong> te permite decidir si se puede ejecutar código específico del hardware.</td>
<td align="left">No existe <strong>una comprobación única</strong> que puedes realizar en tiempo de ejecución para probar las características del dispositivo. Debes probar cada característica de forma específica para decidir si se puede ejecutar código específico del hardware.</td>
<td align="left">Puedes agregar <strong>SDK de extensión de plataforma</strong> al paquete para dirigirte a la funcionalidad adicional que se ofrece en las distintas familias de dispositivo, por ejemplo, teléfonos, escritorio e IoT. Usa la <strong><a href="/uwp/api/windows.foundation.metadata.apiinformation">API ApiInformation</a></strong> para comprobar la presencia de tipos y miembros en tiempo de ejecución y puedes llamar a esos tipos y miembros solo si están presentes.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Compatibilidad con diferentes funcionalidades del dispositivo.</strong> <br><br>Saca provecho de las características de hardware avanzadas y, al mismo tiempo, admite dispositivos que no disponen de ellas.</td>
<td align="left">La <strong>biblioteca de compatibilidad de Android</strong> se puede empaquetar con tu aplicación para que algunas API más recientes estén disponibles para los usuarios con versiones anteriores de Android. Las pruebas de nivel de API en tiempo de ejecución se pueden realizar mediante <strong>Build.Version.SDK_INT</strong>.</td>
<td align="left">Las comprobaciones estándares en tiempo de ejecución se usan para averiguar si las API están disponibles, como el método <strong>class</strong> para comprobar si una clase existe y <strong>respondsToSelector:</strong> para comprobar los métodos en las clases.</td>
<td align="left">Puede usar <strong><a href="/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent">ApiInformation. IsApiContractPresent</a></strong> para identificar si hay un contrato de API con un número principal y secundario especificado. También puedes usar la <strong><a href="/uwp/api/windows.foundation.metadata.apiinformation">API ApiInformation</a></strong> para comprobar la presencia de tipos y miembros en tiempo de ejecución y puedes llamar a esos tipos y miembros solo si están presentes.</td>
</tr>
</tbody>
</table>
<h2 id="notifications">Notificaciones</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concepto general</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP de Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Iconos y distintivos.</strong> <br><br>Presenta actualizaciones a los usuarios en la pantalla principal.</td>
<td align="left">Los <strong>widgets de aplicación</strong> son vistas en la aplicación que se pueden integrar en la pantalla principal y pueden recibir actualizaciones periódicas. <strong>No existe ningún sistema de distintivo</strong> en Android. No existe ningún sistema idéntico a los iconos.</td>
  <td align="left"><strong>Widgets</strong> en iOS mappear en el centro de notificaciones y se implementan como <strong>extensiones de aplicaciones</strong>. También puede Agregar un <strong>distintivo</strong> a su icono con un número que puede cambiar en respuesta a las notificaciones locales o remotas. No hay ningún sistema de mosaicos.</td>
<td align="left">La aplicación tiene un <strong>icono</strong>, que se pueden anclar a la pantalla Inicio y se usa para mostrar la selección de texto, imágenes y un <strong>distintivo</strong> con glifos y números. Puedes actualizar el contenido de los iconos de la aplicación a través de notificaciones de inserción o programaciones predefinidas. Los iconos pueden ser adaptativos y pueden cambiar en función de dónde se muestran.<br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">Crear iconos</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-create-adaptive-tiles">Crear iconos adaptables</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-choosing-a-notification-delivery-method">Elegir un método de entrega de notificaciones</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">Directrices sobre iconos y distintivos</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Mostrar notificaciones.</strong> <br><br>Tipos de notificaciones que se pueden mostrar.</td>
<td align="left">Las notificaciones se pueden mostrar en el <strong>área de notificación</strong> y el <strong>cajón de notificaciones</strong>. Las <strong>notificaciones de visualización frontal</strong> presentan una notificación en una pequeña ventana flotante. A las notificaciones se les pueden agregar acciones mediante la definición de un objeto <strong>PendingIntent</strong>.</td>
<td align="left">Las notificaciones emergentes aparecen como <strong>pancartas</strong> o <strong>alertas</strong>. Puedes agregar botones de acción personalizados a las <strong>notificaciones accionables</strong>, que se definen con el objeto <strong>UIMutableUserNotificationAction</strong>.</td>
<td align="left">Puedes crear notificaciones emergentes adaptativas llamadas <strong>notificaciones del sistema</strong>. Puedes definir notificaciones del sistema en el código XML con contenido visual, <strong>acciones</strong> que pueden ser botones, o entradas y audio.<br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts">Notificaciones del sistema interactivas y adaptables</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-choosing-a-notification-delivery-method">Elegir un método de entrega de notificaciones</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-badges-notifications">Directrices sobre notificaciones del sistema</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Programación de notificaciones locales.</strong> <br><br>Notificaciones locales que tu aplicación envía en una hora programada.</td>
<td align="left">Las notificaciones y acciones se definen mediante un objeto <strong>NotificationCompat.Builder</strong> y se pueden programar y controlar desde la aplicación mediante los objetos <strong>AlarmManager</strong> y <strong>BroadcastReceiver</strong>.</td>
<td align="left">Las notificaciones locales se crean mediante <strong>UILocalNotification</strong>y se pueden programar con <b> UILocalNotification. scheduleLocalNotification:<strong>. | Puede programar una notificación del sistema mediante </strong> <a href="/uwp/api/Windows.UI.Notifications.ScheduledToastNotification">ScheduledToastNotification</a><strong>. Puede enviar una notificación de icono desde la aplicación mediante la </strong> <a href="/uwp/api/Windows.UI.Notifications.TileNotification">clase TileNotification</a> <strong> o programar una notificación de icono con <a href="/uwp/api/Windows.UI.Notifications.ScheduledTileNotification">ScheduledTileNotification</a>.<br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts">Notificaciones del sistema interactivas y adaptables</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-sending-a-local-tile-notification">Enviar una notificación de icono local</a> | | </strong>Enviar notificaciones de envío.</b> Notificación que se envía desde un servidor de notificación de inserción y, opcionalmente, controlada desde la aplicación.</td>
<td align="left"><strong>Google Cloud Messaging</strong> proporciona compatibilidad con las notificaciones de envío para Android.</td>
</tr>
</tbody>
</table>
<h2 id="media-capture-and-rendering">Capturar y representar contenido multimedia</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concepto general</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP de Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Medios de captura.</strong> <br><br>Grabar contenido audiovisual.</td>
<td align="left">El uso de una <strong>intención</strong>, como MediaStore.ACTION_VIDEO_CAPTURE, permite la captura de contenido multimedia con una aplicación de cámara existente. El uso de la biblioteca <strong>android.hardware.camera2</strong> o <strong>camera</strong> permite la implementación de una interfaz de cámara personalizada. Para capturar audio se pueden usar API <strong>MediaRecorder</strong>.</td>
<td align="left">El objeto <strong>UIImagePickerController</strong> permite la captura de vídeo y fotos con la interfaz de usuario del sistema. La clase <strong>AVFoundation</strong>, como <strong>AVCaptureSession</strong>, permite un acceso directo a la cámara. <br/>La clase <strong>AVAudioRecorder</strong> permite la grabación de audio.</td>
<td align="left">Puede capturar fotografías y vídeo mientras usa la interfaz de usuario de la cámara integrada con la <strong><a href="/uwp/api/Windows.Media.Capture.CameraCaptureUI">clase CameraCaptureUI</a></strong>. Puede interactuar con la cámara en un nivel bajo y capturar audio con las clases de <strong><a href="/uwp/api/Windows.Media.Capture">Windows.Media.Capture</a></strong>, como la <strong><a href="/uwp/api/Windows.Media.Capture.MediaCapture">API MediaCapture</a></strong>. <br/><br/><a href="/windows/uwp/audio-video-camera/capture-photos-and-video-with-cameracaptureui">Capturar fotografías y vídeos con CameraCaptureUI</a><br/><br/><a href="/windows/uwp/audio-video-camera/capture-photos-and-video-with-mediacapture">Capturar fotografías y vídeos con MediaCapture</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Reproducción de medios.</strong> <br><br>Reproducir archivos de audio y vídeo.</td>
<td align="left">Para la reproducción de archivos de audio y vídeo se usan las clases <strong>MediaPlayer</strong> y <strong>AudioManager</strong>.</td>
<td align="left">Para la reproducción de archivos de audio y vídeo se usan los objetos <strong>AVKit framework</strong>, <strong>AVAudioPlayer</strong> y <strong>Media Player Framework</strong>.</td>
<td align="left">Puedes usar las clases <strong><a href="/uwp/api/Windows.Media.Core.MediaSource">MediaSource</a></strong>, <strong><a href="/uwp/api/windows.ui.xaml.controls.mediaelement">MediaElement</a></strong> y <strong><a href="/uwp/api/windows.media.playback.mediaplayer">MediaPlayer</a></strong> para reproducir audio y vídeo desde orígenes, tales como archivos locales y remotos.<br/><br/><a href="/windows/uwp/audio-video-camera/media-playback-with-mediasource">Reproducción de contenido multimedia con MediaSource</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Editando medios.</strong> <br><br>Crea nuevos archivos multimedia a partir de grabaciones existentes y aplica efectos especiales.</td>
<td align="left">Para la edición de contenido se pueden usar clases de bajo nivel, como <strong>MediaCodec</strong>, <strong>MediaMuxer</strong> y <strong>android.media.effect</strong>.</td>
<td align="left">Para la edición de contenido se pueden usar las clases del marco <strong>AV Foundation</strong>, <strong>AVMutableComposition</strong>, <strong>AVMutableVideoComposition</strong> y <strong>AVMutableAudioMix</strong>.</td>
<td align="left">Puede usar las API de <strong><a href="/uwp/api/windows.media.editing">Windows. Media. Editing</a></strong> como <strong><a href="/uwp/api/windows.media.editing.mediacomposition">MediaComposition</a></strong> y <strong><a href="/uwp/api/windows.media.editing.mediaclip">MediaClip</a></strong> para crear composiciones multimedia a partir de archivos de audio y vídeo. Puedes agregar superposiciones de imagen y vídeo, combinar clips de vídeo, agregar audio de fondo y aplicar efectos de audio y vídeo.<br/><br/><a href="/windows/uwp/audio-video-camera/media-compositions-and-editing">Composiciones y edición multimedia</a></td>
</tr>
</tbody>
</table>
<h2 id="sensors">Sensores</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concepto general</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP de Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Sensores.</strong> <br><br>Detecta el movimiento, la posición y las propiedades del entorno del dispositivo.</td>
<td align="left">El <strong>marco de sensor</strong> se usa para acceder a los sensores de hardware y software con clases como, por ejemplo, <strong>SensorManager</strong> y <strong>SensorEvent</strong>.</td>
<td align="left">El <strong>marco Core Motion</strong> se usa para acceder a datos de sensor procesados y sin procesar.</td>
<td align="left">Puedes usar las clases en <strong><a href="/uwp/api/windows.devices.sensors">Windows.Devices.Sensors</a></strong> para acceder a las lecturas del sensor y los eventos que se desencadena cuando se reciben nuevos datos de lectura del sensor.<br/><br/><a href="/windows/uwp/devices-sensors/sensors">Sensores</a></td>
</tr>
</tbody>
</table>
<h2 id="location-and-mapping">Ubicación y mapas</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concepto general</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP de Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Cód.</strong> <br><br>Busca la ubicación <strong>actual</strong> del dispositivo y realiza un seguimiento de los <strong>cambios</strong>.</td>
<td align="left">Las API de servicios de ubicación de Google ofrecen acceso de alto nivel a la <strong>última ubicación conocida</strong> con el <strong>proveedor de ubicación combinados</strong> mediante los métodos <strong>getLastLocation</strong> y <strong>requestLocationUpdates</strong>. El acceso de bajo nivel se proporciona en las bibliotecas de Android con el objeto <strong>LocationManager</strong>.</td>
<td align="left">La clase <strong>Core Location</strong> <strong>CLLocationManager</strong> se usa para supervisar la ubicación de un dispositivo, con el objeto <strong>startUpdatingLocation</strong> para el servicio de ubicación estándar y el objeto <strong>startMonitoringSignificantLocationChanges</strong> para el servicio de ubicación de <strong>cambio importante</strong>.</td>
<td align="left">Puede hacer un seguimiento de la ubicación del dispositivo con las clases en <strong><a href="/uwp/api/windows.devices.geolocation">Windows.Devices.Geolocation</a></strong>. Use el <strong><a href="/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync">weblocator. GetGeopositionAsync</a></strong> para leer una sola vez. Usa el objeto <strong><a href="/uwp/api/windows.devices.geolocation.geolocator.positionchanged">Geolocator.PositionChanged</a></strong> para obtener la ubicación periódicamente mediante un temporizador o para recibir una notificación cuando cambie la ubicación.<br/><br/><a href="/windows/uwp/maps-and-location/get-location">Obtener la ubicación del usuario</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Mostrar asignaciones.</strong> <br><br>Muestra un <strong>mapa integrado interactivo</strong> y agrega <strong>puntos de interés</strong>.</td>
<td align="left">Las clases <strong>GoogleMap</strong>, <strong>MapFragment</strong> y <strong>MapView</strong> de la <strong>API Google Maps para Android</strong> permiten integrar mapas en las aplicaciones. Los puntos de interés se pueden mostrar con <strong>marcadores</strong> y la clase <strong>Marker</strong> personalizable.</td>
<td align="left">Los mapas se integran en las aplicaciones de iOS mediante la clase <strong>MKMapView</strong> en el objeto <strong>MapKit framework</strong>. Las <strong>anotaciones</strong> se pueden agregar a las aplicaciones para mostrar puntos de interés mediante clases de objeto como <strong>MKPointAnnotation</strong> y clases de vista como <strong>MKPinAnnotationView</strong>.</td>
<td align="left">Puede incrustar las asignaciones en sus aplicaciones mediante el control integrado de la <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mapcontrol">clase</a></strong> de integración de código XAML, que proporciona vistas 2D, 3D y streetside. Puede Agregar puntos de interés con un PIN, una imagen o una forma mediante el uso de clases como <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mapicon">Mapicon</a></strong>, <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mappolygon">MapPolygon</a></strong> y <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mappolyline">MapPolyline</a></strong>.<br/><br/><a href="/windows/uwp/maps-and-location/display-maps">Mostrar mapas con vistas 2D, 3D y Streetside</a><br/><br/><a href="/windows/uwp/maps-and-location/display-poi">Mostrar puntos de interés (POI) en un mapa</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Perímetro.</strong> <br><br>Supervisa la entrada y salida de una región geográfica determinada.</td>
<td align="left">Las geovallas se supervisan mediante los <strong>servicios de ubicación</strong> en el SDK de Google Play Services.</td>
<td align="left">Las regiones se supervisan mediante la clase <strong>CLCircularRegion</strong> y se registran mediante el objeto <strong>CLLocationManager.startMonitoringForRegion:</strong>.</td>
<td align="left">Puede crear una geovalla con la clase <strong><a href="/uwp/api/windows.devices.geolocation.geofencing.geofence">geovalla</a></strong> y definir los <strong>Estados supervisados</strong> , como la entrada o salida de una región. Controle los eventos de geovalla en primer plano con la <strong><a href="/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor">clase GeofenceMonitor</a></strong>y en segundo plano con la <strong><a href="/uwp/api/windows.applicationmodel.background.locationtrigger">clase Background LocationTrigger</a></strong>.<br/><br/><a href="/windows/uwp/maps-and-location/set-up-a-geofence">Configuración de una geovalla</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Geocodificación y geocodificación inversa.</strong> <br><br>Convierte direcciones en ubicaciones geográficas (geocodificación) y convierte ubicaciones geográficas en direcciones (geocodificación inversa).<br/></td>
<td align="left">Para la geocodificación y geocodificación inversa se usa la clase <strong>Geocoder</strong>.</td>
<td align="left">Para la geocodificación se usa la clase <strong>CLGeocoder</strong>.</td>
<td align="left">Puede realizar la codificación geográfica con la <strong><a href="/uwp/api/windows.services.maps.maplocationfinder">clase MapLocationFinder</a></strong> en <strong><a href="/uwp/api/windows.services.maps">Windows. Services. Maps</a></strong>. Usa el objeto <strong><a href="/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync">FindLocationsAsync</a></strong> para la geocodificación y el objeto <strong><a href="/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync">FindLocationsAtAsync</a></strong> para la geocodificación inversa.<br/><br/><a href="/windows/uwp/maps-and-location/geocoding">Realizar geocodificación y geocodificación inversa</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Rutas y direcciones.</strong> <br><br>Proporciona rutas, distancias e indicaciones entre dos ubicaciones geográficas.</td>
<td align="left">Google ofrece la <strong>API de indicaciones de Google Maps</strong> del servicio web, que se puede usar en Android, aunque no se proporciona ningún SDK.</td>
<td align="left">El kit de mapas proporciona la API <strong>MKDirections</strong>, que se puede usar para obtener información sobre una ruta e indicaciones.</td>
<td align="left">Puedes solicitar una ruta a pie o en coche mediante la clase <strong><a href="/uwp/api/windows.services.maps.maproutefinder">MapRouteFinder</a></strong> en <strong><a href="/uwp/api/windows.services.maps">Windows.Services.Maps</a></strong>. Las rutas se devuelven como una instancia de <strong><a href="/uwp/api/windows.services.maps.maproute">MapRoute</a></strong>, que se puede mostrar fácilmente en un objeto MapControl. Las indicaciones se devuelven dentro del objeto <strong><a href="/uwp/api/windows.services.maps.maproutemaneuver">MapRouteManeuver</a></strong>.<br/><br/><a href="/windows/uwp/maps-and-location/routes-and-directions">Mostrar rutas e indicaciones en un mapa</a></td>
</tr>
</tbody>
</table>
<h2 id="app-to-app-communication">Comunicación entre aplicaciones</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concepto general</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP de Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Invocar otra aplicación.</strong> <br><br>Inicia otra aplicación y, opcionalmente, comparte datos, como vínculos, texto, fotos, vídeos y archivos.</td>
<td align="left">Se usa una <strong>intención implícita</strong> para iniciar otra aplicación, mediante la definición de una <strong>acción</strong> y datos opcionales en una <strong>intención</strong> y su llamada mediante el objeto <strong>startActivityForResult</strong>.<br/></td>
<td align="left">Se pueden usar <strong>extensiones de aplicación</strong> para proporcionar acceso a los datos de aplicación a otra aplicación. <strong>Esquemas de direcciones URL</strong> permite que una dirección URL se pase a otra aplicación.</td>
<td align="left">Puede iniciar otra aplicación que se ha registrado para un URI con <strong><a href="/uwp/api/windows.system.launcher.launchuriasync">Launcher. LaunchUriAsync</a></strong>o <strong><a href="/uwp/api/windows.system.launcher.launchuriforresultsasync">Launcher. LaunchUriForResultsAsync</a></strong> para iniciar los resultados y obtener datos de la aplicación iniciada. Puedes usar el objeto <strong><a href="/uwp/api/windows.system.launcher.launchfileasync">Launcher.LaunchFileAsync</a></strong> para pasar un archivo a otra aplicación y que esta lo procese.<br/><br/>Puedes usar un <strong>contrato para contenido compartido</strong> y compartir datos fácilmente entre aplicaciones.<br/><br/><a href="/windows/uwp/launch-resume/launch-default-app">Iniciar la aplicación predeterminada para un URI</a><br/><br/><a href="/windows/uwp/launch-resume/how-to-launch-an-app-for-results">Iniciar una aplicación para obtener resultados</a><br/><br/><a href="/windows/uwp/launch-resume/launch-the-default-app-for-a-file">Iniciar la aplicación predeterminada de un archivo</a><br/><br/><a href="/windows/uwp/app-to-app/share-data">Compartir datos</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Permite invocar la aplicación.</strong> <br><br>Permite que la aplicación responda a una solicitud de otra aplicación.</td>
<td align="left">Las aplicaciones registran una <strong>actividad de control de intención</strong> con un <strong>filtro de intención</strong> para responder a una intención implícita de otra aplicación.</td>
<td align="left">El empaquetado de una <strong>extensión de aplicación</strong> permite compartir datos con otras aplicaciones. Las aplicaciones pueden registrar un <strong>esquema de dirección URL personalizado</strong> mediante la clave <strong>CFBundleURLTypes</strong> en Info.plist.</td>
<td align="left">Puedes registrar la aplicación para que sea el controlador predeterminado de un <strong>nombre de esquemas de URI</strong> al registrar un <strong><a href="/uwp/api/windows.applicationmodel.activation.activationkind#Protocol">protocolo</a></strong> en el manifiesto de paquete y actualizando el controlador de eventos <strong><a href="/uwp/api/windows.ui.xaml.application.onactivated">Application.OnActivated</a></strong>, lo que devuelve resultados opcionalmente. Del mismo modo, puedes registrar tu aplicación para que sea el controlador predeterminado para determinados tipos de archivo mediante la adición de una declaración en el manifiesto del paquete y el control del evento <strong><a href="/uwp/api/windows.ui.xaml.application.onfileactivated">Application.OnFileActivated</a></strong>.<br/><br/>Puede controlar las solicitudes de contrato de recursos compartidos registrando la aplicación como un destino de recurso compartido en el manifiesto y controlando el evento <strong><a href="/uwp/api/windows.ui.xaml.application.onsharetargetactivated">Application. OnShareTargetActivated</a></strong> .<br/><br/><a href="/windows/uwp/launch-resume/how-to-launch-an-app-for-results">Iniciar una aplicación para obtener resultados</a><br/><br/><a href="/windows/uwp/launch-resume/handle-file-activation">Administrar la activación de archivos</a><br/><br/><a href="/windows/uwp/app-to-app/receive-data">Recibir datos</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Copiar y pegar.</strong> <br><br>Copia y pega texto y otro contenido entre aplicaciones.</td>
<td align="left">Se puede usar el <strong>marco de portapapeles</strong> para implementar las operaciones de copiar y pegar mediante las clases <strong>ClipboardManager</strong> y <strong>ClipData</strong>.</td>
<td align="left">Se pueden usar los objetos <strong>UIPasteboard</strong>, <strong>UIMenuController</strong> y <strong>UIResponderStandardEditActions</strong> para implementar operaciones de copiar y pegar.</td>
<td align="left">Muchos controles XAML predeterminados ya admiten las operaciones de copiar y pegar. Puedes implementar tú mismo operaciones de copiar y pegar mediante las clases <strong><a href="/uwp/api/windows.applicationmodel.datatransfer.datapackage">DataPackage</a></strong> y <strong><a href="/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard">Clipboard</a></strong> en <strong><a href="/uwp/api/Windows.ApplicationModel.DataTransfer">Windows.ApplicationModel.DataTransfer</a></strong>.<br/><br/><a href="/windows/uwp/app-to-app/copy-and-paste">Copiar y pegar</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Arrastrar y colocar.</strong> <br><br>Arrastra y coloca contenido entre aplicaciones.</td>
<td align="left">La operación de arrastrar y colocar se puede implementar en una sola aplicación mediante el <strong>marco de arrastrar y colocar de Android</strong>.</td>
<td align="left">En iOS no se proporciona ninguna API de alto nivel para arrastrar y colocar.</td>
<td align="left">Puedes implementar la operación de arrastrar y colocar en la aplicación para habilitar funcionalidades de arrastrar y colocar de una aplicación a otra, del escritorio a la aplicación y de la aplicación al escritorio. La compatibilidad con operaciones de arrastrar y colocar se implementa en la clase UIElement mediante las propiedades <strong><a href="/uwp/api/windows.ui.xaml.uielement.allowdrop">AllowDrop</a></strong> y <strong><a href="/uwp/api/windows.ui.xaml.uielement.candrag">CanDrag</a></strong>, y los eventos <strong><a href="/uwp/api/windows.ui.xaml.uielement.dragover">DragOver</a></strong> y <strong><a href="/uwp/api/windows.ui.xaml.uielement.drop">Drop</a></strong>.<br/><br/><a href="/windows/uwp/app-to-app/drag-and-drop">Arrastrar y colocar</a></td>
</tr>
</tbody>
</table>
<h2 id="software-design">Diseño de software</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concepto general</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP de Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Patrones de diseño de software.</strong> <br><br>Patrones recomendados o comunes para la plataforma.</td>
<td align="left">Para el desarrollo en Android, no se ha recomendado ni proporcionado ningún patrón formal, aunque es posible que la versión beta del marco de enlace de datos pueda permitir un uso más extendido del patrón <strong>Model-View-ViewModel (MVVM)</strong>. En varios artículos y marcos de terceros se recomiendan los métodos <strong>Model-View-Presenter (MVP)</strong> y <strong>MVVM</strong>.</td>
<td align="left">El patrón <strong>Model-View-Controller (MVC)</strong> es un patrón común que se usa con iOS y se integra en la plataforma.</td>
<td align="left">No estás limitado a un patrón específico cuando se compila para UWP.<br/><br/>Puedes usar el patrón integrado de <a href="/windows/uwp/data-binding/index">enlace de datos</a> para garantizar una separación clara entre las preocupaciones de datos y las de la interfaz de usuario, y evitar tener que incluir código en los controladores de eventos de la interfaz de usuario que luego actualizan los valores de las propiedades.<br/><br/>Puedes ampliar el enlace de datos para seguir el patrón <strong>Model-View-ViewModel (MVVM)</strong>, ya sea usando bibliotecas MVVM de terceros, como <a href="https://archive.codeplex.com/?p=mvvmlight">MVVM Light Toolkit</a>, o implementando tu propia biblioteca y manteniendo la lógica fuera del código subyacente.<br/><br/><a href="/previous-versions/msp-n-p/hh848246(v=pandp.10)">The MVVM Pattern</a> (El patrón MVVM)					<br/><br/><a href="https://github.com/Windows-XAML/Template10/wiki">Plantillas de proyecto Template 10 Visual Studio</a></td>
</tr>
</tbody>
</table>