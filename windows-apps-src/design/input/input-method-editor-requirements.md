---
description: Desarrolle un editor de métodos de entrada (IME) personalizado para ayudar al usuario a escribir texto en un idioma que no se pueda representar fácilmente en un teclado QWERTY estándar.
title: Requisitos del editor de métodos de entrada (IME)
label: Input Method Editor (IME) requirements
template: detail.hbs
keywords: IME, editor de métodos de entrada, entrada, interacción
ms.date: 07/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 74c223aefa525bb6109521c8b91a9a849e2f5586
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030138"
---
# <a name="custom-input-method-editor-ime-requirements"></a>Requisitos del editor de métodos de entrada (IME) personalizados

Estas instrucciones y requisitos pueden ayudarle a desarrollar un editor de métodos de entrada (IME) personalizado para ayudar al usuario a escribir texto en un idioma que no se puede representar fácilmente en un teclado QWERTY estándar.

Para obtener información general sobre los IME, consulte [Editor de métodos de entrada (IME)](input-method-editors.md).

## <a name="default-ime"></a>IME predeterminado

Un usuario puede seleccionar cualquiera de sus IME activos ( **configuración-> tiempo & idioma > Language-> idiomas preferidos-> paquete de idioma-opciones** ) para que sea el IME predeterminado para su idioma preferido.

:::image type="content" source="images/IMEs/ime-preferred-languages.png" alt-text="Configuración de idioma preferido":::

Seleccione el teclado predeterminado en la pantalla de configuración de opciones de idioma para el idioma preferido.

:::image type="content" source="images/IMEs/ime-preferred-languages-keyboard.png" alt-text="Configuración de idioma preferido":::

> [!Important]
> No se recomienda escribir directamente en el registro para establecer el teclado predeterminado para el IME personalizado.

## <a name="compatibility-requirements"></a>Requisitos de compatibilidad

A continuación se indican los requisitos de compatibilidad básicos para un IME personalizado.

### <a name="ime-must-be-compatible-with-windows-apps"></a>El IME debe ser compatible con aplicaciones de Windows

Use el [marco de trabajo de servicios de texto (TSF)](/windows/win32/tsf/text-services-framework) para implementar los IME. Anteriormente, tenía la opción de usar el [Administrador de métodos de entrada (IMM32)](/windows/win32/intl/input-method-manager) para los servicios de entrada. Ahora el sistema bloquea los IME que se implementan mediante el administrador de métodos de entrada (IMM32).

Cuando se inicia una aplicación, TSF carga el archivo DLL de IME para el IME seleccionado actualmente por el usuario. Cuando se carga un IME, está sujeto a las mismas restricciones de contenedor de la aplicación que la aplicación. Por ejemplo, un IME no puede tener acceso a Internet si una aplicación no ha solicitado el acceso a Internet en su manifiesto. Este comportamiento garantiza que los IME no pueden infringir los contratos de seguridad.

TSF es el intermediario entre la aplicación y el IME. TSF comunica los eventos de entrada con el IME y recibe los caracteres de entrada del IME después de que el usuario haya seleccionado un carácter.

Este comportamiento es el mismo que el de las versiones anteriores de Windows, pero la carga en una aplicación de Windows afecta a las posibles capacidades de un IME.

Si el IME necesita proporcionar una funcionalidad o interfaz de usuario diferentes entre las aplicaciones de Windows y las aplicaciones de escritorio, asegúrese de que el archivo DLL que se carga mediante TSF comprueba en qué tipo de aplicación se está cargando. Llame al método [ITfThreadMgrEx:: GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags) en el IME y Compruebe la marca TF_TMF_IMMERSIVEMODE, de modo que el IME desencadene una lógica de aplicación diferente en función del resultado.

Las aplicaciones de Windows no admiten los IME de servicio de texto de tabla (TTS).

> [!NOTE]
> Algunas herramientas para generar los IME de TTS producen IME marcados como malware por Windows.

### <a name="ime-must-be-compatible-with-the-system-tray"></a>El IME debe ser compatible con la bandeja del sistema

No hay ninguna barra de idioma para hospedar los iconos del IME. En su lugar, se muestra un indicador de entrada en la bandeja del sistema que indica la opción de entrada actual. El indicador de entrada solo muestra el icono de personalización de marca del IME para indicar el IME que se está ejecutando actualmente. Además, hay un icono de modo IME que se muestra a la izquierda del icono de personalización de marca del IME para que los usuarios realicen el conmutador de modo IME que se usa con más frecuencia, como activar o desactivar el IME.

