---
author: DelfCo
Description: "Coloca los recursos de cadena de la interfaz de usuario en archivos de recursos. Así, podrás hacer referencia a dichas cadenas desde el código o marcado."
title: Colocar las cadenas de la interfaz de usuario en recursos
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Put UI strings into resources
template: detail.hbs
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: a3e224fc51245a5f91c29da2d745a3740029cda9
ms.sourcegitcommit: 11664964e548a2af30d6e176c515cdbf330934ac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/28/2017
---
# <a name="put-ui-strings-into-resources"></a>Colocar las cadenas de la interfaz de usuario en recursos
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Coloca los recursos de cadena de la interfaz de usuario en archivos de recursos. Así, podrás hacer referencia a dichas cadenas desde el código o marcado.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**ApplicationModel.Resources.ResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br206014)</li>
<li>[**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864)</li>
</ul>
</div>


Este tema muestra los pasos para agregar varios recursos de cadena de idioma a tu aplicación universal de Windows y cómo probarlos brevemente.

## <a name="put-strings-into-resource-files-instead-of-putting-them-directly-in-code-or-markup"></a>Coloca las cadenas en archivos de recursos en lugar de ponerlas directamente en el código o la revisión.


1.  Abre la solución (o crea una nueva) en Visual Studio.

2.  Abre package.appxmanifest en Visual Studio, ve a la pestaña **Aplicación** y (para este ejemplo) establece el idioma predeterminado en "en-US". Si hay varios archivos package.appxmanifest en la solución, haz lo siguiente para cada uno.
    <br>**Nota**  De este modo, se especifica el idioma predeterminado del proyecto. Los recursos de idioma predeterminado se usan cuando los idiomas de visualización o el idioma preferido del usuario no coinciden con los recursos de idioma indicados en la aplicación.
3.  Crea una carpeta que contenga los archivos de recursos.
    1.  En el Explorador de soluciones, haz clic con el botón derecho en el proyecto (el proyecto compartido si la solución contiene varios proyectos) y selecciona **Agregar** &gt; **Nueva carpeta**.
    2.  Asigna el nombre "Strings" a la nueva carpeta.
    3.  Si no se muestra la nueva carpeta en el Explorador de soluciones, selecciona **Proyecto** &gt; **Mostrar todos los archivos** en el menú de Microsoft Visual Studio con el proyecto seleccionado.

4.  Crea una subcarpeta y un archivo de recursos para inglés (Estados Unidos).
    1.  Haz clic con el botón derecho en la carpeta Strings y agrega una nueva carpeta debajo. Asígnale el nombre "en-US". El archivo de recursos debe colocarse en una carpeta a la que se ha asignado un nombre para la etiqueta de idioma [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302). Consulta [Cómo asignar nombre a los recursos mediante calificadores](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324) para obtener información detallada sobre el calificador de idioma y ver una lista de etiquetas de idioma comunes.
    2.  Haz clic con el botón derecho en la carpeta en-US y selecciona **Agregar** &gt; **Nuevo elemento…**.
    3.  Selecciona "Archivo de recursos (.resw)".

    4.  Haz clic en **Agregar**. Esto hace que se agregue un archivo de recursos con el nombre predeterminado "Resources.resw". Te recomendamos que uses este nombre de archivo predeterminado. Las aplicaciones pueden particionar sus recursos en otros archivos, pero debes asegurarte de hacer referencia a estos correctamente (consulta el tema [Cómo cargar recursos de cadenas](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)).
    5.  Si tienes archivos .resx únicamente con recursos de cadena de proyectos de .NET anteriores, selecciona **Agregar** &gt; **Elemento existente**, agrega el archivo .resx y cambia el nombre del archivo a .resw.
    6.  Abre el archivo y usa el editor para agregar estos recursos:


        Strings/en-US/Resources.resw ![agregar recurso, inglés](images/addresource-en-us.png) En este ejemplo, "Greeting.Text" y "Farewell" señalan las cadenas que se mostrarán. "Greeting.Width" es la propiedad Width de la cadena "Greeting". Los comentarios constituyen un buen lugar donde proporcionar instrucciones especiales a los traductores que localizan las cadenas a otros idiomas.

