---
author: Xansky
Description: Describes the requirements for declaring your Universal Windows Platform (UWP) app as accessible in the Microsoft Store.
ms.assetid: 59FA3B87-75A6-4B30-BA7C-A0E769D68050
title: Accesibilidad en la Store
label: Accessibility in the Store
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 990af773c600c2c87cfe6bc477ed1d6799379fb2
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5562895"
---
# <a name="accessibility-in-the-store"></a>Accesibilidad en la Store  



Describe los requisitos para declarar que tu aplicación para la Plataforma universal de Windows (UWP) es accesible en la Microsoft Store.

Al enviar tu aplicación a la Microsoft Store para su certificación, puedes declarar que es accesible. Con ello, haces que sea más fácil que los usuarios que estén interesados en aplicaciones accesibles la descubran, como aquellos que tienen discapacidades visuales. Los usuarios descubren las aplicaciones accesibles mediante el uso del filtro **Accesible** mientras buscan en la Microsoft Store. Al declarar que tu aplicación es accesible, también agregas la etiqueta **Accesible** a su descripción.

Al declarar tu aplicación como accesible, estás comunicando que cumple la [información de accesibilidad básica](basic-accessibility-information.md) que necesitan los usuarios en escenarios principales en alguno o varios de los siguientes elementos:

* El teclado
* Un tema de contraste alto
* Una configuración variable de puntos por pulgada (ppp)
* Tecnología de asistencia común como las funciones de accesibilidad de Windows, que incluyen el narrador, la lupa y el teclado en pantalla.

Debes declarar tu aplicación como accesible si la has compilado para ello y has realizado las pruebas de accesibilidad. Esto implica que has hecho lo siguiente:

* Configurar toda la información de accesibilidad relevante para los elementos de la interfaz de usuario, entre ellos, el nombre, el rol, el valor, etc.
* Implementar una accesibilidad de teclado completa, que permita al usuario realizar lo siguiente:
    * alcanzar los principales escenarios de aplicaciones usando únicamente el teclado;
    * saltar entre elementos de la interfaz de usuario en un orden lógico;
    * navegar entre los elementos de la interfaz de usuario en un control con las teclas de dirección;
    * usar métodos abreviados de teclado para alcanzar una funcionalidad de la aplicación principal;
    * usar los gestos táctiles del narrador para la equivalencia con el tabulador y las flechas para dispositivos sin teclado.
* Garantizar que la interfaz de usuario de tu aplicación es accesible visualmente, es decir, que tiene una relación de contraste de texto mínima de 4.5:1, que no depende solamente del color para transmitir la información, etc.
* Usar herramientas de prueba de accesibilidad como [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) y [**UIAVerify**](https://msdn.microsoft.com/library/windows/desktop/Hh920986) para comprobar la implementación que has realizado de la accesibilidad y resolver todos los errores de prioridad 1 notificados por dichas herramientas.
* Comprobar los principales escenarios de la aplicación de principio a fin usando el narrador, la lupa, el teclado en pantalla, un tema de contraste alto y con la opción de ppp ajustada.

Consulta el tema [Lista de comprobación para accesibilidad](accessibility-checklist.md) para obtener un revisión de estos procedimientos y vínculos a recursos que te ayudarán a llevarlos a cabo.

<span id="related_topics"/>

## <a name="related-topics"></a>Temas relacionados    
* [Accesibilidad](accessibility.md) 