El indicador de entrada muestra el icono de personalización de marca del IME y el icono de modo solo para los IME compatibles. Los IME que no son compatibles no tienen el icono de personalización de marca y el icono de modo mostrados en la bandeja del sistema. En su lugar, el indicador de entrada muestra la abreviatura del idioma en lugar del icono de personalización de marca del IME.

Almacene los iconos del IME en un archivo DLL o EXE, en lugar de un archivo. ico independiente. El diseño de los iconos del IME debe seguir las instrucciones descritas en la siguiente sección pautas de diseño de la interfaz de usuario.

### <a name="ime-branding-icon"></a>Icono de personalización de marca IME

El indicador de entrada obtiene el icono de personalización de marca del IME del archivo DLL del IME mediante el identificador de recurso definido por el IME cuando se registró en el sistema.

### <a name="ime-mode-icon"></a>Icono de modo IME

Es posible que algunos IME deban basarse en el indicador de entrada que se muestra en la bandeja del sistema para mostrar el icono del modo IME. En este caso, el IME pasa el icono del modo IME al indicador de entrada mediante GUID_LBI_INPUTMODE.

Al pasar los iconos del modo IME al indicador de entrada en la bandeja del sistema, el tamaño predeterminado del icono del modo IME es de 16 x 16 píxeles. El escalado de la interfaz de usuario sigue a DPI.

Al pasar el icono del modo IME al indicador de entrada en UAC (control de cuentas de usuario en el escritorio seguro), el tamaño predeterminado del icono del modo IME es 20x20 píxeles. El icono de escala de la interfaz de usuario para el modo IME en UAC sigue a PPI.

## <a name="ime-must-work-in-app-container"></a>El IME debe funcionar en el contenedor de la aplicación

Algunas funciones de IME se ven afectadas en un contenedor de la aplicación.

- **Archivos de diccionario** : con frecuencia, los IME tienen archivos de Diccionario de solo lectura para asignar la entrada del usuario a caracteres específicos. Para tener acceso a estos archivos desde dentro de un contenedor de la aplicación, el IME debe colocarlos en los directorios de Windows o los archivos de programa. De forma predeterminada, estos directorios se pueden leer desde un contenedor de la aplicación, por lo que los IME pueden acceder a los archivos de diccionario que se almacenan en estas ubicaciones. Si el IME debe almacenar el archivo de diccionario en otro lugar, debe manipular explícitamente las [listas de Access Control (ACL)](/windows/win32/secauthz/access-control-lists) de los archivos de diccionario para permitir el acceso desde los contenedores de la aplicación.
- **Actualización de Internet** : Si el IME necesita actualizar sus diccionarios con datos de Internet, no puede hacerlo de manera confiable dentro de un contenedor de la aplicación, ya que no siempre se permite el acceso a Internet. En su lugar, el IME debe ejecutar un proceso de escritorio independiente que sea responsable de actualizar los archivos de diccionario con datos de Internet.
- **Aprendizaje sobre la marcha** : Si un IME se está ejecutando en un contenedor de la aplicación que tiene acceso a Internet, no hay ninguna restricción en los puntos de conexión con los que el IME puede comunicarse. En este caso, un IME puede usar un servidor en la nube para proporcionar servicios de aprendizaje sobre la marcha. Algunos IMEs descargan y cargan los datos proporcionados por el usuario sobre la marcha, mientras el usuario escribe. Dado que el acceso a Internet no está garantizado en un contenedor de la aplicación, es posible que no siempre se permita.
- **Compartir información entre procesos** : los IME pueden necesitar compartir datos sobre las preferencias de entrada del usuario entre las aplicaciones que se encuentran en contenedores de aplicaciones diferentes. Use un servicio web para compartir datos entre aplicaciones.

> [!Important]
> Si intenta eludir las reglas de seguridad del contenedor de la aplicación, el IME puede tratarse como malware y bloquearse.

## <a name="ime-and-touch-keyboard"></a>Teclado táctil y IME

