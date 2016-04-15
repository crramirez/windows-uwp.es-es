---
title: Crear un componente básico de Windows Runtime en C++ y llamarlo desde JavaScript o C#
description: En este tutorial se muestra cómo crear un archivo DLL básico del componente de Windows Runtime que se pueda llamar desde JavaScript, C# o Visual Basic.
ms.assetid: 764CD9C6-3565-4DFF-88D7-D92185C7E452
---

# Tutorial: Crear en C++ un componente básico de Windows Runtime y llamarlo desde JavaScript o C#


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.\]

En este tutorial se muestra cómo crear un archivo DLL básico del componente de Windows Runtime que se pueda llamar desde JavaScript, C# o Visual Basic. Antes de comenzar este tutorial, asegúrate de que conoces conceptos como la interfaz binaria abstracta (ABI), las clases de referencia y las extensiones del componente de Visual C++, que facilitan el trabajo con clases de referencia. Para obtener más información, consulta [Crear componentes de Windows Runtime en C++](creating-windows-runtime-components-in-cpp.md) y [Referencia del lenguaje de Visual C++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx).

## Crear el archivo DLL del componente de C++


En este ejemplo, en primer lugar se crea el proyecto de componente, pero primero se podría crear el proyecto de JavaScript. No importa el orden.

Ten en cuenta que la clase principal del componente contiene ejemplos de definiciones de propiedad y método y una declaración de evento. Se proporcionan solo para mostrar cómo se hace. No son necesarios y, en este ejemplo, reemplazaremos todo el código generado por nuestro propio código.

## **Para crear el proyecto de componente de C++**

1.  En la barra de menús de Visual Studio, elige **Archivo, Nuevo, Proyecto**.
2.  En el cuadro de diálogo **Nuevo proyecto**, en el panel izquierdo, expande **Visual C++** y, a continuación, selecciona el nodo para las aplicaciones universales de Windows.
3.  En el panel central, selecciona **Componente de Windows Runtime** y, a continuación, asigna al proyecto el nombre de WinRT\_CPP.
4.  Elige el botón **Aceptar**.

## **Para agregar una clase activable al componente**

1.  Una clase activable es la que puede crear el código de cliente mediante una **nueva** expresión (**Nuevo** en Visual Basic, o **ref new** en C++). En tu componente, debe declararse como **public ref class sealed**. De hecho, los archivos Class1.h y .cpp ya tienen una clase de referencia. Puedes cambiar el nombre, pero en este ejemplo, usaremos el nombre predeterminado: Class1. Puedes definir clases de referencia adicionales o normales en tu componente si son necesarias. Para obtener más información sobre las clases de referencia, consulta [Sistema de tipo (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx).

2.  Agregar estas \#directivas a Class1.h:

    ```cpp
             private void PrimesUnOrderedButton_Click_1(object sender, RoutedEventArgs e)
            {
                var nativeObject = new WinRT_CPP.Class1();

                StringBuilder sb = new StringBuilder();
                sb.Append("Primes found (unordered): ");
                PrimesUnOrderedResult.Text = sb.ToString();

                // primeFoundEvent is a user-defined event in nativeObject
                // It passes the results back to this thread as they are produced
                // and the event handler that we define here immediately displays them.
                nativeObject.primeFoundEvent += (n) =>
                {
                    sb.Append(n.ToString()).Append(" ");
                    PrimesUnOrderedResult.Text = sb.ToString();
                };

                // Call the async method.
                var asyncResult = nativeObject.GetPrimesUnordered(2, 100000);

                // Provide a handler for the Progress event that the asyncResult
                // object fires at regular intervals. This handler updates the progress bar.
                asyncResult.Progress += (asyncInfo, progress) =>
                    {
                        PrimesUnOrderedProgress.Value = progress;
                    };
            }

            private void Clear_Button_Click(object sender, RoutedEventArgs e)
            {
                PrimesOrderedProgress.Value = 0;
                PrimesUnOrderedProgress.Value = 0;
                PrimesUnOrderedResult.Text = "";
                PrimesOrderedResult.Text = "";
                Result1.Text = "";
            }
    ```

## Ejecución de la aplicación


Selecciona o bien el proyecto de C# o o bien el proyecto de JavaScript como proyecto de inicio abriendo el menú contextual para el nodo del proyecto en el Explorador de soluciones y selecciona **Establecer como proyecto de inicio**. A continuación pulsa F5 para una ejecución con depuración o Ctrl-F5 para una ejecución sin depuración.

## Inspeccionar tu componente en el Explorador de objetos (opcional)


En el Explorador de objetos, puedes inspeccionar todos los tipos de Windows Runtime que se definen en los archivos .winmd. Esto incluye los tipos en el espacio de nombres de plataforma y el espacio de nombres predeterminado. Sin embargo, dado que los tipos en el espacio de nombres Platform::Collections se definen en el archivo de encabezado collections.h, y no en un archivo winmd, no aparecen en el explorador de objetos.

## **Para inspeccionar un componente**

1.  En la barra de menús, elige **Vista, Explorador de objetos** (Ctrl+Alt+J).
2.  En el panel izquierdo del explorador de objetos, expande el nodo WinRT\_CPP para mostrar los tipos y métodos que se definen en tu componente.

## Consejos de depuración


Para una mejor experiencia de depuración, descarga los símbolos de depuración de los servidores de símbolos públicos de Microsoft:

## **Para descargar los símbolos de depuración**

1.  En la barra de menús, elige **Herramientas, Opciones**.
2.  En el cuadro de diálogo de **Opciones**, expande **Depuración** y selecciona **Símbolos**.
3.  Selecciona **Servidores de símbolos de Microsoft** y, a continuación, selecciona el botón **Aceptar**.

Puede tardar algún de tiempo en descargar los símbolos la primera vez. Para un rendimiento más rápido, la próxima vez que pulses F5, especifica un directorio local en el que se almacenarán en caché los símbolos.

Cuando se depura una solución de JavaScript que tiene un archivo DLL del componente, puedes configurar el depurador para habilitar o bien la ejecución paso a paso a través del script, o bien del código nativo en el componente, pero no ambas opciones al mismo tiempo. Para cambiar la configuración, abre el menú contextual para el nodo de proyecto de JavaScript en el Explorador de soluciones y elige **Propiedades, Depurar, Tipo de depurador**.

Asegúrate de seleccionar las funcionalidades adecuadas en el diseñador de paquetes. Puedes abrir el diseñador de paquetes abriendo el archivo Package.appxmanifest. Por ejemplo, si estás intentando acceder mediante programación a archivos en la carpeta Imágenes, asegúrate de seleccionar la casilla de verificación de la **Biblioteca de imágenes** en el panel **Funcionalidades** del diseñador de paquetes.

Si tu código JavaScript no reconoce las propiedades o métodos públicos en el componente, asegúrate de que estás usando la convención Camel de mayúsculas y minúsculas en JavaScript. Por ejemplo, se debe hacer referencia al método `ComputeResult` de C++ como `computeResult` en JavaScript.

Si quitas un proyecto de componente de Windows Runtime de C++ de una solución, también debes quitar manualmente la referencia del proyecto de JavaScript. De lo contrario, no se efectuará la depuración o las operaciones de compilación posteriores. Si es necesario, a continuación puedes agregar una referencia de ensamblado a la DLL.

## Temas relacionados

* [Crear componentes de Windows Runtime en C++](creating-windows-runtime-components-in-cpp.md)



<!--HONumber=Mar16_HO1-->


