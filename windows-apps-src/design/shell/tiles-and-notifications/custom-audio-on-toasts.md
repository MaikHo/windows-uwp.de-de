---
description: Erfahren Sie, wie Sie benutzerdefiniertes Audio für ihre Popup Benachrichtigungen verwenden, damit Ihre APP die eindeutigen Soundeffekte Ihrer Marke Ausdrücken kann.
title: Benutzerdefiniertes Audiogerät
label: Custom audio on toasts
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, UWP, Toast, benutzerdefiniertes Audio, Benachrichtigung, Audio, Sound
ms.localizationpriority: medium
ms.openlocfilehash: 81bec439f17cadb7db0576dafcf4299f0978b192
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054460"
---
# <a name="custom-audio-on-toasts"></a>Benutzerdefiniertes Audiogerät

Popup Benachrichtigungen können benutzerdefinierte Audiodaten verwenden, mit denen Ihre APP die eindeutigen Soundeffekte Ihrer Marke Ausdrücken kann. Beispielsweise kann eine Messaging-App ihren eigenen Messaging Sound in ihren Popup Benachrichtigungen verwenden, sodass der Benutzer sofort weiß, dass er eine Benachrichtigung von der APP erhalten hat, anstatt den generischen Benachrichtigungs Sound zu hören.

## <a name="install-uwp-community-toolkit-nuget-package"></a>Nuget-Paket für das UWP Community Toolkit installieren

Um Benachrichtigungen per Code zu erstellen, wird dringend empfohlen, die UWP Community Toolkit-Benachrichtigungs Bibliothek zu verwenden, die ein Objektmodell für den Inhalt der Benachrichtigungs-XML bereitstellt. Sie könnten die Benachrichtigungs-XML manuell erstellen, aber das ist fehleranfällig und messy. Die Benachrichtigungs Bibliothek im UWP Community Toolkit wird von dem Team erstellt und verwaltet, das über Benachrichtigungen bei Microsoft verfügt.

Installieren Sie [Microsoft. Toolkit. UWP. Benachrichtigungen](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) von nuget (in dieser Dokumentation werden Version 1.0.0 verwendet).


## <a name="add-namespace-declarations"></a>Hinzufügen von Namespace-Deklarationen

`Windows.UI.Notifications` enthält die Kachel und die Toast-APIs. `Microsoft.Toolkit.Uwp.Notifications` schließt die Benachrichtigungs Bibliothek ein.

```csharp
using Microsoft.Toolkit.Uwp.Notifications;
using Windows.UI.Notifications;
```


## <a name="construct-the-notification"></a>Erstellen der Benachrichtigung

Der Inhalt der Popup Benachrichtigung umfasst Text und Bilder sowie Schaltflächen und Eingaben. Weitere Informationen finden Sie unter [Send local Toast](send-local-toast.md) , um einen vollständigen Code Ausschnitt anzuzeigen.

```csharp
ToastContent toastContent = new ToastContent()
{
    Visual = new ToastVisual()
    {
        ... (omitted)
    }
};
```


## <a name="add-the-custom-audio"></a>Hinzufügen der benutzerdefinierten Audiodatei

Windows Mobile hat benutzerdefinierte Audiodaten in Popup Benachrichtigungen immer unterstützt. Der Desktop hat jedoch nur Unterstützung für benutzerdefinierte Audiodaten in Version 1511 (Build 10586) hinzugefügt. Wenn Sie vor Version 1511 einen Toast, der benutzerdefinierte Audiodaten enthält, an ein Desktop Gerät senden, wird der Toast automatisch angezeigt. Daher sollten Sie für Desktop-Pre-Version 1511 die benutzerdefinierte Audiodatei nicht in ihre Popup Benachrichtigung einschließen, damit die Benachrichtigung zumindest das standardmäßige Benachrichtigungs Sound verwendet.

**Bekanntes Problem**: Wenn Sie die Desktop Version 1511 verwenden, funktioniert die benutzerdefinierte Popup-Audiodatei nur, wenn Ihre APP über den Store installiert wird. Dies bedeutet, dass Sie Ihre benutzerdefinierte Audiodatei nicht lokal auf dem Desktop testen können, bevor Sie Sie an den Speicher senden. das Audiogerät funktioniert jedoch nach der Installation aus dem Store problemlos. Wir haben dies im Anniversary Update korrigiert, sodass die benutzerdefinierte Audiodatei aus Ihrer lokal bereitgestellten App ordnungsgemäß funktioniert.

```csharp
?
bool supportsCustomAudio = true;
 
// If we're running on Desktop before Version 1511, do NOT include custom audio
// since it was not supported until Version 1511, and would result in a silent toast.
if (AnalyticsInfo.VersionInfo.DeviceFamily.Equals("Windows.Desktop")
    && !ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 2))
{
    supportsCustomAudio = false;
}
 
if (supportsCustomAudio)
{
    toastContent.Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/Audio/CustomToastAudio.m4a")
    };
}
```

Zu den unterstützten Audiodateitypen zählen...

- .aac
- . flac
- .m4a
- .mp3
- WAV
- .wma


## <a name="send-the-notification"></a>Senden der Benachrichtigung

Nachdem der Popup Inhalt nun fertiggestellt ist, ist das Senden der Benachrichtigung recht einfach.

```csharp
// Create the toast notification from the previous toast content
ToastNotification notification = new ToastNotification(toastContent.GetXml());
             
// And then send the toast
ToastNotificationManager.CreateToastNotifier().Show(notification);
```


## <a name="related-topics"></a>Verwandte Themen

- [Vollständiges Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-toast-with-custom-audio)
- [Lokalen Toast senden](send-local-toast.md)
- [Dokumentation zu Popup Inhalten](adaptive-interactive-toasts.md)