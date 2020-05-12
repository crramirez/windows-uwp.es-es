---
Description: Los cuadros de diálogo y los controles flotantes muestran elementos transitorios de la interfaz de usuario que aparecen cuando el usuario los solicita o cuando sucede algo que requiere notificación o aprobación.
title: Controles de cuadro de diálogo
label: Dialogs
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9208bb85f34e1ad0fd28a89e780ebfca536a8bad
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82969900"
---
# <a name="dialog-controls"></a>Controles de cuadro de diálogo

Los cuadros de diálogo son superposiciones modales en la interfaz de usuario que proporcionan información contextual sobre la aplicación. Los cuadros de diálogo bloquean las interacciones con la ventana de la aplicación hasta que se descarten de forma explícita. A menudo solicitan algún tipo de acción por parte del usuario.

![Ejemplo de un cuadro de diálogo](../images/dialogs/dialog_RS2_delete_file.png)

**Obtención de la biblioteca de la interfaz de usuario de Windows**

|  |  |
| - | - |
| ![Logotipo de WinUI](../images/winui-logo-64x64.png) | La biblioteca de interfaz de usuario de Windows 2.2 o posterior incluye una nueva plantilla para este control que usa esquinas redondeadas. Para obtener más información, consulta [Radio de redondeo](/windows/uwp/design/style/rounded-corner). WinUI es un paquete NuGet que contiene nuevas características de interfaz de usuario y controles para aplicaciones de Windows. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows). |

