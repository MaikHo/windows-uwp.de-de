---
ms.assetid: af3941c0-3508-4ba2-a79e-fc71657c605f
description: In diesem Artikel wird beschrieben, wie Sie beim Aufnehmen von Fotos und Videos mit einer Hilfsklasse die Geräteausrichtung handhaben.
title: Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 18e135e85902bb3e00b7a09b8ee98a4e02b71f15
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362643"
---
# <a name="handle-device-orientation-with-mediacapture"></a>Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“
Wenn Ihre App ein Foto oder Video aufnimmt, das außerhalb der App angezeigt werden soll, z. B. in eine Datei auf dem Gerät des Benutzers gespeichert oder online freigegeben werden soll, ist es wichtig, dass das Bild mit den richtigen Ausrichtungsmetadaten codiert wird. Dadurch wird bei der Anzeige durch eine andere App oder ein anderes Gerät das Bild korrekt ausgerichtet. Die Bestimmung der korrekten Ausrichtungsdaten für eine Mediendatei kann eine komplexe Aufgabe sein, da verschiedene Variablen berücksichtigt werden müssen. Dazu zählen die Ausrichtung des Geräte-Chassis, die Ausrichtung der Anzeige und die Platzierung der Kamera (nach vorne oder nach hinten gerichtete Kamera). 

Um die Handhabung der Ausrichtung zu vereinfachen, empfehlen wir die Verwendung der Hilfsklasse **CameraRotationHelper**, deren vollständige Definition am Ende dieses Artikels bereitgestellt wird. Sie können diese Klasse dem Projekt hinzufügen und dann die Schritte zum Hinzufügen von Ausrichtungsunterstützung zur Kamera-App in diesem Artikel ausführen. Die Hilfsklasse erleichtert auch das Drehen der Steuerelemente in der Kamera-UI, damit sie aus Sicht des Benutzers korrekt gerendert werden.

> [!NOTE] 
> Dieser Artikel baut auf Konzepten und Code auf, die im Artikel [**Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“**](basic-photo-video-and-audio-capture-with-mediacapture.md) erläutert werden. Es wird empfohlen, dass Sie sich mit den grundlegenden Konzepten der Verwendung der **MediaCapture**-Klasse vertraut machen, bevor Sie die Ausrichtungsunterstützung zur App hinzufügen.

## <a name="namespaces-used-in-this-article"></a>In diesem Artikel verwendete Namespaces
Der Beispielcode in diesem Artikel verwendet APIs aus den folgenden Namespaces, die Sie in den Code einschließen sollten. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetOrientationUsing":::

Der erste Schritt beim Hinzufügen der Ausrichtungsunterstützung zur App ist das Sperren der Anzeige, sodass diese sich beim Drehen des Geräts nicht automatisch dreht. Die automatische UI-Drehung eignet sich gut für die meisten Arten von Apps. Es ist jedoch für Benutzer nicht intuitiv, wenn sich die Kameravorschau dreht. Sperren Sie die Bildschirmausrichtung, indem Sie die [**DisplayInformation.AutoRotationPreferences**](/uwp/api/windows.graphics.display.displayinformation.autorotationpreferences)-Eigenschaft auf [**DisplayOrientations.Landscape**](/uwp/api/Windows.Graphics.Display.DisplayOrientations) festlegen. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetAutoRotationPreference":::

## <a name="tracking-the-camera-device-location"></a>Nachverfolgen der Position des Kamerageräts
Um die richtige Ausrichtung für aufgenommene Medien zu berechnen, muss die App die Position des Kamerageräts am Chassis ermitteln. Fügen Sie eine boolesche Membervariable hinzu, um nachzuverfolgen, ob es sich um eine externe Kamera handelt, z. B. eine USB-Webcam. Fügen Sie eine weitere boolesche Variable hinzu, um nachzuverfolgen, ob die Vorschau gespiegelt werden soll. Dies ist bei der Verwendung einer nach vorne gerichteten Kamera der Fall. Fügen Sie außerdem eine Variable zum Speichern eines **DeviceInformation**-Objekts hinzu, das die ausgewählte Kamera darstellt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetCameraDeviceLocationBools":::
:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareCameraDevice":::

