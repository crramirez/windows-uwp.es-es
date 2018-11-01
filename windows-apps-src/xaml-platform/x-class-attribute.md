---
author: jwmsft
description: Configura la compilación de XAML para unir clases parciales entre el marcado y el código subyacente. La clase parcial de código se define en un archivo de código separado y la clase parcial de marcado se crea mediante la generación del código durante la compilación de XAML.
title: Atributo xClass
ms.assetid: 40A7C036-133A-44DF-9D11-0D39232C948F
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6746969b1b717183894d6b941be41c9aca452960
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5874246"
---
# <a name="xclass-attribute"></a>Atributo x:Class


Configura la compilación de XAML para unir clases parciales entre el marcado y el código subyacente. La clase parcial de código se define en un archivo de código separado y la clase parcial de marcado se crea mediante la generación del código durante la compilación de XAML.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML


``` syntax
<object x:Class="namespace.classname"...>
  ...
</object>
```

## <a name="xaml-values"></a>Valores de XAML

| Término | Descripción |
|------|-------------|
| espacio de nombres | Opcional. Especifica un espacio de nombres que contiene la clase parcial identificada por _classname_. Si se especifica _namespace_, un punto (.) separa _namespace_ y _classname_. Si se especifica _namespace_ , se da por hecho que _classname_ no tiene espacio de nombres. |
| classname | Obligatorio. Especifica el nombre de la clase parcial que conecta el código XAML cargado y el código subyacente para ese XAML. | 

## <a name="remarks"></a>Observaciones

**x:Class** se puede declarar como un atributo para cualquier elemento que sea la raíz de un árbol de archivos u objetos XAML y que se compile mediante acciones de compilación, o para la raíz de [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) en la definición de una aplicación compilada. Declarar **x:Class** en cualquier elemento que no sea una raíz de nodo y en cualquier circunstancia referente a un archivo XAML que no esté compilado mediante la acción de compilación **Page**, genera un error en tiempo de compilación.

La clase que se utiliza como **x:Class** no puede ser una clase anidada.

El valor del atributo **x:Class** debe ser una cadena que especifique el nombre completo de una clase. Puedes omitir la información del espacio de nombres si esa es también la forma en que se estructura el código subyacente (la definición de la clase comienza en el nivel de clase). El archivo de código subyacente de una definición de página o de aplicación debe estar dentro de un archivo de código que se incluya como parte del proyecto. La clase de código subyacente debe ser pública. La clase de código subyacente debe ser parcial.

## <a name="clr-language-rules"></a>Reglas del lenguaje CLR

Si bien el archivo de código subyacente puede ser un archivo de C++, existen ciertas convenciones que aún siguen el formato del lenguaje CLR para que no haya diferencias en la sintaxis de XAML. En particular, el separador entre los componentes del espacio de nombres y de la clase de nombres de un valor **x:Class** es siempre un punto ("."), aunque el separador para el caso equivalente en el archivo de código C++ asociado al código XAML sea "::". Si declaras espacios de nombres anidados en C++, el separador entre las cadenas sucesivas de espacios de nombres anidados también debe ser "." en lugar de "::" cuando especifiques la parte *namespace* del valor **x:Class**.

