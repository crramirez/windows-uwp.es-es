---
Description: Compara las características de plataforma entre iOS, Android y Windows 10.
Search.Product: eADQiWindows 10XVcnh
title: Asignación del concepto de aplicaciones de Windows para desarrolladores de Android e iOS
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 082736c8-2ac3-41b3-b246-e705edc23f34
ms.localizationpriority: medium
ms.openlocfilehash: 2d4739442414b02358f3afea8967b0fc404ff7f9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641340"
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
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Lenguaje de diseño.</strong><br><br>Un conjunto de convenciones que indica el aspecto y comportamiento de las aplicaciones en la plataforma.</td>
<td align="left">En las directrices <strong>Diseño de material de Android</strong> se proporciona un lenguaje visual que deben seguir los diseñadores y desarrolladores de Android.</td>
<td align="left">En <strong>Directrices de interfaz humana</strong> se ofrece asesoramiento para los desarrolladores y diseñadores de iOS.</td>
<td align="left"><a href="https://developer.microsoft.com/en-us/windows/apps/design"><strong>Diseño de aplicaciones de UWP Windows</strong> </a> muestra cómo crear una aplicación con aspecto fantástica en todos los dispositivos Windows 10. Encontrarás aspectos básicos sobre el diseño de la interfaz de usuario (UI), técnicas de diseño dinámico y una lista completa de directrices detalladas.<br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Lenguaje de marcado de la interfaz de usuario.</strong> <br><br>Un lenguaje de marcado que representa y describe una interfaz de usuario y sus componentes. En cada plataforma se proporciona un editor para la edición tanto visual y como de marcado.<br/></td>
<td align="left"><strong>Diseños XML</strong> que se editan mediante <strong>Studio Android</strong> o <strong>Eclipse</strong>.</td>
<td align="left"><strong>XIB</strong> y <strong>guiones gráficos</strong> que se editan mediante el <strong>configurador de interfaz de usuario</strong> en Xcode.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt185595.aspx">XAML</a></strong>, editado mediante <strong><a href="https://www.visualstudio.com/">Microsoft Visual Studio</a></strong> y  <strong><a href="https://msdn.microsoft.com/library/jj171012.aspx">Blend para Visual Studio</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228259.aspx">Plataforma XAML</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228349.aspx">Crear una interfaz de usuario con XAML</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228350.aspx">Definir diseños con XAML</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Controles de interfaz de usuario integrada.</strong> <br><br>Elementos de interfaz de usuario que proporciona la plataforma, como botones, controles de lista y controles de texto.</td>
<td align="left">Clases <strong>view</strong> y <strong>view group</strong> predefinidas conocidas como widgets, diseños, campos de texto, contenedores, controles de fecha y hora, y los controles expertos.</td>
<td align="left"><strong>Vistas</strong> y <strong>controles</strong> que se encuentran en la biblioteca de objetos de Xcode y que se enumeran en el catálogo de interfaz de usuario UIKit. Entre las vistas se incluyen vistas de imagen, vistas de selector y vistas de desplazamiento. Entre los controles se incluyen botones, selectores de fecha y campos de texto.</td>
<td align="left">En la plataforma XAML se proporciona un conjunto amplio de <strong>controles integrados</strong>, como botones, controles de lista, paneles, controles de texto, barras de comandos, selectores, contenido multimedia y la entrada manuscrita.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228345.aspx">Agregar controles y controlar eventos</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Controlar el control de eventos.</strong> <br><br>Define la lógica que se ejecuta cuando se desencadenan eventos en los controles de interfaz de usuario.</td>
<td align="left">Los <strong>controladores de eventos</strong> y <strong>escuchas de eventos</strong> se agregan en XML o mediante programación.</td>
<td align="left">Los controles envían mensajes de <strong>acción</strong> a los <strong>destinos</strong>.</td>
<td align="left">Puedes definir métodos para controlar los eventos de un control XAML en un <strong>archivo de código subyacente</strong> adjunto a la página XAML. Los <strong>controladores de eventos</strong> siempre se escriben en código. Sin embargo, puedes enlazar esos controladores a los eventos en el marcado XAML o en el código.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228345.aspx">Agregar controles y controlar eventos</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt185584.aspx">Introducción a eventos y eventos enrutados</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Enlace de datos.</strong> <br><br>Patrón de diseño de software que permite a la interfaz de usuario de la aplicación representar datos y, opcionalmente, mantenerse sincronizada con dichos datos.</td>
<td align="left">Se proporciona una <strong>biblioteca de enlaces de datos</strong>, aunque se encuentra en la versión beta.</td>
<td align="left">No existe ningún sistema de enlaces integrados en iOS. La <strong>observación clave-valor</strong> se puede ampliar para realizar el enlace de datos, ya sea mediante una biblioteca de terceros o escribiendo código adicional. Los controles usan un método de delegado o devolución de llamada para obtener los datos.</td>
<td align="left">La plataforma UWP controla el <strong>enlace de datos</strong> para ti. La extensión de marcado <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt204783.aspx">{x:Bind}</a></strong> se usa para sacar provecho del enlace de alto rendimiento. <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt204782.aspx">{Binding}</a></strong> se usa para sacar provecho de más características. Luego, solo tienes que configurar el enlace para elegir si la plataforma usará <strong>enlaces unidireccionales</strong> para mostrar los valores de un origen de datos en la interfaz de usuario o si también observará esos valores y actualizará la interfaz de usuario cuando cambien con <strong>enlaces bidireccionales</strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt210947.aspx">Enlace de datos</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Automatización de interfaz de usuario.</strong> <br><br>Acceso mediante programación a los elementos de la interfaz de usuario, lo que permite que las aplicaciones estén accesibles para los productos de tecnología de asistencia y permite el uso de scripts de prueba automatizados para su interacción con la interfaz de usuario.</td>
<td align="left">Los valores <strong>Text labels</strong>, <strong>contentDescription</strong> y <strong>hint</strong> ayudan a garantizar que los elementos de la interfaz de usuario se pueden encontrar mediante la automatización. Android Studio permite escribir pruebas de interfaz de usuario con los marcos de prueba <strong>UI Automator</strong> y <strong>Espresso</strong>.</td>
<td align="left">El <strong>instrumento de automatización</strong> permite escribir scripts de prueba de interfaz de usuario automatizados que identifican elementos mediante opciones de configuración de <strong>accesibilidad</strong> o la posición del elemento en la <strong>jerarquía de elementos</strong>.</td>
<td align="left">Con la configuración inicial, puedes acceder mediante programación a los elementos de interfaz de usuario integrados en UWP mediante la <strong><a href="https://msdn.microsoft.com/library/windows/apps/ee684076.aspx">automatización de la interfaz de usuario</a></strong>.<br/><strong><a href="https://msdn.microsoft.com/library/windows/apps/mt297667.aspx">Automatizaciones del mismo personalizado</a></strong>  permiten proporcionar compatibilidad de automatización para sus propias clases personalizadas de la interfaz de usuario. El <strong><a href="https://msdn.microsoft.com/library/dd286726.aspx#VerifyingCodeUsingCUITCreate">proyecto de pruebas de interfaz de usuario codificada</a></strong> en Visual Studio permite probar automáticamente toda tu aplicación a través de la interfaz de usuario o probar la interfaz de usuario de manera aislada.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Cambiar la apariencia de un control.</strong> <br><br>Edita el tamaño, color y otros atributos.</td>
<td align="left">Los controles tienen <strong>propiedades</strong> que se pueden editar mediante la herramienta del diseñador, en el marcado XML o mediante programación.</td>
<td align="left">Los controles tienen <strong>atributos</strong> que se pueden editar con <strong>Attributes Inspector</strong> en el generador de interfaz de usuario o mediante programación.</td>
<td align="left">Puedes editar las <strong>propiedades</strong> de los controles en el marcado XAML o mediante programación, con Visual Studio y Blend para Visual Studio.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228345.aspx">Agregar controles y controlar eventos</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Estilos visuales reutilizables.</strong> <br><br>Aplica cambios visuales en un número de controles en un formato reutilizable.</td>
<td align="left">Los <strong>estilos XML</strong> son conjuntos de propiedades que se aplican a uno o varios controles.</td>
<td align="left">En la configuración inicial, iOS no admite estilos visuales reutilizables, pero el protocolo UIAppearance permite que varios controles compartan atributos comunes.</td>
<td align="left">Puedes crear <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx">estilos</a></strong> reutilizables, que se puede aplicar a varios controles y almacenar en un <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.resourcedictionary.aspx">ResourceDictionary</a></strong> para facilitar su reutilización.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465381.aspx">Inicio rápido: Aplicar estilos a controles</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>La estructura visual de los controles de edición.</strong> <br><br>Personaliza la estructura visual de un control más allá de la simple modificación de propiedades o atributos; por ejemplo, al mover el texto de una casilla por debajo de esta.</td>
<td align="left">En Android no existe ningún método simple para editar la estructura visual de los controles.</td>
<td align="left">En iOS no existe ningún método simple para editar la estructura visual de los controles.</td>
<td align="left">Para personalizar la estructura visual de un control, puedes copiar y editar su <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.controltemplate.aspx">plantilla de control</a></strong> en el marcado XAML.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465374.aspx">Inicio rápido: Plantillas de control</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Gestos de toque integrados.</strong> <br><br>Proporciona compatibilidad con la entrada táctil personalizada mediante el control de eventos de gestos resumidos de alto nivel, como pulsación y doble pulsación en las vistas y los controles.</td>
<td align="left">Los <strong>detectores de gestos</strong> detectan gestos táctiles comunes, como por ejemplo, desplazamiento, presión prolongada, pulsación, doble pulsación y deslizamiento.</td>
<td align="left">El marco UIKit proporciona <strong>reconocedores de gestos</strong> integrados que detectan gestos táctiles, como por ejemplo, pulsación, reducción, panorámica, deslizamiento, rotación y presión prolongada.</td>
<td align="left">Los <strong>elementos de interfaz de usuario</strong> permiten controlar los <strong>eventos de gestos estáticos</strong>, como por ejemplo, pulsación, doble pulsación, pulsación derecha y mantenimiento de la pulsación, así como <strong>eventos de gestos de manipulación</strong>, como por ejemplo, deslizamiento, deslizamiento rápido, giro, reducción y estiramiento. Los eventos de gestos son <strong>eventos enrutados</strong> y se pueden controlar mediante objetos primarios que contiene el elemento secundario UIElement.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185617.aspx">Interacciones táctiles</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185599.aspx#gestures__manipulations__and_interactions">Interacciones del usuario personalizadas: gestos, manipulaciones e interacciones</a></td>
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
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Diseños.</strong> <br><br>El diseño define la estructura de la interfaz de usuario.</td>
<td align="left">El diseño está compuesto por <strong>grupos de vistas</strong>, tales como <strong>LinearLayout</strong> y <strong>RelativeLayout</strong>, que pueden anidar otras vistas o grupos de vistas.</td>
<td align="left">El diseño está compuesto por un objeto <strong>UIViewController</strong> que contiene objetos <strong>UIView</strong>, que se pueden anidar.</td>
<td align="left">El código XAML que proporciona un sistema de diseño flexible compuesto por <strong>clases de panel de diseño</strong>, como <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.canvas.aspx">Canvas</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.grid.aspx">Grid</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.relativepanel.aspx">RelativePanel</a></strong> y <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx">StackPanel</a></strong> para diseños estáticos y dinámicos. <strong><a href="https://msdn.microsoft.com/library/ms171352.aspx">Propiedades</a></strong>  sirven para controlar el tamaño y posición de los elementos.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx">Definir diseños con XAML</a><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Exploración del mismo nivel.</strong> <br><br>Presentación al usuario de métodos de navegación entre páginas de la misma importancia jerárquica.</td>
<td align="left">Las <strong>pestañas</strong>, las <strong>vistas de deslizamiento</strong> y los <strong>cajones de navegación</strong> proporcionan una <strong>navegación lateral</strong>.</td>
<td align="left">Los <strong>controladores de la barra de pestañas</strong>, <strong>controladores de vista en dos paneles</strong> y <strong>controladores de vista de página</strong> permite una navegación entre vistas de jerarquía igual.</td>
<td align="left">Puedes mostrar una lista persistente de vínculos o pestañas situadas encima de contenido mediante <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn997788.aspx">pestañas o tablas dinámicas</a></strong>. El <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn997787.aspx">panel de navegación o vista en dos paneles</a></strong> permite mostrar una lista de vínculos junto con el contenido.<br/><br/><a href="https://msdn.microsoft.com/windows/uwp/layout/navigation-basics">Navegación</a><br/><br/><a href="https://msdn.microsoft.com/windows/uwp/layout/navigate-between-two-pages">Desplazarse entre las dos páginas</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Navegación jerárquica.</strong> <br><br>Navegación entre las páginas principales y secundarias de una jerarquía.</td>
<td align="left">Las <strong>listas</strong>, <strong>listas de cuadrículas</strong>, <strong>botones</strong> y otros controles proporcionan una <strong>navegación descendente</strong> cuando se usan con <strong>intenciones</strong> para cargar otras <strong>actividades</strong>.</td>
<td align="left">Los <strong>controladores de navegación</strong> permiten a los usuarios navegar entre los niveles de una jerarquía.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/dn449149.aspx">Concentradores</a></strong>  permiten mostrar al usuario una vista previa del contenido que se puede seleccionar para navegar a las páginas secundarias. <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn997765.aspx">Detalles y maestras</a></strong>  permiten a los usuarios elegir entre una lista de los resúmenes de elemento que se muestra al lado de la sección de detalles correspondiente.<br/><br/><a href="https://msdn.microsoft.com/windows/uwp/layout/navigation-basics">Navegación</a><br/><br/><a href="https://msdn.microsoft.com/windows/uwp/layout/navigate-between-two-pages">Desplazarse entre las dos páginas</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Realizar una copia de navegación del botón.</strong> <br><br>Navega hacia atrás por una aplicación.</td>
<td align="left">La <strong>Atrás</strong> y <strong>de</strong> botones dentro de la barra de acciones proporcionan <strong>ancestral</strong> y <strong>temporal</strong> de navegación usando la <strong>pila de retroceso</strong>.</td>
<td align="left">Al <strong>controlador de navegación</strong> se le puede agregar un botón Atrás.<br/></td>
<td align="left">Puedes controlar fácilmente las presiones del botón Atrás de software o hardware mediante la <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.frame.backstack.aspx">propiedad BackStack</a></strong>, que permite a los usuarios recorrer el <strong>historial de navegación</strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt465734.aspx">Navegación con el botón Atrás</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Pantalla de presentación.</strong> <br><br>Muestra una imagen al iniciar la aplicación (se usa principalmente para la personalización de marca).</td>
<td align="left">Las pantallas de presentación no se proporcionan de manera predeterminada y se implementan al editar el <strong>fondo del tema</strong> de las actividades iniciales.</td>
<td align="left">Las aplicaciones deben tener una <strong>imagen de inicio estática</strong> o un <strong>archivo de inicio XIB o de guion gráfico</strong>.</td>
<td align="left">Puedes crear una pantalla de presentación con una <strong>imagen</strong> y un fondo de color. <a href="https://msdn.microsoft.com/library/windows/apps/mt187309.aspx">El tiempo de la pantalla de presentación se puede ampliar</a>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187306.aspx">Agregar una pantalla de presentación</a></td>
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
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Voz.</strong> <br><br>Reconocimiento de voz para la entrada de voz y funcionalidades adicionales de voz.</td>
<td align="left">La entrada de voz puede la puede proporcionar cualquier aplicación que implemente un objeto <strong>RecognizerIntent</strong>, tal como <strong>Búsqueda por voz de Google</strong>. La clase <strong>SpeechRecognizer</strong> permite a las aplicaciones usar la API de reconocimiento de voz de Google.</td>
<td align="left">Las aplicaciones pueden usar la clase <strong>SFSpeechRecognizer</strong> para implementar la entrada de voz y el reconocimiento de voz.</td>
<td align="left">Puedes usar la API de <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185615.aspx">reconocimiento de voz</a></strong> para interactuar con la aplicación en primer plano. Puedes usar las <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185598.aspx">interacciones de Cortana</a></strong> basadas en voz para iniciar aplicaciones en primer o segundo plano y para interactuar con aplicaciones en segundo plano.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185614.aspx">Interacciones de voz</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Entradas de usuario personalizada.</strong> <br><br>Controla las entradas de teclado, mouse y lápiz, entre otras.</td>
<td align="left">La compatibilidad con interacciones incluye <strong>función táctil</strong>, <strong>panel táctil</strong>, <strong>lápiz</strong>, <strong>mouse</strong> y <strong>teclado</strong>. Los movimientos y las entradas se notifican de la misma manera que la función táctil, pero es posible detectar más información sobre el <strong>dispositivo de entrada</strong>.</td>
<td align="left">Se proporciona compatibilidad con <strong>función táctil</strong>, <strong>Apple Pencil</strong> y <strong>teclados</strong> de hardware.</td>
<td align="left">Encontrarás compatibilidad con una amplia variedad de interacciones, como por ejemplo, <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185617.aspx">función táctil</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt187313.aspx">panel táctil</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt187311.aspx">lápiz</a></strong> con entrada digital, <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt187308.aspx">mouse</a></strong> y <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185607.aspx">teclado</a></strong>. Las aplicaciones pueden controlar los datos sin tener que saber qué dispositivo de entrada se usó y, si fuera necesario, se pueden acceder a los datos de dispositivos de entrada sin procesar.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt404610.aspx">Control de la entrada con puntero</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185599.aspx">Interacciones del usuario personalizadas</a></td>
</tr>
</tbody>
</table>
<h2 id="data">Datos</h2>
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
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Datos de la aplicación local.</strong> <br><br>Almacena localmente la configuración y los archivos relacionados con la aplicación.</td>
<td align="left">Los archivos locales se pueden guardar mediante <strong>openFileOutput</strong> y <strong>openFileInput</strong>. La configuración en un <strong>archivo de preferencias compartidas</strong> es accesible mediante <strong>getSharedPreferences</strong>.</td>
<td align="left">Los archivos locales se pueden almacenar en el directorio de <strong>compatibilidad con aplicaciones</strong>, al que se accede a través de la clase <strong>NSFileManager</strong>. La configuración en los archivos de <strong>preferencias</strong> es accesible mediante la clase <strong>NSUserDefaults</strong>.</td>
<td align="left">Las clases <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/br230562.aspx">Windows.Storage</a></strong> controlan el almacenamiento de datos local de manera unificada. La configuración se almacena como un objeto <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.storage.applicationdatacontainer.aspx">ApplicationDataContainer</a></strong>, al que se accede mediante la propiedad <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.storage.applicationdata.localsettings.aspx">ApplicationData.LocalSettings</a></strong>. Los archivos se almacenan en un objeto <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefolder.aspx">StorageFolder</a></strong>, al que se accede mediante la propiedad <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.storage.applicationdata.localfolder.aspx">ApplicationData.LocalFolder</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt299098.aspx">Almacenar y recuperar la configuración y otros datos de aplicación</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Almacenamiento de base de datos local.</strong> <br><br>Almacena los datos de la aplicación en una base de datos relacional, con asignadores relacionales de objetos (ORM), si corresponde.</td>
<td align="left">Se proporciona la base de datos <strong>SQLite</strong>. No hay ORM integrados. Las consultas SQL se ejecutan mediante la clase <strong>SQLiteDatabase</strong>.</td>
<td align="left">Se proporciona la base de datos <strong>SQLite</strong>. El objeto <strong>CoreData</strong> es el marco de gráficos de objetos integrado que se puede usar con SQLite y ofrece una funcionalidad comparable con un ORM.</td>
<td align="left">Puedes almacenar datos mediante <strong>SQLite</strong>. <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592863.aspx">Entity Framework</a></strong>  es un ORM integrado que elimina la necesidad de escribir una gran cantidad de código de acceso a datos y le permite consultar fácilmente la base de datos sin necesidad de escribir SQL. Puedes ejecutar consultas SQL directamente con la <a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592864.aspx">biblioteca SQLite</a>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592862.aspx">Acceso a datos</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Bibliotecas HTTP para el acceso de REST.</strong> <br><br>Bibliotecas integradas que te permiten comunicarte con servicios web y servidores web mediante HTTP(S).<br/></td>
<td align="left">Bibliotecas HTTP <strong>HttpURLConnection</strong> y <strong>Volley</strong>.</td>
<td align="left"><strong>NSURLSession</strong>, <strong>NSURLConnection</strong> y <strong>NSURLDownload</strong>.</td>
<td align="left">Puedes usar la API <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httpclient">HttpClient</a></strong> integrada para acceder a la funcionalidad HTTP común, como por ejemplo, GET, DELETE, PUT, POST, patrones comunes de autenticación, SSL, cookies e información de progreso.</td>
</tr>
<tr class="even">
<td align="left"><strong>Servicios de copia de seguridad en la nube.</strong> <br><br>Servicios de copia de seguridad proporcionados por la plataforma para los datos de la aplicación.</td>
<td align="left">El <strong>administrador de copias de seguridad</strong> de Android controla la creación de copias de seguridad de datos de aplicación en <strong>Android Backup Service</strong> de Google.</td>
<td align="left">Un usuario puede configurar la <strong>copia de seguridad de iCloud</strong> para controlar sus copias de seguridad, como por ejemplo, los datos de aplicación. Aplicaciones que usan <strong>datos principales</strong> compatibles con iCloud, el <strong>almacén de pares clave-valor de iCloud</strong> y el <strong>almacenamiento de documentos de iCloud</strong>.</td>
<td align="left">Los datos de la aplicación que se almacenen mediante las <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.aspx">API ApplicationData</a></strong> de sincronización (por ejemplo <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.roamingfolder.aspx">RoamingFolder</a></strong> y <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.roamingsettings.aspx"><strong>RoamingSettings</strong></a>) se sincronizarán automáticamente en la nube y en los otros dispositivos del usuario. La sincronización se consigue mediante la cuenta de Microsoft del usuario.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/hh465094.aspx">Directrices para perfiles móviles de datos de la aplicación</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>La descarga de archivos HTTP.</strong> <br><br>Descarga archivos de pequeño y gran tamaño a través de HTTP.</td>
<td align="left">Los objetos <strong>URLConnection</strong> y <strong>HTTPURLConnection</strong> se usan para descargar elementos a través de HTTP y FTP. También es posible usar el sistema del <strong>administrador de descargas</strong> para descargar elementos en segundo plano.</td>
<td align="left">Los objetos <strong>NSURLSession</strong> y <strong>NSURLConnection</strong> se pueden usar para descargar archivos a través de HTTP y FTP.</td>
<td align="left">Las <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.networking.backgroundtransfer.aspx">API de transferencia en segundo plano</a></strong> permiten transferir archivos de manera confiable a través de HTTP(S) y FTP, teniendo en cuenta la suspensión de la aplicación, la pérdida de conectividad y los ajustes según la conectividad y duración de la batería. También puedes usar el objeto <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.web.http.httpclient.aspx">HttpClient</a></strong>, que es ideal para archivos más pequeños.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280235.aspx">¿Qué tecnología de red?</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280377.aspx">Transferencias en segundo plano</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Sockets.</strong> <br><br>Crea sockets de datagramas UDP y TCP de bajo nivel para comunicarte con otros dispositivos a través de tu propio protocolo.</td>
<td align="left">La clase <strong>Socket</strong> proporciona sockets TCP y la clase <strong>DatagramSocket</strong> proporciona un socket UDP.</td>
<td align="left">Los objetos <strong>NSStream</strong> y <strong>CFStream</strong> proporcionan sockets TCP, y el objeto <strong>CFSocket</strong> proporciona sockets UDP.</td>
<td align="left">Puedes usar la clase <strong><a href="https://msdn.microsoft.com/library/windows/apps/br241319">DatagramSocket</a></strong> para comunicarte con un socket de datagramas UDP y la clase <strong><a href="https://msdn.microsoft.com/library/windows/apps/br226882">StreamSocket</a></strong> para comunicarte a través de TCP o RFCOMM de Bluetooth.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280233.aspx">Conceptos básicos de redes</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280235.aspx">¿Qué tecnología de red?</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280234.aspx">Información general sobre los sockets</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>WebSockets.</strong> <br><br>Proporciona una comunicación bidireccional entre un cliente y servidor, lo que permite una transferencia de datos en tiempo real.</td>
<td align="left">En Android no existen bibliotecas WebSockets integradas.</td>
<td align="left">En iOS no existen bibliotecas WebSockets integradas.</td>
<td align="left">Las conexiones seguras a servidores compatibles con WebSockets se pueden establecer con la clase <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.networking.sockets.messagewebsocket.aspx">MessageWebSocket</a></strong> para mensajes más pequeños con notificaciones de confirmación y con la clase <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.networking.sockets.streamwebsocket.aspx">StreamWebSocket</a></strong> para transferencias de archivos binarios de mayor tamaño que se pueden leer en secciones.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280233.aspx">Conceptos básicos de redes</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280235.aspx">¿Qué tecnología de red?</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt186447.aspx">Introducción a WebSockets</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Bibliotecas de OAuth.</strong> <br><br>Bibliotecas OAuth que permiten acceder a proveedores OAuth de terceros y la administración de cuentas integrada en la plataforma.</td>
<td align="left">No se proporciona ninguna biblioteca OAuth genérica. La clase <strong>GoogleAuthUtil</strong> se proporciona para la autenticación OAuth con Google Play Services.<br/></td>
<td align="left">No se proporciona ninguna biblioteca OAuth genérica. El <strong>marco de cuentas</strong> proporciona acceso a las cuentas de usuario que ya están almacenadas en el dispositivo, como Facebook y Twitter.</td>
<td align="left">La biblioteca OAuth genérica <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt270196.aspx">Agente de autenticación web</a></strong> permite conectarte a servicios de proveedor de identidades de terceros. La <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt270189.aspx">caja de seguridad de credenciales</a></strong> permite a los usuarios guardar su información de inicio de sesión y usarla en varios dispositivos. El espacio de nombres <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn896755.aspx">Microsoft.Live</a></strong> permite acceder fácilmente a OAuth del SDK de Live para acceder a los servicios Microsoft.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt270184.aspx">Autenticación e identidad de usuario</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/windows.security.authentication.web.aspx">Documentación de la API Windows.Security.Authentication.Web</a><br/><br/><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAuthenticationBroker">Ejemplo de código de WebAuthenticationBroker</a></td>
</tr>
</tbody>
</table>
<h2 id="tooling">Herramientas</h2>
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
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>IDE.</strong> <br><br>Conjunto de herramientas que se usa para crear la aplicación.</td>
<td align="left"><strong>Android Studio</strong> y <strong>Eclipse</strong>, con Google que anima a los desarrolladores hacia el uso de Android Studio.</td>
<td align="left"><strong>Xcode</strong></td>
<td align="left"><strong><a href="https://www.visualstudio.com/features/universal-windows-platform-vs.aspx">Visual Studio</a></strong>  y <strong><a href="https://msdn.microsoft.com/library/jj171012.aspx">Blend para Visual Studio</a></strong> tiene todas las herramientas que necesita para el código, diseño, conectar, depurar, analizar, optimizar y probar aplicaciones para UWP. Visual Studio también te proporciona <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt188754.aspx">emuladores</a></strong> para dispositivos Windows 10 para que puedas probar la aplicación en una amplia variedad de dispositivos emulados.<br/><br/><a href="https://dev.windows.com/downloads">Descargas y herramientas para UWP</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Organización del código.</strong> <br><br>Estructura de carpetas básica de una aplicación, que a menudo se crea a partir de una plantilla inicial.</td>
<td align="left">Archivo <strong>AndroidManifest</strong>, carpeta <strong>java</strong> que contiene los archivos de origen, carpeta <strong>res</strong> con recursos (por ejemplo, diseños y valores), scripts de compilación <strong>Gradle</strong> en Android Studio y scripts de compilación <strong>Ant</strong> en Eclipse.</td>
<td align="left">Archivos de origen y <strong>archivos de compatibilidad</strong>, archivo <strong>Info.plist</strong>, <strong>Main.storyboard</strong> y <strong>LaunchScreen.storyboard</strong>. Las imágenes se almacenan en <strong>bibliotecas de recursos</strong>.</td>
<td align="left">La aplicación para UWP contiene código XAML y archivos de código denominados Example.xaml y Example.xaml.cs, distintas imágenes en la <strong>carpeta Recursos</strong>, una página de inicio (por ejemplo, <strong>MainPage.xaml</strong> y <strong>MainPage.xaml.cs</strong>) y un manifiesto.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/dn765018.aspx">Crear una aplicación "Hello, world"</a></td>
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
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Ciclo de vida de la aplicación.</strong> <br><br>Controla eventos cuando la aplicación se inicia, suspende, reanuda y cierra, y, de este modo, proporciona una oportunidad para guardar y restaurar el estado de la aplicación y ejecutar otras tareas.</td>
<td align="left">Cada actividad tiene su propio <strong>ciclo de vida de la actividad</strong> con estados como, por ejemplo, <strong>reanudada</strong>. <strong>Las devoluciones de llamada del ciclo de vida</strong> como <strong>onResume</strong> se implementan en su <strong>las clases de actividad</strong>.</td>
<td align="left">El <strong>ciclo de vida de aplicación</strong> tiene estados como, por ejemplo, <strong>suspendida</strong>. Los métodos, como <strong>applicationDidEnterBackground:</strong>, se implementan en el <strong>objeto de aplicación de delegado</strong> para ejecutar código cuando se producen cambios de estado.</td>
<td align="left">La aplicación tiene los <strong>estados de ejecución de la aplicación</strong> NotRunning, Activated, Running, Suspending, Suspended y Resuming.<br/><br/>Puedes implementar los métodos de la <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.aspx">clase Application</a></strong> OnLaunched, OnActivated, Suspending o Resuming en tu aplicación para ejecutar código cuando cambie el estado.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt243287.aspx">Ciclo de vida de la aplicación</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Tareas en segundo plano.</strong> <br><br>Tareas que realizan operaciones en segundo plano y siguen ejecutándose cuando la aplicación ya no está en primer plano.</td>
<td align="left">Las aplicaciones pueden iniciar <strong>servicios</strong> que realizan operaciones en segundo plano cuando la aplicación ya no está en primer plano. Los servicios tienen su propio <strong>ciclo de vida</strong> y se registran en el manifiesto.</td>
<td align="left">La <strong>ejecución en segundo plano</strong> solo se permite para tipos de tarea específicos.<br/><br/>Las aplicaciones declaran <strong>tareas en segundo plano admitidas</strong> en el archivo Info.plist mediante el objeto <strong>UIBackgroundModes</strong>.<br/><br/>El sistema controla cuándo se ejecutan tareas en segundo plano y durante cuánto tiempo.</td>
<td align="left">Puedes crear una tarea en segundo plano mediante la implementación de la interfaz <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.aspx">IBackgroundTask</a></strong> y el registro de la tarea en el manifiesto de la aplicación. Puede establecer una tarea para que se desencadene con un <a href="https://msdn.microsoft.com/library/windows/apps/mt186458.aspx"><strong>temporizador</strong></a>, <a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.systemtriggertype.aspx"><strong>desencadenador del sistema</strong></a> y <a href="https://msdn.microsoft.com/library/windows/apps/mt185632.aspx"><strong>desencadenador de mantenimiento</strong></a>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt299103.aspx">Dar soporte a tu aplicación mediante tareas en segundo plano</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt299100.aspx">Crear y registrar una tarea en segundo plano</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187310.aspx">Directrices para tareas en segundo plano</a></td>
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
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Procedimientos recomendados de rendimiento.</strong> <br><br>Directrices para crear aplicaciones que sean rápidas y dinámicas, y que tengan en cuenta la duración de la batería con un tiempo de inicio rápido.</td>
<td align="left">Android ofrece la guía de aprendizaje <strong>Best Practices for Performance</strong>.</td>
<td align="left">iOS proporciona el documento <strong>Performance Overview</strong>.</td>
<td align="left">Puedes leer la <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt270266.aspx">Guía de rendimiento</a></strong> detallada, con secciones que cubren temas como por ejemplo, establecer objetivos de rendimiento, medir el rendimiento, administrar la memoria, animaciones suaves, acceso eficaz al sistema de archivos y herramientas disponibles para la generación de perfiles y rendimiento.</td>
</tr>
<tr class="even">
<td align="left"><strong>Optimización de la vista para una interfaz de usuario con capacidad de respuesta.</strong> <br><br>Mejorar el rendimiento mediante la optimización de vistas.</td>
<td align="left">Optimizar las <strong>jerarquías de diseño</strong> mediante la herramienta Visor de jerarquías, <strong>reutilizar diseños</strong> y cargar <strong>vistas a petición</strong> son técnicas que ayudan a que la interfaz de usuario responda a los subprocesos y evite cuadros de diálogo del tipo &quot;La aplicación no responde&quot; (<strong>ANR</strong>).<br/></td>
<td align="left">Corregir los problemas en la interfaz de usuario relacionados con la <strong>representación fuera de la pantalla</strong>, las <strong>capas mezcladas</strong> y la <strong>rasterización</strong> con la herramienta <strong>Core Animation</strong> para ayudar a que la interfaz de usuario permanezca dinámica.</td>
<td align="left">Puedes <strong>optimizar</strong> fácilmente el <strong>marcado</strong> y los <strong>diseños</strong> XAML siguiendo unos sencillos pasos. Entre las técnicas se incluyen la reducción de la estructura de diseño, la minimización del número de elementos y la minimización del exceso de dibujo. <br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185403.aspx">Mantener la capacidad de respuesta del subproceso de la interfaz de usuario</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt204779.aspx">Optimizar el marcado XAML</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt404609.aspx">Optimiza tu diseño XAML</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Subprocesamiento.</strong> <br><br>Usa los subprocesos para mantener una <strong>interfaz de usuario dinámica</strong> y ejecutar varias <strong>tareas en paralelo</strong>.</td>
<td align="left">Los subprocesos se logran mediante las clases <strong>Runnable</strong>, <strong>Handler</strong>, <strong>ThreadPoolExecutor</strong>, y el nivel superior <strong>AsyncTask</strong>.</td>
<td align="left">Los subprocesos se logran mediante los objetos <strong>NSThread</strong>, <strong>Grand Central Dispatch</strong> y <strong>NSOperation</strong> de mayor nivel.</td>
<td align="left">Puedes trabajar con subprocesos al enviar <strong>elementos de trabajo</strong> al <strong>conjunto de subprocesos</strong> con <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.system.threading.threadpool.runasync.aspx">RunAsync</a></strong>. Puedes usar un temporizador para enviar un elemento de trabajo con <strong><a href="https://msdn.microsoft.com/library/windows/apps/br230590.aspx">CreateTimer</a></strong> y crea un elemento de trabajo repetitivo con <strong><a href="https://msdn.microsoft.com/library/windows/apps/br230589.aspx">CreatePeriodicTimer</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187339.aspx">Enviar un elemento de trabajo al grupo de subprocesos</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187341.aspx">Enviar un elemento de trabajo con un temporizador</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187338.aspx">Crear un elemento de trabajo periódico</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187336.aspx">Procedimientos recomendados para usar el grupo de subprocesos</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Programación asincrónica.</strong> <br><br>Evita la complejidad de los subprocesos al sacar partido de modelos de programación asincrónica para mantener la capacidad de respuesta del subproceso de interfaz de usuario.</td>
<td align="left">El uso de <strong>subprocesos es necesario</strong> para crear clases asincrónicas propias. Algunas clases integradas son asincrónicas.</td>
<td align="left">El uso de <strong>subprocesos es necesario</strong> para crear clases asincrónicas propias. Algunas clases integradas son asincrónicas.</td>
<td align="left">Puedes usar patrones asincrónicos para evitar bloquear el subproceso principal al crear API propias, por ejemplo, con objetos <strong>async</strong> y <strong>await</strong> en C# y Visual Basic. Puedes usar las API asincrónicas integradas que terminan con la palabra <strong>Async</strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187335.aspx">Programación asincrónica</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187337.aspx">Llamar a API asincrónicas en C# o Visual Basic</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Optimización de la vista de lista.</strong> <br><br>Patrones integrados para ayudar en la optimización de las listas de datos, que con frecuencia tienen un rendimiento deficiente cuando se necesitan mostrar grandes cantidades de datos.</td>
<td align="left">El patrón de diseño <strong>ViewHolder</strong> se usa para evitar varias de búsquedas de vistas, lo que permite usar elementos de interfaz de usuario reutilizables.</td>
<td align="left">Se puede crear una variedad de optimizaciones para mejorar el rendimiento del objeto <strong>UITableView</strong>, no hay elementos integrados.</td>
<td align="left">Puedes usar los controles <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx">ListView</a> y <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx">GridView</a>, que proporcionan <strong>virtualización de la interfaz de usuario</strong> en la configuración inicial. Esto ofrece una experiencia de movimiento panorámico y desplazamiento suaves, así como un tiempo de inicio más rápido. También puedes implementar los objetos <a href="https://msdn.microsoft.com/library/windows/apps/system.collections.ilist.aspx">IList</a> e <a href="https://msdn.microsoft.com/library/windows/apps/system.collections.specialized.inotifycollectionchanged.aspx">INotifyCollectionChanged</a> en el origen de datos, lo que proporciona la <strong>virtualización de datos</strong> y mejora aún más el rendimiento.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt204776.aspx">Optimización de interfaz de usuario de ListView y GridView</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt574120.aspx">Virtualización de datos de ListView y GridView</a></td>
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
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Adquisición de la aplicación.</strong> <br><br>Características de la plataforma que permiten al usuario realizar compras desde las aplicaciones.</td>
<td align="left">La <strong>facturación desde la aplicación</strong> la proporciona Google Services. Los productos se agregan a la <strong>Consola para Desarrolladores de Google Play</strong>. Las compras desde la aplicación se implementan con la <strong>Google Play Billing Library</strong>.</td>
<td align="left">Los productos se agregan a <strong>iTunes Connect</strong>. Las compras desde la aplicación se implementan mediante el marco <strong>StoreKit</strong>.<br/><br/>Los productos se compran mediante <strong>SKMutablePayment</strong> y <strong>SKPaymentQueue</strong>.</td>
<td align="left">Las compras de productos desde la aplicación de la aplicación se crean <a href="https://msdn.microsoft.com/library/windows/apps/mt148551.aspx">agregándolas a tu aplicación y enviándolas a la Tienda</a>. <br/><br/>Usas la <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx">clase CurrentApp</a></strong> para definir las compras desde la aplicación. <br/><br/>Usas el objeto <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.requestproductpurchaseasync.aspx">CurrentApp.RequestProductPurchaseAsync</a></strong> para mostrar la interfaz de usuario que permite a los clientes a comprar el producto.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219684.aspx">Habilitar las compras de productos desde la aplicación</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Puede usar en la aplicación de compras.</strong> <br><br>Productos desde la aplicación que se pueden comprar, usar y luego volver a comprar.</td>
<td align="left">Las compras consumibles se habilitan al realizar una compra normal y luego consumir con el objeto <strong>consumePurchase</strong>, lo que permite comprar, usar y luego volver a comprar.</td>
<td align="left">Los productos consumibles se <strong>definen como productos consumibles</strong> en iTunes Connect.</td>
<td align="left">Puedes admitir consumibles al <a href="https://msdn.microsoft.com/library/windows/apps/mt148534.aspx">definir su tipo de producto como Consumible cuando los envíes</a> a la Tienda. Luego, llamas a <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync.aspx">CurrentApp.ReportConsumableFulfillmentAsync</a></strong> después de que se haya realizado una compra de consumible para permitir que el cliente acceda a él.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219683.aspx">Habilitar compras de productos consumibles desde la aplicación</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Pruebas de compras de la aplicación.</strong> <br><br>Se permite probar el código de compra desde la aplicación sin poner la aplicación en la Tienda.</td>
<td align="left">Para realizar pruebas, se usa el <strong>espacio aislado de facturación desde la aplicación</strong>.</td>
<td align="left">Para realizar pruebas, se usan <strong>cuentas de evaluador del espacio aislado</strong>.</td>
<td align="left">Puedes probar las compras desde la aplicación mediante la clase <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx">CurrentAppSimulator</a></strong> en lugar del objeto CurrentApp.<br/><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Versiones de prueba.</strong> <br><br>Se permite limitar fácilmente el contenido o quitar la publicidad según una versión de prueba de una aplicación.</td>
<td align="left">Google Play  <strong>no admite oficialmente las pruebas de aplicación</strong>. Las pruebas o la eliminación de publicidad se logran creando una compra desde la aplicación y tomando la ruta de acceso al código correspondiente al confirmar que la compra se ha realizado correctamente.</td>
<td align="left">La App Store <strong>no admite oficialmente las pruebas de aplicación</strong>. Las pruebas o la eliminación de publicidad se logran creando una compra desde la aplicación y tomando la ruta de acceso al código correspondiente al confirmar que la compra se ha realizado correctamente.</td>
<td align="left">Puedes ofrecer una versión de prueba gratuita de tu aplicación al usar la <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt148548.aspx">opción 'Prueba gratuita'</a></strong> cuando envías la aplicación a la Tienda. Luego puedes usar el objeto <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.licenseinformation.istrial.aspx">LicenseInformation.IsTrial</a></strong> para comprobar el estado de prueba de la aplicación y presentar distintas rutas de código, según corresponda. Puedes registrar el <a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.licenseinformation.licensechanged">evento LicenseChanged</a> para que reciba notificación cuando el usuario cambia el estado de prueba mientras se ejecuta la aplicación.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219685.aspx">Excluye o limita características en una versión de prueba</a></td>
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
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Interfaz de usuario adaptable: diseños flexibles.</strong> <br><br>Se admiten distintos tamaños de pantalla con un alto y ancho flexibles.</td>
<td align="left">Los diseños flexibles se pueden obtener mediante los valores <strong>wrap_content</strong> y <strong>match_parent</strong> en objetos LinearLayout o usando objetos RelativeLayout para la alineación.</td>
<td align="left">Los diseños flexibles se pueden obtener mediante el <strong>modelo adaptativo</strong> con guiones gráficos universales, lo que usa el <strong>diseño automático</strong> con <strong>restricciones</strong> y <strong>rasgos</strong>, como objetos horizontalSizeClass y displayScale que se aplican a los controladores de vista.</td>
<td align="left">Puedes crear un diseño fluido mediante <strong>propiedades de diseño</strong> y <strong>paneles</strong> con una combinación de tamaños fijos y dinámicos.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx#layout_overview">Definir diseños con XAML: Paneles y propiedades de diseño</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/dn958435.aspx">Diseño con capacidad de respuesta 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>UI adaptable: adaptadas a los diseños.</strong> <br><br>Se admiten diferentes tamaños de pantalla con diseños de destino independientes.</td>
<td align="left">Se proporcionan archivos de diseño alternativos para distintas configuraciones de pantalla en el directorio de recursos mediante <strong>calificadores de configuración</strong>, como <strong>pequeño</strong>, <strong>grande</strong>, <strong>ldpi</strong> y <strong>hdpi</strong>, lo que permite dirigirse a diseños personalizados para pantallas de distintos tamaños y densidades.</td>
<td align="left">Define un <strong>guion gráfico de iPhone e iPad independiente</strong> para personalizar los diseños según las distintas familias de dispositivos en una aplicación universal.</td>
<td align="left">Puedes crear un diseño personalizado al definir <strong>archivos de marcado XAML diferentes</strong> por familia de dispositivos.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx#tailored_layouts">Definir diseños con XAML: diseños personalizados</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Interfaz de usuario adaptable: diseños con capacidad de respuesta.</strong> <br><br>Responde a los cambios de tamaño de pantalla, como la rotación, o a un cambio en el tamaño de una ventana.</td>
<td align="left">El uso de diseños flexibles con objetos <strong>LinearLayout</strong> y <strong>RelativeLayout</strong> o proporcionar archivos de diseño alternativos para distintas orientaciones permiten diseños dinámicos.</td>
<td align="left">Cuando el <strong>tamaño</strong> o los <strong>rasgos</strong> de una vista cambian, se aplican las <strong>restricciones</strong> especificadas en los guiones gráficos.</td>
<td align="left">Puedes redistribuir, cambiar la posición, cambiar el tamaño, mostrar o reemplazar fácilmente secciones de la interfaz de usuario en tiempo de ejecución en respuesta a los cambios del tamaño de ventana mediante los objetos <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstate.aspx">VisualState</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager.aspx">VisualStateManager</a></strong> y <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.adaptivetrigger.aspx">AdaptiveTrigger</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx#visual_states_and_state_triggers">Definir diseños con XAML: estados visuales y desencadenadores de estado</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/dn958435.aspx">Diseño con capacidad de respuesta 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Compatibilidad con las capacidades de dispositivo diferentes.</strong> <br><br>Saca provecho de las características de hardware avanzadas y, al mismo tiempo, admite dispositivos que no disponen de ellas.</td>
<td align="left">Las pruebas de las características del dispositivo en tiempo de ejecución mediante el objeto <strong>PackageManager.hasSystemFeature</strong> te permite decidir si se puede ejecutar código específico del hardware.</td>
<td align="left">No existe <strong>una comprobación única</strong> que puedes realizar en tiempo de ejecución para probar las características del dispositivo. Debes probar cada característica de forma específica para decidir si se puede ejecutar código específico del hardware.</td>
<td align="left">Puedes agregar <strong>SDK de extensión de plataforma</strong> al paquete para dirigirte a la funcionalidad adicional que se ofrece en las distintas familias de dispositivo, por ejemplo, teléfonos, escritorio e IoT. Usa la <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.aspx">API ApiInformation</a></strong> para comprobar la presencia de tipos y miembros en tiempo de ejecución y puedes llamar a esos tipos y miembros solo si están presentes.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Compatibilidad con las capacidades de dispositivo diferentes.</strong> <br><br>Saca provecho de las características de hardware avanzadas y, al mismo tiempo, admite dispositivos que no disponen de ellas.</td>
<td align="left">La <strong>biblioteca de compatibilidad de Android</strong> se puede empaquetar con tu aplicación para que algunas API más recientes estén disponibles para los usuarios con versiones anteriores de Android. Las pruebas de nivel de API en tiempo de ejecución se pueden realizar mediante <strong>Build.Version.SDK_INT</strong>.</td>
<td align="left">Las comprobaciones estándares en tiempo de ejecución se usan para averiguar si las API están disponibles, como el método <strong>class</strong> para comprobar si una clase existe y <strong>respondsToSelector:</strong> para comprobar los métodos en las clases.</td>
<td align="left">Puedes usar el objeto <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn949005.aspx">ApiInformation.IsApiContractPresent</a></strong> para identificar si hay un contrato de API con un número principal y secundario especificado. También puedes usar la <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.aspx">API ApiInformation</a></strong> para comprobar la presencia de tipos y miembros en tiempo de ejecución y puedes llamar a esos tipos y miembros solo si están presentes.</td>
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
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Iconos y notificaciones.</strong> <br><br>Presenta actualizaciones a los usuarios en la pantalla principal.</td>
<td align="left">Los <strong>widgets de aplicación</strong> son vistas en la aplicación que se pueden integrar en la pantalla principal y pueden recibir actualizaciones periódicas. En Android no existe ningún <strong>sistema de distintivos</strong>. No existe ningún sistema idéntico a los iconos.</td>
  <td align="left">Los <strong>Widgets</strong> en iOS aparecen en el Centro de notificaciones y se implementan como <strong>extensiones de la aplicación</strong>. Puedes también agregar un <strong>distintivo</strong> al icono con un número que puede cambiar en respuesta a notificaciones locales o remotas. No hay ningún sistema de iconos.</td>