## <a name="select-a-camera-device-and-initialize-the-mediacapture-object"></a>Auswählen eines Kamerageräts und Initialisieren des MediaCapture-Objekts
Im Artikel [**Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“**](basic-photo-video-and-audio-capture-with-mediacapture.md) wird beschrieben, wie Sie das **MediaCapture**-Objekt mit nur wenigen Codezeilen initialisieren. Um die Ausrichtung der Kamera zu unterstützen, fügen wir dem Initialisierungsprozess weitere Schritte hinzu.

Rufen Sie zuerst [**DeviceInformation.FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) auf, und übergeben Sie die Geräteauswahl [**DeviceClass.VideoCapture**](/uwp/api/Windows.Devices.Enumeration.DeviceClass), um eine Liste aller verfügbaren Videoaufzeichnungsgeräte abzurufen. Wählen Sie dann das erste Gerät in der Liste aus, für das die Bereichsposition der Kamera bekannt ist und das mit dem angegebenen Wert übereinstimmt, in diesem Beispiel eine nach vorne gerichtete Kamera. Wird keine Kamera im gewünschten Bereich gefunden, wird die erste oder standardmäßig verfügbare Kamera verwendet.

Wenn ein Kameragerät gefunden wird, wird ein neues [**MediaCaptureInitializationSettings**](/uwp/api/Windows.Media.Capture.MediaCaptureInitializationSettings)-Objekt erstellt, und die [**VideoDeviceId**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.videodeviceid)-Eigenschaft wird auf das ausgewählte Gerät festgelegt. Erstellen Sie als Nächstes das MediaCapture-Objekt, rufen Sie [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) auf, und übergeben Sie das Einstellungsobjekt, um dem System mitzuteilen, dass die ausgewählte Kamera verwendet werden soll.

Überprüfen Sie abschließend, ob der ausgewählte Gerätebereich null oder unbekannt ist. In diesem Fall ist die Kamera extern. Dies bedeutet, dass ihre Drehung ohne Bezug auf die Drehung des Geräts ist. Wenn der Bereich bekannt ist und sich an der Vorderseite des Geräte-Chassis befindet, wissen wir, dass die Vorschau gespiegelt werden soll. Die Variable zur Nachverfolgung ist also festgelegt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetInitMediaCaptureWithOrientation":::
## <a name="initialize-the-camerarotationhelper-class"></a>Initialisieren der CameraRotationHelper-Klasse

Wir beginnen nun mit der Verwendung der **CameraRotationHelper**-Klasse. Deklarieren Sie eine Klassenmembervariable zum Speichern des Objekts. Rufen Sie den Konstruktor auf, und übergeben Sie die Gehäuseposition der ausgewählten Kamera. Die Hilfsklasse verwendet diese Informationen, um die richtige Ausrichtung für aufgenommene Medien, den Vorschaustream und die Benutzeroberfläche zu berechnen. Registrieren Sie einen Handler für das **OrientationChanged**-Ereignis der Hilfsklasse, das ausgelöst wird, wenn die Ausrichtung der Benutzeroberfläche oder des Vorschaustreams aktualisiert werden muss.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareRotationHelper":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetInitRotationHelper":::

## <a name="add-orientation-data-to-the-camera-preview-stream"></a>Hinzufügen von Ausrichtungsdaten zum Kameravorschau-Stream
Das Hinzufügen der richtigen Ausrichtung zu den Metadaten des Vorschaustreams hat keinen Einfluss auf die Anzeige der Vorschau für Benutzer, erleichtert jedoch die richtige Codierung von aus dem Vorschaustream erfassten Frames durch das System.