El IME debe asegurarse de que la interfaz de usuario del panel candidato y otros elementos de la interfaz de usuario no se dibujen debajo del teclado táctil. El teclado táctil se muestra en una banda de orden z superior a todas las aplicaciones y la interfaz de usuario del IME se muestra en la misma banda de orden z que la aplicación en la que está activa. Como resultado, el teclado táctil puede superponerse y ocultar la interfaz de usuario del IME. En la mayoría de los casos, la aplicación debe cambiar el tamaño de su ventana para que tenga en cuenta el teclado táctil. Si una aplicación no cambia de tamaño, el IME todavía puede usar la API [InputPane](/windows/win32/api/shobjidl_core/nn-shobjidl_core-iframeworkinputpane) para obtener la posición del teclado táctil. El IME consulta la propiedad [Location](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iframeworkinputpane-location) o registra un controlador para los eventos de mostrar y ocultar del teclado táctil. El evento show se genera cada vez que el usuario pulsa en un campo de edición, incluso si se muestra el teclado táctil actualmente. El IME puede usar esta API para obtener el espacio de pantalla que usa el teclado táctil antes de que el IME dibuje la interfaz de usuario candidata (u otra), y para redistribuir la interfaz de usuario de los IME para evitar dibujar debajo del teclado táctil.

### <a name="specifying-the-preferred-touch-keyboard-layout"></a>Especificación de la distribución de teclado táctil preferida

El IME puede especificar la distribución del teclado táctil que se va a usar y el IME está habilitado para trabajar con diseños con optimización táctil. Esta funcionalidad se limita a los IME para los idiomas de entrada Coreano, Japonés, Chino simplificado y chino tradicional.

El teclado táctil admite siete diseños, tres de los cuales son diseños clásicos y cuatro de los cuales son diseños optimizados para Touch. Los diseños clásicos tienen el aspecto y se comportan como un teclado físico.

Todos los tres diseños clásicos son para la entrada de chino tradicional en formas diferentes:

- Entrada basada en fonética
- Entrada changjie
- Entrada dayi

Además de los diseños clásicos, hay un diseño optimizado para toque para cada uno de los idiomas de entrada en Coreano, Japonés, Chino simplificado y chino tradicional.

Para usar esta funcionalidad, el IME debe implementar la interfaz [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout) , que se exporta mediante el IME mediante el uso de la API [ITfFunctionProvider](/windows/win32/api/msctf/nn-msctf-itffunctionprovider) de Text Services Framework.

Si el IME no es compatible con la interfaz ITfFnGetPreferredTouchKeyboardLayout, el uso del IME da como resultado el diseño clásico predeterminado para el idioma que se muestra en el teclado táctil.

Si el IME necesita establecer uno de los diseños clásicos como diseño preferido, no se requiere ningún trabajo adicional en el lado del IME más allá de la compatibilidad con las interfaces ITfFnGetPreferredTouchKeyboardLayout y ITfFunctionProvider. Pero se requiere trabajo adicional en el IME para trabajar con los diseños optimizados para Touch y esto se describe en la sección siguiente.

### <a name="touch-optimized-layout"></a>Diseño con optimización táctil

Los teclados táctiles táctiles para los idiomas de entrada Coreano, Japonés, Chino simplificado y chino tradicional muestran un diseño diferente para los modos de conversión IME en y IME. Hay una tecla en el teclado táctil para establecer el modo de conversión de IME en activado o desactivado, pero el modo IME del teclado también puede cambiar a medida que cambia el foco entre los controles de edición.

Los teclados con optimización táctil para los idiomas de entrada japonés, Chino simplificado y chino tradicional contienen una clave, o claves, que el IME usa para navegar por las páginas candidatas. En el caso de japonés y chino simplificado, la clave de página candidata se muestra en el diseño con optimización táctil. En el caso de chino tradicional, existen claves independientes para las páginas candidatas anteriores y siguientes.

Cuando se presionan estas teclas, el teclado táctil llama a la función [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput) para enviar los siguientes caracteres de área de uso privado de Unicode a la aplicación con el foco, que el IME puede interceptar y actuar sobre:

- **Página siguiente (0xF003)** : se envía cuando se presiona la tecla de página candidata en el teclado táctil optimizado para japonés y chino simplificado, o cuando se presiona la tecla de página siguiente en el teclado táctil optimizado para chino tradicional.
- **Página anterior (0xF004)** : se envía cuando se presiona la tecla de página candidata al mismo tiempo que la tecla Mayús en el teclado táctil optimizado para japonés y chino simplificado, o cuando se presiona la tecla de página anterior en el teclado táctil optimizado para chino tradicional.