<td align="left">La aplicación tiene un <strong>icono</strong>, que se pueden anclar a la pantalla Inicio y se usa para mostrar la selección de texto, imágenes y un <strong>distintivo</strong> con glifos y números. Puedes actualizar el contenido de los iconos de la aplicación a través de notificaciones de inserción o programaciones predefinidas. Los iconos pueden ser adaptativos y pueden cambiar en función de dónde se muestran.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt185605.aspx">Crear iconos</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt590880.aspx">Crear iconos adaptables</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt187193.aspx">Elegir un método de entrega de notificaciones</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465403.aspx">Directrices sobre iconos y distintivos</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Mostrar notificaciones.</strong> <br><br>Tipos de notificaciones que se pueden mostrar.</td>
<td align="left">Las notificaciones se pueden mostrar en el <strong>área de notificación</strong> y el <strong>cajón de notificaciones</strong>. Las <strong>notificaciones de visualización frontal</strong> presentan una notificación en una pequeña ventana flotante. A las notificaciones se les pueden agregar acciones mediante la definición de un objeto <strong>PendingIntent</strong>.</td>
<td align="left">Las notificaciones emergentes aparecen como <strong>pancartas</strong> o <strong>alertas</strong>. Puedes agregar botones de acción personalizados a las <strong>notificaciones accionables</strong>, que se definen con el objeto <strong>UIMutableUserNotificationAction</strong>.</td>
<td align="left">Puedes crear notificaciones emergentes adaptativas llamadas <strong>notificaciones del sistema</strong>. Puedes definir notificaciones del sistema en el código XML con contenido visual, <strong>acciones</strong> que pueden ser botones, o entradas y audio.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt631604.aspx">Notificaciones del sistema interactivas y adaptables</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt187193.aspx">Elegir un método de entrega de notificaciones</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465391.aspx">Directrices sobre notificaciones del sistema</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Programación de notificaciones locales.</strong> <br><br>Notificaciones locales que tu aplicación envía en una hora programada.</td>
<td align="left">Las notificaciones y acciones se definen mediante un objeto <strong>NotificationCompat.Builder</strong> y se pueden programar y controlar desde la aplicación mediante los objetos <strong>AlarmManager</strong> y <strong>BroadcastReceiver</strong>.</td>
<td align="left">Notificaciones locales se crean mediante <strong>UILocalNotification</strong>y se pueden programar con <b> UILocalNotification.scheduleLocalNotification:<strong>. | Puede programar una notificación del sistema mediante</strong>  <a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.notifications.scheduledtoastnotification.aspx">ScheduledToastNotification</a><strong>. Puedes enviar una notificación de icono desde la aplicación mediante la</strong> <a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.notifications.tilenotification.aspx">clase TileNotification</a><strong> o programar una notificación de icono mediante el objeto <a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.notifications.scheduledtilenotification.aspx">ScheduledTileNotification</a>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt631604.aspx">Notificaciones del sistema interactivas y adaptables</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt593299.aspx">Enviar una notificación local del mosaico</a> || </strong>Envío de notificaciones push.</b> Notificación que se envía desde un servidor de notificación de inserción y, opcionalmente, controlada desde la aplicación.</td>
<td align="left"><strong>Google Cloud Messaging</strong> proporciona compatibilidad con notificaciones de inserción para Android.</td>
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
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Medios de captura.</strong> <br><br>Grabar contenido audiovisual.</td>
<td align="left">El uso de una <strong>intención</strong>, como MediaStore.ACTION_VIDEO_CAPTURE, permite la captura de contenido multimedia con una aplicación de cámara existente. El uso de la biblioteca <strong>android.hardware.camera2</strong> o <strong>camera</strong> permite la implementación de una interfaz de cámara personalizada. Para capturar audio se pueden usar API <strong>MediaRecorder</strong>.</td>
<td align="left">El objeto <strong>UIImagePickerController</strong> permite la captura de vídeo y fotos con la interfaz de usuario del sistema. La clase <strong>AVFoundation</strong>, como <strong>AVCaptureSession</strong>, permite un acceso directo a la cámara. <br/>La clase <strong>AVAudioRecorder</strong> permite la grabación de audio.</td>
<td align="left">Puedes capturar fotos y vídeo mientras se usa la interfaz de usuario de la cámara integrada con la <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.capture.cameracaptureui.aspx">clase CameraCaptureUI</a></strong>. Puede interactuar con la cámara en un nivel bajo y capturar audio con las clases de <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.capture.aspx">Windows.Media.Capture</a></strong>, como la <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.capture.mediacapture.aspx">API MediaCapture</a></strong>. <br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt282142.aspx">Capturar fotografías y vídeos con CameraCaptureUI</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243896.aspx">Capturar fotografías y vídeos con MediaCapture</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Reproducción multimedia.</strong> <br><br>Reproducir archivos de audio y vídeo.</td>
<td align="left">Para la reproducción de archivos de audio y vídeo se usan las clases <strong>MediaPlayer</strong> y <strong>AudioManager</strong>.</td>
<td align="left">Para la reproducción de archivos de audio y vídeo se usan los objetos <strong>AVKit framework</strong>, <strong>AVAudioPlayer</strong> y <strong>Media Player Framework</strong>.</td>
<td align="left">Puedes usar las clases <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.core.mediasource.aspx">MediaSource</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaelement.aspx">MediaElement</a></strong> y <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.aspx">MediaPlayer</a></strong> para reproducir audio y vídeo desde orígenes, tales como archivos locales y remotos.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592657.aspx">Reproducción de contenido multimedia con MediaSource</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Edición de medios.</strong> <br><br>Crea nuevos archivos multimedia a partir de grabaciones existentes y aplica efectos especiales.</td>
<td align="left">Para la edición de contenido se pueden usar clases de bajo nivel, como <strong>MediaCodec</strong>, <strong>MediaMuxer</strong> y <strong>android.media.effect</strong>.</td>
<td align="left">Para la edición de contenido se pueden usar las clases del marco <strong>AV Foundation</strong>, <strong>AVMutableComposition</strong>, <strong>AVMutableVideoComposition</strong> y <strong>AVMutableAudioMix</strong>.</td>
<td align="left">Puedes usar las API <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.editing.aspx">Windows.Media.Editing</a></strong>, como <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.editing.mediacomposition.aspx">MediaComposition</a></strong> y <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.editing.mediaclip.aspx">MediaClip</a></strong> para crear composiciones multimedia a partir de archivos de audio y vídeo. Puedes agregar superposiciones de imagen y vídeo, combinar clips de vídeo, agregar audio de fondo y aplicar efectos de audio y vídeo.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt204792.aspx">Composiciones y edición multimedia</a></td>
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
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Sensores.</strong> <br><br>Detecta el movimiento, la posición y las propiedades del entorno del dispositivo.</td>
<td align="left">El <strong>marco de sensor</strong> se usa para acceder a los sensores de hardware y software con clases como, por ejemplo, <strong>SensorManager</strong> y <strong>SensorEvent</strong>.</td>
<td align="left">El <strong>marco Core Motion</strong> se usa para acceder a datos de sensor procesados y sin procesar.</td>
<td align="left">Puedes usar las clases en <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.aspx">Windows.Devices.Sensors</a></strong> para acceder a las lecturas del sensor y los eventos que se desencadena cuando se reciben nuevos datos de lectura del sensor.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt187358.aspx">Sensores</a></td>
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
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Ubicación.</strong> <br><br>Busca la ubicación <strong>actual</strong> del dispositivo y realiza un seguimiento de los <strong>cambios</strong>.</td>
<td align="left">Las API de servicios de ubicación de Google ofrecen acceso de alto nivel a la <strong>última ubicación conocida</strong> con el <strong>proveedor de ubicación combinados</strong> mediante los métodos <strong>getLastLocation</strong> y <strong>requestLocationUpdates</strong>. El acceso de bajo nivel se proporciona en las bibliotecas de Android con el objeto <strong>LocationManager</strong>.</td>
<td align="left">El <strong>Core ubicación</strong> <strong>CLLocationManager</strong> clase se utiliza para supervisar la ubicación de un dispositivo, con <strong>startUpdatingLocation</strong> para el servicio de ubicación estándar y <strong>startMonitoringSignificantLocationChanges</strong> para el <strong>cambio significativo</strong> servicio de ubicación.</td>
<td align="left">Puede hacer un seguimiento de la ubicación del dispositivo con las clases en <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.aspx">Windows.Devices.Geolocation</a></strong>. Usa el objeto <strong><a href="https://msdn.microsoft.com/library/windows/apps/br225537.aspx">Geolocator.GetGeopositionAsync</a></strong> para una lectura única. Usa el objeto <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geolocator.positionchanged.aspx">Geolocator.PositionChanged</a></strong> para obtener la ubicación periódicamente mediante un temporizador o para recibir una notificación cuando cambie la ubicación.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219698.aspx">Obtener la ubicación del usuario</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Mostrar mapas.</strong> <br><br>Muestra un <strong>mapa integrado interactivo</strong> y agrega <strong>puntos de interés</strong>.</td>
<td align="left">Las clases <strong>GoogleMap</strong>, <strong>MapFragment</strong> y <strong>MapView</strong> de la <strong>API Google Maps para Android</strong> permiten integrar mapas en las aplicaciones. Los puntos de interés se pueden mostrar con <strong>marcadores</strong> y la clase <strong>Marker</strong> personalizable.</td>
<td align="left">Los mapas se integran en las aplicaciones de iOS mediante la clase <strong>MKMapView</strong> en el objeto <strong>MapKit framework</strong>. Se pueden agregar <strong>anotaciones</strong> a las aplicaciones para mostrar puntos de interés mediante clases de objeto, como por ejemplo, <strong>MKPointAnnotation</strong>, y clases de vista, como por ejemplo, <strong>MKPinAnnotationView</strong>.</td>
<td align="left">Puedes integrar mapas en tus aplicaciones mediante el control XAML integrado <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx">MapControl</a></strong>, que proporciona vistas 2D, 3D y Streetside. Puedes agregar puntos de interés con un marcador, imagen o forma mediante clases como, por ejemplo, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapicon.aspx">MapIcon</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mappolygon.aspx">MapPolygon</a></strong> y <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mappolyline.aspx">MapPolyline</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219695.aspx">Mostrar mapas con vistas 2D, 3D y Streetside</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219696.aspx">Mostrar puntos de interés en un mapa</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Perímetro.</strong> <br><br>Supervisa la entrada y salida de una región geográfica determinada.</td>
<td align="left">Las geovallas se supervisan mediante los <strong>servicios de ubicación</strong> en el SDK de Google Play Services.</td>
<td align="left">Las regiones se supervisan mediante la clase <strong>CLCircularRegion</strong> y se registran mediante el objeto <strong>CLLocationManager.startMonitoringForRegion:</strong>.</td>
<td align="left">Puedes crear una geovalla mediante la clase <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geofencing.geofence.aspx">Geofence</a></strong> y definir los <strong>estados supervisados</strong>, tal como la entrada o salida de una región. Controla los eventos de geovalla en primer plano mediante la <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geofencing.geofencemonitor.aspx">clase GeofenceMonitor</a></strong> y en segundo plano mediante la <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.locationtrigger.aspx">clase en segundo plano LocationTrigger</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219702.aspx">Configurar una geovalla</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Geocodificación y geocodificación inversa.</strong> <br><br>Convierte direcciones en ubicaciones geográficas (geocodificación) y convierte ubicaciones geográficas en direcciones (geocodificación inversa).<br/></td>
<td align="left">Para la geocodificación y geocodificación inversa se usa la clase <strong>Geocoder</strong>.</td>
<td align="left">Para la geocodificación se usa la clase <strong>CLGeocoder</strong>.</td>
<td align="left">Puedes realizar la geocodificación mediante la <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.aspx">clase MapLocationFinder</a></strong> en <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.aspx">Windows.Services.Maps</a></strong>. Usa el objeto <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.findlocationsasync.aspx">FindLocationsAsync</a></strong> para la geocodificación y el objeto <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.findlocationsatasync.aspx">FindLocationsAtAsync</a></strong> para la geocodificación inversa.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219697.aspx">Realizar geocodificación y geocodificación inversa</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Las rutas y direcciones.</strong> <br><br>Proporciona rutas, distancias e indicaciones entre dos ubicaciones geográficas.</td>
<td align="left">Google ofrece la <strong>API de indicaciones de Google Maps</strong> del servicio web, que se puede usar en Android, aunque no se proporciona ningún SDK.</td>
<td align="left">El kit de mapas proporciona la API <strong>MKDirections</strong>, que se puede usar para obtener información sobre una ruta e indicaciones.</td>
<td align="left">Puedes solicitar una ruta a pie o en coche mediante la clase <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maproutefinder.aspx">MapRouteFinder</a></strong> en <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.aspx">Windows.Services.Maps</a></strong>. Las rutas se devuelven como una instancia de <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maproute.aspx">MapRoute</a></strong>, que se puede mostrar fácilmente en un objeto MapControl. Las indicaciones se devuelven dentro del objeto <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maproutemaneuver.aspx">MapRouteManeuver</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219701.aspx">Mostrar rutas e indicaciones en un mapa</a></td>
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
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Invocar otra aplicación.</strong> <br><br>Inicia otra aplicación y, opcionalmente, comparte datos, como vínculos, texto, fotos, vídeos y archivos.</td>
<td align="left">Se usa una <strong>intención implícita</strong> para iniciar otra aplicación, mediante la definición de una <strong>acción</strong> y datos opcionales en una <strong>intención</strong> y su llamada mediante el objeto <strong>startActivityForResult</strong>.<br/></td>
<td align="left">Se pueden usar <strong>extensiones de aplicación</strong> para proporcionar acceso a los datos de aplicación a otra aplicación. <strong>Esquemas de direcciones URL</strong> permite que una dirección URL se pase a otra aplicación.</td>
<td align="left">Puedes iniciar otra aplicación que se ha registrado para un URI con el objeto <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.system.launcher.launchuriasync.aspx">Launcher.LaunchUriAsync</a></strong> o <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.system.launcher.launchuriforresultsasync.aspx">Launcher.LaunchUriForResultsAsync</a></strong> para iniciar los resultados y obtener datos de la aplicación iniciada. Puedes usar el objeto <strong><a href="https://msdn.microsoft.com/library/windows/apps/hh701471.aspx">Launcher.LaunchFileAsync</a></strong> para pasar un archivo a otra aplicación y que esta lo procese.<br/><br/>Puedes usar un <strong>contrato para contenido compartido</strong> y compartir datos fácilmente entre aplicaciones.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228340.aspx">Iniciar la aplicación predeterminada de un URI</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt269386.aspx">Iniciar una aplicación para obtener resultados</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt299102.aspx">Iniciar la aplicación predeterminada de un archivo</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243293.aspx">Compartir datos</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Permite que la aplicación que se debe invocar.</strong> <br><br>Permite que la aplicación responda a una solicitud de otra aplicación.</td>
<td align="left">Las aplicaciones registran una <strong>actividad de control de intención</strong> con un <strong>filtro de intención</strong> para responder a una intención implícita de otra aplicación.</td>
<td align="left">El empaquetado de una <strong>extensión de aplicación</strong> permite compartir datos con otras aplicaciones. Las aplicaciones pueden registrar un <strong>esquema de dirección URL personalizado</strong> mediante la clave <strong>CFBundleURLTypes</strong> en Info.plist.</td>
<td align="left">Puedes registrar la aplicación para que sea el controlador predeterminado de un <strong>nombre de esquemas de URI</strong> al registrar un <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.activationkind.aspx#Protocol">protocolo</a></strong> en el manifiesto de paquete y actualizando el controlador de eventos <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onactivated.aspx">Application.OnActivated</a></strong>, lo que devuelve resultados opcionalmente. Del mismo modo, puedes registrar tu aplicación para que sea el controlador predeterminado para determinados tipos de archivo mediante la adición de una declaración en el manifiesto del paquete y el control del evento <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onfileactivated.aspx">Application.OnFileActivated</a></strong>.<br/><br/>Puedes controlar solicitudes de contrato para contenido compartido al registrar la aplicación como destino de contenido compartido en el manifiesto y controlando el evento <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.application.onsharetargetactivated.aspx">Application.OnShareTargetActivated</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt269386.aspx">Iniciar una aplicación para obtener resultados</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt269385.aspx">Administrar la activación de archivos</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243292.aspx">Recibir datos</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Copiar y pegar.</strong> <br><br>Copia y pega texto y otro contenido entre aplicaciones.</td>
<td align="left">Se puede usar el <strong>marco de portapapeles</strong> para implementar las operaciones de copiar y pegar mediante las clases <strong>ClipboardManager</strong> y <strong>ClipData</strong>.</td>
<td align="left">Se pueden usar los objetos <strong>UIPasteboard</strong>, <strong>UIMenuController</strong> y <strong>UIResponderStandardEditActions</strong> para implementar operaciones de copiar y pegar.</td>
<td align="left">Muchos controles XAML predeterminados ya admiten las operaciones de copiar y pegar. Puedes implementar tú mismo operaciones de copiar y pegar mediante las clases <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx">DataPackage</a></strong> y <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.aspx">Clipboard</a></strong> en <strong><a href="https://msdn.microsoft.com/library/windows/apps/br205967">Windows.ApplicationModel.DataTransfer</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243291.aspx">Copiar y pegar</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Arrastrar y colocar.</strong> <br><br>Arrastra y coloca contenido entre aplicaciones.</td>
<td align="left">La operación de arrastrar y colocar se puede implementar en una sola aplicación mediante el <strong>marco de arrastrar y colocar de Android</strong>.</td>
<td align="left">En iOS no se proporciona ninguna API de alto nivel para arrastrar y colocar.</td>
<td align="left">Puedes implementar la operación de arrastrar y colocar en la aplicación para habilitar funcionalidades de arrastrar y colocar de una aplicación a otra, del escritorio a la aplicación y de la aplicación al escritorio. La compatibilidad con operaciones de arrastrar y colocar se implementa en la clase UIElement mediante las propiedades <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx">AllowDrop</a></strong> y <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx">CanDrag</a></strong>, y los eventos <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx">DragOver</a></strong> y <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx">Drop</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt227651.aspx">Arrastrar y colocar</a></td>
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
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Patrones de diseño de software.</strong> <br><br>Patrones recomendados o comunes para la plataforma.</td>
<td align="left">Para el desarrollo en Android, no se ha recomendado ni proporcionado ningún patrón formal, aunque es posible que la versión beta del marco de enlace de datos pueda permitir un uso más extendido del patrón <strong>Model-View-ViewModel (MVVM)</strong>. En varios artículos y marcos de terceros se recomiendan los métodos <strong>Model-View-Presenter (MVP)</strong> y <strong>MVVM</strong>.</td>
<td align="left">El patrón <strong>Model-View-Controller (MVC)</strong> es un patrón común que se usa con iOS y se integra en la plataforma.</td>
<td align="left">No estás limitado a un patrón específico cuando se compila para UWP.<br/><br/>Puedes usar el patrón integrado de <a href="https://msdn.microsoft.com/library/windows/apps/mt210947.aspx">enlace de datos</a> para garantizar una separación clara entre las preocupaciones de datos y las de la interfaz de usuario, y evitar tener que incluir código en los controladores de eventos de la interfaz de usuario que luego actualizan los valores de las propiedades.<br/><br/>Puedes ampliar el enlace de datos para seguir el patrón <strong>Model-View-ViewModel (MVVM)</strong>, ya sea usando bibliotecas MVVM de terceros, como <a href="https://mvvmlight.codeplex.com/">MVVM Light Toolkit</a>, o implementando tu propia biblioteca y manteniendo la lógica fuera del código subyacente.<br/><br/><a href="https://msdn.microsoft.com/library/hh848246.aspx">The MVVM Pattern</a> (El patrón MVVM)					<br/><br/><a href="https://github.com/Windows-XAML/Template10/wiki">Plantillas de proyecto Template 10 Visual Studio</a></td>
</tr>
</tbody>
</table>
