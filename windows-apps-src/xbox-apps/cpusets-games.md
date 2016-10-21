---
title: CPUSets para el desarrollo de juegos
description: "En este artículo se incluye una descripción general de la API de CPUSets, nueva para la Plataforma universal de Windows (UWP) y con información esencial relevante para el desarrollo de juegos y aplicaciones."
author: hammondsp
translationtype: Human Translation
ms.sourcegitcommit: 9f15d551715d9ccf23e4eb397637f4fafacec350
ms.openlocfilehash: 6065435dc3add0d9bde15dc6bdd355935b8f53cd

---

# CPUSets para el desarrollo de juegos

## Introducción

La Plataforma universal de Windows (UWP) es el núcleo de una amplia gama de dispositivos electrónicos de consumo. Como tal, requiere un API de uso general para satisfacer las necesidades de todos los tipos de aplicaciones, desde juegos y aplicaciones incrustadas hasta software empresarial que se ejecuta en servidores. Al aprovechar la información correcta proporcionada por la API, puedes asegurarte de que tu juego se ejecuta el mejor rendimiento en cualquier hardware.

## API de CPUSets

La API de CPUSets proporciona control sobre los conjuntos de CPU que están disponibles para los subprocesos en los que se realizará la programación. Existen dos funciones disponibles para controlar dónde se programan los subprocesos:
- **SetProcessDefaultCpuSets**: esta función puede usarse para especificar los nuevos subprocesos de conjuntos de CPU que pueden ejecutarse si no se asignan a determinados conjuntos de CPU.
- **SetThreadSelectedCpuSets**: esta función permite limitar los conjuntos de CPU que puede ejecutar un subproceso específico.

Si la función **SetProcessDefaultCpuSets** nunca se ha usado, los subprocesos recién creados se pueden programar en cualquier conjunto de CPU disponible para el proceso. En esta sección se explican los conceptos básicos de la API de CPUSets.

### GetSystemCpuSetInformation

La primera API usada para recopilar información es la función **GetSystemCpuSetInformation**. Esta función rellena la información de una matriz de objetos **SYSTEM_CPU_SET_INFORMATION** que proporciona el código de título. La memoria del destino debe asignarse al código del juego, el tamaño del cual se determina mediante una llamada a **GetSystemCpuSetInformation**. Se requieren dos llamadas a **GetSystemCpuSetInformation** tal como se muestra en el siguiente ejemplo.

```
unsigned long size;
HANDLE curProc = GetCurrentProcess();
GetSystemCpuSetInformation(nullptr, 0, &size, curProc, 0);

std::unique_ptr<uint8_t[]> buffer(new uint8_t[size]);

PSYSTEM_CPU_SET_INFORMATION cpuSets = reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>(buffer.get());
  
GetSystemCpuSetInformation(cpuSets, size, &size, curProc, 0);
```

Cada instancia de **SYSTEM_CPU_SET_INFORMATION** devuelta contiene información sobre una unidad de procesamiento única, también conocida como un conjunto de CPU. Esto no significa necesariamente que representa un único componente físico de hardware. Las CPU que usan hyperthreading tendrán varios núcleos lógicos ejecutándose en un único núcleo de procesamiento físico. La programación de varios subprocesos en diferentes núcleos lógicos que residen en el mismo núcleo físico permite optimizar los recursos de nivel de hardware que, de lo contrario, requerirían más trabajo en el nivel del kernel. Dos subprocesos programados en núcleos lógicos independientes en el mismo núcleo físico deben compartir el tiempo de CPU, pero se ejecutarían de forma más eficaz que si se programaran en el mismo núcleo lógico.

### SYSTEM_CPU_SET_INFORMATION

La información de cada instancia de esta estructura de datos que devuelve **GetSystemCpuSetInformation** contiene información sobre una unidad de procesamiento única en la que se pueden programar los subprocesos. Dada la amplia gama de dispositivos de destino posible, una gran parte de la información de la estructura de datos **SYSTEM_CPU_SET_INFORMATION** puede no ser aplicable al desarrollo de juegos. La Tabla 1 proporciona una explicación de los miembros de datos que son útiles para el desarrollo de juegos.

 **Tabla 1. Miembros de datos útiles para el desarrollo de juegos.**

