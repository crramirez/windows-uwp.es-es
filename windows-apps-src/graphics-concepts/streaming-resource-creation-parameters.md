---
title: Parámetros de creación de recursos de streaming
description: Existen algunas limitaciones en el tipo de recursos de Direct3D que se pueden crear como recurso de streaming.
ms.assetid: 6FC5AD93-6F47-479E-947C-895C99B427BC
keywords:
- Parámetros de creación de recursos de streaming
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1ddb150e570e25af7162a50309b9b0fc30cedf60
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2018
ms.locfileid: "8351672"
---
# <a name="streaming-resource-creation-parameters"></a>Parámetros de creación de recursos de streaming


Existen algunas limitaciones en el tipo de recursos de Direct3D que se pueden crear como recurso de streaming.

<span id="Supported-Resource-Type"></span><span id="supported-resource-type"></span><span id="SUPPORTED-RESOURCE-TYPE"></span>**Tipo de recurso admitido**  
Texture2D\[Array\] (incluido TextureCube\[Array\], que es una variante de Texture2D\[Array\]) o Buffer.

**No admite:** Texture1D\ [Array\].

<span id="Supported-Resource-Usage"></span><span id="supported-resource-usage"></span><span id="SUPPORTED-RESOURCE-USAGE"></span>**Uso de recurso admitido**  
Uso predeterminado.

**No admite:** Dinámico, provisional o inmutable.

<span id="Supported-Resource-Misc-Flags"></span><span id="supported-resource-misc-flags"></span><span id="SUPPORTED-RESOURCE-MISC-FLAGS"></span>**Marcas variadas de los recursos admitidos**  
En mosaico; es decir, emisión (por definición), cubo de textura, dibujo de argumentos indirectos, almacenar en búfer vistas sin procesar permitidas, búfer estructurado, compresión de recursos o generar MIP.

**No admite:** compartido, exclusión mutua con clave, compatible con GDI, identificador de NT compartido, contenido restringido, restringir el recurso compartido, restringir el controlador del recurso compartido, protegido o grupo de iconos.

<span id="Supported-Bind-Flags"></span><span id="supported-bind-flags"></span><span id="SUPPORTED-BIND-FLAGS"></span>**Indicadores de enlace admitidos**  
Enlazar como recurso de sombreador, destino de representación, galería de símbolos de profundidad o acceso sin ordenar.

**No admite:** Enlazar como búfer de constantes, búfer de vértices (admite el enlace un búfer en mosaico como SRV/UAV/RTV es), índice de búfer, salida de flujo, descodificador o codificador de vídeo.

<span id="Supported-Formats"></span><span id="supported-formats"></span><span id="SUPPORTED-FORMATS"></span>**Formatos admitidos**  
Todos los formatos que deberían estar disponibles para la configuración dada, independientemente de esta sea en mosaico, con algunas excepciones.

<span id="Supported-Sample-Description--Multisample-count--quality-"></span><span id="supported-sample-description--multisample-count--quality-"></span><span id="SUPPORTED-SAMPLE-DESCRIPTION--MULTISAMPLE-COUNT--QUALITY-"></span>**Descripción de muestras admitidas (recuento de muestras múltiples, calidad)**  
Cualquiera que se admitiría para la configuración dada, independientemente de que sea en mosaico, con algunas excepciones.

<span id="Supported-Width-Height-MipLevels-ArraySize"></span><span id="supported-width-height-miplevels-arraysize"></span><span id="SUPPORTED-WIDTH-HEIGHT-MIPLEVELS-ARRAYSIZE"></span>**Width/Height/MipLevels/ArraySize admitidos**  
Extensiones completas admitidas por Direct3D. Los recursos de streaming no tienen la restricción respecto al tamaño de la memoria total que se impone a los recursos que no son de streaming. Los recursos de streaming solo están restringidos por los límites generales del espacio de direcciones virtual. Consulte [Espacio de direcciones disponible para recursos de streaming](address-space-available-for-streaming-resources.md).

El contenido inicial de la memoria del grupo de iconos no está definido.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>En esta sección


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="address-space-available-for-streaming-resources.md">Espacio de direcciones disponible para recursos de streaming</a></p></td>
<td align="left"><p>En esta sección se especifica el espacio de direcciones virtual que está disponible para los recursos de streaming.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Creación de recursos de streaming](creating-streaming-resources.md)

 

 




