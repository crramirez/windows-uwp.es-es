---
author: stevewhims
Description: "Cuando se solicita un recurso, puede ser que haya varios candidatos que coincidan en algún grado con el contexto de recurso actual. El Sistema de administración de recursos analizará todos los candidatos y determinará cuál es el mejor candidato a devolver. Este tema describe con detalle ese proceso y proporciona ejemplos."
title: "Cómo compara y elige recursos el sistema de administración"
template: detail.hbs
ms.author: stwhi
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, recursos, imagen, activo, MRT, calificador
localizationpriority: medium
ms.openlocfilehash: 4731ae7add7d5b969ab98da60b3f6740dbbbee1b
ms.sourcegitcommit: 44a24b580feea0f188c7eae36e72e4a4f412802b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2017
---
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

# <a name="how-the-resource-management-system-matches-and-chooses-resources"></a>Cómo compara y elige recursos el sistema de administración

Cuando se solicita un recurso, puede ser que haya varios candidatos que coincidan en algún grado con el contexto de recurso actual. El Sistema de administración de recursos analizará todos los candidatos y determinará cuál es el mejor candidato que se va a devolver. Para ello, se tienen en cuenta todos los calificadores para clasificar a todos los candidatos.

En este proceso de clasificación, se asignan prioridades diferentes a los distintos calificadores: el idioma tiene el mayor impacto en la clasificación global, seguido del contraste, la escala, etc. Para cada calificador, se comparan los calificadores de los candidatos con el valor del calificador de contexto para determinar una calidad de coincidencia. El modo en que se realiza la comparación depende del calificador.

En algunos calificadores, como la escala y el contraste, siempre hay un grado mínimo de coincidencia. Por ejemplo, un candidato calificado como "escala-100" coincide en cierta medida con un contexto de "escala-400", aunque no tanto como un candidato calificado como "escala-200" o (para coincidencia perfecta) "escala-400".

Sin embargo, en el resto de calificadores, como el idioma o la región principal, es posible que haya una comparación no coincidente (además de grados de coincidencia). Por ejemplo, un candidato con la calificación de idioma "en-US" representa una coincidencia parcial en un contexto "en-GB", pero un candidato con la calificación "fr" no es coincidente en modo alguno. De manera similar, un candidato con la calificación de región principal "155" (Europa occidental) coincide bien de alguna manera con un contexto de usuario cuya región principal esté definida en "FR", pero un candidato calificado como "US" no coincide en modo alguno.

Cuando se evalúa un candidato, si hay una comparación no coincidente para cualquier calificador, dicho candidato obtendrá una clasificación no coincidente global y no se seleccionará. De este modo, los calificadores con mayor prioridad tendrán el mayor peso en la selección de la mejor coincidencia, pero incluso un calificador de prioridad baja puede eliminar un candidato debido a una falta de coincidencia.

Un candidato es independiente de un calificador si no está marcado para ese calificador en absoluto. Para cualquier calificador, un candidato neutro representa siempre una coincidencia con el valor de calificador de contexto, pero únicamente con una calidad de coincidencia inferior a cualquier candidato que se hubiera marcado para ese calificador y que tenga cierto grado de coincidencia (exacta o parcial). Por ejemplo, si tenemos candidatos con la calificación "en-US", "en" y "fr", y además un candidato neutro respecto del idioma, en ese caso, para un contexto con valor de calificador de idioma de "en-GB", los candidatos se clasificarán en el siguiente orden: "en", "en-US", neutro y "fr". En este caso, "fr" no coincide en modo alguno, mientras que los demás candidatos coinciden en cierto grado.

El proceso de clasificación global comienza mediante la evaluación de candidatos en relación con el calificador de mayor prioridad, que es el idioma. Las faltas de coincidencia se eliminan. Los candidatos restantes se clasifican en relación con su calidad de coincidencia por idioma. Si hay empates, se tiene en cuenta el siguiente calificador con mayor prioridad, el contraste, y se usa la calidad de coincidencia de contraste para diferencias entre los candidatos empatados. Después del contraste, se usa el calificador de escala para diferenciar el resto de empates y así sucesivamente con tantos calificadores como sea necesario para llegar a una clasificación bien ordenada.

