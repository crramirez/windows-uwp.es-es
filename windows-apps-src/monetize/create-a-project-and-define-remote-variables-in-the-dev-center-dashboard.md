---
Description: Antes de poder ejecutar un experimento en su aplicación de plataforma Universal de Windows (UWP) con un pruebas a/b, debe crear un proyecto y definir las variables remotas en el centro de partners.
title: Crear un proyecto de experimento en el Centro de partners
ms.assetid: C3809FF1-0A6A-4715-B989-BE9D0E8C9013
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP, Microsoft Store Services SDK, pruebas A/B, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: acfd654f02cb7fb727d35271175e59966e2abdc4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650800"
---
# <a name="create-an-experiment-project-in-partner-center"></a>Crear un proyecto de experimento en el Centro de partners

Para empezar a trabajar con la experimentación, cree una experimentación [proyecto](run-app-experiments-with-a-b-testing.md#terms) para la aplicación en el centro de partners y defina las variables remotas que puede tener acceso la aplicación.

Las siguientes instrucciones describen los pasos básicos para crear un proyecto. Para ver un tutorial detallado que muestre de principio a fin el proceso de crear un proyecto y luego ejecutar un experimento, consulta [Crear y ejecutar el primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="instructions"></a>Instrucciones

1. Inicia sesión en el [Centro de partners](https://partner.microsoft.com/dashboard).
2. En **Tus aplicaciones**, selecciona la aplicación para la que deseas crear un experimento.
3. En el panel de navegación, selecciona **Servicios** y, a continuación, selecciona **Experimentación**.
4. En la página **Experimentación**, haz clic en el botón **Nuevo proyecto** en la sección **Proyectos**. Si ya creaste uno o varios proyectos, estos se enumeran en la sección **Proyectos**.
5. En la página **Nuevo proyecto**, escribe un nombre para el nuevo proyecto.
6. En la sección **Variables remotas**, agrega las [variables](run-app-experiments-with-a-b-testing.md#terms) que quieres que estén disponibles para todos los experimentos en este proyecto y define los valores predeterminados para cada variable. Los valores predeterminados que especificaste aquí se usan para el grupo de control de los experimentos y para los usuarios que no participen en el experimento.
  1. Si la sección **Variables remotas** está contraída, haz clic en **Mostrar** en el encabezado de la sección.
  2. Haz clic en **Agregar variable** para crear cada nueva variable que quieras que esté disponible para cualquier experimento en este proyecto y escribe el nombre de la variable y el valor predeterminado de la variable.
  3. Cuando hayas terminado de agregar variables, haz clic en **Guardar**.
3. En la sección **Integración de SDK**, asegúrate de anotar el valor del [id. de proyecto](run-app-experiments-with-a-b-testing.md#terms). Cuando se [codificar la aplicación para la experimentación](code-your-experiment-in-your-app.md), debe hacer referencia a este Id. de proyecto en el código para que pueda recibir datos de variación y notificar los eventos de vista y la conversión a centro de partners.

> [!NOTE]
> No puedes editar, agregar o quitar variables remotas mientras haya un experimento activo en el proyecto. Esta limitación ayuda a proteger la integridad de los datos para el grupo de control del experimento activo.


## <a name="next-steps"></a>Pasos siguientes

Después de crear un proyecto, puedes [codificar la aplicación para la experimentación](code-your-experiment-in-your-app.md) para comenzar a recuperar los valores de variables remotos en la aplicación y puedes [crear un experimento en el proyecto](define-your-experiment-in-the-dev-center-dashboard.md).

## <a name="related-topics"></a>Temas relacionados

* [Codificar la aplicación para la experimentación](code-your-experiment-in-your-app.md)
* [Definir el experimento en el centro de partners](define-your-experiment-in-the-dev-center-dashboard.md)
* [Administrar el experimento en el centro de partners](manage-your-experiment.md)
* [Cree y ejecute su primer experimento con un pruebas A/b](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ejecutar los experimentos de la aplicación con un pruebas A/b](run-app-experiments-with-a-b-testing.md)
