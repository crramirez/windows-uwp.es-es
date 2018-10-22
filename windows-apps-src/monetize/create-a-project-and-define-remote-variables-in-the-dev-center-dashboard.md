---
author: Xansky
Description: Before you can run an experiment in your Universal Windows Platform (UWP) app with A/B testing, you must create a project and define your remote variables in the Dev Center dashboard.
title: Crear un proyecto de experimento en el panel
ms.assetid: C3809FF1-0A6A-4715-B989-BE9D0E8C9013
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP, Microsoft Store Services SDK, Microsoft Store Services SDK, A/B tests, pruebas A/B, experiments, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: 920b411d44d2ae0c64cca7c018e2ccc8755b1163
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2018
ms.locfileid: "5401868"
---
# <a name="create-an-experiment-project-in-the-dashboard"></a>Crear un proyecto de experimento en el panel

Para comenzar con la experimentación, crea un [proyecto](run-app-experiments-with-a-b-testing.md#terms) de experimentación para tu aplicación en el panel del Centro de desarrollo y define las variables remotas a las que puede obtener acceso la aplicación.

Las siguientes instrucciones describen los pasos básicos para crear un proyecto. Para ver un tutorial detallado que muestre de principio a fin el proceso de crear un proyecto y luego ejecutar un experimento, consulta [Crear y ejecutar el primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="instructions"></a>Instrucciones

1. Inicia sesión en el [panel del Centro de desarrollo](https://dev.windows.com/overview).
2. En **Tus aplicaciones**, selecciona la aplicación para la que deseas crear un experimento.
3. En el panel de navegación, selecciona **Servicios** y, a continuación, selecciona **Experimentación**.
4. En la página **Experimentación**, haz clic en el botón **Nuevo proyecto** en la sección **Proyectos**. Si ya creaste uno o varios proyectos, estos se enumeran en la sección **Proyectos**.
5. En la página **Nuevo proyecto**, escribe un nombre para el nuevo proyecto.
6. En la sección **Variables remotas**, agrega las [variables](run-app-experiments-with-a-b-testing.md#terms) que quieres que estén disponibles para todos los experimentos en este proyecto y define los valores predeterminados para cada variable. Los valores predeterminados que especificaste aquí se usan para el grupo de control de los experimentos y para los usuarios que no participen en el experimento.
  1. Si la sección **Variables remotas** está contraída, haz clic en **Mostrar** en el encabezado de la sección.
  2. Haz clic en **Agregar variable** para crear cada nueva variable que quieras que esté disponible para cualquier experimento en este proyecto y escribe el nombre de la variable y el valor predeterminado de la variable.
  3. Cuando hayas terminado de agregar variables, haz clic en **Guardar**.
3. En la sección **Integración de SDK**, asegúrate de anotar el valor del [id. de proyecto](run-app-experiments-with-a-b-testing.md#terms). Cuando [codifiques tu aplicación para los experimentos](code-your-experiment-in-your-app.md), debes hacer referencia a este id. de proyecto en el código para que puedas recibir datos de variación y notificar los eventos de vista y conversión al Centro de desarrollo.

> [!NOTE]
> No puedes editar, agregar o quitar variables remotas mientras haya un experimento activo en el proyecto. Esta limitación ayuda a proteger la integridad de los datos para el grupo de control del experimento activo.


## <a name="next-steps"></a>Pasos siguientes

Después de crear un proyecto, puedes [codificar la aplicación para la experimentación](code-your-experiment-in-your-app.md) para comenzar a recuperar los valores de variables remotos en la aplicación y puedes [crear un experimento en el proyecto](define-your-experiment-in-the-dev-center-dashboard.md).

## <a name="related-topics"></a>Temas relacionados

* [Programar tu aplicación para los experimentos](code-your-experiment-in-your-app.md)
* [Definición de tu experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md)
* [Administración de tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md)
* [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ejecuta experimentos para aplicaciones con pruebas A/B](run-app-experiments-with-a-b-testing.md)
