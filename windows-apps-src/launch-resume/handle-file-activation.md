---
title: Administrar la activación de archivos
description: Una aplicación se puede registrar para convertirse en el controlador predeterminado de un tipo de archivo específico.
ms.assetid: A0F914C5-62BC-4FF7-9236-E34C5277C363
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 4377db57b3cd713bae8f9c80a0116d016722be19
ms.sourcegitcommit: 90fe7a9a5bfa7299ad1b78bbef289850dfbf857d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2020
ms.locfileid: "84756541"
---
# <a name="handle-file-activation"></a>Administrar la activación de archivos

**API importantes**

-   [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
-   [**Windows.UI.Xaml.Application.OnFileActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)

La aplicación se puede registrar para que se convierta en el controlador predeterminado para un tipo de archivo determinado. Tanto las aplicaciones de escritorio de Windows como las aplicaciones de la Plataforma universal de Windows (UWP) pueden registrarse para convertirse en el controlador de archivos predeterminado. Si el usuario elige tu aplicación como controlador predeterminado de un tipo de archivo específico, la aplicación se activará cuando se inicie dicho tipo de archivo.

Te recomendamos que solo registres tu aplicación para un tipo de archivo si tienes previsto administrar todos los inicios de archivos de ese tipo. Si tu aplicación solo necesita usar el tipo de archivo internamente, no tienes que registrarla para que sea el controlador predeterminado. Si, en efecto, eliges registrarla para un tipo de archivo, debes proporcionar al usuario final todas las funciones que se esperan cuando tu aplicación se active para ese tipo de archivo. Por ejemplo, un visor de imágenes puede registrarse para mostrar un archivo .jpg. Para obtener más información sobre las asociaciones de archivos, consulta [Directrices para los tipos de archivo y URI](https://docs.microsoft.com/windows/uwp/files/index).

En estos pasos se muestra cómo realizar el registro de un tipo de archivo personalizado, .alsdk, y cómo se activa una aplicación cuando el usuario inicia un archivo .alsdk.

> **Nota:**    En las aplicaciones UWP, ciertos URI y extensiones de archivo se reservan para su uso por parte de las aplicaciones integradas y el sistema operativo. Se ignorarán los intentos de registrar aplicaciones con una extensión de archivo o URI reservada. Para más información, consulta [Nombres de esquemas de URI y archivos reservados](reserved-uri-scheme-names.md).

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>Paso 1: Especificar el punto de extensión en el manifiesto del paquete

La aplicación recibe eventos de activación solo para las extensiones de archivo listadas en el manifiesto del paquete. Aquí se muestra cómo debes indicar que la aplicación controla los archivos con la extensión `.alsdk`.

1.  En el **Explorador de soluciones**, haz doble clic en package.appxmanifest para abrir el diseñador de manifiestos. Selecciona la pestaña **Declaraciones** y, en la lista desplegable **Declaraciones disponibles**, selecciona **Asociaciones de tipos de archivo** y luego haz clic en **Agregar**. Consulta [Identificadores de programación](https://docs.microsoft.com/windows/desktop/shell/fa-progids) para obtener más detalles sobre los identificadores que las asociaciones de archivos utilizan.

    A continuación se muestra una breve descripción de cada uno de los campos que puedes rellenar en el diseñador de manifiestos:

| Campo | Descripción |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nombre para mostrar** | Especifica el nombre para mostrar de un grupo de tipos de archivo. El nombre para mostrar se usa para identificar el tipo de archivo en [Establecer programas predeterminados](https://docs.microsoft.com/windows/desktop/shell/default-programs) en el **Panel de control**. |
| **Logotipo** | Especifica el logotipo que se usa para identificar el tipo de archivo en el escritorio y en [Establecer programas predeterminados](https://docs.microsoft.com/windows/desktop/shell/default-programs) en el **Panel de control**. Si no se especifica ningún logotipo, se usa el logotipo pequeño para la aplicación. |
| **Información sobre herramientas** | Especifica la [Información sobre herramientas](https://docs.microsoft.com/windows/desktop/shell/fa-progids) de un grupo de tipos de archivo. Este texto de información sobre herramientas aparece cuando el usuario mantiene el puntero sobre el icono de un archivo de este tipo. |
| **Nombre** | Elige un nombre para un grupo de tipos de archivos que comparten el mismo nombre para mostrar, logotipo, información y marcas de edición. Elige un nombre de grupo que pueda continuar siendo el mismo a lo largo de las actualizaciones de la aplicación. **Nota**  El nombre debe estar completamente en minúsculas. |
| **Tipo de contenido** | Especifica el tipo de contenido MIME, como **image/jpeg**, de un tipo de archivo concreto. **Nota importante sobre los tipos de contenido permitidos:** Esta es una lista ordenada alfabéticamente de los tipos de contenido MIME que no se pueden especificar en el manifiesto del paquete porque están reservados o prohibidos: **application/force-download**, **application/octet-stream**, **application/unknown**, **application/x-msdownload**. |
| **Tipo de archivo** | Especifica el tipo de archivo para el que se va a registrar, precedido de un punto, por ejemplo, “.jpeg”. **Tipos de archivos reservados y prohibidos:** Consulta [Tipos de archivos y nombres de esquema de URI reservados](reserved-uri-scheme-names.md) para obtener una lista ordenada alfabéticamente de los tipos de archivo para aplicaciones integradas que no se pueden registrar para las aplicaciones para UWP porque están reservados o prohibidos. |

2.  Escriba `alsdk` como **nombre**.
3.  Escribe `.alsdk` como el valor de **File Type**.
4.  Escriba "images \\Icon.png" como el logotipo.
5.  Presiona Ctrl+S para guardar el cambio realizado en package.appxmanifest.

Los pasos anteriores permiten agregar un elemento [**Extension**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-1-extension) como este al manifiesto del paquete. La categoría **windows.fileTypeAssociation** indica que la aplicación controla los archivos con la extensión `.alsdk`.

```xml
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap:FileTypeAssociation Name="alsdk">
            <uap:Logo>images\icon.png</uap:Logo>
            <uap:SupportedFileTypes>
              <uap:FileType>.alsdk</uap:FileType>
            </uap:SupportedFileTypes>
          </uap:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
```

## <a name="step-2-add-the-proper-icons"></a>Paso 2: Agregar los iconos adecuados

Las aplicaciones que se convierten en predeterminadas para un tipo de archivo muestran sus iconos en varios lugares del sistema. Por ejemplo, estos iconos se muestran en:

-   Vista de elementos del explorador de Windows, menús contextuales y la cinta de opciones
-   Panel de control de programas predeterminados
-   Selector de archivos
-   Resultados de búsqueda en la pantalla Inicio

Incluya un icono de 44 x 44 con el proyecto para que su logotipo pueda aparecer en esas ubicaciones. Ajusta el aspecto del logotipo del icono de la aplicación y usa el color de fondo de la aplicación en lugar de usar un icono transparente. Extiende el logotipo hasta el borde sin que quede espacio. Prueba los iconos en fondos de color blanco. Consulte [directrices para recursos](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/app-assets) de iconos e iconos para obtener más detalles sobre los iconos.

## <a name="step-3-handle-the-activated-event"></a>Paso 3: Administrar el evento activado

El controlador de eventos [**OnFileActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated) recibe todos los eventos de activación de archivos.

```csharp
protected override void OnFileActivated(FileActivatedEventArgs args)
{
       // TODO: Handle file activation
       // The number of files received is args.Files.Size
       // The name of the first file is args.Files[0].Name
}
```

```vb
Protected Overrides Sub OnFileActivated(ByVal args As Windows.ApplicationModel.Activation.FileActivatedEventArgs)
      ' TODO: Handle file activation
      ' The number of files received is args.Files.Size
      ' The name of the first file is args.Files(0).Name
End Sub
```

```cppwinrt
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs const& args)
{
    // TODO: Handle file activation.
    auto numberOfFilesReceived{ args.Files().Size() };
    auto nameOfTheFirstFile{ args.Files().GetAt(0).Name() };
}
```

```cpp
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs^ args)
{
    // TODO: Handle file activation
    // The number of files received is args->Files->Size
    // The name of the first file is args->Files->GetAt(0)->Name
}
```

> [!NOTE]
> Cuando se inicia mediante un contrato de archivo, asegúrese de que el botón atrás vuelva a la pantalla que inició la aplicación y no al contenido anterior de la aplicación.

Se recomienda crear un nuevo **marco** XAML para cada evento de activación que abra una nueva página. De este modo, la pila de navegación hacia arriba del nuevo marco XAML no contiene ningún contenido anterior que la aplicación pueda tener en la ventana actual cuando se suspende. Si decide usar un solo **marco** XAML para el inicio y para los contratos de archivos, debe borrar las páginas del diario de navegación del **marco**antes de ir a una nueva página.

Cuando la aplicación se inicia a través de la activación de archivos, debe considerar la posibilidad de incluir una interfaz de usuario que permita al usuario volver a la página superior de la aplicación.

## <a name="remarks"></a>Comentarios

Los archivos que recibes pueden provenir de un origen que no es de confianza. Se recomienda que valides el contenido de un archivo antes de realizar una acción en él.

## <a name="related-topics"></a>Temas relacionados

### <a name="complete-example"></a>Ejemplo completo

* [Ejemplo de inicio por asociación](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching)

### <a name="concepts"></a>Conceptos

* [Programas predeterminados](https://docs.microsoft.com/windows/desktop/shell/default-programs)
* [Modelo de asociación de tipos de archivo y protocolos](https://docs.microsoft.com/windows/desktop/w8cookbook/file-type-and-protocol-associations-model)

### <a name="tasks"></a>Tareas

* [Iniciar la aplicación predeterminada de un archivo](launch-the-default-app-for-a-file.md)
* [Controlar la activación de URI](handle-uri-activation.md)

### <a name="guidelines"></a>Instrucciones

* [Directrices sobre tipos de archivo y URI](https://docs.microsoft.com/windows/uwp/files/index)

### <a name="reference"></a>Referencia
* [Windows.ApplicationModel.Activation.FileActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
* [Windows.UI.Xaml.Application.OnFileActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)
