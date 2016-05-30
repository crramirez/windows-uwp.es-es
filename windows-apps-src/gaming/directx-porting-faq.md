---
author: mtoepke
title: Preguntas más frecuentes sobre la migración a DirectX 11
description: Respuestas a las preguntas más frecuentes sobre la migración de juegos a la Plataforma universal de Windows (UWP).
ms.assetid: 79c3b4c0-86eb-5019-97bb-5feee5667a2d
---

# Preguntas más frecuentes sobre la migración a DirectX 11


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Respuestas a las preguntas más frecuentes sobre la migración de juegos a la Plataforma universal de Windows (UWP).

## ¿La migración del juego implicará un conjunto de operaciones de buscar y reemplazar en métodos de API o debo pensar en un proceso más complejo?


Direct3D 11 es una actualización significativa de Direct3D 9. Hay varios cambios de paradigma, entre ellos distintas API para el adaptador de gráficos virtualizados y su contexto, así como un nuevo nivel de polimorfismo para recursos de dispositivo. Tu juego aún puede usar hardware de gráficos esencialmente de la misma manera, pero tendrás que aprender sobre la nueva arquitectura de API que presenta Direct3D 11 y actualizar cada parte del código de gráficos para usar los componentes correctos de API. Consulta [Conceptos y consideraciones de migración](porting-considerations.md).

## ¿Para qué sirve el nuevo contexto de dispositivo? ¿Qué se supone que debo reemplazar, el dispositivo Direct3D 9 por el dispositivo Direct3D 11, su contexto o ambos?


El dispositivo Direct3D ahora se usa para crear recursos en memoria de vídeo, mientras que el contexto de dispositivo se usa para establecer el estado de canalización y generar comandos de representación. Para obtener más información, consulta el artículo que te indicará [cuáles son los cambios más importantes desde Direct3D 9](understand-direct3d-11-1-concepts.md)

##  ¿Tengo que actualizar el temporizador del juego para UWP?


