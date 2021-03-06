---
title: Reconocimiento de emociones con la Face API
description: Face API toma una expresión facial de una imagen como entrada y devuelve los datos que incluyen los niveles de confianza a través de un conjunto de emociones para cada cara de la imagen. En este artículo se explica cómo usar Face API para que reconozca las emociones para evaluar una aplicación de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: d703de90378991d262a4b056b9ebc98d183e3fb8
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "53056495"
---
# <a name="emotion-recognition-using-the-face-api"></a>Reconocimiento de emociones con la Face API

[![Descargar ejemplo](~/media/shared/download.png) descargar el ejemplo](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)

_Face API toma una expresión facial de una imagen como entrada y devuelve los datos que incluyen los niveles de confianza a través de un conjunto de emociones para cada cara de la imagen. En este artículo se explica cómo usar Face API para que reconozca las emociones para evaluar una aplicación de Xamarin.Forms._

## <a name="overview"></a>Información general

Face API puede realizar la detección de emociones para detectar la ira, desprecio, asco, miedo, felicidad, neutralidad, tristeza y sorpresa, en una expresión facial. Universalmente y culturas estas emociones se comunican a través de las mismas expresiones faciales básicas. Además de devolver un resultado de emociones para una expresión facial, Face API puede también devuelve un rectángulo de selección de caras detectadas. Tenga en cuenta que se debe obtener una clave de API para usar Face API. Esto puede obtenerse en [pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

Reconocimiento de emociones puede realizarse a través de una biblioteca de cliente y una API de REST. En este artículo se centra en realizar el reconocimiento de emociones a través de la API de REST. Para obtener más información acerca de la API de REST, consulte [Face API de REST](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

Face API puede usarse también para reconocer las expresiones faciales de personas en vídeo y puede devolver un resumen de sus emociones. Para obtener más información, consulte [cómo analizar vídeos en tiempo real](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/).

Para obtener más información acerca de Face API, consulte [Face API](/azure/cognitive-services/face/overview/).

## <a name="authentication"></a>Autenticación

Cada solicitud realizada a Face API necesita una clave de API que se debe especificar como el valor de la `Ocp-Apim-Subscription-Key` encabezado. En el ejemplo de código siguiente se muestra cómo agregar la clave de API para el `Ocp-Apim-Subscription-Key` encabezado de una solicitud:

```csharp
public FaceRecognitionService()
{
  _client = new HttpClient();
  _client.DefaultRequestHeaders.Add("ocp-apim-subscription-key", Constants.FaceApiKey);
}
```

Error al pasar una clave de API válida para Face API producirá un error en la 401 respuesta.

## <a name="performing-emotion-recognition"></a>Realizar el reconocimiento de emociones

Reconocimiento de emociones se realiza mediante una solicitud POST que contiene una imagen a la `detect` API en `https://[location].api.cognitive.microsoft.com/face/v1.0`, donde `[location]]` es la región utiliza para obtener la clave de API. Los parámetros de solicitud opcionales son:

- `returnFaceId` – Si se deben devolver faceIds de las caras detectadas. El valor predeterminado es `true`.
- `returnFaceLandmarks` – Si se deben devolver los puntos de referencia de las caras detectadas. El valor predeterminado es `false`.
- `returnFaceAttributes` : si desea analizar y volver a especifican uno o más atributos faciales. Los atributos de cara admitidos incluyen `age`, `gender`, `headPose`, `smile`, `facialHair`, `glasses`, `emotion`, `hair`, `makeup`, `occlusion`, `accessories`, `blur`, `exposure`, y `noise`. Tenga en cuenta que el análisis de atributos de cara tiene costo adicional de cálculo y el tiempo.

Contenido de la imagen se debe colocar en el cuerpo de la solicitud POST como una dirección URL o datos binarios.

> [!NOTE]
> Formatos de archivo de imagen admitidos son JPEG, PNG, GIF y BMP, y el tamaño de archivo permitido es de 1KB a 4MB.

En la aplicación de ejemplo, el proceso de reconocimiento de emociones se invoca mediante una llamada a la `DetectAsync` método:

```csharp
Face[] faces = await _faceRecognitionService.DetectAsync(photoStream, true, false, new FaceAttributeType[] { FaceAttributeType.Emotion });
```

Esta llamada al método especifica la secuencia que contiene los datos de imagen, que se deben devolver faceIds, que no deben devolverse faciales, y que se debe analizar la emoción de la imagen. También especifica que los resultados se devolverán como una matriz de `Face` objetos. A su vez, el `DetectAsync` método invoca el `detect` API de REST que realiza el reconocimiento de emociones:

```csharp
public async Task<Face[]> DetectAsync(Stream imageStream, bool returnFaceId, bool returnFaceLandmarks, IEnumerable<FaceAttributeType> returnFaceAttributes)
{
  var requestUrl =
    $"{Constants.FaceEndpoint}/detect?returnFaceId={returnFaceId}" +
    "&returnFaceLandmarks={returnFaceLandmarks}" +
    "&returnFaceAttributes={GetAttributeString(returnFaceAttributes)}";
  return await SendRequestAsync<Stream, Face[]>(HttpMethod.Post, requestUrl, imageStream);
}
```

Este método genera un URI de solicitud y, a continuación, envía la solicitud para el `detect` API a través de la `SendRequestAsync` método.

> [!NOTE]
> Debe usar la misma región en las llamadas de Face API que utilizó para obtener las claves de suscripción. Por ejemplo, si ha obtenido las claves de suscripción desde el `westus` región, que será el punto de conexión de detección de caras `https://westus.api.cognitive.microsoft.com/face/v1.0/detect`.

### <a name="sending-the-request"></a>Enviar la solicitud

El `SendRequestAsync` método realiza la solicitud POST a Face API y devuelve el resultado como un `Face` matriz:

```csharp
async Task<TResponse> SendRequestAsync<TRequest, TResponse>(HttpMethod httpMethod, string requestUrl, TRequest requestBody)
{
  var request = new HttpRequestMessage(httpMethod, Constants.FaceEndpoint);
  request.RequestUri = new Uri(requestUrl);
  if (requestBody != null)
  {
    if (requestBody is Stream)
    {
      request.Content = new StreamContent(requestBody as Stream);
      request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
    }
    else
    {
      // If the image is supplied via a URL
      request.Content = new StringContent(JsonConvert.SerializeObject(requestBody, s_settings), Encoding.UTF8, "application/json");
    }
  }

  HttpResponseMessage responseMessage = await _client.SendAsync(request);
  if (responseMessage.IsSuccessStatusCode)
  {
    string responseContent = null;
    if (responseMessage.Content != null)
    {
      responseContent = await responseMessage.Content.ReadAsStringAsync();
    }
    if (!string.IsNullOrWhiteSpace(responseContent))
    {
      return JsonConvert.DeserializeObject<TResponse>(responseContent, s_settings);
    }
    return default(TResponse);
  }
  else
  {
    ...
  }
  return default(TResponse);
}
```

Si la imagen se proporciona a través de una secuencia, el método basa la solicitud POST ajustando el flujo de imagen en un `StreamContent` instancia, que proporciona contenido HTTP basado en una secuencia. Como alternativa, si la imagen se proporciona a través de una dirección URL, el método crea la solicitud POST ajustando la dirección URL en un `StringContent` instancia, que proporciona contenido HTTP basado en una cadena.

A continuación, se envía la solicitud POST a `detect` API. La respuesta es leer, se deserializa y se devuelve al método de llamada.

El `detect` API enviará el código de estado HTTP 200 (OK) en la respuesta, siempre que la solicitud es válida, lo que indica que la solicitud es correcta y que la información solicitada está en la respuesta. Para obtener una lista de posibles respuestas de error, consulte [Face API de REST](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

### <a name="processing-the-response"></a>Procesamiento de la respuesta

Se devuelve la respuesta de API en formato JSON. Los siguientes datos JSON muestran un mensaje de respuesta correcta típico que proporciona los datos solicitados por la aplicación de ejemplo:

```json
[  
   {  
      "faceId":"8a1a80fe-1027-48cf-a7f0-e61c0f005051",
      "faceRectangle":{  
         "top":192,
         "left":164,
         "width":339,
         "height":339
      },
      "faceAttributes":{  
         "emotion":{  
            "anger":0.0,
            "contempt":0.0,
            "disgust":0.0,
            "fear":0.0,
            "happiness":1.0,
            "neutral":0.0,
            "sadness":0.0,
            "surprise":0.0
         }
      }
   }
]
```

Un mensaje de respuesta correcta se compone de una matriz de entradas de cara ordenados según el tamaño del rectángulo de cara en orden descendente, mientras que una respuesta vacía no indica ningún caras detectadas. Cada uno de ellos reconoce cara incluye una serie de atributos de cara opcional, que se especifican mediante el `returnFaceAttributes` argumento para el `DetectAsync` método.

En la aplicación de ejemplo, la respuesta JSON se deserializa en una matriz de `Face` objetos. Al interpretar los resultados de Face API, las emociones detectadas se deben interpretar como la emoción con la puntuación más alta, como las puntuaciones se normalizan para que sumen uno. Por lo tanto, la aplicación de ejemplo muestra la emoción reconocida con la puntuación más alta para la cara detectada más grande en la imagen. Esto se consigue con el código siguiente:

```csharp
emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
```

Captura de pantalla siguiente muestra el resultado del proceso de reconocimiento de emociones en la aplicación de ejemplo:

![](emotion-recognition-images/emotion-recognition.png "Reconocimiento de emociones")

## <a name="summary"></a>Resumen

En este artículo se explica cómo usar Face API para que reconozca las emociones para evaluar una aplicación de Xamarin.Forms. Face API toma una expresión facial de una imagen como entrada y devuelve los datos que incluyen la confianza entre un conjunto de emociones para cada cara de la imagen.

## <a name="related-links"></a>Vínculos relacionados

- [Face API](/azure/cognitive-services/face/overview/).
- [Todo Cognitive Services (ejemplo)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Face API de REST](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
