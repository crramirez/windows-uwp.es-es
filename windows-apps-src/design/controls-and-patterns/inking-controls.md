---
Description: Herramientas de lápiz que se describen
title: Controles de entrada manuscrita
label: Inking Controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 97eae5f3-c16b-4aa5-b4a1-dd892cf32ead
ms.localizationpriority: medium
ms.openlocfilehash: 848d682b2672e1a397e3ac867ae981c707873e8e
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970420"
---
# <a name="inking-controls"></a>Controles de entrada manuscrita



Hay dos controles diferentes que facilitan la entrada manuscrita en aplicaciones de Windows: [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) e [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar).

El control InkCanvas representa la entrada manuscrita como un trazo de lápiz (con la configuración predeterminada de color y espesor) o un trazo de borrado. Este control es una superposición transparente que no incluye ninguna interfaz de usuario integrada para cambiar las propiedades de trazo de lápiz predeterminadas.

> [!NOTE]
> InkCanvas se puede configurar para admitir funcionalidades similares para la entrada del mouse y táctil.

Dado que el control InkCanvas no ofrece soporte para cambiar la configuración de trazo de lápiz predeterminada, se puede emparejar con un control InkToolbar. El control InkToolbar contiene una colección personalizable y extensible de botones que activan características relacionadas con las entradas de lápiz en un control InkCanvas asociado.

De manera predeterminada, el control InkToolbar incluye botones para dibujar, borrar, resaltar y mostrar una regla. Dependiendo de la característica, en un control flotante se proporcionan otros comandos y opciones de configuración, como los destinados a definir el color y el grosor del trazo o a borrar todas las entradas de lápiz.

> [!NOTE]
> InkToolbar admite la entrada manuscrita y del mouse, y se puede configurar para que reconozca la entrada táctil.

<img src="images/ink-tools-invoked-toolbar.png" width="300" alt="InkToolbar palette flyout">

> **API importantes**: [Clase InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), [Clase InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar), [Clase InkPresenter](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter), [Clase Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking)


## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa el control InkCanvas cuando necesites habilitar las funciones de entrada manuscrita básicas en tu aplicación sin proporcionar ninguna configuración de entrada de lápiz al usuario.

De manera predeterminada, los trazos se representan como entrada de lápiz al usar la punta del lápiz (un bolígrafo negro con un grosor de 2 píxeles) y como un borrador al usar el extremo borrador. Si no hay ningún extremo borrador, el control InkCanvas puede configurarse para procesar la entrada de la punta del lápiz como trazos de borrado.

Empareja el control InkCanvas con un control InkToolbar para proporcionar una interfaz de usuario para activar las características de entrada de lápiz y establecer propiedades de entrada de lápiz básicas, como el tamaño y el color del trazo o la forma de la punta del lápiz.