[
              **QueryPerformanceCounter**
            ](https://msdn.microsoft.com/library/windows/desktop/ms644904), junto con [**QueryPerformanceFrequency**](https://msdn.microsoft.com/library/windows/desktop/ms644905), sigue siendo la mejor manera de implementar un temporizador para las aplicaciones para UWP.

Debes tener en cuenta un aspecto de los temporizadores y el ciclo de vida de una aplicación para UWP. La acción de suspensión y reanudación es diferente a cuando un jugador reinicia un juego de escritorio, ya que tu juego reanudará una instantánea de la última vez que estuvo activo. Si transcurre mucho tiempo, por ejemplo, algunas semanas, algunas implementaciones del temporizador del juego podrían tener errores leves. Puedes usar eventos de ciclo de vida de la aplicación para restablecer el temporizador cuando se reanuda el juego.

Los juegos que todavía usan la instrucción RDTSC deben actualizarse. Consulta [Procesadores de núcleo múltiple y tiempos de juego](https://msdn.microsoft.com/library/windows/desktop/ee417693).

## El código de mi juego se basa en D3DX y DXUT. ¿Existe algo que pueda ayudarme a migrar el código?


El proyecto del [kit de herramientas de DirectX (DirectXTK)](http://go.microsoft.com/fwlink/p/?LinkID=248929) para la comunidad ofrece clases auxiliares para usar con Direct3D 11.

##  ¿Cómo mantengo las rutas de código para el escritorio y la Tienda Windows?


La serie de artículos de Chuck Walbourn titulados [Dual-use Coding Techniques for Games (Uso dual de técnicas de codificación de juegos)](http://go.microsoft.com/fwlink/p/?LinkID=286210) te proporcionará información para que puedas orientarte acerca de cómo compartir código entre las rutas de código de la Tienda Windows y el escritorio.

##  ¿Cómo cargo recursos de imagen en mi aplicación DirectX para UWP?


Hay dos rutas de acceso a API para cargar imágenes:

-   La canalización de contenido convierte imágenes en archivos DDS usados como recursos de textura en Direct3D. Consulta [Usar activos 3D en el juego o aplicación](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx).
-   [Windows Imaging Component](https://msdn.microsoft.com/library/windows/desktop/ee719902) puede usarse para cargar imágenes en varios formatos, además de usarse para mapas de bits de Direct2D y recursos de textura de Direct3D.

También puedes usar DDSTextureLoader y WICTextureLoader, de [DirectXTK](http://go.microsoft.com/fwlink/p/?LinkID=248929) o [DirectXTex](http://go.microsoft.com/fwlink/p/?LinkID=248926).

## ¿Dónde está el SDK de DirectX?


El SDK de DirectX se incluye como parte de Windows SDK. El SDK de DirectX más reciente que se separó del Windows SDK fue el de junio de 2010. Las muestras de Direct3D están en la Galería de código junto con el resto de las muestras de aplicaciones de la Tienda Windows.

## ¿Qué sucede con los redistribuibles de DirectX?


La mayoría de los componentes del Windows SDK ya están incluidos en versiones compatibles del sistema operativo o no tienen ningún componente DLL (como DirectXMath). Todos los componentes de API de Direct3D que las aplicaciones para UWP pueden usar ya están disponibles para tu juego. No es necesario redistribuirlos.

Las aplicaciones de escritorio Win32 todavía usan DirectSetup, por lo tanto, si también estás actualizando la versión de escritorio de tu juego, consulta [Implementación de Direct3D 11 para desarrolladores de juegos](https://msdn.microsoft.com/library/windows/desktop/ee416644).

## ¿Cómo puedo actualizar el código de escritorio a DirectX 11 antes de salir de Efectos?


Consulta [Efectos para la actualización a Direct3D 11](http://go.microsoft.com/fwlink/p/?LinkId=271568). Efectos 11 ayuda a quitar dependencias en encabezados heredados del SDK de DirectX. Está pensado para que se use como ayuda de migración solo en aplicaciones de escritorio.

##  ¿Hay alguna ruta trazada para migrar un juego de DirectX 8 a UWP?


Sí:

-   Lee [Converting to Direct3D 9 (Convertir a Direct3D 9)](https://msdn.microsoft.com/library/windows/desktop/bb204851).
-   Asegúrate de que tu juego no tenga remanentes de la canalización fija. Consulta [Características desusadas](https://msdn.microsoft.com/library/windows/desktop/cc308047).
-   Luego sigue la ruta de migración de DirectX 9: [Migrar de D3D 9 a UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

##  ¿Puedo migrar mi juego DirectX 10 u 11 a UWP?


Los juegos de escritorio de DirectX 10.x y 11 son fáciles de migrar a UWP. Consulta [Migrar a Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476190).

## ¿Cómo elijo el dispositivo de pantalla correcto en un sistema de varios monitores?


El usuario selecciona en qué monitor aparecerá tu aplicación. Deja que Windows proporcione el adaptador correcto llamando a [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) con el primer parámetro establecido en **nullptr**. A continuación, obtén el elemento [**IDXGIDevice interface**](https://msdn.microsoft.com/library/windows/desktop/bb174527) del dispositivo, llama a [**GetAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174531) y usa el adaptador DXGI para crear la cadena de intercambio.

## ¿Cómo activo el suavizado de contorno?


El suavizado de contorno (muestreo múltiple) se habilita cuando creas el dispositivo Direct3D. Puedes enumerar la compatibilidad del muestreo múltiple llamando a [**CheckMultisampleQualityLevels**](https://msdn.microsoft.com/library/windows/desktop/ff476499); a continuación, establece las opciones multimuestra en el elemento [**DXGI\_SAMPLE\_DESC structure**](https://msdn.microsoft.com/library/windows/desktop/bb173072) cuando llames a [**CreateSurface**](https://msdn.microsoft.com/library/windows/desktop/bb174530).

## Mi juego usa multithreading o representación diferida para representar. ¿Qué debo saber para Direct3D 11?


Para empezar, visita [Introducción a multithreading en Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476891). Para obtener una lista de las diferencias principales, consulta [Diferencias de subprocesos entre versiones de Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476890). Ten en cuenta que la representación diferida usa un *contexto diferido* de dispositivo en lugar de un *contexto inmediato*.

## ¿Dónde puedo encontrar más información sobre la canalización programable desde Direct3D 9?


Consulta estos temas:

-   [Guía de programación para HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509635)
-   [Preguntas más frecuentes sobre Direct3D 10](https://msdn.microsoft.com/library/windows/desktop/ee416643)

## ¿Qué debo usar en lugar del formato de archivo .x para mis modelos?


Dado que aún no tenemos un reemplazo oficial del formato de archivo .x, muchas de las muestras usan el formato SDKMesh. Visual Studio también tiene una [canalización de contenido](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx) que compila varios formatos populares en archivos CMO. Estos archivos pueden cargarse con código mediante el kit de inicio de Visual Studio 3D o con [DirectXTK](http://go.microsoft.com/fwlink/p/?LinkID=248929).

## ¿Cómo depuro mis sombreadores?


Microsoft Visual Studio 2015 incluye herramientas de diagnóstico para elementos gráficos de DirectX. Consulta [Depurar gráficos de DirectX](https://msdn.microsoft.com/library/windows/apps/hh315751.aspx).

##  ¿Cuál es el equivalente de la función *x* en Direct3D 11?


Consulta la información acerca de la [asignación de funciones](feature-mapping.md#function-mapping) proporcionada en el artículo "Asignar características de DirectX 9 a las API de DirectX 11".

##  ¿Cuál es el equivalente DXGI\_FORMAT del formato de superficie *y*?


Consulta la información acerca de la [asignación de formatos de superficie](feature-mapping.md#surface-format-mapping) proporcionada en el artículo "Asignar características de DirectX 9 a las API de DirectX 11".

 

 






<!--HONumber=May16_HO2-->


