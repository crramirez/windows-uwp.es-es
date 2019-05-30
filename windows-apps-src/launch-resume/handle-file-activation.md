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
ms.openlocfilehash: 28188acfd3999c0b384326f013a0ba1bdf71a34f
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370753"
---
# <a name="handle-file-activation"></a>Administrar la activación de archivos

**API importantes**

-   [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
-   [**Windows.UI.Xaml.Application.OnFileActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)

La aplicación puede registrarse para convertirse en el controlador predeterminado para un tipo de archivo determinado. Tanto las aplicaciones de escritorio de Windows como las aplicaciones de la Plataforma universal de Windows (UWP) pueden registrarse para convertirse en el controlador de archivos predeterminado. Si el usuario elige tu aplicación como controlador predeterminado de un tipo de archivo específico, la aplicación se activará cuando se inicie dicho tipo de archivo.

Te recomendamos que solo registres tu aplicación para un tipo de archivo si tienes previsto administrar todos los inicios de archivos de ese tipo. Si tu aplicación solo necesita usar el tipo de archivo internamente, no tienes que registrarla para que sea el controlador predeterminado. Si, en efecto, eliges registrarla para un tipo de archivo, debes proporcionar al usuario final todas las funciones que se esperan cuando tu aplicación se active para ese tipo de archivo. Por ejemplo, un visor de imágenes puede registrarse para mostrar un archivo .jpg. Para obtener más información sobre las asociaciones de archivos, consulta [Directrices para los tipos de archivo y URI](https://docs.microsoft.com/windows/uwp/files/index).

En estos pasos se muestra cómo realizar el registro de un tipo de archivo personalizado, .alsdk, y cómo se activa una aplicación cuando el usuario inicia un archivo .alsdk.

> **Tenga en cuenta**  en aplicaciones UWP, ciertas identificadores URI y extensiones de archivo están reservadas para uso por aplicaciones integradas y el sistema operativo. Se ignorarán los intentos de registrar aplicaciones con una extensión de archivo o URI reservada. Para más información, consulta [Nombres de esquemas de URI y archivos reservados](reserved-uri-scheme-names.md).

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>Paso 1: Especifique el punto de extensión en el manifiesto del paquete

La aplicación recibe eventos de activación solo para las extensiones de archivo listadas en el manifiesto del paquete. Aquí se muestra cómo debes indicar que la aplicación controla los archivos con la extensión `.alsdk`.

1.  En el **Explorador de soluciones**, haz doble clic en package.appxmanifest para abrir el diseñador de manifiestos. Selecciona la pestaña **Declaraciones** y, en la lista desplegable **Declaraciones disponibles**, selecciona **Asociaciones de tipos de archivo** y luego haz clic en **Agregar**. Consulta [Identificadores de programación](https://docs.microsoft.com/windows/desktop/shell/fa-progids) para obtener más detalles sobre los identificadores que las asociaciones de archivos utilizan.

    A continuación se muestra una breve descripción de cada uno de los campos que puedes rellenar en el diseñador de manifiestos:

| Campo | Descripción |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nombre para mostrar** | Especifica el nombre para mostrar de un grupo de tipos de archivo. El nombre para mostrar se usa para identificar el tipo de archivo en [Establecer programas predeterminados](https://docs.microsoft.com/windows/desktop/shell/default-programs) en el **Panel de control**. |
| **Logo** | Especifica el logotipo que se usa para identificar el tipo de archivo en el escritorio y en [Establecer programas predeterminados](https://docs.microsoft.com/windows/desktop/shell/default-programs) en el **Panel de control**. Si no se especifica ningún logotipo, se usa el logotipo pequeño para la aplicación. |
| **Sugerencia de información** | Especifica la [Información sobre herramientas](https://docs.microsoft.com/windows/desktop/shell/fa-progids) de un grupo de tipos de archivo. Este texto de información sobre herramientas aparece cuando el usuario mantiene el puntero sobre el icono de un archivo de este tipo. |
| **Name** | Elige un nombre para un grupo de tipos de archivos que comparten el mismo nombre para mostrar, logotipo, información y marcas de edición. Elige un nombre de grupo que pueda continuar siendo el mismo a lo largo de las actualizaciones de la aplicación. **Nota**  El nombre debe estar completamente en minúsculas. |
| **Tipo de contenido** | Especifica el tipo de contenido MIME, como **image/jpeg**, de un tipo de archivo concreto. **Nota importante acerca de los tipos de contenido permitidos:** Esta es una lista alfabética de los tipos de contenido MIME que no se puede escribir en el manifiesto del paquete porque está reservados o prohibidos: **/force-descargar la aplicación**, **application/octet-stream**, **aplicación o desconocido**, **application/x-msdownload**. |
| **Tipo de archivo** | Especifica el tipo de archivo para el que se va a registrar, precedido de un punto, por ejemplo, “.jpeg”. **Tipos de archivo prohibido y no reservados:** Consulte [tipos de archivos y los nombres de esquema de URI reservada](reserved-uri-scheme-names.md) para una lista alfabética de los tipos de archivos para aplicaciones integradas que no se puede registrar para aplicaciones UWP porque está reservados o prohibidos. |

2.  Escribe `alsdk` como el valor de **Name**.
3.  Escribe `.alsdk` como el valor de **File Type**.
4.  Escriba "imágenes\\Icon.png" como el logotipo.
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

-   Vista de elementos del Explorador de Windows, menús contextuales y la cinta
-   Panel de control de programas predeterminados
-   Selector de archivos
-   Resultados de búsqueda en la pantalla Inicio

Incluir un icono de 44 x 44 con el proyecto para que tu logotipo puede aparecer en estas ubicaciones. Ajusta el aspecto del logotipo del icono de la aplicación y usa el color de fondo de la aplicación en lugar de usar un icono transparente. Extiende el logotipo hasta el borde sin que quede espacio. Prueba los iconos en fondos de color blanco. Consulta [Directrices para los activos de iconos](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/app-assets) para obtener más información sobre los iconos.

## <a name="step-3-handle-the-activated-event"></a>Paso 3: Controlar el evento activado

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
> Cuando se inicia a través del contrato del archivo, asegúrese de que ese botón Atrás, vuelve al usuario a la pantalla que se inició la aplicación y no al contenido anterior de la aplicación.

Le recomendamos que cree un nuevo XAML **marco** para cada evento de activación que se abre una nueva página. De este modo, el backstack de navegación para el nuevo marco de XAML no contiene cualquier contenido anterior que podría tener la aplicación en la ventana actual cuando se suspende. Si decide usar un único XAML **marco** de lanzamiento y contratos de archivo, debe borrar las páginas en el **marco**del diario de navegación antes de navegar a una página nueva.

Cuando se inicia la aplicación a través de la activación de archivo, debe considerar como la interfaz de usuario que permite al usuario volver a la página principal de la aplicación.

## <a name="remarks"></a>Comentarios

Los archivos que recibes pueden provenir de un origen que no es de confianza. Se recomienda que valides el contenido de un archivo antes de realizar una acción en él.

## <a name="related-topics"></a>Temas relacionados

### <a name="complete-example"></a>Ejemplo completo

* [Ejemplo de inicio de asociación](https://go.microsoft.com/fwlink/p/?LinkID=231484)

### <a name="concepts"></a>Conceptos

* [Programas predeterminados](https://docs.microsoft.com/windows/desktop/shell/default-programs)
* [Tipo de archivo y el modelo de las asociaciones de protocolo](https://docs.microsoft.com/windows/desktop/w8cookbook/file-type-and-protocol-associations-model)

### <a name="tasks"></a>Tareas

* [Iniciar la aplicación predeterminada de un archivo](launch-the-default-app-for-a-file.md)
* [Controlar la activación de URI](handle-uri-activation.md)

### <a name="guidelines"></a>Instrucciones

* [Directrices para los tipos de archivo y los URI](https://docs.microsoft.com/windows/uwp/files/index)

### <a name="reference"></a>Referencia
* [Windows.ApplicationModel.Activation.FileActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
* [Windows.UI.Xaml.Application.OnFileActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)
