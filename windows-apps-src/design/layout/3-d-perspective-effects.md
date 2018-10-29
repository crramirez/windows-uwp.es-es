---
author: Jwmsft
ms.assetid: 90F07341-01F4-4205-8161-92DD2EB49860
title: Efectos de perspectiva 3D para interfaces de usuario XAML
description: Puedes usar transformaciones de perspectiva para aplicar efectos 3D al contenido de tus aplicaciones de WindowsRuntime. Por ejemplo, puedes crear la ilusión de que un objeto gira hacia ti o se aleja, como se muestra aquí.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f0b50ca4cb3f74c7c8e8bccb4fceb58e0151bcf2
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "5740574"
---
# <a name="3-d-perspective-effects-for-xaml-ui"></a>Efectos de perspectiva 3D para interfaces de usuario XAML


Puedes usar transformaciones de perspectiva para aplicar efectos 3D al contenido de tus aplicaciones de WindowsRuntime. Por ejemplo, puedes crear la ilusión de que un objeto gira hacia ti o se aleja, como se muestra aquí.

![Imagen con transformación de perspectiva](images/3dsimple.png)

Otro uso habitual de las transformaciones de perspectiva es la organización de objetos relacionados entre sí para crear un efecto 3D, como aquí.

![Apilar objetos para crear un efecto 3D](images/3dstacking.png)

Además de crear objetos 3D estáticos, puedes animar las propiedades de la transformación de perspectiva para crear efectos 3D en movimiento.

Acabas de ver las transformaciones de perspectiva aplicadas a imágenes, pero puedes aplicar estos efectos a cualquier [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911), incluidos los controles. Por ejemplo, puedes aplicar un efecto 3D a todo un contenedor de controles como este:

![Efecto 3D aplicado a un contenedor de elementos](images/skewedstackpanel.png)

Este es el código XAML usado para crear este ejemplo:

```xml
<StackPanel Margin="35" Background="Gray">    
    <StackPanel.Projection>        
        <PlaneProjection RotationX="-35" RotationY="-35" RotationZ="15"  />    
    </StackPanel.Projection>    
    <TextBlock Margin="10">Type Something Below</TextBlock>    
    <TextBox Margin="10"></TextBox>    
    <Button Margin="10" Content="Click" Width="100" />
</StackPanel>
```

Aquí nos hemos centrado en las propiedades de [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/BR210192) que se usan para girar y mover objetos en el espacio 3D. El siguiente ejemplo te permite experimentar con estas propiedades y ver su efecto en un objeto.

## <a name="planeprojection-class"></a>Clase PlaneProjection

Puedes aplicar efectos 3D a cualquier [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) estableciendo la propiedad [**Projection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection) del UIElement mediante una clase [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/BR210192). **PlaneProjection** define cómo se representa la transformación en el espacio. El ejemplo siguiente muestra un caso sencillo.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35"   />
    </Image.Projection>
</Image>
```

Esta figura muestra cómo se representa la imagen. Los ejes x, y y z se muestran como líneas rojas. La imagen se gira hacia atrás 35 grados alrededor del eje X mediante la propiedad [**RotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationx).

![RotateX menos 35 grados](images/3drotatexminus35.png)

La propiedad [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy) gira alrededor del eje Y del centro de rotación.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35"   />
    </Image.Projection>
</Image>
```

![RotateY menos 35 grados](images/3drotateyminus35.png)

La propiedad [**RotationZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationz) gira alrededor del eje Z del centro de rotación (una línea perpendicular al plano del objeto).

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationZ="-45"/>
    </Image.Projection>
</Image>
```

![RotateZ menos 45 grados](images/3drotatezminus35.png)

Las propiedades de rotación pueden especificar un valor de giro positivo o negativo en cualquiera de las direcciones. El número absoluto puede ser mayor que 360, que permite girar el objeto más de una rotación completa.

Puedes mover el centro de rotación mediante las propiedades [**CenterOfRotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationx), [**CenterOfRotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationy) y [**CenterOfRotationZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationz). De forma predeterminada, los ejes de rotación pasan por el centro del objeto, lo que hace que el objeto gire alrededor de su centro. Si mueves el centro de rotación al borde exterior del objeto, girará alrededor de dicho borde. Los valores predeterminados de **CenterOfRotationX** y **CenterOfRotationY** son 0,5 y el valor predeterminado de **CenterOfRotationZ** es 0. En el caso de **CenterOfRotationX** y **CenterOfRotationY**, los valores entre 0 y 1 establecen el punto dinámico en algún lugar del objeto. Un valor 0 indica un borde del objeto, y 1 indica el borde opuesto. Se permiten valores fuera de este intervalo, que moverán el centro de rotación como corresponda. Debido a que el eje Z del centro de rotación se dibuja a través del plano del objeto, puedes mover el centro de rotación detrás del objeto mediante un número negativo, o delante del objeto (hacia ti) mediante un número positivo.

[**CenterOfRotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationx) mueve el centro de rotación a lo largo del eje X paralelo al objeto, mientras que [**CenterOfRotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationy) mueve el centro de rotación a lo largo del eje Y del objeto. Las siguientes ilustraciones muestran cómo se usan los diferentes valores para **CenterOfRotationY**.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35" CenterOfRotationY="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationY = "0.5" (predeterminado)**

![CenterOfRotationY es igual a 0,5](images/3drotatexminus35.png)
```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35" CenterOfRotationY="0.1"/>
    </Image.Projection>
