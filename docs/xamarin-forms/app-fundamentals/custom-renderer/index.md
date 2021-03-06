---
title: Representadores personalizados de Xamarin.Forms
description: Los representadores personalizados permiten que los desarrolladores reemplacen la representación de los controles nativos en cada plataforma, para personalizar la apariencia y el comportamiento de los controles de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 3a59cbb8385fe201d7eef086092c9b393a21cc71
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "57666386"
---
# <a name="xamarinforms-custom-renderers"></a>Representadores personalizados de Xamarin.Forms

_Las interfaces de usuario de Xamarin.Forms se representan mediante los controles nativos de la plataforma de destino, lo que permite que las aplicaciones de Xamarin.Forms conserven la apariencia adecuada para cada plataforma. Los representadores personalizados permiten que los desarrolladores reemplacen este proceso para personalizar la apariencia y el comportamiento de los controles de Xamarin.Forms en cada plataforma._

## <a name="introduction-to-custom-renderersintroductionmd"></a>[Introducción a los representadores personalizados](introduction.md)

Los representadores personalizados proporcionan un método eficaz para personalizar la apariencia y el comportamiento de los controles de Xamarin.Forms. Se pueden usar para pequeños cambios de estilo o para una personalización sofisticada del diseño y el comportamiento específicos de una plataforma. En este artículo se proporciona una introducción a los representadores personalizados y se describe el proceso para crear un representador personalizado.

## <a name="renderer-base-classes-and-native-controlsrenderersmd"></a>[Clases base y controles nativos del representador](renderers.md)

Todos los controles de Xamarin.Forms tienen un representador que lo acompaña para cada plataforma y que crea una instancia de un control nativo. En este artículo se enumeran las clases de representador y control nativo que implementan cada página, diseño, vista y celda de Xamarin.Forms.

## <a name="customizing-an-entryentrymd"></a>[Personalización de una entrada](entry.md)

El control [`Entry`](xref:Xamarin.Forms.Entry) de Xamarin.Forms permite que se edite una sola línea de texto. En este artículo se muestra cómo crear un representador personalizado para el control `Entry`, lo que permite que los desarrolladores reemplacen la representación nativa de forma predeterminada con su propia personalización específica de la plataforma.

## <a name="customizing-a-contentpagecontentpagemd"></a>[Personalización de una página de contenido](contentpage.md)

Un [`ContentPage`](xref:Xamarin.Forms.ContentPage) es un elemento visual que muestra una vista única y ocupa la mayor parte de la pantalla. En este artículo se muestra cómo crear un representador personalizado para la página `ContentPage`, lo que permite que los desarrolladores reemplacen la representación nativa de forma predeterminada con su propia personalización específica de la plataforma.

## <a name="customizing-a-mapmapindexmd"></a>[Personalización de un mapa](map/index.md)

Xamarin.Forms.Maps proporciona una abstracción multiplataforma para mostrar mapas que usan la API de mapa nativo en cada plataforma y proporcionar una experiencia de mapa rápida y familiar para los usuarios. En este tema se muestra cómo crear representadores personalizados para el control `Map`, lo que permite que los desarrolladores reemplacen la representación nativa de forma predeterminada por una personalización propia específica de la plataforma.

## <a name="customizing-a-listviewlistviewmd"></a>[Personalización de la clase ListView](listview.md)

[`ListView`](xref:Xamarin.Forms.ListView) de Xamarin.Forms es una vista que muestra una colección de datos como una lista vertical. En este artículo se muestra cómo crear un representador personalizado que encapsula los controles de lista específica de la plataforma y los diseños de celda nativa, lo que permite tener más control sobre el rendimiento del control de lista nativa.

## <a name="customizing-a-viewcellviewcellmd"></a>[Personalización de la clase ViewCell](viewcell.md)

[`ViewCell`](xref:Xamarin.Forms.ViewCell) de Xamarin.Forms es una celda que se puede agregar a [`ListView`](xref:Xamarin.Forms.ListView) o [`TableView`](xref:Xamarin.Forms.TableView), y que contiene una vista definida por el desarrollador. En este artículo se muestra cómo crear un representador personalizado para un `ViewCell` que se hospeda dentro de un control `ListView` de Xamarin.Forms. Esto impide que se llame varias veces a los cálculos de diseño de Xamarin.Forms durante el desplazamiento de `ListView`.

## <a name="implementing-a-viewviewmd"></a>[Implementación de una vista](view.md)

Los controles de interfaces de usuario personalizadas de Xamarin.Forms deben derivar de la clase [`View`](xref:Xamarin.Forms.View), que se usa para colocar los diseños y los controles en la pantalla. En este artículo se muestra cómo crear un representador personalizado para un control personalizado de Xamarin.Forms que se usa para mostrar una secuencia de vídeo de vista previa de la cámara del dispositivo.

## <a name="implementing-a-hybridwebviewhybridwebviewmd"></a>[Implementación de una clase HybridWebView](hybridwebview.md)

En este artículo se muestra cómo crear un representador personalizado para un control personalizado `HybridWebView`, que muestra cómo mejorar los controles específicos de la plataforma web para permitir que el código C# se invoque desde JavaScript.

## <a name="implementing-a-video-playervideo-playerindexmd"></a>[Implementación de un reproductor de vídeo](video-player/index.md)

En este artículo se muestra cómo escribir los representadores para implementar un control personalizado `VideoPlayer` que puede reproducir vídeos de la web, vídeos insertados como recursos de la aplicación o vídeos almacenados en la biblioteca de vídeos en el dispositivo del usuario. Se muestran varias técnicas, incluida la implementación de métodos y propiedades enlazables de solo lectura.


## <a name="related-links"></a>Vínculos relacionados

- [Efectos](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Representadores personalizados (vídeo de Xamarin University)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
