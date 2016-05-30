---
author: jnHs
Description: La página Propiedades de la aplicación del proceso de envío de aplicaciones te permite definir la categoría de la aplicación e indicar las preferencias de hardware u otras declaraciones.
title: Introducir las propiedades de la aplicación
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
---

# Introducir las propiedades de la aplicación

La página **Propiedades de la aplicación** del [proceso de envío de aplicaciones](app-submissions.md) te permite definir la categoría de la aplicación e indicar las preferencias de hardware u otras declaraciones. A continuación, te guiaremos a través de las opciones de esta página y todo lo que debes tener en cuenta al escribir la información.

> **Nota**  Las clasificaciones por edades son ahora una página independiente del proceso de envío. Para más información, consulta [Clasificaciones por edades](age-ratings.md).

## Categoría y subcategoría

En esta sección, debes indicar la categoría (y la subcategoría, si procede) que debe usar la Tienda para clasificar la aplicación. Especificar una categoría es necesario para enviar la aplicación.

Para más información, consulta [Tabla de categoría y subcategoría](category-and-subcategory-table.md).

## Preferencias de hardware


En esta sección, tienes la opción de indicar si determinadas características de hardware son necesarias para que la aplicación se ejecute correctamente y para interactuar con ella.

Si es así, la Tienda Windows intentará detectar si el dispositivo de un cliente admite la característica de hardware seleccionada. Si se detecta que no es así, se muestra una advertencia que informa al cliente de que está intentando descargar una aplicación que declara una preferencia por ese hardware. Los clientes que usen dispositivos de Windows 10 también verán las características seleccionadas enumeradas en la sección **Requisitos de hardware** de la lista de aplicaciones de la Tienda.

Esto no impedirá que los usuarios descarguen tu aplicación en dispositivos que no tienen el hardware apropiado, pero no podrán evaluarla ni opinar sobre ella en esos dispositivos.

> **Importante**  Excepto en el caso de la **pantalla táctil**, estas advertencias solo se muestran a los clientes con dispositivos Windows 10 que no tengan las características seleccionadas.

Además de la selección que realizamos aquí, te recomendamos agregar a la aplicación comprobaciones en tiempo de ejecución para el hardware especificado, dado que la Tienda no siempre puede detectar si al dispositivo de un cliente le falta la característica seleccionada. De todos modos, el cliente podrá descargar la aplicación, aunque le muestre una advertencia.

> **Sugerencia**  Si quieres evitar que tu aplicación para UWP se descargue en un dispositivo que no cumpla con los requisitos mínimos de memoria o nivel de DirectX, puedes designar los requisitos mínimos en un archivo XML de StoreManifest. Para obtener más información, consulta [Esquema StoreManifest (Windows 10)](https://msdn.microsoft.com/library/windows/apps/mt617335).

## Declaraciones de la aplicación


Puedes agregar casillas en esta sección para indicar si alguna de las declaraciones se aplica a la aplicación. Esto puede afectar a la manera en que se muestra la aplicación, a si se ofrece a determinados clientes o al modo en que los clientes pueden usarla.

Para más información, consulta el tema sobre las [Declaraciones de la aplicación](app-declarations.md).


<!--HONumber=May16_HO2-->


