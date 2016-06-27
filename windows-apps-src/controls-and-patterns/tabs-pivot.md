---
author: Jwmsft
Description: "Las pestañas y los controles dinámicos permiten a los usuarios navegar entre contenido al que se accede con frecuencia."
title: "Pestañas y controles dinámicos"
ms.assetid: 556BC70D-CF5D-4295-A655-D58163CC1824
label: Tabs and pivots
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 6585a75f08a64b7bb8f27fd32a227fa49bb0f3f4

---
# Control dinámico y pestañas

Los controles dinámicos y el patrón de pestañas se usan para navegar por categorías de contenido distintas a las que se accede con frecuencia. Los controles dinámicos y las pestañas se componen de dos o más paneles de contenido con encabezados de categoría correspondientes. Los encabezados se mantienen en pantalla y tienen un estado de selección que se muestra con claridad, para que los usuarios conozcan siempre en qué categoría están.
![Ejemplos de pestañas](images/HIGSecOne_Tabs.png)

Las pestañas son una variante visual de control dinámico y se crean mediante el control [**Pivot**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx). [
              Una **Muestra de código**
            ](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlPivot), que indica cómo personalizar el control dinámico está disponible en GitHub.

<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [**Clase Pivot**](https://msdn.microsoft.com/library/windows/apps/dn608241)

## Patrón de pestañas/controles dinámicos

Al crear una aplicación con el patrón de pestañas/controles dinámicos, hay algunas variables de diseño importantes a tener en cuenta.

- **Etiquetas de encabezado.**  Los encabezados pueden tener un icono con texto o solo texto.
- **Alineación de encabezados.**  Los encabezados pueden dejarse centrados o alineados a la izquierda.
- **Navegación de nivel inferior o de nivel superior.**  Las pestañas o los controles dinámicos se pueden usar para cualquier nivel de navegación. Opcionalmente, el [panel de navegación](nav-pane.md) puede servir como nivel principal con pestañas o controles dinámicos que actúen como secundarios.
- **Compatibilidad con gestos táctiles.**  En los dispositivos que admiten los gestos táctiles, puedes usar uno de los dos conjuntos de interacción para navegar entre las categorías de contenido:
    1. Toca un encabezado de pestaña/control dinámico para navegar a esa categoría.
    2. Desplázate hacia la izquierda o la derecha en el área de contenido para navegar a la categoría adyacente.

## Ejemplos

Control dinámico predeterminado en los recordatorios de Cortana.

![Ejemplo de control dinámico en los recordatorios de Cortana](images/pivot_cortana-reminders.png)

Patrón de pestañas en la aplicación Alarmas y reloj.

![Ejemplo de pestañas en Alarmas y reloj](images/tabs_alarms-and-clock.png)

## Crear un control dinámico

El control [**Pivot**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx) incluye la funcionalidad básica que se describe en esta sección.

Este XAML crea un control dinámico básico con 3 secciones de contenido.

```xaml
<Pivot x:Name="rootPivot" Title="Pivot Title">
    <PivotItem Header="Pivot Item 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 1."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 2."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 3."/>
    </PivotItem>
</Pivot>
```

### Elementos de tabla dinámica

La tabla dinámica es un [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx), de modo que puede contener una colección de elementos de cualquier tipo. Cualquier elemento que agregues a la tabla dinámica que no sea explícitamente un [**PivotItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx) se encapsula implícitamente en un PivotItem. Dado que una tabla dinámica a menudo se usa para navegar entre páginas de contenido, es común para rellenar la colección [**Elementos**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) directamente con los elementos de la interfaz de usuario de XAML. O puedes establecer la propiedad [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) en un origen de datos. Los elementos enlazados en ItemsSource pueden ser de cualquier tipo, pero si no son explícitamente PivotItems, debes definir una [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) y [**HeaderTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx) para especificar cómo se muestran los elementos.

Puedes usar la propiedad [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) para obtener o configurar el elemento activo de la tabla dinámica. Usa la propiedad [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) para obtener o configurar el índice del elemento activo.

### Encabezados dinámicos

Puedes usar las propiedades [**LeftHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) y [**RightHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) para agregar otros controles al encabezado dinámico.

### Interacción dinámica

El control ofrece estas interacciones de gestos táctiles:

-   Tocar un encabezado de elemento de control dinámico permite navegar al contenido de la sección de dicho encabezado.
-   Pasar el dedo a la izquierda o la derecha en un encabezado de elemento de control dinámico permite navegar a la sección adyacente.
-   Pasar el dedo a la izquierda o la derecha en el contenido de una sección permite navegar a la sección adyacente.

El control tiene dos modos:

**Inmóvil**

-   Los controles dinámicos están inmóviles cuando todos los encabezados de control dinámico caben en el espacio permitido.
-   Al tocar una etiqueta de tabla dinámica se navega a la página correspondiente, aunque no se moverá la propia tabla dinámica. La tabla dinámica activa se destacará.

**Carrusel**

-   Las tablas dinámicas giran cuando no caben todos los encabezados de tabla dinámica dentro del espacio permitido.
-   Al tocar una etiqueta de control dinámico se navega a la página correspondiente y, después, la etiqueta de control dinámico activa gira a la primera posición.
-   Gira elementos en un bucle de carrusel de la última a la primera sección de control dinámico.

## Recomendaciones

-   Basa la alineación de los encabezados de pestaña/control dinámico en el tamaño de la pantalla. Para tamaños de pantalla inferiores a 720 epx, normalmente funciona mejor la alineación al centro, mientras que en la mayoría de casos se recomienda la alineación a la izquierda para anchos de pantalla superiores a 720 epx.
-   Evita usar más de 5 encabezados cuando uses el modo de carrusel (ida y vuelta), ya que un bucle de más de 5 puede resultar confuso.
-   Usa el patrón de pestañas solo si los elementos de control dinámico tienen iconos diferentes.
-   Incluye texto en los encabezados de elementos de control dinámico para ayudar a los usuarios a comprender el significado de cada sección de control dinámico. Los iconos no son necesariamente descriptivos para todos los usuarios.



## Temas relacionados

[Conceptos básicos de diseño de la navegación](https://msdn.microsoft.com/library/windows/apps/dn958438)



<!--HONumber=Jun16_HO3-->


