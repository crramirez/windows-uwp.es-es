---
title: Conceptos básicos sobre la muestra de Marble Maze
description: En este documento se describen las características fundamentales del proyecto de Marble Maze. por ejemplo, cómo utiliza Visual C++ en el entorno de Windows Runtime, cómo se crea y estructura, y cómo se compila.
ms.assetid: 73329b29-62e3-1b36-01db-b7744ee5b4c3
ms.date: 08/22/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, ejemplo, DirectX, aspectos básicos
ms.localizationpriority: medium
ms.openlocfilehash: 714641be3c5ae6e202f0d6b7da5a1a4c1fc93d86
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165329"
---
# <a name="marble-maze-sample-fundamentals"></a>Conceptos básicos sobre la muestra de Marble Maze




En este tema se describen las características fundamentales del proyecto de Marble Maze, &mdash; por ejemplo, cómo usa Visual C++ en el entorno de Windows Runtime, cómo se crea y estructura, y cómo se compila. En el tema también se describen algunas de las convenciones que se usan en el código.

> [!NOTE]
> El código de ejemplo que corresponde a este documento se encuentra en el [ejemplo Game Marble Maze de DirectX](https://github.com/microsoft/Windows-appsample-marble-maze).

Estos son algunos de los puntos principales que se tratan en este documento para cuando se planea y se desarrolla un juego para la Plataforma universal de Windows (UWP).

-   Use la plantilla **aplicación DirectX 11 (Windows universal C++/CX)** de Visual Studio para crear el juego DirectX UWP.
-   Windows Runtime proporciona clases e interfaces para que puedas desarrollar aplicaciones para UWP de una manera más moderna y orientada a los objetos.
-   Use referencias a objetos con el símbolo Hat (^) para administrar la duración de Windows Runtime variables, [Microsoft:: WRL:: ComPtr](/cpp/windows/comptr-class) para administrar la duración de los objetos com y [STD:: Shared \_ ptr](/cpp/standard-library/shared-ptr-class) o [STD:: Unique \_ ptr](/cpp/standard-library/unique-ptr-class) para administrar la duración de todos los demás objetos de C++ asignados por el montón.
-   En la mayoría de los casos, usa el controlador de excepciones en vez de los códigos de resultados para tratar los errores inesperados.
-   Use [anotaciones sal](/visualstudio/code-quality/using-sal-annotations-to-reduce-c-cpp-code-defects) junto con las herramientas de análisis de código para ayudar a detectar errores en la aplicación.

## <a name="creating-the-visual-studio-project"></a>Creación del proyecto de Visual Studio


Si ha descargado y extraído el ejemplo, puede abrir el archivo **MarbleMaze_VS2017. sln** (en la carpeta de **C++** ) en Visual Studio y tendrá el código delante de usted.

Cuando creamos el proyecto de Visual Studio para Marble Maze, empezamos con un proyecto existente. Sin embargo, si aún no tiene un proyecto existente que proporcione la funcionalidad básica que requiere su juego de DirectX UWP, se recomienda que cree un proyecto basado en la plantilla **aplicación DirectX 11 de Visual Studio (Windows C++/CX)** , ya que proporciona una aplicación 3D básica en funcionamiento. Para ello, siga estos pasos.

1. En Visual Studio 2019, seleccione **archivo > nuevo > proyecto...**

2. En la ventana **crear un nuevo proyecto** , seleccione **aplicación DirectX 11 (universal Windows-C++/CX)**. Si no ve esta opción, es posible que no tenga instalados los componentes necesarios. &mdash; para ello, consulte [modificación de Visual Studio 2019 mediante la adición o eliminación de cargas de trabajo y componentes](/visualstudio/install/modify-visual-studio) para obtener información sobre cómo instalar componentes adicionales.

![Nuevo proyecto](images/vs2019-marble-maze-sample-fundamentals-1.png)

3. Seleccione **siguiente**y, a continuación, escriba un **nombre de proyecto**, una **Ubicación** para los archivos que se van a almacenar y un **nombre de solución**y, a continuación, seleccione **crear**.



Una configuración importante del proyecto en la plantilla **aplicación DirectX 11 (Windows universal C++/CX)** es la opción **/ZW** , que permite al programa usar las extensiones de lenguaje de Windows Runtime. Esta opción está habilitada de manera predeterminada al usar la plantilla de Visual Studio. Vea [establecer las opciones del compilador](/cpp/build/reference/setting-compiler-options) para obtener más información sobre cómo establecer las opciones del compilador en Visual Studio.

> **PRECAUCIÓN**    La opción **/ZW** no es compatible con opciones como **/CLR**. En el caso de **/CLR**, esto significa que no se puede establecer como destino tanto el .NET Framework como el Windows Runtime del mismo proyecto de Visual C++.

 

Todas las aplicaciones de UWP que se adquieren del Microsoft Store se incluyen en forma de un paquete de la aplicación. El paquete de la aplicación incluye un manifiesto del paquete, que contiene información sobre tu aplicación. Por ejemplo, puedes especificar las funcionalidades (es decir, el acceso requerido a recursos del sistema protegidos o datos del usuario) de tu aplicación. Si tu aplicación necesita algún tipo de funcionalidad, usa el manifiesto del paquete para declarar la funcionalidad necesaria. El manifiesto también te permite especificar propiedades del proyecto, como las rotaciones admitidas del dispositivo, las imágenes de los iconos y la pantalla de presentación. Puede editar el manifiesto abriendo **Package. appxmanifest** en el proyecto. Para más información sobre los paquetes de la aplicación, consulta [Empaquetado de aplicaciones](../packaging/index.md).

##  <a name="building-deploying-and-running-the-game"></a> Compilar, implementar y ejecutar el juego

En los menús desplegables en la parte superior de Visual Studio, a la izquierda del botón verde reproducir, seleccione la configuración de implementación. Se recomienda establecerlo como destino de **depuración** en la arquitectura del dispositivo (**x86** para 32 bits, **x64** para 64 bits) y en el **equipo local**. También puede probar en un **equipo remoto**o en un **dispositivo** conectado a través de USB. A continuación, haga clic en el botón verde reproducir para compilar e implementar en el dispositivo.

![Depura bits Máquina local](images/marble-maze-sample-fundamentals-2.png)

###  <a name="controlling-the-game"></a> Control del juego

Puede usar la función táctil, el acelerómetro, el controlador de Xbox One o el mouse para controlar Marble Maze.

-   Usa el control de dirección del mando para cambiar el elemento de menú activo.
-   Use la función táctil, el botón A o inicio del controlador o el mouse para seleccionar un elemento de menú.
-   Usa la entrada táctil, el acelerómetro, el stick analógico izquierdo o el mouse para inclinar el laberinto.
-   Use la función táctil, el botón A o inicio del controlador o el mouse para cerrar menús como la tabla de puntuación alta.
-   Use el botón Inicio del controlador o la tecla P del teclado para pausar o reanudar el juego.
-   Usa el botón Back del mando o la tecla Inicio del teclado para reiniciar el juego.
-   Cuando la tabla de puntuación alta esté visible, use el botón atrás del controlador o la tecla Inicio del teclado para borrar todas las puntuaciones.

##  <a name="code-conventions"></a> Convenciones del código


Windows Runtime es una interfaz de programación que puedes usar para crear aplicaciones para UWP que se ejecutan solo en un entorno de aplicación especial. Dichas aplicaciones usan funciones, tipos de datos y dispositivos autorizados, y se distribuyen desde el Microsoft Store. En el nivel inferior, Windows Runtime consta de una interfaz binaria de aplicaciones (ABI). La ABI es un contrato binario de nivel inferior que hace que varios lenguajes de programación, como JavaScript, los lenguajes .NET y Visual C++, puedan acceder a las API de Windows Runtime.

Para llamar a las API de Windows Runtime desde JavaScript y .NET, estos lenguajes requieren proyecciones que son específicas de cada entorno de lenguaje. Cuando llamas a una API de Windows Runtime desde JavaScript o .NET, estás invocando la proyección que, a su vez, llama a la función de ABI subyacente. Aunque puedes llamar a las funciones de ABI directamente en C++, Microsoft proporciona también proyecciones para C++ porque facilitan mucho el consumo de las API de Windows en tiempo de ejecución al tiempo que mantienen un alto rendimiento. Microsoft también proporciona extensiones de lenguaje para Visual C++ que admiten específicamente las proyecciones de Windows Runtime. Muchas de estas extensiones de lenguaje recuerdan a la sintaxis del lenguaje C++/CLI. Sin embargo, en vez de tener como destino CLR (Common Language Runtime), las aplicaciones nativas usan esta sintaxis para tener Windows Runtime como destino. El modificador de la referencia a objetos, o símbolo circunflejo (^), es una parte importante de esta nueva sintaxis porque permite que se eliminen automáticamente los objetos en tiempo de ejecución mediante el recuento de referencias. En vez de llamar a métodos como [AddRef](/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref) y [Release](/windows/desktop/api/unknwn/nf-unknwn-iunknown-release) para administrar la duración de un objeto de Windows Runtime, el tiempo de ejecución elimina el objeto cuando no hay ningún otro componente que le haga referencia, por ejemplo, cuando deja el ámbito o cuando estableces todas las referencias como **nullptr**. Otro aspecto importante del uso de Visual C++ para crear aplicaciones para UWP es la palabra clave **ref new**. Usa **ref new** en lugar de **new** para crear objetos de Windows Runtime con recuento de referencias. Para obtener más información, consulta [Sistema de tipos (C++/CX)](/cpp/cppcx/type-system-c-cx).

> [!IMPORTANT]
> Solo tiene que usar **^** y **ref nuevo** cuando cree Windows Runtime objetos o cree componentes de Windows Runtime. Puedes usar la sintaxis estándar de C++ cuando escribas código principal de la aplicación que no use Windows Runtime.

Marble Maze usa **^** junto con **Microsoft:: WRL:: ComPtr** para administrar los objetos asignados por montón y minimizar las pérdidas de memoria. Se recomienda usar ^ para administrar la duración de Windows Runtime variables, **ComPtr** para administrar la duración de las variables com (como cuando se usa DirectX) y **STD:: Shared \_ ptr** o **STD:: Unique \_ ptr** para administrar la duración de todos los demás objetos de C++ asignados por el montón.

 

Para más información sobre las extensiones de lenguaje disponibles para una aplicación para UWP con C++, consulta [Referencia de lenguaje Visual C++ (C++/CX)](/cpp/cppcx/visual-c-language-reference-c-cx).

###  <a name="error-handling"></a>Control de errores

Marble Maze usa el controlador de excepciones como método principal para tratar los errores inesperados. Aunque el código de los juegos tradicionalmente usa códigos de error o de registro, como los valores **HRESULT**, para indicar los errores, el controlador de excepciones tiene dos ventajas principales. En primer lugar, hace que el código sea más fácil de leer y mantener. Desde el punto de vista del código, el controlador de excepciones es una manera más eficaz de propagar un error a una rutina que pueda administrar dicho error. Normalmente, el uso de códigos de error requiere que cada función propague los errores explícitamente. Una segunda ventaja es que puedes configurar el depurador de Visual Studio para que se interrumpa cuando ocurra una excepción de manera que puedas parar inmediatamente en el lugar y contexto del error. Windows Runtime también usa mucho el control de excepciones. Por lo tanto, si usas el control de excepciones en tu código puedes combinar todo el control de errores en un solo modelo.

Recomendamos que uses las siguientes convenciones en tu modelo de administración de errores:

-   Usa excepciones para comunicar errores inesperados.
-   No uses excepciones para controlar el flujo de código.
-   Captura solo las excepciones que puedas administrar y de las que te puedas recuperar de forma segura. De lo contrario, no captures la excepción y deja que la aplicación termine.
-   Cuando llames a una rutina de DirectX que devuelva **HRESULT**, usa la función **DX::ThrowIfFailed**. Esta función se define en [DirectXHelper. h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/master/C%2B%2B/Shared/DirectXHelper.h). **ThrowIfFailed** inicia una excepción si el **valor HRESULT** proporcionado es un código de error. Por ejemplo, **el \_ puntero E** hace que **ThrowIfFailed** genere [Platform:: NullReferenceException](/cpp/cppcx/platform-nullreferenceexception-class).

    Cuando uses **ThrowIfFailed**, pon la llamada de DirectX en una línea aparte para mejorar la lectura del código, tal como se muestra en el siguiente ejemplo.

    ```cpp
    // Identify the physical adapter (GPU or card) this device is running on.
    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );
    ```

-   Aunque se recomienda evitar el uso de **HRESULT** para errores inesperados, es más importante evitar el uso del control de excepciones para controlar el flujo de código. Por lo tanto, se prefiere usar un valor de retorno **HRESULT** cuando sea necesario para controlar el flujo de código.

###  <a name="sal-annotations"></a>Anotaciones SAL

Usa anotaciones de SAL junto con herramientas de análisis de código para detectar errores en tu aplicación.

Al usar el lenguaje de anotaciones de código fuente (SAL) de Microsoft, puedes anotar, o describir, cómo las funciones usan sus parámetros. Las anotaciones de SAL también describen valores de retorno. Las anotaciones de SAL funcionan con la herramienta de análisis de código C/C++ para detectar los posibles defectos en el código fuente C y C++. Entre los errores de codificación más frecuentes notificados por esta herramienta se incluyen saturaciones de búfer, memoria sin inicializar, desreferencias de puntero NULL, y pérdidas de memoria y recursos.

Considere el método **BasicLoader:: LoadMesh** , que se declara en [BasicLoader. h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/e62d68a85499e208d591d2caefbd9df62af86809/C%2B%2B/Shared/BasicLoader.h). Este método usa `_In_` para especificar que *filename* es un parámetro de entrada (y, por lo tanto, solo se leerá desde), `_Out_` para especificar que *vertexBuffer* y *indexBuffer* son parámetros de salida (y, por consiguiente, solo se escribirán en) y `_Out_opt_` para especificar que *vertexCount* y *indexCount* son parámetros de salida opcionales (y se pueden escribir en ellos). Como *vertexCount* y *indexCount* son parámetros de salida opcionales, pueden ser **nullptr**. La herramienta de análisis de código C/C++ examina las llamadas a este método para asegurarse de que los parámetros que pase cumplan estos criterios.

```cpp
void LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    );
```

Para realizar el análisis de código en la aplicación, en la barra de menús, elija **Compilar > ejecutar análisis de código en la solución**. Para más información sobre el análisis de código, consulta [Analizar la calidad del código C/C++ mediante el análisis de código](/visualstudio/code-quality/analyzing-c-cpp-code-quality-by-using-code-analysis).

La lista completa de anotaciones disponibles está definida en sal.h. Para obtener más información, consulta [Anotaciones de SAL](/cpp/c-runtime-library/sal-annotations).

## <a name="next-steps"></a>Pasos siguientes


Lee [Estructura de la aplicación Marble Maze](marble-maze-application-structure.md) para obtener información sobre cómo está estructurado el código de la aplicación Marble Maze y para ver las diferencias de las aplicaciones para UWP con DirectX respecto a las aplicaciones de escritorio tradicionales.

## <a name="related-topics"></a>Temas relacionados


* [Estructura de la aplicación Marble Maze](marble-maze-application-structure.md)
* [Desarrollo de Marble Maze, un juego para UWP en C++ y DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 