> [!NOTE] 
> Para realizar una mayor personalización del trazo de lápiz que se representa en un control InkCanvas, usa el objeto subyacente [InkPresenter](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/InkCanvas">abrir la aplicación y ver InkCanvas en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

**Microsoft Edge**

Microsoft Edge usa los controles InkCanvas e InkToolbar para las **notas web**.  
![El control InkCanvas se usa para la entrada de lápiz en Microsoft Edge](images/ink-tools-edge.png)

**Área de trabajo de Windows Ink**

Los controles InkCanvas e InkToolbar también se usan para las opciones **Bloc de bocetos** y **Anotación en captura de pantalla** en el **Área de trabajo de Windows Ink**.  
![Control InkToolbar en el Área de trabajo de Windows Ink](images/ink-tools-ink-workspace.png)

## <a name="create-an-inkcanvas-and-inktoolbar"></a>Crear un control InkCanvas y un control InkToolbar

Para agregar un control InkCanvas a tu aplicación se requiere una sola línea de marcado:

```xaml
<InkCanvas x:Name="myInkCanvas"/>
```

> [!NOTE]
> Para la personalización detallada de InkCanvas mediante InkPresenter, consulta el artículo ["Interacciones de lápiz y Windows Ink en aplicaciones de Windows"](https://docs.microsoft.com/windows/uwp/design/input/pen-and-stylus-interactions).

El control InkToolbar debe usarse junto con un control InkCanvas. La incorporación de un control InkToolbar (con todas las herramientas integradas) en la aplicación requiere una línea de marcado adicional:

 ```xaml
<InkToolbar TargetInkCanvas="{x:Bind myInkCanvas}"/>
 ```

Esta línea muestra el control InkToolbar siguiente:
<img src="images/ink-tools-uninvoked-toolbar.png" width="250" alt="Basic InkToolbar">

### <a name="built-in-buttons"></a>Botones integrados

El control InkToolbar incluye los siguientes botones integrados:

**Lápices**

- Bolígrafo: dibuja un trazo opaco sólido con una punta de lápiz redonda. El tamaño del trazo depende de la presión del lápiz que se detecte.
- Lápiz: dibuja un trazo de borde suave, semitransparente y con textura (útil para efectos de sombreado superpuestos) con una punta de lápiz redonda. El color del trazo (oscuridad) depende de la presión del lápiz que se detecte.
- Marcador de resaltado: dibuja un trazo semitransparente con una punta de lápiz rectangular.

Puedes personalizar los atributos de paleta de colores y tamaño (mínimo, máximo y predeterminado) en el control flotante de cada lápiz.

**Herramienta**

- Borrador: elimina cualquier trazo de lápiz que se toque. Ten en cuenta que se eliminará todo el trazo de lápiz, no solo la parte que se encuentre debajo del trazo de borrador.

**Alternancia**

- Regla: muestra u oculta la regla. Al dibujar cerca del borde de la regla, el trazo de lápiz se ajusta a la regla.  
 ![Elemento visual Regla asociado al control InkToolbar](images/inking-tools-ruler.png)

Aunque esta es la configuración predeterminada, tienes control total sobre los botones integrados que se incluyen en el control InkToolbar para tu aplicación.

### <a name="custom-buttons"></a>Botones personalizados

El control InkToolbar consta de dos grupos de tipos de botones:

1. Un grupo de botones de "herramienta" que contiene los botones para dibujar, borrar y resaltar integrados. Las herramientas y los lápices personalizados se agregan aquí.
> [!NOTE]
> La selección de características es mutuamente exclusiva.

2. Un grupo de botones de "alternancia" que contiene el botón de regla integrado. Las alternancias personalizadas se agregan aquí.
> [!NOTE]
> Las características no son mutuamente exclusivas y se pueden usar simultáneamente con otras herramientas activas.

Dependiendo de la aplicación y de la funcionalidad de entrada manuscrita necesaria, puedes agregar cualquiera de los siguientes botones (enlazados a sus características de entrada de lápiz personalizada) al control InkToolbar:

- Lápiz personalizado: lápiz para el cual la aplicación host define las propiedades de punta de lápiz y paleta de colores del trazo, como la forma, la rotación y el tamaño.
- Herramienta personalizada: herramienta que no es un lápiz definida por la aplicación host.
- Alternancia personalizada: establece el estado de una característica definida por la aplicación en activado o desactivado. Cuando se activa, la característica funciona junto con la herramienta activa.

> [!NOTE]
> No puedes cambiar el orden de visualización de los botones integrados. El orden de presentación predeterminado es: bolígrafo, lápiz, marcador de resaltado, borrador y regla. Los lápices personalizados se agregan al último lápiz predeterminado, los botones de la herramienta personalizada se agregan entre el último botón del lápiz y el botón de borrador, y los botones de alternancia personalizada se agregan detrás del botón de regla. (Los botones personalizados se agregan en el orden en que se especifican).

Si bien el control InkToolbar puede ser un elemento de primer nivel, por lo general se expone a través de un comando o botón "Entrada manuscrita". Se recomienda usar el glifo EE56 de la fuente Segoe MDL2 Assets como un icono de nivel superior.

## <a name="inktoolbar-interaction"></a>Interacción del control InkToolbar

Todos los botones de lápiz y herramienta integrados incluyen un menú de control flotante donde se pueden establecer las propiedades de entrada de lápiz, así como la forma y el tamaño de la punta del lápiz. Un "glifo de extensión" ![Glifo de InkToolbar](images/ink-tools-glyph.png) se muestra en el botón para indicar la existencia del control flotante.

El control flotante se muestra cuando se selecciona de nuevo el botón de una herramienta activa. Cuando se cambia el color o el tamaño, el control flotante se descarta automáticamente y se puede reanudar la entrada manuscrita. Las herramientas y los lápices personalizados pueden usar el control flotante predeterminado o especificar un control flotante personalizado.

El borrador también tiene un control flotante que proporciona el comando **Borrar todas las entradas de lápiz**.  
![InkToolbar con el control flotante de borrador invocado](images/ink-tools-erase-all-ink.png)

 Para obtener información sobre la personalización y la extensibilidad, echa un vistazo a la [muestra de SimpleInk](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk).

## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

- El control InkCanvas, y la entrada manuscrita en general, funcionan mejor con un lápiz activo. Sin embargo, se recomienda admitir la entrada manuscrita con la entrada del mouse y táctil (incluido el lápiz pasivo) si tu aplicación lo requiere.
- Usa un control InkToolbar con el control InkCanvas para proporcionar la configuración y las características de entrada manuscrita básicas. Ambos controles InkCanvas e InkToolbar pueden personalizarse mediante programación.
- El control InkToolbar, y la entrada manuscrita en general, funcionan mejor con un lápiz activo. Sin embargo, la entrada manuscrita con el mouse y táctil puede admitirse si tu aplicación lo requiere.
- Si admites la entrada manuscrita con la entrada táctil, se recomienda usar el icono de "ED5F" de la fuente "Segoe MLD2 Assets" para el botón de alternancia, con una información sobre herramientas "Escritura táctil".
- Si se proporciona selección de trazo, se recomienda usar el icono de EF20 de la fuente Segoe MLD2 Assets para el botón de herramienta, con una información sobre herramientas "Herramienta de selección".
- Si se usa más de un control InkCanvas, se recomienda usar un solo control InkToolbar para controlar la entrada manuscrita en lienzos.
- Para obtener un rendimiento óptimo, se recomienda modificar el control flotante predeterminado, en lugar de crear uno personalizado para las herramientas predeterminadas y personalizadas.

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de SimpleInk](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk): muestra los 8 escenarios en torno a las funcionalidades de personalización y extensibilidad de los controles InkCanvas e InkToolbar. Cada escenario proporciona pautas básicas sobre las situaciones de entrada manuscrita e implementaciones de controles comunes.
- [Muestra de ComplexInk](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk): muestra escenarios de entradas manuscritas más avanzados.
- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Interacciones de lápiz y Windows Ink en aplicaciones de Windows](https://docs.microsoft.com/windows/uwp/design/input/pen-and-stylus-interactions)
- [Reconocer trazos de Windows Ink como texto y formas](https://docs.microsoft.com/windows/uwp/design/input/convert-ink-to-text)
- [Almacenar y recuperar datos de trazos de lápiz de Windows Ink](https://docs.microsoft.com/windows/uwp/design/input/save-and-load-ink)