## <a name="associate-controls-to-resources"></a>Asocia los controles a los recursos.

Tienes que asociar cada control que necesita texto localizado con el archivo .resw. Haces esto usando el atributo **x:Uid** en todos tus elementos XAML de la siguiente manera:

```XML
<TextBlock x:Uid="Greeting" Text="" />
```

Para el nombre de recurso, proporcionas el valor del atributo **Uid**, además de especificar qué propiedad obtendrá la cadena traducida (en este caso, la propiedad Text). Puedes especificar otros valores o propiedades para diferentes idiomas, como Greeting.Width, pero ten cuidado con las propiedades relacionadas con el diseño de este tipo. Debes tratar de permitir que los controles se distribuyan dinámicamente según la pantalla del dispositivo.

Ten en cuenta que las propiedades se tratan de manera diferente en los archivos resw como AutomationProperties.Name. Tienes que escribir el espacio de nombres explícitamente de la siguiente manera:

```XML
MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name
```

## <a name="add-string-resource-identifiers-to-code-and-markup"></a>Agrega identificadores de recursos de cadena al código y al marcado.

En tu código, puedes hacer referencia de forma dinámica a las cadenas:

**C#**
```CSharp
var loader = new Windows.ApplicationModel.Resources.ResourceLoader();
var str = loader.GetString("Farewell");
```

**C++**
```cpp
auto loader = ref new Windows::ApplicationModel::Resources::ResourceLoader();
auto str = loader->GetString("Farewell");
```


## <a name="add-folders-and-resource-files-for-two-additional-languages"></a>Agrega carpetas y archivos de recursos para dos idiomas adicionales.


1.  Agrega otra carpeta debajo de la carpeta Cadenas para alemán. Asigna el nombre "de-DE" a la carpeta de alemán (Alemania).
2.  Crea otro archivo de recursos en la carpeta de-DE y agrega lo siguiente:

    strings/de-DE/Resources.resw

    ![agrega recurso, alemán](images/addresource-de-de.png)


3.  Crea una carpeta más llamada "fr-FR" para francés (Francia). Crea un nuevo archivo de recursos y agrega lo siguiente:

    strings/fr-FR/Resources.resw
    
    ![add resource, agregar recursos, french, francés](images/addresource-fr-fr.png)

## <a name="build-and-run-the-app"></a>Compila y ejecuta la aplicación.


Prueba la aplicación con el idioma para mostrar predeterminado.

1.  Presiona F5 para compilar y ejecutar la aplicación.
2.  Ten en cuenta que los saludos de bienvenida y despedida (greeting y farewell) se muestran en el idioma preferido del usuario.
3.  Sal de la aplicación.

Prueba la aplicación con otros idiomas.

1.  Abre **Configuración** en el dispositivo.
2.  Selecciona **Hora e idioma**.
3.  Selecciona **Región e idioma** (o en un teléfono o emulador de teléfono, **Idioma**).
4.  Ten en cuenta que el idioma que se mostró al ejecutar la aplicación es el idioma que está más arriba en la lista, es decir, inglés, alemán o francés. Si tu idioma de nivel superior no es ninguno de estos tres, la aplicación retrocede al siguiente de la lista que admita la aplicación.
5.  Si no tienes ninguno de estos tres idiomas en la máquina, agrega a la lista los que falten haciendo clic en **Agregar un idioma**.
6.  Para probar la aplicación con otro idioma, selecciona el idioma en la lista y haz clic en **Establecer como predeterminado** (o en un teléfono o emulador de teléfono, mantén pulsado el idioma en la lista y, a continuación, pulsa **Subir** hasta que esté en la parte superior). Después ejecuta la aplicación.

## <a name="related-topics"></a>Temas relacionados


* [Cómo asignar un nombre a los recursos mediante calificadores](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
* [Cómo cargar recursos de cadenas](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)
* [La etiqueta de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
 

 