| Nombre del miembro  | Tipo de datos | Descripción |
| ------------- | ------------- | ------------- |
| Tipo  | CPU_SET_INFORMATION_TYPE  | Tipo de información de la estructura. Si su valor no es **CpuSetInformation**, debe omitirse.  |
| Id  | unsigned long  | Identificador de conjunto de CPU especificado. Este es el identificador que debe usarse con las funciones de conjunto de CPU como **SetThreadSelectedCpuSets**.  |
| Group  | unsigned short  | Especifica el "grupo de procesadores" del conjunto de CPU. Los grupos de procesadores permiten que un equipo tenga más de 64 núcleos lógicos, así como el intercambio directo de las CPU mientras se ejecuta el sistema. Es poco común ver un equipo que no es un servidor con más de un grupo. A menos que escribas aplicaciones destinadas a ejecutarse en servidores grandes o granjas de servidores, es mejor usar conjuntos de CPU en un solo grupo porque la mayoría de equipos de consumo solo tienen un grupo de procesadores. Todos los demás valores de esta estructura guardan relación con el miembro Group.  |
| LogicalProcessorIndex  | unsigned char  | Índice relativo de grupo del conjunto de CPU.  |
| CoreIndex  | unsigned char  | Índice relativo de grupo del núcleo de la CPU física donde se encuentra el conjunto de CPU.  |
| LastLevelCacheIndex  | unsigned char  | Índice relativo de grupo de la última memoria caché asociada a este conjunto de CPU. Esta es la memoria caché más lenta, a menos que el sistema use nodos NUMA, normalmente la caché L2 o L3.  |

<br />

