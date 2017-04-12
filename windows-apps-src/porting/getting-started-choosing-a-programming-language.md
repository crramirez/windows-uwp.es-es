---
author: mcleblanc
title: "Elección de un lenguaje de programación"
ms.assetid: 6CA46432-BF03-4B20-9187-565B3503B497
description: "Elección de un lenguaje de programación"
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: b20066ffbd32984d20fdbdcc0915ef10332c6f12
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="getting-started-choosing-a-programming-language"></a>Introducción: elección de un lenguaje de programación

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## <a name="choosing-a-programming-language"></a>Elección de un lenguaje de programación

Antes de continuar, debes conocer los lenguajes de programación que puedes elegir cuando desarrollas aplicaciones para la Plataforma universal de Windows (UWP). Aunque en los tutoriales de este artículo se usa C#, puedes desarrollar aplicaciones para UWP con uno o varios lenguajes de programación (consulta [Lenguajes, herramientas y marcos](https://msdn.microsoft.com/library/windows/apps/dn465799)).

Puedes usar C++, C#, MicrosoftVisual Basic y JavaScript para desarrollar. JavaScript usa el marcado HTML5 para el diseño de la UI, mientras que el resto de los lenguajes emplea un lenguaje de marcado conocido como *lenguaje XAML* para describir sus UI.

Aunque en este artículo nos centramos en C#, los otros lenguajes ofrecen ventajas únicas que te podrían interesar. Por ejemplo, si uno de los factores de mayor importancia es el rendimiento de la aplicación, especialmente para aplicaciones con muchos gráficos, C++ podría ser la elección correcta. La versión Microsoft .NET de Visual Basic es excelente para desarrolladores de aplicaciones en Visual Basic. JavaScript con HTML5 es excelente para los desarrolladores con experiencia en desarrollo web. Para obtener más información, consulta uno de los recursos siguientes:

-   [Crear la primera aplicación de la Tienda Windows con C++](https://msdn.microsoft.com/library/windows/apps/hh974580)
-   [Crear la primera aplicación de la Tienda Windows con C# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/hh974581)
-   [Crear la primera aplicación de la Tienda Windows con JavaScript](https://msdn.microsoft.com/library/windows/apps/br211385)
-   [Crear la primera aplicación de la Tienda de Windows Phone con C# o Visual Basic](http://go.microsoft.com/fwlink/p/?LinkID=397877)
-   [WinJS en Windows Phone 8.1](http://go.microsoft.com/fwlink/p/?LinkID=397879)

**Nota:** Para las aplicaciones que usan elementos gráficos en 3D, los estándares OpenGL y OpenGL ES no están disponibles de forma nativa para las aplicaciones para UWP. Si no quieres reescribir el código OpenGL ES en Microsoft DirectX, tal vez te interese informarte sobre **Angle**. Angle es un proyecto actualmente en curso diseñado para convertir OpenGL a DirectX mediante la traducción de llamadas de la API de OpenGL a llamadas de la API de DirectX. Para conocer más, consulta lo siguiente:
-   [Ángulo](https://code.google.com/p/angleproject/)
-   [Crear tu primera aplicación de la Tienda Windows con DirectX](https://msdn.microsoft.com/library/windows/apps/br229580)
-   [Ejemplos de aplicaciones de la Tienda Windows con DirectX](http://go.microsoft.com/fwlink/p/?LinkId=263603)
-   [¿Dónde está el SDK de DirectX?](https://msdn.microsoft.com/library/windows/desktop/ee663275)

## <a name="giving-c-a-go"></a>Dale una oportunidad a C#

Como desarrollador de iOS, estás acostumbrado a Objective-C y Swift. El lenguaje de programación de Microsoft que más se les parece es C#. Creemos que C# es el lenguaje más fácil y rápido de aprender y usar para la mayoría de los desarrolladores y las aplicaciones. Por eso la información y los tutoriales de este artículo se centran en este lenguaje. Si quieres obtener más información sobre C#, consulta estos recursos:

-   [Crear la primera aplicación de la Tienda Windows con C# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/hh974581)
-   [Ejemplos de aplicaciones de la Tienda Windows que usan C#](http://go.microsoft.com/fwlink/p/?LinkId=263453)
-   [Visual C#](http://go.microsoft.com/fwlink/p/?LinkId=263450)

A continuación se muestra una clase escrita en Objective-C y C#. Primero se muestra la versión en Objective-C, seguida de la versión en C#.

```obj-c
// Objective-C header: SampleClass.h.

#import <Foundation/Foundation.h>

@interface SampleClass : NSObject {
    BOOL localVariable;
}

@property (nonatomic) BOOL localVariable;

-(int) addThis: (int) firstNumber andThis: (int) secondNumber;

@end
```

```obj-c
// Objective-C implementation.

#import "SampleClass.h"

@implementation SampleClass

@synthesize localVariable = _localVariable;

- (id)init {
    self = [super init];
    if (self) {
        localVariable = true;
    }
    return self;
}

-(int) addThis: (int) firstNumber andThis: (int) secondNumber {
    return firstNumber + secondNumber;
}

@end
```

```obj-c
// Objective-C usage.

SampleClass *mySampleClass = [[SampleClass alloc] init];
mySampleClass.localVariable = false;
int result = [mySampleClass addThis:1 andThis:2];
```

Ahora, la versión en C#. Verás que, al igual que en Swift, el encabezado y la implementación no se encuentran en archivos separados.

```csharp
// C# header and implementation.

using System;

namespace MyApp  // Defines this code' s scope.
{
    class SampleClass
    {
        private bool localVariable;

        public SampleClass() // Constructor.
        {
            localVariable = true;
        }

        public bool myLocalVariable // Property.
        {
            get
            {
                return localVariable;
            }
            set
            {
                localVariable = value; 
            }
        }

        public int AddTwoNumbers(int numberOne, int numberTwo)
        {
            return numberOne + numberTwo;
        }        
    }
}
```

```csharp
// C# usage.

SampleClass mySampleClass = new SampleClass();
mySampleClass.myLocalVariable = false;
int result = mySampleClass.AddTwoNumbers(1, 2);
```

C# es un lenguaje sencillo de entender e incluye muchos marcos y clases de soportes que componen .NET. En muy poco tiempo, disfrutarás escribiendo tu código sin un solo corchete a la vista.

## <a name="next-step"></a>Paso siguiente

[Introducción: desplazamiento por Visual Studio](getting-started-getting-around-in-visual-studio.md)