> **API de plataforma:** [Clase ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Use los cuadros de diálogo para notificar a los usuarios información importante o para solicitar información adicional o confirmación para completar una acción.

Para obtener recomendaciones sobre cuándo usar un control flotante frente a cuándo usar un cuadro de diálogo (un control similar), vea [Cuadros de diálogo y controles flotantes](index.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="../images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para abrirla y ver <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> o <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> en acción.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="general-guidelines"></a>Directrices generales

-   Identifica claramente el problema o el objetivo del usuario en la primera línea del texto del cuadro de diálogo.
-   El título del cuadro de diálogo es la instrucción principal y es opcional.
    -   Usa un título corto para explicar lo que se necesita hacer con el cuadro de diálogo.
    -   Puedes omitir el título si vas a usar el cuadro de diálogo para transmitir un mensaje sencillo, un error o una pregunta. Deja que el texto del contenido transmita esta información esencial.
    -   Asegúrate de que el título esté relacionado directamente con las opciones de botón.
-   El contenido del cuadro de diálogo contiene el texto descriptivo y es obligatorio.
    -   Presenta el mensaje, el error o la pregunta de bloqueo de la manera más sencilla posible.
    -   Si se usa un cuadro de diálogo, usa el área de contenido para proporcionar más detalles o definir terminología. No repitas el título con otras palabras ligeramente distintas.
-   Al menos debe aparecer un botón de cuadro de diálogo.
    -   Asegúrese de que el cuadro de diálogo tiene al menos un botón correspondiente a una acción segura y no destructiva como "Entendido", "Cerrar" o "Cancelar". Use la API de CloseButton para agregar este botón.
    -   Use respuestas específicas al contenido o a la instrucción principal como texto del botón. Un ejemplo sería: "¿Quieres permitir que nombreDeAplicación acceda a tu ubicación?", seguido de los botones "Permitir" y "Bloquear". Cuando las respuestas son específicas, se comprenden con mayor rapidez y la toma de decisiones resulta eficaz.
    - Asegúrese de que el texto de los botones de acción sea conciso. Las cadenas cortas permiten al usuario realizar una selección de manera rápida y segura.
    - Además de la acción segura y no destructiva, opcionalmente podrá presentar al usuario uno o dos botones de acción relacionados con la instrucción principal. Estos botones de acción "hacerlo" confirman el principal punto del cuadro de diálogo. Use las API de PrimaryButton y SecondaryButton APIs para agregar estas acciones "hacerlo".
    - Los botones de acción "hacerlo" deberían aparecer a la izquierda del resto. La acción segura y no destructiva debe aparecer como el botón situado más a la derecha.
    - Opcionalmente, puede elegir diferenciar uno de los tres botones como el botón predeterminado del cuadro de diálogo. Use la API de DefaultButton para diferenciar uno de los botones.
-   No uses cuadros de diálogo en el caso de los errores que son contextuales para un lugar específico de la página, como los errores de validación (en los campos de contraseña, por ejemplo); usa el lienzo de la aplicación para mostrar errores en línea.
- Use la [clase ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) para compilar la experiencia de cuadro de diálogo. No use la API de MessageDialog en desuso.

## <a name="how-to-create-a-dialog"></a>Creación de un cuadro de diálogo
Para crear un cuadro de diálogo, usa la [clase ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog). Puedes crear un cuadro de diálogo en el código o el marcado. Aunque suele ser más fácil definir los elementos de la interfaz de usuario en XAML, en el caso de un cuadro de diálogo simple, es más sencillo usar código solamente. En este ejemplo se crea un cuadro de diálogo para notificar al usuario que no hay conexión Wi-Fi y luego se usa el método [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) para mostrarlo.

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

Cuando el usuario hace clic en un botón de cuadro de diálogo, el método [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) devuelve una enumeración [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) para que sepas en qué botón hace clic el usuario.

El cuadro de diálogo de este ejemplo formula una pregunta y usa el valor devuelto [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) para determinar la respuesta del usuario.

```csharp
private async void DisplayDeleteFileDialog()
{
    ContentDialog deleteFileDialog = new ContentDialog
    {
        Title = "Delete file permanently?",
        Content = "If you delete this file, you won't be able to recover it. Do you want to delete it?",
        PrimaryButtonText = "Delete",
        CloseButtonText = "Cancel"
    };

    ContentDialogResult result = await deleteFileDialog.ShowAsync();

    // Delete the file if the user clicked the primary button.
    /// Otherwise, do nothing.
    if (result == ContentDialogResult.Primary)
    {
        // Delete the file.
    }
    else
    {
        // The user clicked the CLoseButton, pressed ESC, Gamepad B, or the system back button.
        // Do nothing.
    }
}
```

## <a name="provide-a-safe-action"></a>Proporcione una acción de prueba de errores
Dado que los cuadros de diálogo bloquean la interacción del usuario, y que los botones son el mecanismo principal para que los usuarios descarten el cuadro de diálogo, asegúrese de que el diálogo contiene al menos un botón "seguro" y un botón no destructivo como "Cerrar" o "Entendido". **Todos los cuadros de diálogo deben contener al menos un botón de acción segura para cerrar el cuadro de diálogo.** Esto garantiza que el usuario puede cerrar con confianza el cuadro de diálogo sin realizar una acción.<br>![Un cuadro de diálogo con un botón](../images/dialogs/dialog_RS2_one_button.png)

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

Cuando los cuadros de diálogo se usan para mostrar una pregunta de bloqueo, su cuadro de diálogo debe presentar al usuario los botones de acción relacionados con la pregunta. El botón "seguro" y no destructivo puede ir acompañado de uno o dos botones de acción "hacerlo". Cuando se le presenten al usuario varias opciones, asegúrate de que los botones explican con claridad las acciones "hacerlo" y segura o "no hacerlo" relacionadas con la pregunta propuesta.

![Un cuadro de diálogo con dos botones](../images/dialogs/dialog_RS2_two_button.png)

```csharp
private async void DisplayLocationPromptDialog()
{
    ContentDialog locationPromptDialog = new ContentDialog
    {
        Title = "Allow AppName to access your location?",
        Content = "AppName uses this information to help you find places, connect with friends, and more.",
        CloseButtonText = "Block",
        PrimaryButtonText = "Allow"
    };

    ContentDialogResult result = await locationPromptDialog.ShowAsync();
}
```

Los cuadros de diálogo de tres botones se usan cuando le presentan al usuario dos acciones "hacerlo" y una acción "no hacerlo". Se deben usar cuadros de diálogo de tres botones con moderación con distinciones claras entre la acción secundaria y la acción seguro/cerrar.

![Cuadro de diálogo de tres botones](../images/dialogs/dialog_RS2_three_button.png)

```csharp
private async void DisplaySubscribeDialog()
{
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it"
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

## <a name="the-three-dialog-buttons"></a>Los tres botones de cuadro de diálogo
ContentDialog cuenta con tres tipos diferentes de botones que puede usar para compilar una experiencia de cuadro de diálogo.

- **CloseButton** (obligatorio): representa la acción no destructiva y segura que permite al usuario salir del cuadro de diálogo. Aparece como el botón que se encuentra situado más a la derecha.
- **PrimaryButton** (opcional): representa la primera acción "hacerlo". Aparece como el botón que se encuentra situado más a la izquierda.
- **SecondaryButton** (opcional): representa la segunda acción "hacerlo". Aparece como el botón central.

Con los botones integrados, los botones se colocarán correctamente; asegúrese de que responden correctamente a eventos de teclado, de que el área de comandos permanece visible incluso cuando el teclado en pantalla está activado y de que el aspecto del cuadro de diálogo es coherente con otros cuadros de diálogo.

### <a name="closebutton"></a>CloseButton
Cada cuadro de diálogo debe contener un botón de acción segura y no destructiva que permita al usuario salir del cuadro de diálogo con confianza.

Use la API de ContentDialog.CloseButton para crear este botón. Esto le permite crear la experiencia de usuario adecuada para todas las entradas como el mouse, el teclado, las funciones táctiles y el controlador para juegos. Esta experiencia tendrá lugar cuando:
<ol>
    <li>El usuario haga clic o pulse en el CloseButton </li>
    <li>El usuario presione el botón del sistema Atrás </li>
    <li>El usuario presione la tecla ESC del teclado </li>
    <li>El usuario presione el botón B del controlador para juegos </li>
</ol>

Cuando el usuario hace clic en un botón de cuadro de diálogo, el método [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) devuelve una enumeración [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) para que sepas en qué botón hace clic el usuario. Si se presiona en CloseButton se devuelve ContentDialogResult.None.

### <a name="primarybutton-and-secondarybutton"></a>PrimaryButton y SecondaryButton
Además del CloseButton, opcionalmente puede presentar al usuario uno o dos botones de acción relacionados con la instrucción principal.
Aproveche el PrimaryButton para la primera acción "hacerlo" y el SecondaryButton para la segunda acción "hacerlo". En los cuadros de diálogo de tres botones, el PrimaryButton suele representar la acción afirmativa "hacerlo", mientras que el SecondaryButton suele representar una acción "hacerlo" neutra o secundaria.
Por ejemplo, una aplicación puede avisar al usuario que se suscriba a un servicio. El PrimaryButton como la acción "hacerlo" afirmativa hospedaría el texto Suscribirse mientras que el SecondaryButton como la acción "hacerlo" hospedaría el texto Pruébalo. El CloseButton permitiría al usuario cancelar sin llevar a cabo cualquiera de las acciones.

Cuando el usuario hace clic en el PrimaryButton, el método [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) devuelve ContentDialogResult.Primary.
Cuando el usuario hace clic en el SecondaryButton, el método [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) devuelve ContentDialogResult.Secondary.

![Cuadro de diálogo de tres botones](../images/dialogs/dialog_RS2_three_button.png)

### <a name="defaultbutton"></a>DefaultButton
Opcionalmente, puede elegir diferenciar uno de los tres botones como el botón predeterminado. Especificar el botón predeterminado hace que se produzca lo siguiente:
- El botón recibe el tratamiento visual del botón de énfasis
- El botón responderá a la tecla ENTRAR automáticamente
    - Cuando el usuario presiona la tecla ENTRAR del teclado, se activará el controlador de clic asociado con el botón predeterminado y el ContentDialogResult devolverá el valor asociado con el botón predeterminado
    - Si el usuario ha situado el foco del teclado en un control que controla ENTRAR, el botón predeterminado no responderá al presionar ENTRAR
- El botón recibirá el foco automáticamente cuando se abra el cuadro de diálogo, a menos que el contenido del cuadro de diálogo contenga un elemento de interfaz de usuario activable.

Use la propiedad ContentDialog.DefaultButton para indicar el botón predeterminado. De manera predeterminada, no se establece ningún botón predeterminado.

![Un cuadro de diálogo de tres botones con un botón predeterminado](../images/dialogs/dialog_RS2_three_button_default.png)

```csharp
private async void DisplaySubscribeDialog()
{
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it",
        DefaultButton = ContentDialogButton.Primary
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

## <a name="confirmation-dialogs-okcancel"></a>Cuadros de diálogo de confirmación (Aceptar/Cancelar)
Un cuadro de diálogo de confirmación ofrece a los usuarios la posibilidad de confirmar que desean realizar una acción. Pueden confirman la acción o cancelarla.
Un cuadro de diálogo de confirmación típico tiene dos botones: un botón de afirmación ("Aceptar") y un botón de cancelación.

<ul>
    <li>
        <p>En general, el botón afirmación debe estar a la izquierda (el botón primario) y el botón de cancelación (el botón secundario) debe estar en la derecha.</p>
        <img alt="An OK/cancel dialog" src="../images/dialogs/dialog_RS2_delete_file.png" />
    </li>
    <li>Como se explicó en la sección de recomendaciones generales, usa botones con texto que identifique respuestas específicas a la instrucción principal o al contenido.
    </li>
</ul>

> Algunas plataformas colocan el botón de afirmación a la derecha en lugar de a la izquierda. Entonces, ¿por qué recomendamos colocarlo a la izquierda?  Si supones que la mayoría de los usuarios son diestros y sujetan su teléfono con esa mano, realmente resulta más cómodo presionar el botón de afirmación cuando está a la izquierda, porque es más probable que el botón esté dentro del arco del pulgar del usuario. Los botones situados en el lado derecho de la pantalla requerirán que el usuario cambie la posición de su pulgar hacia dentro a una posición menos cómoda.

## <a name="contentdialog-in-appwindow-or-xaml-islands"></a>ContentDialog en AppWindow o islas de Xaml

> NOTA: Esta sección es aplicable a las aplicaciones diseñadas para Windows 10, versión 1903 o posterior. Las Islas de XAML y AppWindow no están disponibles en versiones anteriores. Para obtener más información acerca de las versiones, consulte el [aplicaciones de versión adaptable](../../../debug-test-perf/version-adaptive-apps.md).

De manera predeterminada, los cuadros de diálogo muestran de forma modal con respecto a la raíz [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview). Cuando use ContentDialog dentro de cualquiera isla [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) o [XAML](/windows/apps/desktop/modernize/xaml-islands), deberá establecer manualmente la [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) en el cuadro de diálogo a la raíz del host XAML.

Para ello, establezca la propiedad de XamlRoot del ContentDialog a la misma XamlRoot como un elemento que ya está en el AppWindow o isla de XAML, como se muestra aquí.

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    // Use this code to associate the dialog to the appropriate AppWindow by setting
    // the dialog's XamlRoot to the same XamlRoot as an element that is already present in the AppWindow.
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
    {
        noWifiDialog.XamlRoot = elementAlreadyInMyAppWindow.XamlRoot;
    }

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

> [!WARNING]
> Solo puede haber uno ContentDialog abierto en cada subproceso a la vez. Al intentar abrir dos ContentDialogs producirá una excepción, incluso si está intentando abrir en AppWindows independiente.

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados
- [Información sobre herramientas](../tooltips.md)
- [Menús y menú contextual](../menus.md)
- [Clase Flyout](/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [Clase ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)
