---
ms.assetid: 64F7FC51-E8AC-4098-9C5F-0172E4724B5C
title: Rendimiento
description: Obtenga información sobre cómo organizar el flujo de trabajo de rendimiento, solucionar problemas de animaciones y los problemas de velocidad de fotogramas y ajustar el tiempo de inicio, tiempo de navegación de páginas y el uso de la memoria.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f81052336985eee866fec6ae370d2795e8001859
ms.sourcegitcommit: 0c4bbaf1c119a84002748cdcf02e1449835559c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92133098"
---
# <a name="performance"></a>Rendimiento


Los usuarios esperan que sus aplicaciones sean dinámicas, que su uso sea natural y que no agoten fácilmente la batería. Técnicamente, el rendimiento es un requisito no funcional, pero tratarlo como una característica te ayudará a cumplir las expectativas de los usuarios. Especificar objetivos y realizar mediciones son factores clave. Determina cuáles son los escenarios críticos para el rendimiento, define lo que significa un buen rendimiento. A continuación, puedes realizar mediciones desde el principio y con la suficiente frecuencia durante el ciclo de vida del proyecto para estar seguro que cumples los objetivos. En esta sección se muestra cómo organizar el flujo de trabajo de rendimiento, solucionar problemas de animaciones y los problemas de velocidad de fotogramas y ajustar el tiempo de inicio, tiempo de navegación de páginas y el uso de la memoria.

Si aún no lo has hecho, un paso que ya hemos visto y que permite realizar mejoras significativas en el rendimiento es simplemente migrar la aplicación a Windows 10. Varias optimizaciones de XAML (por ejemplo, [{x:Bind}](../xaml-platform/x-bind-markup-extension.md)) solo están disponibles en las aplicaciones de Windows 10. Consulta [Migración de aplicaciones a Windows 10](../porting/index.md) y la sesión de //build/ [Cambio a la Plataforma universal de Windows](https://channel9.msdn.com/Events/Build/2015/3-741).

| Tema | Descripción |
|-------|-------------|
| [Planificación del rendimiento](planning-and-measuring-performance.md) | Los usuarios esperan que sus aplicaciones sean dinámicas, que su uso sea natural y que no agoten fácilmente la batería. Técnicamente, el rendimiento es un requisito no funcional, pero tratarlo como una característica te ayudará a cumplir las expectativas de los usuarios. Especificar objetivos y realizar mediciones son factores clave. Determina cuáles son los escenarios críticos para el rendimiento, define lo que significa un buen rendimiento. A continuación, realiza mediciones al principio y con la suficiente frecuencia durante el ciclo de vida del proyecto, para estar seguro de que cumples los objetivos. |
| [Optimización de la actividad en segundo plano](optimize-background-activity.md) | Crea aplicaciones para UWP que colaboren con el sistema para usar tareas en segundo plano de una manera que produzca un consumo eficiente de la batería. |
| [Optimización de interfaz de usuario de ListView y GridView](optimize-gridview-and-listview.md) | Mejora el rendimiento y el tiempo de inicio de [<strong>GridView</strong>](/uwp/api/Windows.UI.Xaml.Controls.GridView) mediante la virtualización de la interfaz de usuario, la reducción de elementos y la actualización progresiva de esos elementos. |
| [Virtualización de datos de ListView y GridView](listview-and-gridview-data-optimization.md) | Mejora el rendimiento y el tiempo de inicio de [<strong>GridView</strong>](/uwp/api/Windows.UI.Xaml.Controls.GridView) mediante la virtualización de datos. |
| [Mejorar el rendimiento de la recolección de elementos no utilizados](improve-garbage-collection-performance.md) | La memoria de las aplicaciones para la Plataforma universal de Windows (UWP) escritas en C# y Visual Basic se administra de manera automática con el recolector de elementos no usados de .NET. En esta sección se resume el comportamiento y los procesos recomendados de rendimiento del recolector de elementos no usados de .NET para las aplicaciones para UWP. |
| [Mantener la capacidad de respuesta del subproceso de la interfaz de usuario](keep-the-ui-thread-responsive.md) | Los usuarios esperan que las aplicaciones sigan respondiendo mientras realizan cálculos, independientemente del tipo de equipo. Esto significa cosas distintas en función de la aplicación. En el caso de algunas aplicaciones, esto se traduce en la necesidad de proporcionar una física más realista, cargar los datos desde el disco o desde la Web con mayor rapidez, presentar escenas complejas y navegar entre páginas velozmente, encontrar direcciones al instante o procesar datos con rapidez. Independientemente del tipo de cálculo, los usuarios quieren que las aplicaciones respondan a su entrada y que nunca parezca que dejan de responder mientras &quot;están pensando&quot;. |
| [Optimizar el marcado XAML](optimize-xaml-loading.md) | El análisis del marcado XAML para crear objetos en la memoria requiere mucho tiempo para una interfaz de usuario compleja. Estas son algunas acciones que puedes realizar para mejorar el análisis del marcado XAML, el tiempo de carga y la eficiencia de la memoria de tu aplicación. | 
| [Optimiza tu diseño XAML](optimize-your-xaml-layout.md) | El diseño puede ser una parte costosa de una aplicación XAML; tanto en la sobrecarga de memoria como en el uso de la CPU. A continuación te mostramos algunos sencillos pasos para mejorar el rendimiento de diseño de la aplicación XAML. | 
| [Sugerencias de rendimiento de MVVM y lenguaje](mvvm-performance-tips.md) | En este tema se describen algunos aspectos a tener en cuenta acerca del rendimiento y que están relacionados con la elección de patrones de diseño del software y del lenguaje de programación. |
| [Procedimientos recomendados para mejorar el rendimiento del inicio de la aplicación](best-practices-for-your-app-s-startup-performance.md) | Mejora la manera de controlar el inicio y la activación de la aplicación para crear aplicaciones para UWP con el tiempo de inicio optimizado. |
| [Optimizar las animaciones, multimedia e imágenes](optimize-animations-and-media.md) | Crea aplicaciones para la Plataforma universal de Windows (UWP) con animaciones suaves, alta velocidad de fotogramas y capturas multimedia y reproducciones de alto rendimiento. |
| [Optimizar la suspensión y reanudación](optimize-suspend-resume.md) | Crea aplicaciones para UWP que optimicen el uso del sistema de ciclo de vida de los procesos, para poder reanudarlo de forma eficaz tras su suspensión o finalización. |
| [Optimizar el acceso a archivos](optimize-file-access.md) | Crea aplicaciones para UWP que puedan obtener acceso al sistema de archivos de forma eficaz y así poder evitar problemas de rendimiento debido a la latencia de discos o a los ciclos de memoria/CPU. |
| [Componentes de Windows Runtime y optimización de la interoperabilidad](windows-runtime-components-and-optimizing-interop.md) | Crea aplicaciones para UWP que usen componentes de UWP e interactúen con los tipos administrados y nativos, al mismo tiempo que evitan problemas de rendimiento de la interoperabilidad. |
| [Herramientas de creación de perfiles y rendimiento](tools-for-profiling-and-performance.md) | Microsoft proporciona varias herramientas que te ayudarán a mejorar el rendimiento de tu aplicación para UWP.|