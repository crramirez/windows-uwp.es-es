---
author: payzer
title: "Referencia de API de la directiva de actualización del kit de desarrollo Device Portal Xbox Developer"
description: "Obtén información acerca de cómo establecer la directiva de actualización de la consola mediante programación."
translationtype: Human Translation
ms.sourcegitcommit: 8f02e0c2f6fa30a3ac56945347c5bec253189bd8
ms.openlocfilehash: f9313d3c8b93ba13074c547f1f63c9f3204f0f58

---

NOTA: tendrás disponible esta API en la siguiente vista previa de desarrollador.

# Referencia de API de la directiva de actualización del sistema   
Puedes usar esta API para ver qué directiva de actualización se aplica a la consola y cambiarla por una nueva.

IMPORTANTE: la mayoría de consolas recibirán una respuesta de "acceso denegado" al intentar llamar a esta API. Esto se debe a que no todas las consolas de desarrollo tienen la capacidad de cambiar su directiva de actualización.

Asimismo, esta API afecta a la directiva de actualización de consolas en modo de desarrollador, no a la directiva de las consolas comercializadas.

## Obtener la directiva de actualización de la consola

**Solicitud**

Puedes usar la siguiente solicitud para obtener la directiva de actualización de la consola.

Método      | URI de la solicitud
:------     | :-----
GET | /ext/update/policy
<br />
**Parámetros de URI**

- Ninguno

**Encabezados de la solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**   
La respuesta es una matriz JSON que contiene la pertenencia de la consola al grupo de actualización del sistema. Cada objeto tiene los siguientes campos:   

Id: (cadena) id. del grupo de actualización.   
Nombre descriptivo: (cadena) nombre para mostrar del grupo de actualización.   
Descripción: ("cadena") descripción del grupo de actualización.
IsDevKitGroup: (verdadero | falso) indica si el grupo de actualización es para las compilaciones de desarrollador.
ResourceSetID: (cadena) ignorar; lo usa la infraestructura de actualización del sistema.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | La solicitud se realizó correctamente.
4XX | Códigos de error
5XX | Códigos de error

## Establecer la directiva de actualización del sistema de la consola
Puedes usar esta API para cambiar la pertenencia de la consola al grupo de actualización del sistema.

Nota: las consolas únicamente pueden estar en un solo grupo de actualización del sistema.

**Solicitud**

Puedes usar la siguiente solicitud para establecer la pertenencia de la consola al grupo de actualización del sistema.

Método      | URI de la solicitud
:------     | :-----
POST | /ext/update/policy
<br />
**Parámetros de URI**

- Ninguno

**Encabezados de la solicitud**

- Ninguno

**Cuerpo de la solicitud**   
El cuerpo de la solicitud es un objeto JSON que contiene los siguientes campos:   
GroupIdToJoin: (cadena) id. del grupo de actualización del sistema en el que quieres incluir la consola.  
GroupIdToLeave: (cadena) id. del grupo de actualización del sistema del que quieres quitar la consola.

Todos los campos son necesarios.

Los elementos GroupID posibles son:   
* No hay ninguna actualización: "b2dbed33-2845-44cc-a7a1-4a9afb29d8d9"   
* Recuperación de producción más reciente: "7432ae99-8c09-48dd-99f9-a2697499e240"   
* Recuperación de vista previa más reciente: "a8153054-1a1b-47cc-acc9-9aed90c1f8db"    

**Respuesta**   

- Ninguna

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | La solicitud se realizó correctamente.
4XX | Códigos de error
5XX | Códigos de error

<br />
**Familias de dispositivos disponibles**

* Windows Xbox




<!--HONumber=Aug16_HO3-->


