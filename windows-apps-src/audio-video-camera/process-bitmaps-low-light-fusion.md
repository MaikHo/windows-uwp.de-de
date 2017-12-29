---
author: laurenhughes
description: "In diesem Artikel wird erläutert, wie Sie die LowLightFusion-Klasse zum Verarbeiten von Bitmaps nutzen."
title: Verarbeitung von Bitmaps mit der Low Light Fusion-API
ms.author: lahugh
ms.date: 11/02/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, Uwp, low light Fusion, Bitmaps, bildbearbeitung
localizationpriority: medium
ms.openlocfilehash: 989571063603a7133f39961b4b32bdc60fc373dc
ms.sourcegitcommit: d0c93d734639bd31f264424ae5b6fead903a951d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2017
---
# <a name="process-bitmaps-with-the-lowlightfusion-api"></a>Verarbeitung von Bitmaps mit der LowLightFusion-API

Bilder bei schwachem Licht lassen sich nur schwer mit guter Bildqualität aufnehmen, vor allem auf mobilen Geräten mit fester Blende und Sensorgröße. Zur Kompensation von schlechten Lichtverhältnissen können Geräte die Belichtungszeit oder Sensorverstärkung erhöhen, was zu Bewegungsunschärfe und erhöhtem Bildrauschen führen kann. 

Die [LowLightFusion](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion)-Klasse verbessert die Qualität von Bildern bei schwachem Licht durch Abtastung von Pixelinformationen aus mehreren Bildern in unmittelbarer zeitlicher Nähe, d. h. kurze Burst-Bilder, um Rauschen und Bewegungsunschärfe zu reduzieren. Dies ist z. B. nützlich, um eine Fotobearbeitungsanwendung hinzuzufügen.

Diese Funktion wird auch durch die [AdvancedPhotoCapture](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)-Klasse zur Verfügung gestellt, die den Low Light Fusion-Algorithmus bei Bedarf auf eine Reihe von Bildern direkt nach der Aufnahme anwendet. Weitere Informationen zur Implementierung dieser Funktion finden Sie unter [Fotoaufnahme bei schlechten Lichtverhältnissen](https://docs.microsoft.com/windows/uwp/audio-video-camera/high-dynamic-range-hdr-photo-capture#low-light-photo-capture).

## <a name="prepare-the-images-for-processing"></a>Vorbereiten von Bildern für die Bearbeitung

In diesem Beispiel zeigen wir Ihnen, wie Sie die [LowLightFusion](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion)-Klasse und den [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) verwenden können, damit ein Benutzer mehrere Bilder auswählen kann, um Low Light Fusion-durchzuführen.

Zuerst müssen wir herausfinden, wie viele Bilder (auch als Frames bekannt) der Algorithmus akzeptiert, und eine Liste erstellen, um diese Frames zu halten.

[!code-cs[SnippetGetMaxLLFFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetGetMaxLLFFrames)]

Sobald wir festgelegt haben, wie viele Frames der Low Light Fusion-Algorithmus akzeptiert, können wir [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) verwenden, um den Benutzer auswählen zu lassen, welche Bilder im Algorithmus verwendet werden sollen.

[!code-cs[SnippetGetFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetGetFrames)]

Nachdem wir nun die richtige Anzahl von Frames ausgewählt haben, müssen wir die Frames in [SoftwareBitmaps](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) dekodieren und sicherstellen, dass die SoftwareBitmaps im richtigen Format für LowLightFusion vorliegen.

[!code-cs[SnippetDecodeFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetDecodeFrames)]


## <a name="fuse-the-bitmaps-into-a-single-bitmap"></a>Verarbeiten der Bitmaps zu einer einzigen Bitmap

Da wir nun eine korrekte Anzahl von Frames in einem akzeptablen Format haben, können wir die **[FuseAsync](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion#Windows_Media_Core_LowLightFusion_FuseAsync_Windows_Foundation_Collections_IIterable_Windows_Graphics_Imaging_SoftwareBitmap__)**-Methode verwenden, um den Low Light Fusion-Algorithmus anzuwenden. Unser Ergebnis wird das bearbeitete Bild mit verbesserter Klarheit in Form einer SoftwareBitmap sein. 

[!code-cs[SnippetFuseFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetFuseFrames)]

Schließlich bereinigen wir die resultierende SoftwareBitmap, indem wir sie kodieren und in ein benutzerfreundliches, „normales“ Bild speichern, ähnlich den Eingangsbildern, mit denen wir begonnen haben.

[!code-cs[SnippetEncodeFrame](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetEncodeFrame)]


## <a name="before-and-after"></a>Vorher und nachher

Hier ist ein Beispiel für ein Eingangsbild und das resultierende Ausgangsbild nach Anwendung des Low Light Fusion Algorithmus.

> [!div class="mx-tableFixed"] 
| Frame 1 | Frame 2 | Low Light Fusion-Ausgabe | 
|---------|---------|-------------------------|
| ![Erstes Eingabeframe für den Low Light Fusion Algorithmus](./images/LLF-Input1.png) | ![Zweites Eingabeframe für den Low Light Fusion Algorithmus](./images/LLF-Input2.png) | ![Ergebnisframe des Low Light Fusion Algorithmus](./images/LLF-Output.png) |

Anhand des Eingangsframes und des Ausgangsframes sehen Sie, dass die Beleuchtung und die Schatten verbessert wurden.

## <a name="related-topics"></a>Verwandte Themen 
[LowLightFusion-Klasse](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion)  
[LowLightFusionResult-Klasse](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusionresult)