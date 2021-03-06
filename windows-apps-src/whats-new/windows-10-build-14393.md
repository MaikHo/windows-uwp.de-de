---
title: Neuigkeiten in Windows 10, Build 14393
description: Windows 10, Build 14393, und neue Entwicklertools stellen Tools, Features und Umgebungen zur Verfügung, die durch die neue Universelle Windows-Plattform unterstützt werden.
keywords: Windows 10, 1607, 14393
ms.date: 11/02/2017
ms.topic: article
ms.assetid: f95cd87b-f959-4148-a9bd-ba0b90d16e71
ms.localizationpriority: medium
ms.openlocfilehash: da3a23420dc2262b2ddbfbf913590de6eabdcf36
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173784"
---
# <a name="whats-new-in-windows-10-for-developers-build-14393"></a>Neuigkeiten für Entwickler in Windows 10, Build 14393

Windows 10 Build 14939 (auch bekannt als Anniversary Update oder Version 1607), in Kombination mit Visual Studio 2019 und dem aktualisierten SDK, bietet die Tools, Features und Umgebungen für die Entwicklung eindrucksvoller Apps für die Universelle Windows-Plattform. Nach der [Installation der Tools und des SDKs](https://developer.microsoft.com/windows/downloads#_blank) unter Windows 10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/create-uwp-apps.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

Hier finden Sie eine Liste der neuen und verbesserten Features, die für Entwickler von Interesse sind. Eine grobe Liste mit neuen Namespaces, die dem Windows SDK hinzugefügt wurden, finden Sie in den [API-Änderungen unter Windows 10, Build 14393](windows-10-build-14393-api-diff.md). Weitere Informationen zu den Highlights dieses Updates finden Sie unter [Die Highlights in Windows 10](https://developer.microsoft.com/windows/windows-10-for-developers).

## <a name="windows-10-build-14393---july-2016"></a>Windows 10, Build 14393 – Juli 2016

Feature | Beschreibung
 :---- | :----
Zugriffstasten für XAML-Elemente | Mit der neuen [**AccessKey**](/uwp/api/windows.ui.xaml.uielement.accesskey)-Eigenschaft und dem [**AccessKeyManager**](/uwp/api/windows.ui.xaml.input.accesskeymanager)-Element können Sie die Barrierefreiheit für den Tastaturzugriff in Ihrer App verbessern.
Animierte GIF-Unterstützung | Das XAML-[**Image**](/uwp/api/windows.ui.xaml.controls.image)-Element unterstützt nun animierte GIFs. Sie können diese neuen Member in [**BitmapImage**](/uwp/api/windows.ui.xaml.media.imaging.bitmapimage) verwenden, um Wiedergabe zu steuern: [**AutoPlay**](/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.autoplay), [**IsAnimatedBitmap**](/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.isanimatedbitmap), [**IsPlaying**](/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.isplaying), [**Play**](/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.play), [**Stop**](/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.stop).
App-Erweiterbarkeit | Schreiben Sie [Erweiterungen](https://github.com/microsoft/App-Extensibility-Sample) für Ihre UWP-Apps. Ermöglichen Sie Ihrer UWP-App das Hosten von Inhalten, die von anderen UWP-Apps bereitgestellt werden.  Sie können schreibgeschützte Inhalte dieser Apps ermitteln und enumerieren und darauf zugreifen.
Bewertungstests | Bei [Prüfung](/education/windows/take-tests-in-windows-10?f=255&MSPPError=-2147217396) handelt es sich um eine browserbasierte App, die gesperrte Onlinebewertungen für wichtige Prüfungen rendert. Hindern Sie Schüler und Studenten mithilfe der [Prüfungs-APIs](../apps-for-education/take-a-test-api.md) daran, während einer Prüfung andere Computer oder Internetressourcen zu verwenden.
Intelligenter Hintergrundübertragungsdienst (Background Intelligent Transfer Service, BITS) | Die [BITS](/windows/desktop/Bits/background-intelligent-transfer-service-portal)-COM-APIs und PowerShell-Cmdlets (sofern verfügbar) können jetzt in einer PowerShell-Remotesitzung verwendet werden. Dies ist insbesondere beim Verwalten von Windows Server 2016 Technical Preview-Versionen praktisch, die über keine Funktion für die lokale Anmeldung verfügen. Über PowerShell-Remotesitzungen gestartete BITS-Aufträge werden im Benutzerkontokontext der Sitzung ausgeführt und machen nur Fortschritte, wenn diesem Benutzerkonto mindestens eine aktive lokale Anmeldesitzung oder PowerShell-Remotesitzung zugeordnet ist. Ausführliche Informationen zum Verwalten von Sitzungen für Übertragungen mit langer Ausführungsdauer finden Sie unter [So verwalten Sie PowerShell-Remotesitzungen](/windows/desktop/Bits/using-windows-powershell-to-create-bits-transfer-jobs).<br/><br/>In früheren Windows-Versionen, die [BITS-Hilfsprogrammtoken](/windows/desktop/Bits/helper-tokens-for-bits-transfer-jobs) unterstützt haben, musste der Aufgabenbesitzer Administratorrechte besitzen, um ein Hilfsprogrammtoken festzulegen. In dieser Version kann ein BITS-Aufgabenbesitzer nun auch Hilfsprogrammtoken festlegen, ohne Administrator zu sein, sofern das Hilfsprogrammtoken nicht über Administratorfunktionen verfügt. Dadurch werden Sicherheitsrisiken von Tools für Hintergrunddownloads oder -aktualisierungen gemindert, da diese Tools nicht unter einem Konto mit Administratorrechten, sondern unter dem NetworkService-Konto mit niedrigeren Berechtigungen ausgeführt werden.
Verbesserte Unterstützung für farbige Schriftarten | Direct2D unterstützt nun das Rendering einer größeren Anzahl von farbigen Schriftartformaten. Dadurch können Entwickler mehr Schriftarten in ihren Direct2D-Apps verwenden als je zuvor. Unter anderem werden folgende Elemente unterstützt: <br/>&bull; OpenType-Tabelle vom Typ „sbix“. Dadurch werden farbige Bitmapinhalte in Schriftarten ermöglicht.<br/>&bull; OpenType-Tabelle vom Typ „SVG“. Dadurch werden SVG-Inhalte in Schriftarten ermöglicht.<br/>&bull; OpenType-Tabelle vom Typ „CBDT“. Infolge dieser Unterstützung werden farbige Bitmapinhalte in Schriftarten unterstützt. <br/><br/>Direct2D unterstützt diese farbigen Schriftartformate automatisch, wenn das Flag **D2D1_DRAW_TEXT_OPTIONS_ENABLE_COLOR_FONT** aktiviert ist.  Weitere Informationen finden Sie in den folgenden Themen: <br>&bull;[**ID2D1DeviceContext4**](/windows/win32/api/d2d1_3/nn-d2d1_3-id2d1devicecontext4)<br>&bull;[**D2D1_DRAW_TEXT_OPTIONS**](/windows/win32/api/d2d1/ne-d2d1-d2d1_draw_text_options)<br>&bull;[**ID2D1SvgGlyphStyle**](/windows/win32/api/d2d1_3/nn-d2d1_3-id2d1svgglyphstyle)     
Dynamischer CommandBar-Überlauf | Primäre Befehle in einem [**Commandbar**](../design/controls-and-patterns/app-bars.md)-Element werden nun automatisch in das Überlaufmenü verschoben, wenn der Platz für die Darstellung aller Befehle nicht ausreicht.
Kompositionsinteraktions-API| Der neue [**Windows.UI.Composition.Interactions**](/uwp/api/windows.ui.composition.interactions)-Namespace ermöglicht den Zugriff auf die Eingabe zur Implementierung von Animationen und Effekten.  Diese neue API-Familie für die visuelle Ebene trägt dazu bei, dass die App auch bei hoher Auslastung des UI-Threads schnell reagiert und reibungslos läuft.  
Windows.UI.Composition | Der [**Windows.UI.Composition**](/uwp/api/windows.ui.composition)-Namespace hat Unterstützung für zahlreiche Features hinzugefügt, unter anderem für: <br/><br/>&bull; Schatten: Dieses Feature ermöglicht eine realitätsnahe Tiefendarstellung in Ihrer App. <br/>&bull; Szenenbeleuchtung: Mit diesem Feature können Sie Ihre Steuerelemente und UI besser perspektivisch darstellen, indem Sie eine Lichtquelle auf sie richten und dadurch einen anderen Effekt für Ihre Anwendung bereitstellen.<br/>&bull; Weichzeichnereffekt: Mit diesem Feature können Sie wichtige Informationen hervorheben und andere Informationen unscharf darstellen. Sie können die unscharfe Darstellung von UI animieren und sie somit lebendiger gestalten.  <br/>&bull; Implizite Animationen: Mit impliziten Animationen können Sie visuelle Elemente animieren, wenn sich Eigenschaften für diese Elemente ändern. Sie können mithilfe von impliziten Animationen Layoutanimationen ausführen, d. h. wenn sich das Layout für eine App ändert, können Sie die Änderung animieren. <br/>&bull; CompositionBackdropBrush: Bei „CompositionBackdropBrush“ handelt es sich um einen neuen Pinseltyp, mit dem das aktuelle RenderTarget-Element als Eingabe für einen Effekt ausgewählt werden kann.<br/>&bull; LayerVisual: Ermöglicht die Anwendung eines Effekts auf eine Sammlung von visuellen Elementen. Beispiel: Anwenden von Graustufen auf einen Teil der UI, um einen deaktivierten Bereich der UI zu kennzeichnen<br/>&bull; CompositionMashBrush: Ermöglicht die Angabe einer Deckkraftmaske.<br/>&bull; Beschnitttransformationen: Ermöglicht die Anwendung von Transformationen auf Beschnittrechtecke.<br/>&bull; Oberflächenpinseltransformationen: Ermöglicht die Anwendung von Transformationen auf „CompositionSurfaceBrush“.<br/>&bull; CompositionNineGridBrush: Ermöglicht die Angabe von Einblendungen zur Größenanpassung im Vordergrund eines Bilds (Raster mit neun Bereichen) oder das Erstellen rechteckiger Rahmen in Volltonfarbe.<br/>&bull; Ergänzungen zu Ausdruckszeichenfolgen: Ausdruckszeichenfolgen unterstützen neue Funktionen, Ausdrucksoperatoren und Schlüsselwörter.
Verbundene Animationen | Mit [**ConnectedAnimationService**](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) können Sie Animationen ausführen, während der Benutzer zwischen Szenen oder Seiten wechselt. Weitere Informationen finden Sie im Video [Verbundene Animationen](https://channel9.msdn.com/Events/Build/2016/P485).
Verbundene Apps | Ermitteln Sie über die Cloud verbundene Geräte oder Geräte in der Nähe, und entwickeln Sie Umgebungen, die den nahtlosen Wechsel zwischen diesen Geräten ermöglichen. Ausführliche Informationen finden Sie unter [Verbundene Apps und Geräte](../launch-resume/connected-apps-and-devices.md).
Desktop-App-Konverter | Desktop-App-Konverter ist ein Tool, mit dem Sie vorhandene für .NET 4.6.1 oder Win32 geschriebene Desktop-Apps für die universelle Windows-Plattform (UWP) konvertieren können.
Entwickeln von Apps für Barrierefreiheit | Entwickeln Sie basierend auf den [Richtlinien für die App-Barrierefreiheit](https://developer.microsoft.com/windows/accessible-apps) barrierefreie Software für höhere Benutzerfreundlichkeit und Kundenzufriedenheit. Lassen Sie sich durch Geschichten zu barrierefreien Technologieprodukten inspirieren. Auf diesem neuen Hub finden Sie Informationen zur barrierefreien Gestaltung Ihrer Apps für alle Benutzer.
Direct3D | Die Direct 3D-Dokumentation enthält jetzt viele neue Themen. Weitere Informationen zu diesen Updates finden Sie unter **Windows 10, Version 1607** auf der [Seite mit neuen Versionen von Direct3D 12](/windows/desktop/direct3d12/new-releases).
Spiele: Unterstützung für Arcade-Joystick und Rennlenkrad | [**Windows.Gaming.Input**](/uwp/api/windows.gaming.input) unterstützt zwei neue Klassen von Eingabegeräten: Arcade-Joysticks und Rennlenkräder. Da dadurch die Unterstützung von Arcade-Joysticks und Rennlenkrädern als Klasse eines Geräts ermöglicht wird, müssen Sie keinen speziellen Code für einzelne Versionen dieser Geräte schreiben. Alle Xbox 360- und Xbox One-Geräte dieser Klassen und ausgewählte PC (HID)-Geräte werden unterstützt.
Spiele: Unterstützung der Kraftrückmeldung | Die [**Windows.Gaming.Input.ForceFeedback**](/uwp/api/windows.gaming.input.forcefeedback)-API bietet Unterstützung für die Steuerung der Kraftrückmeldung von PC (HID)-Rennlenkrädern.
Spiele: OEM-Unterstützung für benutzerdefinierte WinRT-Klassenbibliotheken für neue Eingabegeräte | Die [**Windows.Gaming.Input.Custom**](/uwp/api/windows.gaming.input.custom)-API bietet Unterstützung für Fremdzubehör-OEMs und ermöglicht das Erstellen benutzerdefinierter WinRT-Klassenbibliotheken für deren Xbox 360- und Xbox One-Zubehör.     
Globalisierung | Klassen im neuen [**Windows.Globalization.PhoneNumberFormatting**](/uwp/api/windows.globalization.phonenumberformatting)-Namespace unterstützen die Formatierung, die Überprüfung und den Abgleich von Telefonnummern mit breiter globaler Unterstützung. Die neuen Klassen unterstützen zahlreiche lokale Standardformate und sogar die inkrementelle Formatierung partieller Nummernangaben während der Eingabe.
In-App-Einkäufe und App-Lizenzen | Der [**System.Services.Store**](/uwp/api/windows.services.store)-Namespace stellt eine neue API für In-App-Einkäufe und den Zugriff auf Store-Lizenzinformationen für Ihre App bereit. Weitere Informationen finden Sie unter [Ermöglichen von In-App-Produktkäufen](../monetize/enable-in-app-product-purchases.md).
InkToolbar | [**InkToolbar**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbar) ist ein Steuerelement für eine universelle Windows-App, das eine anpassbare und erweiterbare Sammlung von Schaltflächen enthält, die Freihanddaten in einem verknüpften **InkCanvas**-Element aktivieren.<br/><br/>Die Symbolleiste enthält standardmäßig Schaltflächen zum Zeichnen, Löschen, Hervorheben sowie zum Anzeigen eines Lineals. Abhängig vom Feature werden weitere Einstellungen und Befehle, z. B. für Freihandfarbe, Strichstärke und das Löschen aller Elemente, in einem Flyout bereitgestellt.<br/><br/>**InkToolbar** kann darüber hinaus mit eigenen Stiften, Tools und weiteren Freihandeingabefeatures angepasst werden.
MAX_PATH-Einschränkungen entfernt | MAX_PATH-Einschränkungen wurden aus allgemeinen APIs für Win32-Dateien und -Verzeichnisse entfernt. Das Verhalten kann optional aktiviert werden. Ausführliche Informationen finden Sie unter [Dateibenennung, Pfade und Namespaces](/windows/desktop/FileIO/naming-a-file) im Abschnitt **Beschränkung der maximalen Länge von Pfaden**.
Kamera: Medienframe-Reader |Der neue [**Windows.Media.Capture.Frames**](/uwp/api/Windows.Media.Capture.Frames)-Namespace stellt APIs für das Lesen von Medienframes aus verfügbaren Quellen bereit. Hierzu zählen Farb-, Tiefen- und Infrarotkameras sowie Audiogeräte und sogar benutzerdefinierte Framequellen (etwa für Skeletal-Tracking-Frames). Dieses Feature wurde für die Verwendung von Apps entworfen, die Medienframes in Echtzeit verarbeiten, wie beispielsweise Augmented-Reality- und Tiefenkamera-Apps.
Medienwiedergabe | Für die Medienwiedergabe in Apps wird die Verwendung der [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer)-Klasse empfohlen. Dabei kann bei Bedarf die [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)-Klasse zum Rendern von Medien in XAML verwendet werden. Verbesserungen der **MediaPlayer**-Klasse: Möglichkeit, Audio auf einem spezifischen Endpunkt wiederzugeben, Einführung der [**MediaPlaybackSession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession)-Klasse für die Verwaltung des Playerzustands, Zusammendrücken und Zoomen für Video, Möglichkeit, Videos auf einer [**Windows.UI.Composition**](/uwp/api/Windows.UI.Composition)-Oberfläche zu rendern, und Bereitstellung der [**MediaTimelineController**](/uwp/api/Windows.Media.MediaTimelineController)-Klasse für die Synchronisierung der Wiedergabe mehrerer Medienplayer.<br>Beim Öffnen eines Medienelements mit [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) können Sie nun Medienfehler ermitteln, die aufgrund von nicht unterstützten oder nur partiell unterstützten Codecs auftreten.<br><br>Die neue [**MediaBreakManager**](/uwp/api/Windows.Media.Playback.MediaBreakManager)-Klasse zum schnellen und einfachen Hinzufügen von Medienunterbrechungen zu einer [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem)-Klasse ermöglicht Ihnen das einfache Erstellen, Planen und Verwalten von Medienunterbrechungen wie Audio- und Videoanzeigen.<br><br>Die **MediaPlayer**-Klasse wird nun automatisch in Steuerelemente für den Systemmedientransport (System Media Transport Controls, SMTC) integriert. Die neue [**MediaPlaybackCommandManager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager)-Klasse ermöglicht es Ihnen, SMTC-Befehle teilweise oder vollständig zu überschreiben.<br><br>Mit dem neuen Einzelprozessmodell können Sie nun Medien im Hintergrund ausführen. Dieses Modell ist wesentlich einfacher zu implementieren als das vorherige Modell, das zwei Prozesse umfasste. Dank einer neuen Manifestfunktion können Sie dem System mitteilen, dass Ihre App Medien im Hintergrund wiedergeben muss. Mit den neuen App-Lebenszyklusereignissen [**EnteredBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) und [**LeavingBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) können Sie bei der Ausführung im Hintergrund nicht verwendete Ressourcen freigeben.
Microsoft Edge | Microsoft Edge fügt Unterstützung für die Fetch-, Streams- und Beacon-APIs hinzu. Fetch ist der moderne Ersatz für **XMLHttpRequests** und fügt Funktionen auf niedrigerer Ebene für Anforderungen und Antworten hinzu. Die Funktion zum Streamen von Datenquellen wurde ebenfalls hinzugefügt. Streams ermöglicht das Lesen großer Datenmengen aus Quellen. Dabei muss nicht mehr die gesamte Quelle gepuffert werden, um den Lesevorgang zu ermöglichen. Die Beacon-API bietet eine effiziente Möglichkeit zum Senden von Informationen wie etwa kritischer Anwendungs- und Messinformationen an einen Server über Beacons(unidirektionale Anforderungen). Die Beacon-API ist vollständig asynchron und muss keine Anforderung verarbeiten. Dadurch wird die Anforderung zu einer nicht blockierenden Anforderung.<br/><br/>Weitere Informationen zu neuen APIs in Microsoft Edge finden Sie im Microsoft Edge-Entwicklerhandbuch unter [Neuigkeiten](https://developer.microsoft.com/microsoft-edge/platform/documentation/dev-guide).  
Neue App-Lebenszyklusereignisse | Dem [App-Lebenszyklus](../launch-resume/app-lifecycle.md?f=255&MSPPError=-2147217396) wurden zwei neue Ereignisse hinzugefügt, mit denen Sie einfach ermitteln können, wenn sich der Sichtbarkeitsstatus Ihrer App ändert.
Aus einem Einzelprozess bestehende Hintergrundaktivität | Die Ausführung des Hintergrundcodes ist viel einfacher und erfordert nicht mehr die Erstellung einer Hintergrundaufgabe. Sie können Code im Hintergrund als separaten Prozess oder direkt in der Vordergrundanwendung ausführen. Ausführliche Informationen finden Sie unter [Hintergrundaktivität mit dem Einzelprozessmodell](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#z3OjoTRbQMVX1puj.99).
Social Media-Erweiterbarkeits- und Visitenkarten-APIs für die Kontakte-App | Integrieren Sie App-basierte Nachrichten sowie Aktionen für Sprach- und Videoanrufe direkt in Visitenkarten. Verwenden Sie Kontaktzuordnungen, um Social Media-Inhalte in der Ansicht „Neuigkeiten“ der Kontakte-App bereitzustellen.
StreamSocket | [  **StreamSocket**](/uwp/api/windows.networking.sockets.streamsocket) wurden neue **GetEndpointPairsAsync**-Methoden hinzugefügt, mit denen Sie eine Liste der Endpunkte abrufen können, die von einer DNS-Suche für einen bestimmten Dienst unter einem bestimmten Hostnamen zurückgegeben werden. Dies ist nützlich, wenn der Dienst tatsächlich auf mehreren Servern gehostet wird. Ihr Code kann also versuchen, eine Verbindung mit mehreren potenziellen Internetanbietern herzustellen, und die erste hergestellte Verbindung nutzen.
Kacheln und Benachrichtigungen | Signalbenachrichtigungen werden jetzt auf der Taskleiste angezeigt. <br/><br/> Mit den neuen [verfolgbaren Kachelbenachrichtigungen](../design/shell/tiles-and-notifications/chaseable-tile-notifications.md) können Sie ermitteln, was auf der Live-Kachel einer App angezeigt wurde, als der Benutzer auf die Kachel geklickt hat. <br/><br/> Verwenden Sie ein [Favoritenbild](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md#hero-image), [adaptiven Inhalt](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md#adaptive-content), [Zuschreibungstext](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md#attribution-text) und [Kontextmenüaktionen](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md#context-menu-actions) in Ihren Popupbenachrichtigungen. <br/><br/> Mit dem [Benachrichtigungs-Listener](../design/shell/tiles-and-notifications/notification-listener.md) können Sie auf alle Popupbenachrichtigungen zugreifen, einschließlich Benachrichtigungen, die nicht aus Ihrer eigenen App stammen. <br/><br/> [Benachrichtigungsspiegelung](../design/shell/tiles-and-notifications/notification-mirroring.md) sorgt dafür, dass Benachrichtigungen von Ihrem Smartphone auf Ihrem Desktop angezeigt werden. <br/><br/> [Universelles Schließen](../design/shell/tiles-and-notifications/universal-dismiss.md) ermöglicht es Ihnen, Benachrichtigungen über alle Geräte hinweg zu verknüpfen, sodass Benachrichtigungen, die der Benutzer auf einem Gerät schließt, überall geschlossen werden.
Textsortierung | [  **Windows.Data.Text**](/uwp/api/windows.data.text) wurden neue Methoden hinzugefügt, um das Sortieren von Text anhand einer Phonemsortierreihenfolge zu unterstützen. Diese Vorgehensweise ist in erster Linie für die Verwendung bei der Sortierung von Daten wie etwa Eigennamen in Japanisch gedacht, da es hier wichtig ist, Namen in phonetischer Reihenfolge und nicht in Zeichencodepunkt-Reihenfolge zu sortieren.
Textsuche in XAML-Kombinationsfeldern | Bei der Eingabe durch einen Benutzer in ein Kombinationsfeld werden Kandidaten angezeigt, die der Zeichenfolge des Benutzers entsprechen.
Benutzeroberflächenautomatisierung | Benutzeroberflächenautomatisierungs-Anbieter können das System nun durch Aufruf der [**UiaRaiseChangesEvent**](/windows/desktop/api/uiautomationcoreapi/nf-uiautomationcoreapi-uiaraisechangesevent)-Funktion über Änderungen an Dokumenten informieren.
UWP auf Xbox One | Dieses Update umfasst die erste vollständige Version der universellen Windows-Plattform (UWP) auf Xbox One. Es enthält neue Features, Updates für vorhandene Features und Fehlerbehebungen. Weitere Informationen finden Sie im Thema [UWP auf Xbox One](../xbox-apps/index.md).
Verknüpfung zwischen Web und App | Verknüpfen Sie Ihre App mit einer Website. Wenn Benutzer einen Link zu Ihrer Website öffnen, wird stattdessen Ihre App geöffnet. Ausführliche Informationen finden Sie unter [Unterstützung der Verknüpfung zwischen Web und App mit App-URI-Handlern](../launch-resume/web-to-app-linking.md).
WebSockets | Sowohl [**MessageWebSockets**](/uwp/api/windows.networking.sockets.messagewebsocket) also auch [**StreamWebSockets**](/uwp/api/windows.networking.sockets.streamwebsocket) wurde Unterstützung hinzugefügt, um Folgendes zu ermöglichen: Anzeigen des Serverzertifikats, Anzeigen von Zwischenzertifikaten, die vom Server während der SSL-Aushandlung gesendet wurden, Ausführen der Überprüfung für benutzerdefinierte Serverzertifikate und Angeben, dass bestimmte Serverzertifikatfehler ignoriert werden sollen.
Windows Information Protection (WIP)-APIs | [**WIP**](../enterprise/wip-hub.md) ist ein Satz von Features auf Desktops, Laptops, Tablets und Smartphones für die mobile Geräteverwaltung (Mobile Device Management, MDM). Mit WIP können Unternehmen besser steuern, wie ihre Daten auf vom Unternehmen verwalteten Geräten behandelt werden. <br/><br/>Sie können mithilfe von WIP-APIs Apps erstellen, die Datenrichtlinien berücksichtigen und dafür sorgen, dass diese Richtlinien keine Auswirkungen auf die persönlichen Daten von Mitarbeitern haben. Richtlinienadministratoren vertrauen Ihrer App und darauf, dass nur die Unternehmensdaten verwendet werden. Und Mitarbeiter schätzen es, dass ihre persönlichen Daten auf den Geräten unangetastet bleiben, auch wenn sie die Registrierung des Geräts in der mobilen Geräteverwaltung (Mobile Device Management, MDM) aufheben oder das Unternehmen ganz verlassen.
Windows IOT Core | Windows IoT Core bietet nun vollständige Unterstützung für Raspberry Pi 3 sowie eine Remoteanzeigeumgebung. Dies ermöglicht Benutzern die Remoteanzeige und -steuerung von UWP-Anwendungen auf einem IoT Core-Gerät.
Entsperren von Windows mit Begleitgeräten (IoT) | Ein Begleitgerät ist ein Gerät, das in Verbindung mit dem Windows 10-Desktopgerät zur Verbesserung der Benutzerauthentifizierung verwendet werden kann. Mit dem [Begleitgeräteframework](../security/companion-device-unlock.md) kann ein Begleitgerät umfangreiche Funktionen für Microsoft Passport bereitstellen, auch wenn Windows Hello nicht verfügbar ist (beispielsweise, wenn das Windows 10-Desktopgerät über keine Kamera für die Gesichtsauthentifizierung oder kein Fingerabdrucklesegerät verfügt).
Winsock | TCP-Sockets können nun mit [Winsock](https://tools.ietf.org/html/rfc7413) konfiguriert werden und [RFC 7413](https://tools.ietf.org/html/rfc7413) TCP Fast Open verwenden. Dazu muss die TCP_FASTOPEN-Socketoption festgelegt werden.