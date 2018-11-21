---
author: jwmsft
ms.assetid: 159681E4-BF9E-4A57-9FEE-EC7ED0BEFFAD
title: Sugerencias de rendimiento de MVVM y lenguaje
description: En este tema se describen algunos aspectos a tener en cuenta acerca del rendimiento y que están relacionados con la elección de patrones de diseño del software y del lenguaje de programación.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b39f68d6f5f00f8e33a080b77614b9ba11627989
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "7423654"
---
# <a name="mvvm-and-language-performance-tips"></a>Sugerencias de rendimiento de MVVM y lenguaje


En este tema se describen algunos aspectos a tener en cuenta acerca del rendimiento y que están relacionados con la elección de patrones de diseño del software y del lenguaje de programación.

## <a name="the-model-view-viewmodel-mvvm-pattern"></a>Patrón Model-View-ViewModel (MVVM)

El patrón Model-View-ViewModel (MVVM) es común en muchas aplicaciones XAML. (MVVM es muy similar a la descripción del Fowler del patrón Model-View-Presenter, pero se ha adaptado para XAML). El problema con el patrón MVVM es que puede provocar inadvertidamente aplicaciones con demasiadas capas y demasiadas asignaciones. Estas son las motivaciones para el uso de MVVM.

-   **Separación de problemas**. Siempre es útil dividir un problema en partes más pequeñas y un patrón como MVVM o MVC es una forma de dividir una aplicación (o incluso un control único) en partes más pequeñas: la vista real, un modelo lógico de la vista (modelo de vista) y la lógica de aplicación independiente de la vista (el modelo). En particular, es un flujo de trabajo popular para que los diseñadores posean la vista con una herramienta, los desarrolladores posean el modelo con otra herramienta los integradores de diseño posean el modelo de vista con ambas herramientas.
-   **Pruebas de unidad**. Puedes realizar la prueba de unidad para el modelo de vista (y, en consecuencia, el modelo) independiente de la vista, por lo que no hace falta depender de la creación de ventanas, impulsar entradas, etc. Al mantener la vista pequeña, puedes probar una gran parte de la aplicación sin tener que crear una ventana.
-   **Agilidad en los cambios de la experiencia del usuario**. La vista suele ver los cambios más frecuentes y los más recientes, a medida que la experiencia del usuario se ajusta en función de los comentarios de los usuarios finales. Al mantener la vista independiente, estos cambios se pueden aplicar más rápidamente y con menos renovación de la aplicación.

Hay varias definiciones concretas del patrón MVVM y marcos de terceros que ayudan a implementarlo. Sin embargo, una adherencia estricta a cualquier variación del patrón puede resultar en aplicaciones con mucha más sobrecarga que se pueda justificar.

-   Los enlaces de datos XAML (la extensión de marcado {Binding}) se diseñaron en parte para habilitar los patrones de modelos y vistas. Sin embargo, {Binding} aporta un conjunto de trabajo no trivial y la sobrecarga de la CPU. La creación de un elemento {Binding} provoca una serie de asignaciones, y la actualización de un destino de enlace puede provocar reflejos y conversiones boxing. Estos problemas se han solucionado con la extensión de marcado {x:Bind}, que compila los enlaces en tiempo de compilación. **Recomendación:** usa {x:Bind}.
-   Es común en MVVM conectar Button.Click al modelo de vista con un objeto ICommand, tales como las aplicaciones auxiliares comunes DelegateCommand o RelayCommand. Sin embargo, estos comandos son asignaciones adicionales, incluido el agente de escucha de eventos CanExecuteChanged, lo que agrega al conjunto de trabajo y al tiempo de inicio o navegación de la página. **Recomendación:** como alternativa al uso de la cómoda interfaz ICommand, puedes tener en cuenta la posibilidad de colocar los controladores de eventos en el código subyacente, adjuntarlos a los eventos de vista y llamar a un comando en el modelo de vista cuando se generen los eventos. También tendrás que agregar código adicional para deshabilitar el elemento Button cuando el comando no está disponible.
-   Es común en MVVM crear una página con todas las configuraciones posibles de la interfaz de usuario y luego contraer partes del árbol enlazando la propiedad Visibility a las propiedades en la máquina virtual. Esto agrega tiempo de inicio innecesariamente y posiblemente al conjunto de trabajo (ya que algunas partes del árbol quizás nunca se hagan visibles). **Recomendaciones:** usa la función [x:Load attribute](../xaml-platform/x-load-attribute.md) o [x:DeferLoadStrategy attribute](../xaml-platform/x-deferloadstrategy-attribute.md) para aplazar del inicio las partes innecesarias del árbol. Además, crea controles de usuario separados para los diferentes modos de la página y usa el código subyacente para mantener cargados solo los controles necesarios.

## <a name="ccx-recommendations"></a>Recomendaciones para C++/CX

-   **Usa la versión más reciente**. Se presentan mejoras continuamente en el rendimiento del compilador de C++/CX. Asegúrate de que crear tu aplicación con el conjunto de herramientas más reciente.
-   **Deshabilita RTTI (/GR-)**. RTTI está activado de manera predeterminada en el compilador, por lo tanto, a menos que tu entorno de compilación lo desactiva, probablemente lo estés usando. RTTI tiene una sobrecarga significativa y, a menos que el código dependa mucho de él, deberías desactivarlo. El marco de XAML no tiene ningún requisito que exige que tu código use RTTI.
-   **Evita el uso elementos ppltasks intensivos**. Los elementos Ppltasks son muy convenientes cuando se llama a las API de WinRT asincrónicas, pero incluyen una sobrecarga significativa en relación con el tamaño del código. El equipo de C++/CX está trabajando en una función del lenguaje (await) que proporcionará un rendimiento mucho mejor. Mientras tanto, equilibra el uso de los elementos ppltasks en las rutas de acceso activas del código.
-   **Evita el uso de C++/CX en la "lógica de negocios" de la aplicación**. C++/CX está diseñado para ofrecer una manera cómoda de acceder a las API de WinRT desde las aplicaciones C++. Hace uso de contenedores que tienen sobrecarga. Debes evitar el uso de C++/CX dentro de la lógica o modelo de negocio de tu clase y usarlo en los límites entre el código y WinRT.