Estos caracteres se envían como entrada Unicode. En el siguiente párrafo se detalla cómo extraer la información de caracteres durante las notificaciones de receptor de eventos clave que recibirá el IME de servicios de texto. Estos valores de caracteres no se definen en ningún archivo de encabezado, por lo que deberá definirlos en el código.

Para interceptar la entrada de teclado, el IME debe registrarse como receptor de eventos de clave. Para la entrada Unicode que se genera mediante la función SendInput, el parámetro WPARAM de las devoluciones de llamada [ITfKeyEventSink](/windows/win32/api/msctf/nn-msctf-itfkeyeventsink) (onkeydown, onkeyup, OnTestKeyDown, OnTestKeyUp) siempre contiene la clave virtual VK_PACKET y no identifica el carácter directamente.

Implemente la siguiente secuencia de llamada para tener acceso al carácter:

```cpp
// Keyboard state
BYTE abKbdState[256];
if (!GetKeyboardState(abKbdState))
{
   return 0;
}

// Map virtual key to character code
WCHAR wch;
if (ToUnicode(VK_PACKET, 0, abKbdState, &wch, 1, 0) == 1)
{
   return wch;
}
```

## <a name="ime-search-integration"></a>Integración de la búsqueda de IME

Proporcione a los usuarios características de búsqueda a través del contrato de búsqueda y la integración con el panel de búsqueda.

:::image type="content" source="images/IMEs/ime-search-pane.png" alt-text="Configuración de idioma preferido" para el servicio que proporciona los resultados. Por motivos de rendimiento, las aplicaciones a menudo limitan la coincidencia entre 5 y 20 de las alternativas más relevantes.

## <a name="ui-design-guidelines"></a>Directrices de diseño de la interfaz de usuario

Todos los IME deben seguir las instrucciones de experiencia del usuario que se describen en [diseñar y codificar aplicaciones de Windows](../index.md).

### <a name="dont-use-sticky-windows"></a>No use ventanas rápidas

Las ventanas del IME deben aparecer solo cuando sea necesario y no deben ser visibles en todo momento. Cuando no es necesario que los usuarios escriban, las ventanas IME no deberían mostrarse. La ventana del IME no debe ser una ventana de pantalla completa. Las ventanas IME no deben superponerse entre sí. Las ventanas deben diseñarse en un estilo de Windows y seguir el escalado de la interfaz de usuario.

### <a name="ime-icons"></a>Iconos IME

Hay dos tipos de iconos IME: iconos de personalización de marca e iconos de modo. Todos los iconos del IME deben diseñarse solo con colores negros y blancos. Los nuevos iconos del IME prestados por la apariencia de glifo de los iconos de la bandeja del sistema. Este estilo se ha creado para que todos los lenguajes puedan usarlo para complementar la apariencia de familial mientras también se diferencian entre sí.

El formato de archivo para los iconos IME es ICO. Debe proporcionar los siguientes tamaños de icono.

- 16x16 píxeles
- 20x20 píxeles
- 24x24 píxeles
- 32x32 píxeles
- 40 x 40 píxeles
- 48x48 píxeles

Asegúrese de que los iconos de 32 bits con canal alfa se proporcionan en todas las resoluciones.

Los iconos de la marca IME se definen mediante un cuadro blanco en el que se coloca un glifo tipográfico representado en un tipo de letra moderno. Cada equipo de lenguaje elige cada glifo de definición. El glifo es negro. El cuadro incluye un trazo exterior de 1 píxel en negro con una opacidad del 50%. Las versiones "nuevas" se definen mediante una esquina redondeada en la parte superior izquierda del cuadro.

Los iconos del modo IME se definen mediante un glifo tipográfico blanco en un tipo de letra moderno que incluye un trazo exterior de 1 píxel en negro con una opacidad del 50%.

| Icono | Descripción |
| --- | --- |
| :::image type="content" source="images/IMEs/ime-brand-icon-traditional-chinese.png" alt-text="Configuración de idioma preferido"::: | Ejemplo de icono de marca IME para chino tradicional ChangeJie. |
| :::image type="content" source="images/IMEs/ime-brand-icon-traditional-chinese-new.png" alt-text="Configuración de idioma preferido"::: | Ejemplo de icono de marca IME para chino tradicional ChangeJie. |
| :::image type="content" source="images/IMEs/ime-mode-icon-chinese.png" alt-text="Configuración de idioma preferido"::: | Icono de modo IME de ejemplo. |

