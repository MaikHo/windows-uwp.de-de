---
ms.assetid: cf0d2709-21a1-4d56-9341-d4897e405f5d
title: Exemplarische Vorgehensweise zur Fehlerbehandlung in XAML/C#
description: In dieser exemplarischen Vorgehensweise erfahren Sie, wie Sie Fehler von einem adcontrol-Element in einer XAML/c#-App abfangen und behandeln.
ms.date: 02/18/2020
ms.topic: article
keywords: 'Windows 10, UWP, Werbung, Werbung, Fehlerbehandlung, XAML, c #'
ms.localizationpriority: medium
ms.openlocfilehash: 7ad3d7dfcbf191960fba03808270a55d3128d4f0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158584"
---
# <a name="error-handling-in-xamlc-walkthrough"></a>Exemplarische Vorgehensweise zur Fehlerbehandlung in XAML/C#

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie AD-bezogene Fehler in Ihrer APP erfassen. In dieser exemplarischen Vorgehensweise wird ein [adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) -Element verwendet, um eine Banner-Werbeeinblendungen anzuzeigen, aber die allgemeinen Konzepte in dieser exemplarischen Vorgehensweise gelten auch für austauschbare

In diesen Beispielen wird davon ausgegangen, dass Sie eine XAML/C#-App haben, die ein **AdControl** enthält. Schritt-für-Schritt-Anleitungen, die zeigen, wie ein **AdControl** zu Ihrer App hinzugefügt wird, finden Sie unter [AdControl in XAML und .NET](adcontrol-in-xaml-and--net.md). 

1.  Suchen Sie in der Datei "MainPage.xaml" nach der Definition für das **AdControl**. Dieser Code sieht folgendermaßen aus.
    ``` xml
    <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="test"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300" />
    ```

2.   Weisen Sie nach der Eigenschaft **Width**, jedoch vor dem Endtag, dem Ereignis [ErrorOccurred](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.erroroccurred) den Namen eines Fehlerereignishandlers zu. In dieser exemplarischen Vorgehensweise ist der Name des Fehlerereignishandlers **OnAdError**.
    ``` xml
    <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="test"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300"
      ErrorOccurred="OnAdError"/>
    ```

3.  Um einen Fehler zur Laufzeit zu generieren, erstellen Sie ein zweites **AdControl**-Element mit einer anderen Anwendungs-ID. Da alle **AdControl**-Objekte in einer Anwendung die gleiche Anwendungs-ID verwenden müssen, wird durch das Erstellen eines zusätzlichen **AdControl** mit einer anderen Anwendungs-ID ein Fehler ausgelöst.

    Definieren Sie ein zweites **AdControl** in der Datei "MainPage.xaml" direkt nach dem ersten **AdControl**, und legen Sie für die Eigenschaft [ApplicationId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) den Wert "0" fest.
    ``` xml
    <UI:AdControl
        ApplicationId="0"
        AdUnitId="test"
        HorizontalAlignment="Left"
        Height="250"
        Margin="10,265,0,0"
        VerticalAlignment="Top"
        Width="300"
        ErrorOccurred="OnAdError" />
    ```

4.  Fügen Sie in der Datei "MainPage.xaml.cs" den folgenden Ereignishandler **OnAdError** der **MainPage**-Klasse hinzu. Dieser Ereignishandler schreibt Informationen in das Visual Studio **Ausgabe**-Fenster.
    ``` csharp
    private void OnAdError(object sender, AdErrorEventArgs e)
    {
        System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name +
            "): " + e.ErrorMessage + " ErrorCode: " + e.ErrorCode.ToString());
    }
    ```

4.  Erstellen Sie das Projekt, und führen Sie es aus. Nach Ausführen der Anwendung wird eine Meldung ähnlich der Meldung unten im **Ausgabe**-Fenster von Visual Studio angezeigt.
    ```json
    AdControl error (): MicrosoftAdvertising.Shared.AdException: all ad requests must use the same application ID within a single application (0, d25517cb-12d4-4699-8bdc-52040c712cab) ErrorCode: ClientConfiguration
    ```

## <a name="related-topics"></a>Zugehörige Themen

* [Anzeigenbeispiele bei GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)