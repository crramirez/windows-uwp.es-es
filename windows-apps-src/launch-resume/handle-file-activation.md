---
author: TylerMSFT
title: Administrar la activación de archivos
description: Una aplicación se puede registrar para convertirse en el controlador predeterminado de un tipo de archivo específico.
ms.assetid: A0F914C5-62BC-4FF7-9236-E34C5277C363
ms.author: twhitney
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 9f1e41c3e09d9a711ce9174a5a658a55c7c44abd
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "7426563"
---
# <a name="handle-file-activation"></a>Administrar la activación de archivos

**API importantes**

-   [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224716)
-   [**Windows.UI.Xaml.Application.OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)

La aplicación pueda registrar para convertirse en el controlador predeterminado de un tipo de archivo determinado. Tanto las aplicaciones de escritorio de Windows como las aplicaciones de la Plataforma universal de Windows (UWP) pueden registrarse para convertirse en el controlador de archivos predeterminado. Si el usuario elige tu aplicación como controlador predeterminado de un tipo de archivo específico, la aplicación se activará cuando se inicie dicho tipo de archivo.

Te recomendamos que solo registres tu aplicación para un tipo de archivo si tienes previsto administrar todos los inicios de archivos de ese tipo. Si tu aplicación solo necesita usar el tipo de archivo internamente, no tienes que registrarla para que sea el controlador predeterminado. Si, en efecto, eliges registrarla para un tipo de archivo, debes proporcionar al usuario final todas las funciones que se esperan cuando tu aplicación se active para ese tipo de archivo. Por ejemplo, un visor de imágenes puede registrarse para mostrar un archivo .jpg. Para obtener más información sobre las asociaciones de archivos, consulta [Directrices para los tipos de archivo y URI](https://msdn.microsoft.com/library/windows/apps/hh700321).

En estos pasos se muestra cómo realizar el registro de un tipo de archivo personalizado, .alsdk, y cómo se activa una aplicación cuando el usuario inicia un archivo .alsdk.

> **Nota**en aplicaciones para UWP, algunas extensiones de archivo y URI se reservan para su uso por las aplicaciones integradas y el sistema operativo. Se ignorarán los intentos de registrar aplicaciones con una extensión de archivo o URI reservada. Para más información, consulta [Nombres de esquemas de URI y archivos reservados](reserved-uri-scheme-names.md).

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>Paso 1: Especificar el punto de extensión en el manifiesto del paquete

La aplicación recibe eventos de activación solo para las extensiones de archivo listadas en el manifiesto del paquete. Aquí se muestra cómo debes indicar que la aplicación controla los archivos con la extensión `.alsdk`.

1.  En el **Explorador de soluciones**, haz doble clic en package.appxmanifest para abrir el diseñador de manifiestos. Selecciona la pestaña **Declaraciones** y, en la lista desplegable **Declaraciones disponibles**, selecciona **Asociaciones de tipos de archivo** y luego haz clic en **Agregar**. Consulta [Identificadores de programación](https://msdn.microsoft.com/library/windows/desktop/cc144152) para obtener más detalles sobre los identificadores que las asociaciones de archivos utilizan.

    A continuación se muestra una breve descripción de cada uno de los campos que puedes rellenar en el diseñador de manifiestos:

| Campo | Descripción |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Display Name** | Especifica el nombre para mostrar de un grupo de tipos de archivo. El nombre para mostrar se usa para identificar el tipo de archivo en [Establecer programas predeterminados](https://msdn.microsoft.com/library/windows/desktop/cc144154) en el **Panel de control**. |
| **Logotipo** | Especifica el logotipo que se usa para identificar el tipo de archivo en el escritorio y en [Establecer programas predeterminados](https://msdn.microsoft.com/library/windows/desktop/cc144154) en el **Panel de control**. Si no se especifica ningún logotipo, se usa el logotipo pequeño para la aplicación. |
| **Información sobre herramientas** | Especifica la [Información sobre herramientas](https://msdn.microsoft.com/library/windows/desktop/cc144152) de un grupo de tipos de archivo. Este texto de información sobre herramientas aparece cuando el usuario mantiene el puntero sobre el icono de un archivo de este tipo. |
| **Nombre** | Elige un nombre para un grupo de tipos de archivos que comparten el mismo nombre para mostrar, logotipo, información y marcas de edición. Elige un nombre de grupo que pueda continuar siendo el mismo a lo largo de las actualizaciones de la aplicación. **Nota** El nombre debe estar completamente en minúsculas. |
| **Content Type** | Especifica el tipo de contenido MIME, como **image/jpeg**, de un tipo de archivo concreto. **Nota importante sobre los tipos de contenido permitidos:** Esta es una lista ordenada alfabéticamente de los tipos de contenido MIME que no se pueden especificar en el manifiesto del paquete porque están reservados o prohibidos: **application/force-download**, **application/octet-stream**, **application/unknown**, **application/x-msdownload**. |
| **File type** | Especifica el tipo de archivo para el que se va a registrar, precedido de un punto, por ejemplo, “.jpeg”. **Tipos de archivos reservados y prohibidos:** Consulta [Tipos de archivos y nombres de esquema de URI reservados](reserved-uri-scheme-names.md) para obtener una lista ordenada alfabéticamente de los tipos de archivo para aplicaciones integradas que no se pueden registrar para las aplicaciones para UWP porque están reservados o prohibidos. |

2.  Escribe `alsdk` como el valor de **Name**.
3.  Escribe `.alsdk` como el valor de **File Type**.
4.  Escribe “images\\Icon.png” como el valor de Logo.
5.  Presiona Ctrl+S para guardar el cambio realizado en package.appxmanifest.

Los pasos anteriores permiten agregar un elemento [**Extension**](https://msdn.microsoft.com/library/windows/apps/br211400) como este al manifiesto del paquete. La categoría **windows.fileTypeAssociation** indica que la aplicación controla los archivos con la extensión `.alsdk`.

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

Incluir un icono de 44 x 44 con el proyecto para que tu logotipo puede aparecer en estas ubicaciones. Ajusta el aspecto del logotipo del icono de la aplicación y usa el color de fondo de la aplicación en lugar de hacer que el icono sea transparente. Extiende el logotipo hasta el borde sin que quede espacio. Prueba los iconos en fondos de color blanco. Consulta [Directrices para los activos de iconos](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/app-assets) para obtener más información sobre los iconos.

## <a name="step-3-handle-the-activated-event"></a>Paso 3: Administrar el evento activado

El controlador de eventos [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331) recibe todos los eventos de activación de archivos.

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
> Cuando se inicia mediante un contrato de archivo, asegúrate de que el botón Atrás lleve al usuario volver a la pantalla que inició la aplicación y no al contenido anterior de la aplicación.

Te recomendamos que crees un nuevo XAML **marco** para cada evento de activación que abra una nueva página. De este modo, el objeto backstack de navegación para el nuevo marco de XAML no contiene ningún contenido anterior que la aplicación en la ventana actual al pasar a suspensión. Si decides usar un solo **marco** de XAML para el inicio y para los contratos de archivo, debe borrar las páginas en el **marco**del diario de navegación antes de ir a una página nueva.

Cuando se inicia la aplicación a través de la activación de archivos, considera la posibilidad de interfaz de usuario que permite al usuario volver a la página superior de la aplicación.

## <a name="remarks"></a>Observaciones

Los archivos que recibes pueden provenir de un origen que no es de confianza. Se recomienda que valides el contenido de un archivo antes de realizar una acción en él. Para más información, consulta [Escribir código seguro](http://go.microsoft.com/fwlink/p/?LinkID=142053).

## <a name="related-topics"></a>Temas relacionados

### <a name="complete-example"></a>Ejemplo completo

* [Ejemplo de inicio por asociación](http://go.microsoft.com/fwlink/p/?LinkID=231484)

### <a name="concepts"></a>Conceptos

* [Programas predeterminados](https://msdn.microsoft.com/library/windows/desktop/cc144154)
* [Modelo de asociación de tipos de archivo y protocolos](https://msdn.microsoft.com/library/windows/desktop/hh848047)

### <a name="tasks"></a>Tareas

* [Iniciar la aplicación predeterminada de un archivo](launch-the-default-app-for-a-file.md)
* [Controlar la activación de URI](handle-uri-activation.md)

### <a name="guidelines"></a>Instrucciones

* [Directrices sobre tipos de archivo y URI](https://msdn.microsoft.com/library/windows/apps/hh700321)

### <a name="reference"></a>Referencia
* [Windows.ApplicationModel.Activation.FileActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/br224716)
* [Windows.UI.Xaml.Application.OnFileActivated](https://msdn.microsoft.com/library/windows/apps/br242331)
