---
description: Cuando se solicita un recurso, podría haber varios candidatos que coincidan hasta cierto punto con el contexto de recursos actual. El sistema de administración de recursos analizará todos los candidatos y determinará el mejor candidato para devolver. En este tema se describe con detalle ese proceso y se proporcionan ejemplos.
title: Cómo el sistema de administración de recursos compara y elige recursos
template: detail.hbs
ms.date: 10/23/2017
ms.topic: article
keywords: windows 10, uwp, resource, image, asset, MRT, qualifier
ms.localizationpriority: medium
ms.openlocfilehash: d430aae696b0f021e2412a73f137ea6db826937b
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031858"
---
# <a name="how-the-resource-management-system-matches-and-chooses-resources"></a>Cómo el sistema de administración de recursos compara y elige recursos
Cuando se solicita un recurso, podría haber varios candidatos que coincidan hasta cierto punto con el contexto de recursos actual. El sistema de administración de recursos analizará todos los candidatos y determinará el mejor candidato para devolver. Para ello, se deben tener en cuenta todos los calificadores para clasificar a todos los candidatos.

En este proceso de clasificación, a los diferentes calificadores se les asignan diferentes prioridades: el lenguaje tiene el mayor impacto en la clasificación general, seguido del contraste, de la escala, etc. Para cada calificador, los calificadores candidatos se comparan con el valor de calificador de contexto para determinar una calidad de coincidencia. La comparación se realiza según el calificador.

Para obtener detalles específicos sobre cómo se realiza la coincidencia de etiquetas de idioma, vea [Cómo coincide el sistema de administración de recursos con las etiquetas de idioma](how-rms-matches-lang-tags.md).

En algunos calificadores, como la escala y el contraste, siempre hay un grado mínimo de coincidencia. Por ejemplo, un candidato calificado para "Scale-100" coincide con un contexto de "Scale-400" a cierto grado pequeño, aunque no así como a un candidato calificado para "Scale-200" o (para una coincidencia perfecta) "Scale-400".

Sin embargo, en el caso de otros calificadores, como el idioma o la región principal, es posible tener una comparación no coincidente (así como los grados de coincidencia). Por ejemplo, un candidato calificado para el idioma como "en-US" es una coincidencia parcial para un contexto de "en-GB", pero un candidato calificado como "fr" no es una coincidencia. Del mismo modo, una candidata calificada para la región de inicio como "155" (Europa occidental) coincide en un contexto de un usuario con un valor de región principal de "FR", pero un candidato calificado como "EE. UU." no coincide.

Cuando se evalúa un candidato, si hay una comparación sin coincidencia para un calificador, ese candidato obtendrá una clasificación no coincidente general y no se seleccionará. De esta manera, los calificadores de mayor prioridad pueden tener el mayor peso en la selección de la mejor coincidencia, pero incluso un calificador de prioridad baja puede eliminar a un candidato debido a una falta de coincidencia.

Un candidato es neutro en relación con un calificador si no está marcado para ese calificador. En el caso de cualquier calificador, un candidato neutro siempre es una coincidencia para el valor del calificador de contexto, pero solo con una calidad de coincidencia inferior a cualquier candidato marcado para ese calificador y tiene cierto grado de coincidencia (exacta o parcial). Por ejemplo, si tenemos candidatos calificados para "en-US", "en", "fr" y también un candidato independiente del lenguaje, para un contexto con un valor de calificador de idioma de "en-GB", los candidatos se clasificarán en el siguiente orden: "en", "en-US", neutro y "fr". En este caso, "fr" no coincide, mientras que los demás candidatos coinciden con cierto grado.

El proceso de clasificación general comienza por la evaluación de los candidatos en relación con el calificador de prioridad más alta, que es el idioma. Las no coincidencias se eliminan. Los candidatos restantes se clasifican en relación con su calidad de coincidencia para el idioma. Si hay algún valor de vinculación, se considera el calificador de mayor prioridad, contraste, con la calidad de la coincidencia para diferenciar entre los candidatos asociados. Después del cambio, el calificador de escala se usa para diferenciar los lazos restantes, y así sucesivamente a través de todos los calificadores que se necesitan para llegar a una clasificación bien ordenada.

Si no se tienen en cuenta todos los candidatos debido a que los calificadores no coinciden con el contexto, el cargador de recursos pasa por un segundo paso buscando un candidato predeterminado para mostrarlo. Los candidatos predeterminados se determinan durante la creación del archivo PRI y son necesarios para garantizar que siempre hay algún candidato para seleccionar en cualquier contexto de tiempo de ejecución. Si un candidato tiene algún calificador que no coincide y no es un valor predeterminado, ese candidato de recurso se inicia de forma permanente.

En el caso de todos los candidatos de recursos todavía en consideración, el cargador de recursos examina el valor de calificador de contexto de prioridad más alta y elige el que tiene la mejor coincidencia o la mejor puntuación predeterminada. Cualquier coincidencia real se considera mejor que una puntuación predeterminada.

Si hay un empate, se inspecciona el siguiente valor de calificador de contexto de prioridad más alto y el proceso continúa hasta que se encuentra una mejor coincidencia.

## <a name="example-of-choosing-a-resource-candidate"></a>Ejemplo de elección de un candidato de recurso
Tenga en cuenta estos archivos.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
fr/images/contrast-high/logo.scale-400.jpg
fr/images/contrast-high/logo.scale-100.jpg
de/images/logo.jpg
```

Y Supongamos que se trata de la configuración del contexto actual.

```console
Application language: en-US; fr-FR;
Scale: 400
Contrast: Standard
```

El sistema de administración de recursos elimina tres de los archivos, ya que el contraste alto y el idioma alemán no coinciden con el contexto definido por la configuración. Eso deja a estos candidatos.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

En el caso de los candidatos restantes, el sistema de administración de recursos utiliza el calificador de contexto de prioridad más alta, que es el idioma. Los recursos en inglés están más cerca que los de francés, ya que el inglés se muestra antes que el francés en la configuración.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
```

Después, el sistema de administración de recursos utiliza el calificador de contexto de prioridad siguiente, escala. Por lo tanto, se devuelve el recurso.

```console
en/images/logo.scale-400.jpg
```

Puede usar el método [**NamedResource. ResolveAll**](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live) avanzado para recuperar todos los candidatos en el orden en que coinciden con los valores de contexto. En el ejemplo que se acaba de recorrer, **ResolveAll** devuelve candidatos en este orden.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

## <a name="example-of-producing-a-fallback-choice"></a>Ejemplo de creación de una opción de reserva
Tenga en cuenta estos archivos.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

Y Supongamos que se trata de la configuración del contexto actual.

```console
User language: de-DE;
Scale: 400
Contrast: High
```

Se eliminan todos los archivos porque no coinciden con el contexto. Por lo tanto, se especifica una fase predeterminada, donde el valor predeterminado (consulte [compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md)) durante la creación del archivo PRI fue este.

```console
Language: fr-FR;
Scale: 400
Contrast: Standard
```

Esto deja todos los recursos que coinciden con el usuario actual o el predeterminado.

```console
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

El sistema de administración de recursos utiliza el calificador de contexto de prioridad más alta, idioma, para devolver el recurso con nombre con la puntuación más alta.

```console
de/images/contrast-standard/logo.jpg
```

## <a name="important-apis"></a>API importantes
* [NamedResource. ResolveAll](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live)

## <a name="related-topics"></a>Temas relacionados
* [Compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md)