Los otros miembros de datos proporcionan información que es bastante improbable que describa las CPU en equipos de consumo u otros dispositivos de consumo y es poco probable que resulte útil. La información que proporcionan los datos devueltos puede usarse para organizar subprocesos de diversas maneras. En la sección [Consideraciones para el desarrollo de juegos](#considerations-for-game-development) de estas notas del producto se detallan algunas formas de aprovechar estos datos para optimizar la asignación de subprocesos.

A continuación se incluyen algunos ejemplos del tipo de información recopilada de las aplicaciones para UWP que se ejecutan en distintos tipos de hardware.

**Tabla 2. Información devuelta de una aplicación para UWP que se ejecuta en Microsoft Lumia 950. Este es un ejemplo de un sistema que tiene varias cachés de último nivel. El Lumia 950 incluye un procesador Qualcomm 808 Snapdragon que contiene una CPU ARM Cortex A57 de doble núcleo y una CPU ARM Cortex A53 de cuatro núcleos.**

  ![Tabla 2](images/cpusets-table2.png)

**Tabla 3. Información devuelta de una aplicación para UWP que se ejecuta en un equipo típico. Este es un ejemplo de un sistema que usa hyperthreading; cada núcleo físico tiene dos núcleos lógicos en los que se pueden programar subprocesos. En este caso, el sistema contiene una CPU Intel Xeon E5-2620.**

  ![Tabla 3](images/cpusets-table3.png)

**Tabla 4. Información devuelta de una aplicación para UWP en un dispositivo Microsoft Surface Pro 4 de cuatro núcleos. Este sistema tenía una CPU Intel Core i5-6300.**

  ![Tabla 4](images/cpusets-table4.png)

### SetThreadSelectedCpuSets

Ahora que está disponible la información sobre los conjuntos de CPU, puede usarse para organizar los subprocesos. El identificador de un subproceso creado con **CreateThread** se pasa a esta función junto con una matriz de identificadores de conjuntos de CPU en los que se puede programar el subproceso. Se muestra un ejemplo de su uso en el siguiente código.

```
HANDLE audioHandle = CreateThread(nullptr, 0, AudioThread, nullptr, 0, nullptr);
unsigned long cores [] = { cpuSets[0].CpuSet.Id, cpuSets[1].CpuSet.Id };
SetThreadSelectedCpuSets(audioHandle, cores, 2);
```
En este ejemplo, se crea un subproceso basado en una función que se declara como **AudioThread**. Posteriormente, este subproceso se puede programar en uno de los dos conjuntos de CPU. La propiedad del subproceso del conjunto de CPU no es exclusiva. Los subprocesos creados sin bloquearse en un conjunto de CPU específico pueden tomar tiempo de **AudioThread**. De igual modo, también se pueden bloquear otros subprocesos creados en uno o ambos conjuntos de CPU posteriormente.

### SetProcessDefaultCpuSets

El contrario de **SetThreadSelectedCpuSets** es **SetProcessDefaultCpuSets**. Cuando se crean subprocesos, no es necesario bloquearlos en determinados conjuntos de CPU. Si no quieres que estos subprocesos se ejecuten en conjuntos de CPU específicos (aquellos que usan el subproceso de representación o el subproceso de audio, por ejemplo), puedes usar esta función para especificar en qué núcleos se pueden programar estos subprocesos.

## Consideraciones para el desarrollo de juegos

Como hemos visto, la API de CPUSets proporciona una gran cantidad de información y flexibilidad para la programación de subprocesos. En lugar de adoptar el enfoque de abajo arriba de intentar encontrar usos para estos datos, resulta más eficaz adoptar el enfoque de arriba a abajo de averiguar cómo se pueden usar los datos para admitir escenarios comunes.

### Trabajar con hyperthreading y subprocesos críticos en el tiempo

Este método es eficaz si el juego tiene algunos subprocesos que deben ejecutarse en tiempo real junto con otros subprocesos de trabajo que requieren relativamente poco tiempo de CPU. Algunas tareas, como la música de fondo continua, deben ejecutarse sin interrupciones para una experiencia de juego perfecta. Incluso un único fotograma de colapso de un subproceso de audio puede causar la aparición de mensajes o problemas, por lo cual es muy importante que reciba la cantidad de tiempo de CPU en cada fotograma.

Al usar **SetThreadSelectedCpuSets** junto con **SetProcessDefaultCpuSets** puedes garantizar que los subprocesos intensos no se interrumpan a causa de subprocesos de trabajo. **SetThreadSelectedCpuSets** puede usarse para asignar subprocesos intensos a conjuntos de CPU específicos. **SetProcessDefaultCpuSets** puede usarse para garantizar que los subprocesos sin asignar creados se coloquen en otros conjuntos de CPU. En el caso de las CPU que usan hyperthreading, también es importante para tener en cuenta los núcleos lógicos en el mismo núcleo físico. Los subprocesos de trabajo no deberían poder ejecutarse en núcleos lógicos que comparten el mismo núcleo físico que un subproceso que quieres ejecutar con la capacidad de respuesta en tiempo real. El siguiente código muestra cómo determinar si un equipo usa hyperthreading.

```
unsigned long retsize = 0;
(void)GetSystemCpuSetInformation( nullptr, 0, &retsize,
    GetCurrentProcess(), 0);
 
std::unique_ptr<uint8_t[]> data( new uint8_t[retsize] );
if ( !GetSystemCpuSetInformation(
    reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>( data.get() ),
    retsize, &retsize, GetCurrentProcess(), 0) )
{
    // Error!
}
 
std::set<DWORD> cores;
std::vector<DWORD> processors;
uint8_t const * ptr = data.get();
for( DWORD size = 0; size < retsize; ) {
    auto info = reinterpret_cast<const SYSTEM_CPU_SET_INFORMATION*>( ptr );
    if ( info->Type == CpuSetInformation ) {
         processors.push_back( info->CpuSet.Id );
         cores.insert( info->CpuSet.CoreIndex );
    }
    ptr += info->Size;
    size += info->Size;
}
 
bool hyperthreaded = processors.size() != cores.size();
```

Si el sistema usa hyperthreading, es importante que el conjunto de conjuntos de CPU predeterminados no incluya núcleos lógicos en el mismo núcleo físico que los subprocesos en tiempo real. Si el sistema no usa hyperthreading, solo es necesario para asegurarse de que los conjuntos de CPU predeterminados no incluyen el mismo núcleo que el conjunto de CPU que ejecuta el subproceso de audio.

Un ejemplo de organización de subprocesos basada en núcleos físicos puede encontrarse en el ejemplo de CPUSets disponible en el repositorio de GitHub, cuyo link se encuentra en la sección [Recursos adicionales](#additional-resources).

### Reducir el costo de la coherencia de caché con la memoria caché de último nivel

La coherencia de caché es el concepto en que la memoria caché es la misma en varios recursos de hardware que actúan en los mismos datos. Si los subprocesos se programan en diferentes núcleos, pero que funcionan en los mismos datos, es posible que estén funcionando en copias independientes de los datos en memorias caché diferentes. Para obtener los resultados correctos, estas cachés deben mantenerse coherentes entre sí. Mantener la coherencia entre varias cachés es relativamente costoso, pero es necesario para que cualquier sistema de varios núcleos funcione. Además, está completamente fuera del control del código de cliente; el sistema subyacente funciona independientemente para mantener las memorias caché actualizadas mediante el acceso a los recursos de memoria compartidos entre núcleos.

Si el juego tiene varios subprocesos que comparten una cantidad considerable de datos, puedes reducir al mínimo el costo de la coherencia de caché. Para ello, asegúrate de que estén programados en conjuntos de CPU que comparten una caché de último nivel. La caché de último nivel es la más lenta disponible para un núcleo en sistemas que no usan nodos NUMA. Es extremadamente raro que un equipo de juegos use nodos NUMA. Si los núcleos no comparten una caché de último nivel, mantener la coherencia requerirá acceder a recursos de memoria de mayor nivel y, por tanto, más lentos. El bloqueo de dos subprocesos para separar conjuntos de CPU que comparten una caché y un núcleo físico puede proporcionar un rendimiento aún mayor que su programación en núcleos físicos independientes si no requieren más del 50% del tiempo en un fotograma determinado. 

Este ejemplo de código muestra cómo determinar si los subprocesos que se comunican con frecuencia pueden compartir una caché de último nivel.

```
unsigned long retsize = 0;
(void)GetSystemCpuSetInformation(nullptr, 0, &retsize,
    GetCurrentProcess(), 0);
 
std::unique_ptr<uint8_t[]> data(new uint8_t[retsize]);
if (!GetSystemCpuSetInformation(
    reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>(data.get()),
    retsize, &retsize, GetCurrentProcess(), 0))
{
    // Error!
}
 
unsigned long count = retsize / sizeof(SYSTEM_CPU_SET_INFORMATION);
bool sharedcache = false;
 
std::map<unsigned char, std::vector<SYSTEM_CPU_SET_INFORMATION>> cachemap;
for (size_t i = 0; i < count; ++i)
{
    auto cpuset = reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>(data.get())[i];
    if (cpuset.Type == CPU_SET_INFORMATION_TYPE::CpuSetInformation)
    {
        if (cachemap.find(cpuset.CpuSet.LastLevelCacheIndex) == cachemap.end())
        {
            std::pair<unsigned char, std::vector<SYSTEM_CPU_SET_INFORMATION>> newvalue;
            newvalue.first = cpuset.CpuSet.LastLevelCacheIndex;
            newvalue.second.push_back(cpuset);
            cachemap.insert(newvalue);
        }
        else
        {
            sharedcache = true;
            cachemap[cpuset.CpuSet.LastLevelCacheIndex].push_back(cpuset);
        }
    }
}
```

El diseño de caché que se muestra en la figura 1 es un ejemplo del tipo de diseño que se puede ver de un sistema. Esta figura es una ilustración de las cachés de un Microsoft Lumia 950. La comunicación entre subprocesos que se produce entre la CPU 256 y la CPU 260 supondría una sobrecarga significativa porque requeriría que el sistema mantuviese la coherencia de sus cachés L2.

**Figura 1. Arquitectura de caché que se encuentra en un dispositivo Microsoft Lumia 950.**

![Caché del Lumia 950](images/cpusets-lumia950cache.png)

## Resumen

La API de CPUSets disponible para el desarrollo de UWP proporciona una cantidad considerable de información y control sobre las opciones de multithreading. La complejidad adicional en comparación con las API multiproceso de desarrollo de Windows presenta alguna curva de aprendizaje, pero la mayor flexibilidad permite, en última instancia, un rendimiento superior en una amplia gama de equipos de consumo y otros destinos de hardware.

## Recursos adicionales
- [Conjuntos de CPU (MSDN)](https://msdn.microsoft.com/library/windows/desktop/mt186420(v=vs.85).aspx)
- [Muestra de CPUSets proporcionada por ATG](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/CPUSets)
- [UWP en Xbox One](index.md)




<!--HONumber=Aug16_HO3-->