Si todos los candidatos se quitan de la consideración debido a calificadores que no coinciden con el contexto, el cargador de recursos realiza un segundo pase para que se muestre un candidato predeterminado. Los candidatos predeterminados se determinan durante la creación del archivo PRI y son necesarios para garantizar que siempre haya algún candidato que seleccionar en cualquier contexto de tiempo de ejecución. Si un candidato tiene calificadores que no coincidan y no sean predeterminados, ese candidato de recurso se dejará de tener en consideración permanentemente.

Para todos los candidatos de recursos que sigan teniéndose en cuenta, el cargador de recursos busca el valor de calificador de contexto de mayor prioridad y elige el que coincida mejor o el que tenga mejor puntuación predeterminada. Cualquier coincidencia real se considerará mejor que una puntuación predeterminada.

Si hay un empate, se inspeccionará el valor de calificador de contexto de mayor prioridad siguiente y el proceso continuará hasta que se encuentre la mejor coincidencia.

## <a name="example-of-choosing-a-resource-candidate"></a>Ejemplo de elección de un candidato de recurso

Supónganse estos archivos.

```
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
fr/images/contrast-high/logo.scale-400.jpg
fr/images/contrast-high/logo.scale-100.jpg
de/images/logo.jpg
```

Y supóngase que esta sea la configuración en el contexto actual.

```
Application language: en-US; fr-FR;
Scale: 400
Contrast: Standard
```

El Sistema de administración de recursos elimina tres de los archivos, porque el contraste alto y el idioma alemán no coinciden con el contexto definido por la configuración. Eso deja a estos candidatos.

```
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

Para los candidatos restantes, el sistema de administración de recursos usa el calificador de contexto de prioridad más alta, que es el idioma. Los recursos de inglés son una mejor coincidencia que los de francés, porque el inglés aparece antes del francés en la configuración.

```
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
```

A continuación, el sistema de administración de recursos usa el calificador de contexto de mayor prioridad siguiente, la escala. Por ello, este será el recurso devuelto.

```
en/images/logo.scale-400.jpg
```

Puedes usar el método avanzado [**NamedResource.ResolveAll**](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource?branch=live#Windows_ApplicationModel_Resources_Core_NamedResource_ResolveAll_Windows_ApplicationModel_Resources_Core_ResourceContext_) para recuperar todos los candidatos en el orden en que coincidan con la configuración de contexto. Para el ejemplo en el que nos hemos desenvuelto, **ResolveAll** devuelve candidatos en este orden.

```
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

## <a name="example-of-producing-a-fallback-choice"></a>Ejemplo de producción de una elección de reserva

Supónganse estos archivos.

```
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

Y supóngase que esta sea la configuración en el contexto actual.

```
User language: de-DE;
Scale: 400
Contrast: High
```

Todos los archivos se eliminan porque no coinciden con el contexto. De modo que entramos en un paso predeterminado, donde el valor predeterminado (véase [Compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md)) durante la creación del archivo PRI era este.

```
Language: fr-FR;
Scale: 400
Contrast: Standard
```

Esto deja todos los recursos que coincidan con el usuario actual o con el predeterminado.

```
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

El sistema de administración de recursos usa el calificador de contexto de prioridad más alta, el idioma, para devolver el recurso con nombre que tenga la mayor puntuación.

```
de/images/contrast-standard/logo.jpg
```

## <a name="important-apis"></a>API importantes

* [NamedResource.ResolveAll](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource?branch=live#Windows_ApplicationModel_Resources_Core_NamedResource_ResolveAll_Windows_ApplicationModel_Resources_Core_ResourceContext_)

## <a name="related-topics"></a>Temas relacionados

* [Compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md)