### <a name="owned-window"></a>Ventana propiedad

Para mostrar la interfaz de usuario candidata, un IME debe establecer su ventana como ventana de propiedad, de modo que se pueda mostrar sobre la aplicación que se está ejecutando actualmente. Use el método [ITfContextView:: GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd) para recuperar la ventana a la que pertenece. Si GetWnd devuelve un error o un NULLHWND, llame a la función [GetFocus](/windows/win32/api/msctf/nf-msctf-itfthreadmgr-getfocus) .

`if (FAILED(pView->GetWnd(&parentWndHandle)) || (parentWndHandle == nullptr)) { parentWndHandle = GetFocus(); }`

### <a name="ime-candidate-window-interaction-with-light-dismiss-surfaces"></a>Interacción de la ventana de candidato de IME con superficies de descartado claro

El modelo descartable para ventanas emergentes se denomina "descartar luz" porque es fácil para un usuario cerrar dichas ventanas. Para que los IME funcionen bien en el modelo de interacción de Windows, las ventanas del IME deben participar en el modelo de descartación ligera.

Para participar en el modelo de descartado de luz, el IME debe generar tres nuevos eventos de Windows mediante la función [NotifyWinEvent](/windows/win32/api/winuser/nf-winuser-notifywinevent) o una función similar. Estos nuevos eventos son:

- **EVENT_OBJECT_IME_SHOW** : genera este evento cuando el IME se vuelve visible.
- **EVENT_OBJECT_IME_HIDE** : genera este evento cuando el IME está oculto.
- **EVENT_OBJECT_IME_CHANGE** : genera este evento cuando el IME se mueve o cambia de tamaño.

### <a name="declaring-compatibility"></a>Declarar compatibilidad

Los IME declaran que son compatibles registrando la categoría GUID_TFCAT_TIPCAP_IMMERSIVESUPPORT para su IME mediante [ITfCategoryMgr:: RegisterCategory](/windows/win32/api/msctf/nf-msctf-itfcategorymgr-registercategory).

### <a name="set-the-default-ime-mode-to-on"></a>Establecer el modo IME predeterminado en activado

Proporcionamos una mejor experiencia de usuario para los IME.

## <a name="dpi-scaling-support-for-desktop-applications"></a>Compatibilidad de escala de PPP para aplicaciones de escritorio

La compatibilidad mejorada con el escalado de PPP permite consultar el nivel de reconocimiento de PPP declarado de cada proceso del escritorio para determinar si es necesario escalar la interfaz de usuario. En un escenario de varios monitores, Windows escala la interfaz de usuario de forma adecuada para diferentes configuraciones de PPP en cada monitor.

Dado que el IME se ejecuta en el contexto del proceso de cada aplicación, no debe declarar un nivel de reconocimiento de PPP para el IME. Esto garantiza que el IME se ejecute en el nivel de reconocimiento de PPP del proceso actual.

Para asegurarse de que todos los elementos de la interfaz de usuario de IME tienen paridad de escalado con los elementos de la interfaz de usuario del proceso en el que se ejecuta, debe responder adecuadamente a distintos valores de PPP.

> [!NOTE]
> Para garantizar la paridad con nuevas aplicaciones de escritorio, el IME debe admitir el reconocimiento de PPP por monitor, pero no debe declarar un nivel de conocimiento. El sistema determina los requisitos de escalado adecuados en cada escenario.

Para más información sobre los requisitos de compatibilidad de escala de PPP para las aplicaciones de escritorio, consulte [alta resolución de PPP](/windows/win32/hidpi/high-dpi-desktop-application-development-on-windows).

## <a name="ime-installation"></a>Instalación de IME

Si compila el IME mediante Microsoft Visual Studio, cree una experiencia de instalación para el IME mediante el uso de un instalador de terceros, como InstallShield de Flexera software.

En los pasos siguientes se muestra cómo usar InstallShield para crear un proyecto de instalación para el archivo DLL de IME.

