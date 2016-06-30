---
author: TylerMSFT
title: "Administrar la activación de archivos"
description: "Una aplicación se puede registrar para convertirse en el controlador predeterminado de un tipo de archivo específico."
ms.assetid: A0F914C5-62BC-4FF7-9236-E34C5277C363
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 9c6358bccdea55a7c3749388c35aa770f4325960

---

# Controlar la activación de archivos


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224716)
-   [**Windows.UI.Xaml.Application.OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)

Una aplicación se puede registrar para convertirse en el controlador predeterminado de un tipo de archivo específico. Tanto las aplicaciones de Plataforma clásica de Windows (CWP) como las aplicaciones de Plataforma universal de Windows (UWP) pueden registrarse para convertirse en el controlador de archivo predeterminado. Si el usuario elige tu aplicación como controlador predeterminado de un tipo de archivo específico, la aplicación se activará cuando se inicie dicho tipo de archivo.

Te recomendamos que solo registres tu aplicación para un tipo de archivo si tienes previsto administrar todos los inicios de archivos de ese tipo. Si tu aplicación solo necesita usar el tipo de archivo internamente, no tienes que registrarla para que sea el controlador predeterminado. Si, en efecto, eliges registrarla para un tipo de archivo, debes proporcionar al usuario final todas las funciones que se esperan cuando tu aplicación se active para ese tipo de archivo. Por ejemplo, un visor de imágenes puede registrarse para mostrar un archivo .jpg. Para obtener más información sobre las asociaciones de archivos, consulta [Directrices para los tipos de archivo y URI](https://msdn.microsoft.com/library/windows/apps/hh700321).

En estos pasos se muestra cómo realizar el registro de un tipo de archivo personalizado, .alsdk, y cómo se activa una aplicación cuando el usuario inicia un archivo .alsdk.

> **Nota**  En las aplicaciones para UWP, algunas extensiones de archivo y URI se reservan para que las usen aplicaciones integradas y del sistema operativo. Se ignorarán los intentos de registrar aplicaciones con una extensión de archivo o URI reservada. Para más información, consulta [Nombres de esquemas de URI y archivos reservados](reserved-uri-scheme-names.md).

## Paso 1: Especificar el punto de extensión en el manifiesto del paquete


La aplicación recibe eventos de activación solo para las extensiones de archivo listadas en el manifiesto del paquete. Aquí se muestra cómo debes indicar que la aplicación controla los archivos con la extensión `.alsdk`.

1.  En el **Explorador de soluciones**, haz doble clic en package.appxmanifest para abrir el diseñador de manifiestos. Selecciona la pestaña **Declaraciones** y, en la lista desplegable **Declaraciones disponibles**, selecciona **Asociaciones de tipos de archivo** y luego haz clic en **Agregar**. Consulta [Identificadores de programación](https://msdn.microsoft.com/library/windows/desktop/cc144152) para obtener más detalles sobre los identificadores que las asociaciones de archivos utilizan.

    A continuación se muestra una breve descripción de cada uno de los campos que puedes rellenar en el diseñador de manifiestos:

| Campo | Descripción |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Display Name** | Especifica el nombre para mostrar de un grupo de tipos de archivo. El nombre para mostrar se usa para identificar el tipo de archivo en [Establecer programas predeterminados](https://msdn.microsoft.com/library/windows/desktop/cc144154) en el **Panel de control**. |
| **Logotipo** | Especifica el logotipo que se usa para identificar el tipo de archivo en el escritorio y en [Establecer programas predeterminados](https://msdn.microsoft.com/library/windows/desktop/cc144154) en el **Panel de control**. Si no se especifica ningún logotipo, se usa el logotipo pequeño para la aplicación. |
| **Información sobre herramientas** | Especifica la [Información sobre herramientas](https://msdn.microsoft.com/library/windows/desktop/cc144152) de un grupo de tipos de archivo. Este texto de información sobre herramientas aparece cuando el usuario mantiene el puntero sobre el icono de un archivo de este tipo. |
| **Nombre** | Elige un nombre para un grupo de tipos de archivos que comparten el mismo nombre para mostrar, logotipo, información y marcas de edición. Elige un nombre de grupo que pueda continuar siendo el mismo a lo largo de las actualizaciones de la aplicación. **Nota**  El nombre debe estar completamente en minúsculas. |
| **Content Type** | Especifica el tipo de contenido MIME, como **image/jpeg**, de un tipo de archivo concreto. **Nota importante sobre los tipos de contenido permitidos:  **Esta es una lista ordenada alfabéticamente de los tipos de contenido MIME que no se pueden especificar en el manifiesto del paquete porque están reservados o prohibidos: **application/force-download**, **application/octet-stream**, **application/unknown**, **application/x-msdownload**. |
| **File type** | Especifica el tipo de archivo que se va a registrar, precedido de un punto, por ejemplo, ".jpeg". **Tipos de archivos reservados y prohibidos** Consulta [Tipos de archivos y nombres de esquema de URI reservados](reserved-uri-scheme-names.md) para obtener una lista ordenada alfabéticamente de los tipos de archivo para aplicaciones integradas que no se pueden registrar para las aplicaciones para UWP porque están reservados o prohibidos. |

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

## Paso 2: Agregar los iconos adecuados


Las aplicaciones que se convierten en predeterminadas para un tipo de archivo muestran sus iconos en varios lugares del sistema. Por ejemplo, estos iconos se muestran en:

-   Vista Elementos del Explorador de Windows, menús contextuales y la Cinta
-   Panel de control de programas predeterminados
-   Selector de archivos
-   Resultados de búsqueda en la pantalla Inicio

Ajusta el aspecto del logotipo del icono de la aplicación y usa el color de fondo de la aplicación en lugar de usar un icono transparente. Extiende el logotipo hasta el borde sin que quede espacio. Prueba los iconos en fondos de color blanco. Para ver ejemplos de iconos, consulta el [ejemplo de inicio por asociación](http://go.microsoft.com/fwlink/p/?LinkID=620490).
![el explorador de soluciones con una vista de los archivos de la carpeta imágenes. hay versiones de 16, 32, 48 y 256 píxeles de ‘icon.targetsize’ y ‘smalltile-sdk’](images/seviewofimages.png)

## Paso 3: Administrar el evento activado


El controlador de eventos [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331) recibe todos los eventos de activación de archivos.

> [!div class="tabbedCodeSnippets"]
```vb
Protected Overrides Sub OnFileActivated(ByVal args As Windows.ApplicationModel.Activation.FileActivatedEventArgs)
      ' TODO: Handle file activation
      ' The number of files received is args.Files.Size
      ' The name of the first file is args.Files(0).Name
End Sub
```
```cpp
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs^ args)
{
       // TODO: Handle file activation
       // The number of files received is args->Files->Size
       // The first file is args->Files->GetAt(0)->Name
}
```
```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
       // TODO: Handle file activation
       // The number of files received is args.Files.Size
       // The name of the first file is args.Files[0].Name
}
```

    > **Note**  When launched via File Contract, make sure that Back button takes the user back to the screen that launched the app and not to the app's previous content.

Se recomienda que las aplicaciones creen un nuevo elemento Frame de XAML para cada evento de activación que abra una nueva página. Así, la navegación hacia atrás del nuevo marco XAML no incluirá nada del contenido anterior que la aplicación tuviera en la ventana actual al pasar a suspensión. Las aplicaciones que decidan usar un solo elemento Frame de XAML para los contratos Iniciar y Archivo deben borrar las páginas del diario de navegación del elemento Frame antes de ir a una página nueva.

Si se inician a través de la activación de archivo, las aplicaciones deberían plantearse incluir una interfaz de usuario que permita al usuario volver a la página superior de la aplicación.

## Observaciones


Los archivos que recibes pueden provenir de un origen que no es de confianza. Se recomienda que valides el contenido de un archivo antes de realizar una acción en él. Para más información, consulta [Escribir código seguro](http://go.microsoft.com/fwlink/p/?LinkID=142053).

> **Nota**  Este artículo está orientado a desarrolladores de Windows 10 que programan aplicaciones para la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Temas relacionados

**Ejemplo completo**

* [Ejemplo de inicio por asociación](http://go.microsoft.com/fwlink/p/?LinkID=231484)

**Conceptos**

* [Programas predeterminados](https://msdn.microsoft.com/library/windows/desktop/cc144154)
* [Modelo de asociación de tipos de archivo y protocolos](https://msdn.microsoft.com/library/windows/desktop/hh848047)

**Tareas**

* [Iniciar la aplicación predeterminada de un archivo](launch-the-default-app-for-a-file.md)
* [Controlar la activación de URI](handle-uri-activation.md)

**Instrucciones**

* [Directrices sobre tipos de archivo y URI](https://msdn.microsoft.com/library/windows/apps/hh700321)

**Referencia**
* [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224716)
* [**Windows.UI.Xaml.Application.OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)

 

 



<!--HONumber=Jun16_HO4-->


