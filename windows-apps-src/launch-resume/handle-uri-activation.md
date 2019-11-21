---
title: Controlar la activación de URI
description: Aprende a registrar una aplicación para convertirla en el controlador predeterminado de un nombre de esquema de identificador uniforme de recursos (URI).
ms.assetid: 92D06F3E-C8F3-42E0-A476-7E94FD14B2BE
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7e4124cd347b4fda3716fcfcdd9c51717fcd0fc6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260472"
---
# <a name="handle-uri-activation"></a>Controlar la activación de URI

**API importantes**

-   [**Windows. ApplicationModel. Activation. ProtocolActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs)
-   [**Windows. UI. Xaml. Application. Onactivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated)

Aprende a registrar una aplicación para convertirla en el controlador predeterminado de un nombre de esquema de identificador uniforme de recursos (URI). Tanto las aplicaciones de escritorio de Windows como las aplicaciones de la Plataforma universal de Windows (UWP) pueden registrarse para convertirse en el controlador predeterminado para un nombre de esquema URI. Si el usuario elige tu aplicación como el controlador predeterminado de un nombre de esquema de URI, esta se activará cada vez que ese tipo de URI se inicie.

Te recomendamos que la registres para un nombre de esquema de URI solo si esperas controlar todos los inicios de URI de ese tipo de esquema de URI. Si, en efecto, eliges registrarla para un nombre de esquema de URI, deberás proporcionar al usuario final todas las funciones que se esperan cuando tu aplicación se active para ese esquema de URI. Por ejemplo, una aplicación registrada para el nombre de esquema de URI mailto: debe abrirse en un nuevo mensaje de correo electrónico para que el usuario pueda redactarlo. Para obtener más información sobre las asociaciones de URI, consulta [Directrices y lista de comprobación de tipos de archivo y URI](https://docs.microsoft.com/windows/uwp/files/index).

En estos pasos se muestra cómo registrar un nombre de esquema de URI personalizado, `alsdk://`, y cómo activar la aplicación cuando el usuario inicia un URI `alsdk://`.

> [!NOTE]
> En las aplicaciones UWP, ciertos URI y extensiones de archivo se reservan para su uso por parte de las aplicaciones integradas y el sistema operativo. Se ignorarán los intentos de registrar aplicaciones con una extensión de archivo o URI reservada. Consulta [Tipos de archivos y nombres de esquema de URI reservados](reserved-uri-scheme-names.md) para obtener una lista alfabética de esquemas de Uri que no se pueden registrar para las aplicaciones para UWP porque están reservados o prohibidos.

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>Paso 1: Especificar el punto de extensión en el manifiesto del paquete

La aplicación recibe eventos de activación solo para los nombres de esquema de URI indicados en el manifiesto del paquete. A continuación se muestra cómo debes indicar que la aplicación controle el nombre de esquema de URI `alsdk`.

1. En el **Explorador de soluciones**, haz doble clic en package.appxmanifest para abrir el diseñador de manifiestos. Selecciona la pestaña **Declaraciones** y, en la lista desplegable **Declaraciones disponibles**, selecciona **Protocolo** y luego haz clic en **Agregar**.

    A continuación se muestra una breve descripción de cada uno de los campos que puedes rellenar en el diseñador de manifiestos para el protocolo (para obtener más detalles, consulta [**Manifiesto del paquete AppX**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-extension)):

| Campo | Descripción |
|-------|-------------|
| **Logotipo** | Especifica el logotipo que se usa para identificar el nombre de esquema de URI en la opción [Establecer programas predeterminados](https://docs.microsoft.com/windows/desktop/shell/default-programs) del **Panel de control**. Si no se especifica ningún logotipo, se usará el logotipo pequeño para la aplicación. |
| **Nombre para mostrar** | Especifica el nombre para mostrar para identificar el nombre de esquema de URI en la opción [Establecer programas predeterminados](https://docs.microsoft.com/windows/desktop/shell/default-programs) del **Panel de control**. |
| **Nombre** | Elige un nombre para el esquema de URI. |
|  | **Nota**  El nombre debe estar completamente en minúsculas. |
|  | **Tipos de archivos reservados y prohibidos** Consulta [Tipos de archivos y nombres de esquema de URI reservados](reserved-uri-scheme-names.md) para obtener una lista ordenada alfabéticamente de esquemas de URI que no se pueden registrar para las aplicaciones para UWP porque están reservados o prohibidos. |
| **Ejecutable** | Especifica el archivo ejecutable de inicio predeterminado para el protocolo. Si no se especifica, se usará el archivo ejecutable de la aplicación. Si se especifica, la cadena debe tener entre 1 y 256 caracteres de longitud, debe terminar con ". exe" y no puede contener estos caracteres: &gt;, &lt;,:, " &#124;,,? o \*. Si se especifica, también se usará el elemento **Entry point**. Si no se especifica el elemento **Entry point**, se usará el punto de entrada definido para la aplicación. |
| **Punto de entrada** | Especifica la tarea que administra la extensión del protocolo. Suele ser el nombre completo en el espacio de nombres de un tipo de Windows en tiempo de ejecución. Si no se especifica, se usará el punto de entrada de la aplicación. |
| **Página de inicio** | La página web que administra el punto de extensibilidad. |
| **Grupo de recursos** | Una etiqueta que puedes usar para agrupar las activaciones de extensión con fines de administración de recursos. |
| **Desired View** (solo Windows) | Especifica el campo **Desired View** para indicar la cantidad de espacio que necesita la ventana cuando se inicia para el nombre del esquema de URI. Los posibles valores de **Desired View** son **Default**, **UseLess**, **UseHalf**, **UseMore** o **UseMinimum**.<br/>**Nota:** Windows tiene en cuenta diferentes factores a la hora de determinar el tamaño final de la ventana de la aplicación de destino, como por ejemplo, la preferencia de la aplicación de origen, el número de aplicaciones en pantalla, la orientación de la pantalla, etc. El establecimiento de **Desired View** no garantiza un comportamiento de ventanas específico para la aplicación de destino.<br/>**Familia de dispositivos móviles: Vista deseada** no se admite en la familia de dispositivos móviles. |

2. Escribe `images\Icon.png` como el valor de **Logo**.
3. Escribe `SDK Sample URI Scheme` como **Display name**.
4. Escribe `alsdk` como el valor de **Name**.
5. Presiona Ctrl+S para guardar el cambio realizado en package.appxmanifest.

    Esto agrega un elemento [**Extension**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-1-extension) como este al manifiesto del paquete. La categoría **windows.protocol** indica que la aplicación controla el nombre de esquema de URI `alsdk`.

```xml
    <Applications>
        <Application Id= ... >
            <Extensions>
                <uap:Extension Category="windows.protocol">
                  <uap:Protocol Name="alsdk">
                    <uap:Logo>images\icon.png</uap:Logo>
                    <uap:DisplayName>SDK Sample URI Scheme</uap:DisplayName>
                  </uap:Protocol>
                </uap:Extension>
          </Extensions>
          ...
        </Application>
   <Applications>
```

## <a name="step-2-add-the-proper-icons"></a>Paso 2: Agregar los iconos adecuados

Las aplicaciones que se convierten en predeterminadas para un nombre de esquema URI muestran sus iconos en varios lugares del sistema, como el panel de control Programas predeterminados. Incluye un icono de 44 x 44 con el proyecto para este propósito. Ajusta el aspecto del logotipo del icono de la aplicación y usa el color de fondo de la aplicación en lugar de usar un icono transparente. Extiende el logotipo hasta el borde sin que quede espacio. Prueba los iconos en fondos de color blanco. Vea [iconos y logotipos](https://docs.microsoft.com/windows/uwp/design/style/app-icons-and-logos) de la aplicación para obtener más detalles sobre los iconos.

## <a name="step-3-handle-the-activated-event"></a>Paso 3: Administrar el evento activado

El controlador de eventos [**OnActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated) recibe todos los eventos de activación. La propiedad **Kind** indica el tipo de evento de activación. Este ejemplo está configurado para controlar eventos de activación de [**Protocol**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ActivationKind).

```csharp
public partial class App
{
   protected override void OnActivated(IActivatedEventArgs args)
  {
      if (args.Kind == ActivationKind.Protocol)
      {
         ProtocolActivatedEventArgs eventArgs = args as ProtocolActivatedEventArgs;
         // TODO: Handle URI activation
         // The received URI is eventArgs.Uri.AbsoluteUri
      }
   }
}
```

```vb
Protected Overrides Sub OnActivated(ByVal args As Windows.ApplicationModel.Activation.IActivatedEventArgs)
   If args.Kind = ActivationKind.Protocol Then
      ProtocolActivatedEventArgs eventArgs = args As ProtocolActivatedEventArgs
      
      ' TODO: Handle URI activation
      ' The received URI is eventArgs.Uri.AbsoluteUri
 End If
End Sub
```

```cppwinrt
void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs const& args)
{
    if (args.Kind() == Windows::ApplicationModel::Activation::ActivationKind::Protocol)
    {
        auto protocolActivatedEventArgs{ args.as<Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs>() };
        // TODO: Handle URI activation  
        auto receivedURI{ protocolActivatedEventArgs.Uri().RawUri() };
    }
}
```

```cpp
void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs^ args)
{
   if (args->Kind == Windows::ApplicationModel::Activation::ActivationKind::Protocol)
   {
      Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs^ eventArgs =
          dynamic_cast<Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs^>(args);
      
      // TODO: Handle URI activation  
      // The received URI is eventArgs->Uri->RawUri
   }
}
```

> [!NOTE]
> Cuando se inicia a través de un contrato de protocolo, asegúrese de que el botón atrás vuelva al usuario a la pantalla que inició la aplicación y no al contenido anterior de la aplicación.

El siguiente código inicia mediante programación la aplicación a través de su URI:

```csharp
   // Launch the URI
   var uri = new Uri("alsdk:");
   var success = await Windows.System.Launcher.LaunchUriAsync(uri)
```

Para obtener más detalles acerca de cómo iniciar una aplicación mediante un URI, consulta [Iniciar la aplicación predeterminada de un URI](launch-default-app.md).

Se recomienda que las aplicaciones creen un nuevo elemento [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) de XAML para cada evento de activación que abra una nueva página. De esta forma, la navegación hacia atrás del nuevo elemento **Frame** de XAML no incluirá ningún contenido anterior que la aplicación tuviera en la ventana actual al pasar a suspensión. Las aplicaciones que decidan usar un solo elemento **Frame** de XAML para los contratos inicio y archivo deben borrar las páginas del diario de navegación del elemento **Frame** antes de ir a una página nueva.

Si las aplicaciones se inician a través de la activación de protocolo, sería necesario plantearse que incluyeran una interfaz de usuario que permita al usuario volver a la parte superior de la aplicación.

## <a name="remarks"></a>Observaciones

Cualquier aplicación o sitio web puede usar tu nombre de esquema de URI, incluidos los malintencionados. Por este motivo, los datos que incluyes en el URI podrían provenir de un origen que no es de confianza. Te desaconsejamos que realices una acción permanente en función de los parámetros que recibes en el URI. Puedes usar parámetros de URI, por ejemplo, para que la aplicación se inicie en una página de la cuenta del usuario, pero no te recomendamos que los uses para modificar directamente la cuenta del usuario.

> [!NOTE]
> Si va a crear un nuevo nombre de esquema de URI para la aplicación, asegúrese de seguir las instrucciones de [RFC 4395](https://tools.ietf.org/html/rfc4395). Esto garantiza que el nombre cumpla los estándares de los esquemas de URI.

> [!NOTE]
> Cuando se inicia a través de un contrato de protocolo, asegúrese de que el botón atrás vuelva al usuario a la pantalla que inició la aplicación y no al contenido anterior de la aplicación.

Es recomendable que las aplicaciones creen un nuevo elemento [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) de XAML para cada evento de activación que abra un nuevo destino de URI. De esta forma, la navegación hacia atrás del nuevo elemento **Frame** de XAML no incluirá ningún contenido anterior que la aplicación tuviera en la ventana actual al pasar a suspensión.

Si prefieres que las aplicaciones usen un único elemento [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) de XAML para los contratos de inicio y protocolo, borra las páginas en el diario de navegación del elemento **Frame** antes de ir a una página nueva. Cuando el inicio se realiza mediante un contrato de protocolo, considera la posibilidad de incluir una interfaz de usuario que permita al usuario volver al comienzo de la aplicación.

## <a name="related-topics"></a>Temas relacionados

### <a name="complete-sample-app"></a>Aplicación de ejemplo completa

- [Ejemplo de inicio de asociación](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching)

### <a name="concepts"></a>Conceptos

- [Programas predeterminados](https://docs.microsoft.com/windows/desktop/shell/default-programs)
- [Modelo de tipo de archivo y asociaciones URI](https://docs.microsoft.com/windows/desktop/w8cookbook/file-type-and-protocol-associations-model)

### <a name="tasks"></a>Tareas

- [Iniciar la aplicación predeterminada de un URI](launch-default-app.md)
- [Administrar la activación de archivos](handle-file-activation.md)

### <a name="guidelines"></a>Instrucciones

- [Directrices para tipos de archivo y URI](https://docs.microsoft.com/windows/uwp/files/index)

### <a name="reference"></a>Referencia

- [Manifiesto del paquete AppX](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-extension)
- [Windows. ApplicationModel. Activation. ProtocolActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs)
- [Windows. UI. Xaml. Application. Onactivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated)