Starten Sie die Kameravorschau durch Aufrufen von [**MediaCapture.StartPreviewAsync**](/uwp/api/windows.media.capture.mediacapture.startpreviewasync). Überprüfen Sie zuvor die Membervariable, um festzustellen, ob die Vorschau gespiegelt werden soll (für eine nach vorne gerichtete Kamera). Wenn dies der Fall ist, legen Sie die [**FlowDirection**](/uwp/api/windows.ui.xaml.frameworkelement.flowdirection)-Eigenschaft von [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement), das in diesem Beispiel den Namen *PreviewControl* hat, auf [**FlowDirection.RightToLeft**](/uwp/api/Windows.UI.Xaml.FlowDirection) fest. Rufen Sie nach dem Starten der Vorschau die Hilfsmethode **SetPreviewRotationAsync** auf, um die Drehung der Vorschau festzulegen. Es folgt die Implementierung dieser Methode.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetStartPreviewWithRotationAsync":::

Wir legen die Vorschaudrehung in einer separaten Methode fest, sodass sie bei Änderung der Telefonausrichtung aktualisiert werden kann, ohne den Vorschaustream zu initialisieren. Wenn es sich um eine externe Kamera handelt, wird keine Aktion ausgeführt. Andernfalls wird die **CameraRotationHelper**-Methode **GetCameraPreviewOrientation** aufgerufen und gibt die richtige Ausrichtung für den Vorschaustream zurück. 

Zum Festlegen der Metadaten werden die Vorschaustreameigenschaften durch Aufrufen von [**VideoDeviceController.GetMediaStreamProperties**](/uwp/api/windows.media.devices.videodevicecontroller.getmediastreamproperties) abgerufen. Erstellen Sie als Nächstes die GUID, die das Media Foundation Transform (MFT)-Attribut für die Drehung des Videostreams darstellt. In C++ können Sie die Konstante [**MF_MT_VIDEO_ROTATION**](/windows/desktop/medfound/mf-mt-video-rotation) verwenden, während Sie in C# den GUID-Wert manuell angeben müssen. 