</Image>
```

**CenterOfRotationY = "0.1"**

![CenterOfRotationY es igual a 0,1](images/3dcenterofrotationy0point1.png)

Observa cómo gira la imagen alrededor del centro cuando la propiedad [**CenterOfRotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationy) se establece en el valor predeterminado de 0,5 y gira cerca del borde superior cuando se establece en 0,1. Verás un comportamiento similar cuando se cambia la propiedad [**CenterOfRotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationx) para que se mueva al lugar donde la propiedad [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy) gira el objeto.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35" CenterOfRotationX="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationX = "0.5" (predeterminado)**

![CenterOfRotationX es igual a 0,5](images/3drotateyminus35.png)
```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35" CenterOfRotationX="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationX = "0.9" (borde derecho)**

![CenterOfRotationX es igual a 0,9](images/3dcenterofrotationx0point9.png)

Usa [**CenterOfRotationZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationz) para situar el centro de rotación por encima o por debajo del plano del objeto. De esta manera, puedes rotar el objeto alrededor del punto, de forma similar a un planeta que gira alrededor de una estrella.

## <a name="positioning-an-object"></a>Posicionamiento de un objeto

Hasta ahora, has aprendido cómo girar un objeto en el espacio. Puedes colocar estos objetos que giran en el espacio relacionados entre sí con una de estas propiedades:

-   [**LocalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetx) mueve un objeto a lo largo del eje X del plano de un objeto que gira.
-   [**LocalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsety) mueve un objeto a lo largo del eje Y del plano de un objeto que gira.
-   [**LocalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetz) mueve un objeto a lo largo del eje Z del plano de un objeto que gira.
-   [**GlobalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetx) mueve un objeto a lo largo del eje X alineado con la pantalla.
-   [**GlobalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsety) mueve un objeto a lo largo del eje Y alineado con la pantalla.
-   [**GlobalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetz) mueve un objeto a lo largo del eje Z alineado con la pantalla.

**Desplazamiento local**

Las propiedades [**LocalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetx), [**LocalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsety) y [**LocalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetz) trasladan un objeto a lo largo del eje respectivo del plano del objeto después de que haya girado. Por consiguiente, la rotación del objeto determina la dirección en que este se traslada. Para demostrar este concepto, el ejemplo siguiente anima **LocalOffsetX** de 0 a 400 y [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy) de 0 a 65grados.

Observa en el ejemplo anterior que el objeto se mueve a lo largo de su propio eje X. Muy al principio de la animación, cuando el valor de [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy) es casi cero (paralelo a la pantalla), el objeto se mueve a lo largo de la pantalla en la dirección X, pero a medida que el objeto gira acercándose, se mueve hacia ti a lo largo del eje X del plano del objeto. Por otra parte, si animaste la propiedad **RotationY** a -65 grados, el objeto se alejará describiendo una curva.

[**LocalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsety) funciona de manera similar a [**LocalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetx), excepto que se mueve a lo largo del eje vertical, por lo que cambiar [**RotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationx) afecta a la dirección en la que **LocalOffsetY** mueve el objeto. En el ejemplo siguiente, **LocalOffsetY** se anima de 0 a 400 y **RotationX** de 0 a 65grados.

[**LocalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetz) traslada el objeto perpendicular al plano del objeto como si se hubiera dibujado un vector directamente a través del centro, desde detrás del objeto hacia ti. Para demostrar cómo funciona **LocalOffsetZ**, en el ejemplo siguiente se anima **LocalOffsetZ** de 0 a 400 y [**RotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationx) de 0 a 65grados.

Al principio de la animación, cuando el valor de [**RotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationx) está cerca de cero (paralelo a la pantalla), el objeto se mueve directamente hacia ti, pero a medida que la cara del objeto gira hacia abajo, este se mueve hacia abajo.

**Desplazamiento global**

Las propiedades [**GlobalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetx), [**GlobalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsety) y [**GlobalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetz) trasladan el objeto a lo largo de los ejes relativos a la pantalla. Es decir, a diferencia de las propiedades de desplazamiento local, el eje por el que se mueve el objeto es independiente de cualquier rotación que se le aplique. Estas propiedades resultan útiles cuando solo quieres mover el objeto a lo largo de los ejes X, Y o Z de la pantalla sin preocuparte por la rotación que le aplique.

El ejemplo siguiente anima [**GlobalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetx) de 0 a 400 y [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy) de 0 a 65grados.

Observa en este ejemplo que el objeto no cambia su curso a medida que gira. Esto se debe a que el objeto se mueve a lo largo del eje x de la pantalla independientemente de su rotación.

## <a name="positioning-an-object"></a>Posicionamiento de un objeto

Puedes usar los tipos [**Matrix3DProjection**](https://msdn.microsoft.com/library/windows/apps/BR210128) y [**Matrix3D**](https://msdn.microsoft.com/library/windows/apps/BR243266) para escenarios semi-3D más complejos que los que son posibles con [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/BR210192). **Matrix3DProjection** proporciona una matriz de transformación 3D completa para aplicar a cualquier [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911), por lo que puedes aplicar matrices de perspectiva y matrices de transformación de modelos arbitrarias a los elementos. Ten en cuenta que estas API son mínimas y, si las usas, tendrás que escribir el código que crea correctamente las matrices de transformación 3D. Por este motivo, es más fácil usar **PlaneProjection** para escenarios 3D sencillos.
