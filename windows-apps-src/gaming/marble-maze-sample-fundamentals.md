---
author: mtoepke
title: "Conceptos básicos sobre la muestra de Marble Maze"
description: "En este documento se describen las características fundamentales del proyecto Marble Maze, como la forma en que usa Visual C++ en el entorno de Windows Runtime, cómo se crea y se estructura, y cómo se compila."
ms.assetid: 73329b29-62e3-1b36-01db-b7744ee5b4c3
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 5a9df995078763df73542a4101e73e147517b1eb

---

# Conceptos básicos sobre la muestra de Marble Maze


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


En este documento se describen las características fundamentales del proyecto Marble Maze, como la forma en que usa Visual C++ en el entorno de Windows Runtime, cómo se crea y se estructura, y cómo se compila. El documento también describe varias convenciones que se usan en el código.

> 
            **Nota**  El código de ejemplo correspondiente a este documento se encuentra en la [muestra de Marble Maze con DirectX](http://go.microsoft.com/fwlink/?LinkId=624011).

 
## 
Estos son algunos de los puntos principales que se tratan en este documento para cuando se planea y se desarrolla un juego para la Plataforma universal de Windows (UWP).

-   Usa la plantilla de **DirectX 11 App (Windows universal)** en una aplicación con C++ para crear tu juego para UWP. Usa Visual Studio para compilar un proyecto de aplicación para UWP del mismo modo que lo harías con un proyecto estándar.
-   Windows Runtime proporciona clases e interfaces para que puedas desarrollar aplicaciones para UWP de una manera más moderna y orientada a los objetos.
-   Usa referencias a objetos con el símbolo circunflejo (^) para administrar la duración de las variables de Windows Runtime, [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) para administrar la duración de los objetos COM y [**std::shared\_ptr**](https://msdn.microsoft.com/library/windows/apps/bb982026.aspx) o [**std::unique\_ptr**](https://msdn.microsoft.com/library/windows/apps/ee410601.aspx) para administrar la duración de todos los demás objetos de C++ asignados por montón.
-   En la mayoría de los casos, usa el controlador de excepciones en vez de los códigos de resultados para tratar los errores inesperados.
-   Usa anotaciones de SAL junto con herramientas de análisis de código para detectar errores en tu aplicación.

## Creación del proyecto de Visual Studio


Si has descargado y extraído la muestra, puedes abrir el archivo de la solución MarbleMaze.sln en Visual Studio para ver el código. También puedes ver el código fuente en la página de la Galería de muestras de MSDN [Muestra del juego Marble Maze con DirectX](http://go.microsoft.com/fwlink/?LinkId=624011) seleccionando la pestaña **Examinar código**.

Cuando creamos el proyecto de Visual Studio para Marble Maze, empezamos con un proyecto existente. Sin embargo, si aún no tienes un proyecto existente que proporcione la funcionalidad básica que requiere tu juego para UWP con DirectX, recomendamos que crees un proyecto basado en la plantilla **DirectX 11 (Windows universal)** de Visual Studio porque proporciona una aplicación 3-D de trabajo básica.

Un valor importante del proyecto en la plantilla **DirectX 11 App (Windows universal)** es la opción **/ZW**, que permite que el programa pueda usar las extensiones del lenguaje de Windows Runtime. Esta opción está habilitada de manera predeterminada al usar la plantilla de Visual Studio.

> 
            **Precaución**  La opción **/ZW** no es compatible con opciones como **/clr**. En el caso de **/clr**, esto significa que no puedes tener .NET Framework y Windows Runtime como destino desde el mismo proyecto de Visual C++.

 

Cada aplicación para UWP que adquieras de la Tienda Windows tiene la forma de paquete de la aplicación. El paquete de la aplicación incluye un manifiesto del paquete, que contiene información sobre tu aplicación. Por ejemplo, puedes especificar las funcionalidades (es decir, el acceso requerido a recursos del sistema protegidos o datos del usuario) de tu aplicación. Si tu aplicación necesita algún tipo de funcionalidad, usa el manifiesto del paquete para declarar la funcionalidad necesaria. El manifiesto también te permite especificar propiedades del proyecto, como las rotaciones admitidas del dispositivo, las imágenes de los iconos y la pantalla de presentación. Para más información sobre los paquetes de la aplicación, consulta [Empaquetado de aplicaciones](https://msdn.microsoft.com/library/windows/apps/mt270969).

##   Compilar, implementar y ejecutar el juego


Compila un proyecto de aplicación para UWP como lo harías con un proyecto estándar. (En la barra de menús, elige **Compilar, Compilar solución**.) Este paso compila el código y lo empaqueta para usarlo como una aplicación UWP.

Después de compilar el proyecto, debes implementarlo. (En la barra de menús, elige **Compilar, Implementar solución**.) Visual Studio también implementa el proyecto cuando ejecutes el juego desde el depurador.

Una vez implementado el proyecto, elige el icono de Marble Maze para ejecutar el juego. De forma alternativa, desde Visual Studio, en la barra de menús, elige **Depurar, Iniciar depuración**.

###   Control del juego

Puedes usar la entrada táctil, el acelerómetro, el mando de la Xbox 360 o el mouse para controlar Marble Maze.

-   Usa el control de dirección del mando para cambiar el elemento de menú activo.
-   Usa la entrada táctil, el botón A , el botón Start o el mouse para seleccionar un elemento del menú.
-   Usa la entrada táctil, el acelerómetro, el stick analógico izquierdo o el mouse para inclinar el laberinto.
-   Usa la entrada táctil, el botón A , el botón Start o el mouse para cerrar menús, como por ejemplo la tabla de puntuaciones máximas.
-   Usa el botón Start o la tecla P para pausar o reanudar el juego.
-   Usa el botón Back del mando o la tecla Inicio del teclado para reiniciar el juego.
-   Cuando la tabla de puntuaciones máximas esté visible, usa el botón Back o la tecla Inicio para borrar las puntuaciones.

##   Convenciones del código


Windows Runtime es una interfaz de programación que puedes usar para crear aplicaciones para UWP que se ejecutan solo en un entorno de aplicación especial. Estas aplicaciones usan funciones autorizadas, tipos de datos y dispositivos, y se distribuyen desde la Tienda Windows. En el nivel inferior, Windows Runtime consta de una interfaz binaria de aplicaciones (ABI). La ABI es un contrato binario de nivel inferior que hace que varios lenguajes de programación, como JavaScript, los lenguajes .NET y Visual C++, puedan acceder a las API de Windows Runtime.

Para llamar a las API de Windows Runtime desde JavaScript y .NET, estos lenguajes requieren proyecciones que son específicas de cada entorno de lenguaje. Cuando llamas a una API de Windows Runtime desde JavaScript o .NET, estás invocando la proyección que, a su vez, llama a la función de ABI subyacente. Aunque puedes llamar a las funciones de ABI directamente en C++, Microsoft proporciona también proyecciones para C++ porque facilitan mucho el consumo de las API de Windows en tiempo de ejecución al tiempo que mantienen un alto rendimiento. Microsoft también proporciona extensiones de lenguaje para Visual C++ que admiten específicamente las proyecciones de Windows Runtime. Muchas de estas extensiones de lenguaje recuerdan a la sintaxis del lenguaje C++/CLI. Sin embargo, en vez de tener como destino CLR (Common Language Runtime), las aplicaciones nativas usan esta sintaxis para tener Windows Runtime como destino. El modificador de la referencia a objetos, o símbolo circunflejo (^), es una parte importante de esta nueva sintaxis porque permite que se eliminen automáticamente los objetos en tiempo de ejecución mediante el recuento de referencias. En vez de llamar a métodos como **AddRef** y **Release** para administrar la duración de un objeto de Windows Runtime, el tiempo de ejecución elimina el objeto cuando no hay ningún otro componente que le haga referencia, por ejemplo, cuando deja el ámbito o cuando estableces todas las referencias como **nullptr**. Otro aspecto importante del uso de Visual C++ para crear aplicaciones para UWP es la palabra clave **ref new**. Usa **ref new** en lugar de **new** para crear objetos de Windows Runtime con recuento de referencias. Para obtener más información, consulta [Sistema de tipos (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822).

> **Importante**  
Solo tienes que usar **^** y **ref new** cuando crees objetos de Windows Runtime o componentes de Windows Runtime. Puedes usar la sintaxis estándar de C++ cuando escribas código principal de la aplicación que no use Windows Runtime.

Marble Maze usa **^** junto con [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) para administrar objetos asignados por montón y minimizar las pérdidas de memoria. Te recomendamos que uses ^ para administrar la duración de las variables de Windows Runtime, **ComPtr** para administrar la duración de las variables COM (como cuando usas DirectX) y std::[**std::shared\_ptr**](https://msdn.microsoft.com/library/windows/apps/bb982026) o [**std::unique\_ptr**](https://msdn.microsoft.com/library/windows/apps/ee410601) para administrar la duración de todos los demás objetos de C++ asignados por montón.

 

Para más información sobre las extensiones de lenguaje disponibles para una aplicación para UWP con C++, consulta [Referencia de lenguaje Visual C++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh699871).

###   Administración de errores

Marble Maze usa el controlador de excepciones como método principal para tratar los errores inesperados. Aunque el código de los juegos tradicionalmente usa códigos de error o de registro, como los valores **HRESULT**, para indicar los errores, el controlador de excepciones tiene dos ventajas principales. En primer lugar, hace que el código sea más fácil de leer y mantener. Desde el punto de vista del código, el controlador de excepciones es una manera más eficaz de propagar un error a una rutina que pueda administrar dicho error. Normalmente, el uso de códigos de error requiere que cada función propague los errores explícitamente. Una segunda ventaja es que puedes configurar el depurador de Visual Studio para que se interrumpa cuando ocurra una excepción de manera que puedas parar inmediatamente en el lugar y contexto del error. Windows Runtime también usa mucho el control de excepciones. Por lo tanto, si usas el control de excepciones en tu código puedes combinar todo el control de errores en un solo modelo.

Recomendamos que uses las siguientes convenciones en tu modelo de administración de errores:

-   Usa excepciones para comunicar errores inesperados.
-   No uses excepciones para controlar el flujo de código.
-   Captura solo las excepciones que puedas administrar y de las que te puedas recuperar de forma segura. De lo contrario, no captures la excepción y deja que la aplicación termine.
-   Cuando llames a una rutina de DirectX que devuelva **HRESULT**, usa la función **DX::ThrowIfFailed**. Esta función se define en DirectXSample.h.**ThrowIfFailed** emite una excepción si el **HRESULT** proporcionado es un código de error. Por ejemplo, **E\_POINTER** hace que **ThrowIfFailed** inicie [**Platform::NullReferenceException**](https://msdn.microsoft.com/library/windows/apps/hh755823.aspx).

    Cuando uses **ThrowIfFailed**, pon la llamada de DirectX en una línea aparte para mejorar la lectura del código, tal como se muestra en el siguiente ejemplo.

    ```cpp
    // Identify the physical adapter (GPU or card) this device is running on.
    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );
    ```

-   Aunque recomendamos que evites el uso de **HRESULT** para los errores inesperados, es más importante evitar el uso del controlador de excepciones para controlar el flujo de código. Por lo tanto, se prefiere usar un valor de retorno **HRESULT** cuando sea necesario para controlar el flujo de código.

###   Anotaciones de SAL

Usa anotaciones de SAL junto con herramientas de análisis de código para detectar errores en tu aplicación.

Al usar el lenguaje de anotaciones de código fuente (SAL) de Microsoft, puedes anotar, o describir, cómo las funciones usan sus parámetros. Las anotaciones de SAL también describen valores de retorno. Las anotaciones de SAL funcionan con la herramienta de análisis de código C/C++ para detectar los posibles defectos en el código fuente C y C++. Los errores de codificación comunes notificados por la herramienta incluyen las saturaciones del búfer, la memoria sin inicializar, las desreferencias de puntero nulas y la pérdida de recursos y de memoria.

Considera el método **BasicLoader::LoadMesh**, que se declara en BasicLoader.h. Este método usa \_In\_ para especificar que *filename* es un parámetro de entrada (y por lo tanto solo se leerá), \_Out\_ para especificar que *vertexBuffer* e *indexBuffer* son parámetros de salida (y por lo tanto solo se escribirá en ellos) y \_Out\_opt\_ para especificar que *vertexCount* e *indexCount* son parámetros de salida opcionales (y se podría escribir en ellos). Como *vertexCount* y *indexCount* son parámetros de salida opcionales, pueden ser **nullptr**. La herramienta de análisis de código C/C++ examina las llamadas a este método para asegurarse de que los parámetros que pase cumplan estos criterios.

```cpp
void LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    );
```

Para realizar un análisis de código en tu aplicación, en la barra de menús elige **Compilar, Ejecutar análisis de código en la solución**. Para más información sobre el análisis de código, consulta [Analizar la calidad del código C/C++ mediante el análisis de código](https://msdn.microsoft.com/library/windows/apps/ms182025.aspx).

La lista completa de anotaciones disponibles está definida en sal.h. Para obtener más información, consulta [Anotaciones de SAL](https://msdn.microsoft.com/library/windows/apps/ms235402.aspx).

## Pasos siguientes


Lee [Estructura de la aplicación Marble Maze](marble-maze-application-structure.md) para obtener información sobre cómo está estructurado el código de la aplicación Marble Maze y para ver las diferencias de las aplicaciones para UWP con DirectX respecto a las aplicaciones de escritorio tradicionales.

## Temas relacionados


* [Estructura de la aplicación Marble Maze](marble-maze-application-structure.md)
* [Desarrollo de Marble Maze, un juego para UWP en C++ y DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 







<!--HONumber=Jun16_HO4-->