Fügen Sie dem Datenstromeigenschaftenobjekt einen Eigenschaftswert hinzu, und geben Sie die GUID als Schlüssel und die Vorschaudrehung als Wert an. Diese Eigenschaft erwartet Werte in Einheiten von Grad entgegen dem Uhrzeigersinn, sodass mithilfe der **CameraRotationHelper**-Methode **ConvertSimpleOrientationToClockwiseDegrees** der einfache Ausrichtungswert konvertiert wird. Rufen Sie abschließend [**SetEncodingPropertiesAsync**](/uwp/api/Windows.Media.Capture.MediaCapture#Windows_Media_Capture_MediaCapture_SetEncodingPropertiesAsync_Windows_Media_Capture_MediaStreamType_Windows_Media_MediaProperties_IMediaEncodingProperties_Windows_Media_MediaProperties_MediaPropertySet_) auf, um die neue Drehungseigenschaft auf den Stream anzuwenden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetSetPreviewRotationAsync":::

Fügen Sie als Nächstes den Handler für das **CameraRotationHelper.OrientationChanged**-Ereignis hinzu. Dieses Ereignis übergibt ein Argument, das Ihnen mitteilt, ob der Vorschaustream gedreht werden muss. Wenn die Ausrichtung des Geräts nach oben oder unten geändert wurde, ist dieser Wert „false“. Wenn die Vorschau gedreht werden muss, rufen Sie **SetPreviewRotationAsync** auf, die zuvor definiert wurde.

Aktualisieren Sie als Nächstes im **OrientationChanged**-Ereignishandler bei Bedarf die Benutzeroberfläche. Rufen Sie die aktuelle empfohlene UI-Ausrichtung durch Aufrufen von **GetUIOrientation** aus der Hilfsklasse ab, und konvertieren Sie den Wert in Grad im Uhrzeigersinn für XAML-Transformationen. Erstellen Sie eine [**RotateTransform**](/uwp/api/Windows.UI.Xaml.Media.RotateTransform) aus dem Ausrichtungswert, und legen Sie die [**RenderTransform**](/uwp/api/windows.ui.xaml.uielement.rendertransform)-Eigenschaft der XAML-Steuerelemente fest. Je nach dem UI-Layout müssen Sie möglicherweise zusätzliche Anpassungen zum einfachen Drehen der Steuerelemente vornehmen. Beachten Sie außerdem, dass alle Aktualisierungen der Benutzeroberfläche im UI-Thread vorgenommen werden müssen. Daher sollten Sie diesen Code innerhalb eines Aufrufs von [**RunAsync**](/uwp/api/Windows.UI.Core.CoreDispatcher#Windows_UI_Core_CoreDispatcher_RunAsync_Windows_UI_Core_CoreDispatcherPriority_Windows_UI_Core_DispatchedHandler_) platzieren.  

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetHelperOrientationChanged":::

## <a name="capture-a-photo-with-orientation-data"></a>Aufnehmen eines Fotos mit Ausrichtungsdaten
Im Artikel [**Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“**](basic-photo-video-and-audio-capture-with-mediacapture.md) wird erläutert, wie Sie ein Foto in einer Datei aufnehmen, indem Sie zuerst in einen In-Memory-Datenstrom aufnehmen und dann mithilfe eines Decoders die Bilddaten aus dem Datenstrom lesen und mithilfe eines Encoders die Bilddaten in eine Datei transcodieren. Aus der **CameraRotationHelper**-Klasse abgerufene Ausrichtungsdaten können während des Transcodierungsvorgangs der Bilddatei hinzugefügt werden.

Im folgenden Beispiel wird ein Foto in einem [**InMemoryRandomAccessStream**](/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream) mit einem Aufruf von [**CapturePhotoToStreamAsync**](/uwp/api/windows.media.capture.mediacapture.capturephototostreamasync) aufgenommen, und ein [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) wird aus dem Stream erstellt. Als Nächstes wird eine [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) erstellt und geöffnet, um einen [**IRandomAccessStream**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream) zum Schreiben in die Datei abzurufen. 

Vor dem Transcodieren der Datei wird die Ausrichtung des Fotos aus der Hilfsklassenmethode **GetCameraCaptureOrientation** abgerufen. Diese Methode gibt ein [**SimpleOrientation**](/uwp/api/Windows.Devices.Sensors.SimpleOrientation)-Objekt zurück, das mit der Hilfsmethode **ConvertSimpleOrientationToPhotoOrientation** in ein [**PhotoOrientation**](/uwp/api/Windows.Storage.FileProperties.PhotoOrientation)-Objekt konvertiert wird. Als Nächstes wird ein neues **BitmapPropertySet**-Objekt erstellt und eine Eigenschaft hinzugefügt, wobei der Schlüssel „System.Photo.Orientation“ und der Wert die Fotoausrichtung ist, ausgedrückt als **BitmapTypedValue**. „System.Photo.Orientation“ ist eine der vielen Windows-Eigenschaften, die als Metadaten einer Bilddatei hinzugefügt werden können. Eine Liste aller fotobezogenen Eigenschaften finden Sie unter [**Windows-Eigenschaften – Foto**](/previous-versions/ff516600(v=vs.85)). Weitere Informationen zum Arbeiten mit Metadaten in Bildern finden Sie unter [**Bildmetadaten**](image-metadata.md).

Schließlich wird der Eigenschaftensatz, der die Ausrichtungsdaten enthält, für den Encoder mit einem Aufruf von [**SetPropertiesAsync**](../develop/index.md) festgelegt, und das Bild wird mit einem Aufruf von [**FlushAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) transcodiert.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetCapturePhotoWithOrientation":::

## <a name="capture-a-video-with-orientation-data"></a>Aufnehmen eines Videos mit Ausrichtungsdaten
Die allgemeine Videoaufnahme wird im Artikel [**Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“**](basic-photo-video-and-audio-capture-with-mediacapture.md) beschrieben. Das Hinzufügen von Ausrichtungsdaten zur Codierung des aufgenommenen Videos erfolgt mit dem gleichen Verfahrens wie weiter oben im Abschnitt zum Hinzufügen von Ausrichtungsdaten zum Vorschaustream beschrieben.

Im folgenden Beispiel wird eine Datei erstellt, in die das aufgenommene Video geschrieben wird. Ein MP4-Codierungsprofil wird mithilfe der statischen [**CreateMp4**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4)-Methode erstellt. Die richtige Ausrichtung für das Video wird aus der **CameraRotationHelper**-Klasse mit einem Aufruf von **GetCameraCaptureOrientation** abgerufen. Da die Drehungseigenschaft erfordert, dass die Ausrichtung in Grad gegen den Uhrzeigersinn ausgedrückt wird, wird die **ConvertSimpleOrientationToClockwiseDegrees**-Hilfsmethode zum Konvertieren des Ausrichtungswerts aufgerufen. Erstellen Sie als Nächstes die GUID, die das Media Foundation Transform (MFT)-Attribut für die Drehung des Videostreams darstellt. In C++ können Sie die Konstante [**MF_MT_VIDEO_ROTATION**](/windows/desktop/medfound/mf-mt-video-rotation) verwenden, während Sie in C# den GUID-Wert manuell angeben müssen. Fügen Sie dem Datenstromeigenschaftenobjekt einen Eigenschaftswert hinzu, und geben Sie die GUID als Schlüssel und die Drehung als Wert an. Rufen Sie schließlich [**StartRecordToStorageFileAsync**](/uwp/api/Windows.Media.Capture.MediaCapture#Windows_Media_Capture_MediaCapture_StartRecordToStorageFileAsync_Windows_Media_MediaProperties_MediaEncodingProfile_Windows_Storage_IStorageFile_) auf, um die mit Ausrichtungsdaten codierte Videoaufnahme zu beginnen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetStartRecordingWithOrientationAsync":::

## <a name="camerarotationhelper-full-code-listing"></a>Vollständiger Code für CameraRotationHelper
Der folgende Codeausschnitt führt den vollständigen Code für die **CameraRotationHelper**-Klasse auf, mit der die Hardwareausrichtungssensoren verwaltet, die richtigen Ausrichtungswerte für Fotos und Videos berechnet und Hilfsmethoden zum Konvertieren zwischen den unterschiedlichen Darstellungsformen der Ausrichtung bereitgestellt werden, die von verschiedenen Windows-Features verwendet werden. Wenn Sie die im oben genannten Artikel gezeigten Anleitungen befolgen, können Sie diese Klasse dem Projekt in der vorliegenden Form hinzufügen, ohne Änderungen vornehmen zu müssen. Natürlich können Sie den folgenden Code an die Anforderungen des jeweiligen Szenarios anpassen.

Diese Hilfsklasse verwendet den [**SimpleOrientationSensor**](/uwp/api/Windows.Devices.Sensors.SimpleOrientationSensor) des Geräts zum Ermitteln der aktuellen Ausrichtung des Geräte-Chassis und die [**DisplayInformation**](/uwp/api/Windows.Graphics.Display.DisplayInformation)-Klasse zum Ermitteln der aktuellen Ausrichtung der Anzeige. Alle diese Klassen bieten Ereignisse, die ausgelöst werden, wenn sich die aktuelle Ausrichtung ändert. Die Seite, auf der sich das Aufnahmegerät befindet – nach vorne gerichtet, nach hinten gerichtet oder extern –, wird verwendet, um festzustellen, ob der Vorschaustream gespiegelt werden soll. Darüber hinaus wird mithilfe der von einigen Geräten unterstützten [**EnclosureLocation.RotationAngleInDegreesClockwise**](/uwp/api/windows.devices.enumeration.enclosurelocation.rotationangleindegreesclockwise)-Eigenschaft die Ausrichtung ermittelt, mit der die Kamera auf dem Chassis montiert wird.

Mit den folgenden Methoden können empfohlene Ausrichtungswerte für die angegebenen Kamera-App-Aufgaben abgerufen werden:

* **GetUIOrientation** – Gibt die vorgeschlagene Ausrichtung für Kamera-UI-Elemente zurück.
* **GetCameraCaptureOrientation** – Gibt die vorgeschlagene Ausrichtung für die Codierung in Bildmetadaten zurück.
* **GetCameraPreviewOrientation** – Gibt die vorgeschlagene Ausrichtung für den Vorschaustream zum Bereitstellen einer natürlicheren Benutzererfahrung zurück.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/CameraRotationHelper.cs" id="SnippetCameraRotationHelperFull":::



## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
