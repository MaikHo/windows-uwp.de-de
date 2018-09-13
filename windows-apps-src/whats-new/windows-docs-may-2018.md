---
author: QuinnRadich
title: Neuigkeiten in Windows-Dokumentation im Mai 2018 – Entwicklung von UWP-apps
description: Neue Features, Videos und entwicklerleitfäden haben die Windows 10-Entwicklerdokumentation für Mai 2018 und die Microsoft-Build-Konferenz hinzugefügt wurde.
keywords: Neuigkeiten, update, features, Anleitungen für Entwickler, Windows 10, Mai, Build
ms.author: quradic
ms.date: 5/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 322bc056411095019dfc027078cbfef7de0883fb
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "3963898"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>Neuigkeiten in der Windows-Entwicklerdokumentation im Mai 2018

Die Entwicklerdokumentation für die Windows-Plattform wird ständig mit Informationen über neue Features für Entwickler aktualisiert. Die folgenden Featureübersichten, entwicklerleitfäden, Videos und Beispiele wurden im Mai mit der [Microsoft Build 2018](https://www.microsoft.com/build) -Entwicklerkonferenz zur Verfügung gestellt.

Nach der [Installation der Tools und des SDKs](http://go.microsoft.com/fwlink/?LinkId=821431) unter Windows10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/create-uwp-apps.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

## <a name="features"></a>Features

### <a name="motion-in-fluent-design"></a>Bewegung in Fluent Design

Der Benutzer der Bewegung in der Fluent Design-Systems wird weiterentwickelt, die auf die Grundlagen der Timing, geschwindigkeitsverlauf, direktionalität und Schwerkraft erstellt. Anwenden von diesen Grundlagen werden Führung des Benutzers durch die app, und durch Reflektieren der natürlichen Welt mit seiner digitalen Erfahrung verbunden. Weitere Informationen finden Sie in diesen Artikeln:

* [Übersicht über die Bewegungen](../design/motion/index.md) wurde aktualisiert, um diese Grundlagen widerspiegeln.
* [In der Praxis Bewegung](../design/motion/motion-in-practice.md) enthält Beispiele für diese Grundlagen innerhalb Ihrer app anwenden.
* [Direktionalität und Schwerkraft](../design/motion/directionality-and-gravity.md) festigt das mentale Modell des Benutzers, der Ihre app.
* [Timing und geschwindigkeitsverlauf](../design/motion/timing-and-easing.md) wird die Bewegung in Ihrer app Realismus hinzugefügt.

![Bewegung in Aktion zu sehen](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Fluent Design-Updates

Visual Updates und geringfügigen Änderungen wurden die folgenden Fluent Design-Seiten vorgenommen:

* [Ausrichtung, Abstände, Ränder](../design/layout/alignment-margin-padding.md)
* [Farbe](../design/style/color.md)
* [Befehlsgrundlagen](../design/basics/commanding-basics.md)
* [Fluent Design für Windows-apps](../design/fluent-design-system/index.md)
* [Einführung in das app-design](../design/basics/design-and-ui-intro.md)
* [Navigationsgrundlagen](../design/basics/navigation-basics.md)
* [Reaktionsfähige Designtechniken](../design/layout/responsive-design.md)
* [Bildschirmgrößen und Breakpoints](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Übersicht über die Stil](../design/style/index.md)
* [Schreibstil](../design/style/writing-style.md)

Darüber hinaus haben wir die folgenden Seiten mit neuen Informationen zu ihren Inhaltsbereiche umgeschrieben:

* [Symbole](../design/style/icons.md) bietet jetzt praktische Empfehlungen für Symbole und wodurch sie geklickt werden kann.
* [Typografie](../design/style/typography.md) konsolidiert Informationen aus ähnlichen Artikeln Sie alles an einem Ort mit aktualisierten Anleitungen und Illustrationen einfügen.

![Bild mit Farbpalette](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>App-Installer-Dateien in Visual Studio

App-Installer-Dateien können mit Visual Studio 2017, Update 15.7 jetzt erstellt werden. [Erfahren Sie, wie Sie Visual Studio zum Erstellen einer App-Installer-Datei verwenden](../packaging/create-appinstallerfile-vs.md) und aktivieren Sie automatische Updates zu Ihren apps. Wenn Probleme auftreten, finden Sie unter [Problembehandlung bei der Installation mit der App-Installer-Datei](../packaging/troubleshoot-appinstaller-issues.md) , um allgemeine Probleme und Lösungen anzuzeigen.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Edge-WebView-Steuerelement von Windows Forms und WPF-Apps

Anzeigen von Webinhalten in Ihrer desktop-Anwendung mit der WebView-Steuerelement, das zuvor nur für UWP-Anwendungen verfügbar. Dieses Steuerelement verwendet der Microsoft Edge Modul zu eine Ansicht einbetten, rendert Rich-Text HTML formatiert Inhalte von einem Remotewebserver, dynamisch generiertem Code oder Inhaltsdateien rendern. Suchen Sie das WebView-Steuerelement in der neuesten Version der der [Windows Community Toolkit.](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

Für andere Steuerelemente wie WebView Windows Community Toolkit in zukünftigen Versionen suchen. Weitere Informationen finden Sie unter [Host-UWP-Steuerelemente in WPF- oder Windows Forms-Anwendung.](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>Eingabe via anvisieren und Interaktionen

[Verfolgen Sie den Blick, die Aufmerksamkeit und die Präsenz eines Benutzers anhand der Position und Bewegung seiner Augen.](../design/input/gaze-interactions.md) Diese leistungsstarke neue Methode zur Verwendung und Interaktion mit Ihren UWP-apps ist besonders hilfreich als unterstützende Technologie. Die blickeingabe bietet zudem überzeugende Möglichkeiten für Spiele (einschließlich zielerfassung und -Verfolgung) und andere interaktiven Szenarien, in denen herkömmliche Eingabegeräte (Tastatur, Maus, Touch) nicht verfügbar sind.

### <a name="msix-packaging-format"></a>MSIX-Paketformat

Auf der Microsoft Build 2018 Konferenz angekündigt, ist MSIX ein neues Containerization-Paket-Format, das für alle Windows-Anwendungen, einschließlich der Win32, Windows Forms, WPF und UWP gilt. Dieses neue Format erbt großartige Features von UWP:

* Stabile Installation und Aktualisierung. 
* Verwaltet Sicherheitsmodell mit einem flexiblen Funktion-System.
* Unterstützung für den Microsoft Store, Enterprise Management und viele benutzerdefinierte Verteilung Modelle.

Tools zum Erstellen dieser Pakete werden in einer zukünftigen Version von Visual Studio und Windows SDK verfügbar sein.

Das MSIX Packaging-Format ist ein open-Source-Format das unsere Partner mit das MSIX-Ökosystem mit Tools und Lösungen für ihr unterstützen erleichtert. Weitere Informationen zu den MSIX Packaging-Format zu erhalten, finden Sie in [MSIX-SDK](https://github.com/Microsoft/msix-packaging). 

![MSIX Verpackung image](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>Optionale Pakete mit ausführbarem Code

Optionale Pakete in Ihrer app können nun ausführbaren C#-Code enthalten. [Hier erfahren Sie, wie Sie mithilfe von Visual Studio so konfigurieren Sie optionale Add-on-Pakete, um das Haupt-app-Paket zu unterstützen.](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>Seitenübergänge

[Seitenübergänge](../design/motion/page-transitions.md) navigieren Benutzer zwischen Seiten in einer app. Sie können Benutzern das Verständnis, wo sie in der Navigationshierarchie sind, und geben Sie Feedback über die Beziehung zwischen Seiten.

### <a name="project-rome"></a>Projekt Rome

Das Projekt "ROME"-Team hat ihre IOS- und Android-SDKs, neue Features wie Aktivitäten des Benutzers und Umgestaltung Großteil ihres Codes, um eine konsistente Programmierung über die verschiedenen SDKs anzubieten überholt. [Alle neuen API-Referenz und Vorgehensweisen Dokumente](https://docs.microsoft.com/windows/project-rome/) werden während der Build 2018 Developer-Konferenz gerechnet.

### <a name="sets"></a>Gruppen

Die Funktion ist in Windows-Insider Preview-builds verfügbar. Wenn Sie die Funktion zu verwenden, wird Sie app in einem Fenster gezeichnet, die mit anderen apps, zu jeder app müssen eine eigene Registerkarte in der Titelleiste freigegeben werden kann. [Entwerfen für Sets](../design/shell/design-for-sets.md) enthält eine Anleitung zum Optimieren Sie Ihre app für den in der Benutzeroberfläche legt die bestmögliche Erfahrung zu bieten.

## <a name="developer-guidance"></a>Anleitungen für Entwickler

### <a name="get-started"></a>Erste Schritte

Wir haben unsere Get revitalized Inhalt mit neuen Lernpfade gestartet. Diese neuen Themen darauf abzielen, bieten neue Windows 10-Entwickler mit Informationen über eine Reihe häufiger Aufgaben, die sie möglicherweise erreichen möchten. Sie sind nicht Lernprogramme und tragbaren eine exemplarische Vorgehensweise nicht bereitstellen, sondern stattdessen weisen Sie darauf hin, in denen Dokumentation vorhanden ist und wie sie zu verwenden. Sehen Sie sich die überarbeitete [Programmieren](../get-started/create-uwp-apps.md) Seite, oder lesen, jede einzelne Lernpfad:

* [Erstellen eines Formulars](../get-started/construct-form-learning-track.md)
* [Anzeigen von Kunden in einer Liste](../get-started/display-customers-in-list-learning-track.md)
* [Speichern und Laden von Einstellungen](../get-started/settings-learning-track.md)
* [Arbeiten mit Dateien](../get-started/fileio-learning-track.md)

![Erhalten gestartete image](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>Bericht zur Anzeigenleistung

[Bericht zur anzeigen Performance](../publish/advertising-performance-report.md) in Dev Center-Dashboard bietet jetzt Messwerte. Wir haben auch den [Optimieren der Sichtbarkeit von anzeigeneinheiten](../monetize/optimize-ad-unit-viewability.md) -Artikel, um Empfehlungen für das Optimieren der anzeigbarkeit Ihrer anzeigen bereitstellen hinzugefügt.

### <a name="targeted-push-notifications"></a>Benutzerorientierte Pushbenachrichtigungen

Die Seite " [Notifications](../publish/send-push-notifications-to-your-apps-customers.md) " im Dev Center-Dashboard bietet jetzt zusätzliche Analysedaten für alle Ihre Benachrichtigungen in Diagramm- und weltkartenform Kartenansichten.

## <a name="videos"></a>Videos

### <a name="cwinrt"></a>C++/WinRT

C++ / WinRT ist eine neue Art der Erstellung und Nutzung von Windows-Runtime-APIs. Es verfügt über alleinige in Headerdateien implementiert und Ihnen einen erstklassigen Zugriff auf moderne app-Funktionen bietet. [Das Video ansehen](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) , um zu erfahren, wie es funktioniert, dann [Lesen die Entwicklerdokumentation](../cpp-and-winrt-apis/index.md) für Weitere Informationen.

### <a name="multi-instance-uwp-apps"></a>UWP-Apps mit mehreren Instanzen

Windows kann jetzt Sie mehrere Instanzen von Ihrer UWP-app mit jeweils in eine eigene getrennten Prozess ausgeführt werden. [Das Video ansehen](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) , um zu erfahren, wie Sie eine neue app erstellen, die dieses Feature, dann [Lesen Sie die Entwicklerdokumentation](../launch-resume/multi-instance-uwp.md) für Weitere Hinweise zur Verwendung unterstützt und warum Sie dieses Feature verwenden.

## <a name="samples"></a>Beispiele

### <a name="customer-database-tutorial"></a>Lernprogramm für die Kunden

In diesem Lernprogramm erstellt eine einfache UWP-app für die Verwaltung von eine Liste der Kunden und Konzepte und Methoden, die nützlich vorherrschen führt. Es führt Sie durch die Implementierung von UI-Elemente und Vorgänge lokalen SQLite-Datenbank hinzufügen und losen Hinweise zum Herstellen einer Verbindung mit einer remote-REST-Datenbank, wenn Sie fortfahren möchten. [Sehen Sie sich hier das Lernprogramm](../enterprise/customer-database-tutorial.md)