- Instale Visual Studio.
- Inicie Visual Studio.
- En el menú **archivo** , elija **nuevo** y seleccione **proyecto** . Se abre el cuadro de diálogo **Nuevo proyecto** .
- En el panel izquierdo, vaya a **plantillas > otros tipos de proyectos > instalación e implementación** , haga clic en **Habilitar InstallShield Limited Edition** y, a continuación, haga clic en **Aceptar** . Siga las instrucciones de instalación.
- Reinicie Visual Studio.
- Abra el archivo de solución de IME (. sln).
- En Explorador de soluciones, haga clic con el botón secundario en la solución, seleccione **Agregar** y, a continuación, seleccione **nuevo proyecto** . Se abre el cuadro de diálogo **Agregar nuevo proyecto** .
- En el control de vista de árbol izquierdo, vaya a **plantillas > otros tipos de proyectos > InstallShield Limited Edition** .
- En la ventana central, haga clic en **proyecto de InstallShield Limited Edition** .
- En el cuadro de texto **nombre** , escriba "SetupIME" y haga clic en **Aceptar** .
- En el cuadro de diálogo **Asistente para proyectos** , haga clic en información de la **aplicación** .
- Rellene el nombre de la empresa y los demás campos.
- Haga clic en **archivos de aplicación** .
- En el panel izquierdo, haga clic con el botón secundario en la carpeta **[INSTALLDIR]** y seleccione **nueva carpeta** . Asigne a la carpeta el nombre "plugins".
- Haga clic en **Agregar archivos** . Desplácese hasta el archivo DLL de IME y agréguelo a la carpeta **plugins** . Repita este paso para el Diccionario IME.
- Haga clic con el botón secundario en el archivo DLL de IME y seleccione **propiedades** . Se abrirá el cuadro de diálogo **propiedades** .
- En el cuadro de diálogo **propiedades** , haga clic en la pestaña **configuración de com & .net** .
- En **tipo de registro** , seleccione **auto-registro** y haga clic en **Aceptar** .
- Compile la solución. La DLL de IME se compila y InstallShield crea un archivo setup.exe que permite a los usuarios instalar el IME en Windows.

Para crear su propia experiencia de instalación, llame al método [ITfInputProcessorProfileMgr:: RegisterProfile](/windows/win32/api/msctf/nf-msctf-itfinputprocessorprofilemgr-registerprofile) para registrar el IME durante la instalación. No escriba directamente entradas del registro.

Si el IME debe ser utilizable inmediatamente después de la instalación, llame a [InstallLayoutOrTip](/windows/win32/tsf/installlayoutortip) para agregar el IME a los métodos de entrada habilitados para el usuario, con el siguiente formato para el parámetro PSZ:

`<LangID 1>:{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}`

## <a name="ime-accessibility"></a>Accesibilidad de IME

Implemente la Convención siguiente para que sus IME cumplan los requisitos de accesibilidad y para trabajar con el narrador. Para que se pueda acceder a las listas de candidatos, los IME deben seguir esta Convención.

- La lista de candidatos debe tener un **UIA_AutomationIdPropertyId** igual a "IME_Candidate_Window" para las listas de candidatos de conversión o "IME_Prediction_Window" para las listas de candidatos de predicción.
- Cuando aparece y desaparece la lista de candidatos, genera eventos de tipo **UIA_MenuOpenedEventId** y **UIA_MenuClosedEventId** , respectivamente.
- Cuando cambia el candidato seleccionado actualmente, la lista de candidatos genera una **UIA_SelectionItem_ElementSelectedEventId** . El elemento seleccionado debe tener una propiedad **UIA_SelectionItemIsSelectedPropertyId** igual a **true** .
- El **UIA_NamePropertyId** de cada elemento de la lista de candidatos debe ser el nombre del candidato. Opcionalmente, puede proporcionar información adicional para eliminar la ambigüedad de los candidatos a través de **UIA_HelpTextPropertyId** .

## <a name="related-topics"></a>Temas relacionados

- [Editor de métodos de entrada (IME)](input-method-editors.md)
- [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout)
- [ITfCompartmentEventSink](/windows/win32/api/msctf/nn-msctf-itfcompartmenteventsink)
- [ITfThreadMgrEx::GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags)
- [ITfContextView::GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)
- [TF_INPUTPROCESSORPROFILE](/windows/win32/api/msctf/ns-msctf-tf_inputprocessorprofile)
- [ITfFnSearchCandidateProvider](/windows/win32/api/ctffunc/nn-ctffunc-itffnsearchcandidateprovider)
- [ITfIntegratableCandidateListUIElement](/windows/win32/api/ctffunc/nn-ctffunc-itfintegratablecandidatelistuielement)
- [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput)
- [Accesibilidad](../accessibility/accessibility.md)
