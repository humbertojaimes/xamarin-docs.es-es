---
title: ApiDefinitions y StructsAndEnums archivos
description: Este documento describe los archivos ApiDefinitions.cs y StructsAndEnums.cs que genera Sharpie objetivo. Estos archivos, a continuación, se usan para acceder al código de Objective-C desde C#.
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: df8d4508db14116a5b36e893f161ac891d58dc46
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855187"
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions y StructsAndEnums archivos

Cuando el objetivo Sharpie se ha ejecutado correctamente, genera `Binding/ApiDefinitions.cs` y `Binding/StructsAndEnums.cs` archivos.
Estos dos archivos se agregan a un proyecto de enlace en Visual Studio para Mac o pasa directamente a la `btouch` o `bmac` herramientas para producir el enlace final.

En *algunos* casos estos archivos generados pueden ser todo lo que necesita, sin embargo, con más frecuencia el desarrollador tendrá que modificar manualmente estos archivos para corregir cualquier problema que no se puede controlar automáticamente la herramienta (por ejemplo, los marcados generan con un [ `Verify` atributo](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

Algunos de los siguientes pasos son:

- **Ajuste los nombres**: a veces desea ajustar los nombres de métodos y clases para que coincida con las instrucciones de diseño de .NET Framework.
- **Métodos o propiedades**: la heurística utilizada por objetivo Sharpie a veces, seleccionará un método para convertirse en una propiedad. En este momento, podría decidir si este es el comportamiento previsto o no.
- **Enlazar eventos**: puede vincular sus clases con las clases de delegado y generar automáticamente los eventos para aquellos.
- **Enlazar notificaciones**: no es posible extraer el contrato de API de notificaciones de los archivos de encabezado puro, esto requerirá un viaje a la documentación de API. Si desea que las notificaciones fuertemente tipadas, deberá actualizar el resultado.
- **Protección de API**: en este punto, puede optar por proporcionar constructores adicionales, agregue los métodos (para permitir la sintaxis de inicialización en construcción en C#), la sobrecarga de operadores e implementar sus propias interfaces en el archivo de definiciones adicionales.

Consulte la [una API de enlace](~/cross-platform/macios/binding/objective-c-libraries.md) descripción para ver cómo encajan estos archivos en el proceso de enlace, tal como se muestra en el diagrama siguiente:

![](apidefinitions-structsandenums-images/binding-flowchart.png "El proceso de enlace se muestra en este diagrama")

Hacer referencia a la [referencias de los tipos de enlace](~/cross-platform/macios/binding/binding-types-reference.md) para obtener más información sobre el contenido de estos archivos.

## <a name="related-links"></a>Vínculos relacionados

- [Curso de Xamarin University: Creación de una biblioteca de enlaces de Objective-c.](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Curso de Xamarin University: Compilar una biblioteca de enlaces de Objective-C con Sharpie objetivo](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
