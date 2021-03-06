---
ms.assetid: 949D1CE0-DD7D-420E-904D-758FADEBE85A
title: Aktivieren von Gerätefunktionen
description: In diesem Lernprogramm wird beschrieben, wie Gerätefunktionen in Microsoft Visual Studio deklariert werden. Diese Funktionen ermöglichen Ihrer App die Verwendung von Kameras, Mikrofonen, Positionssensoren und anderen Geräten.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 51481e30f3dc2f60bf4483e4a22d48b5df533994
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159714"
---
# <a name="enable-device-capabilities"></a>Aktivieren von Gerätefunktionen



In diesem Lernprogramm wird beschrieben, wie Gerätefunktionen in Microsoft Visual Studio deklariert werden. Diese Funktionen ermöglichen Ihrer App die Verwendung von Kameras, Mikrofonen, Positionssensoren und anderen Geräten.

## <a name="specify-the-device-capabilities-your-app-will-use"></a>Angeben der von der App verwendeten Gerätefunktionen


Windows-Apps erfordern eine Angabe im App-Paketmanifest, wenn Sie bestimmte Gerätetypen verwenden. In Visual Studio können Sie die meisten Funktionen mit dem [Manifest-Designer](/previous-versions/br230259(v=vs.140)) deklarieren oder die Funktionen wie unter [So wird's gemacht: Angeben von Gerätefunktionen in einem Paketmanifest (manuell)](/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest) beschrieben manuell hinzufügen. In diesem Lernprogramm wird vorausgesetzt, dass Sie den Manifest-Designer verwenden.

**Hinweis**    Einige Gerätetypen, z. b. Drucker, Scanner und Sensoren, müssen nicht im App-Paket Manifest deklariert werden.

-   Doppelklicken Sie im Projektmappen-Explorer von Visual Studio auf die Paketmanifestdatei **Package.appxmanifest**.
-   Öffnen Sie die Registerkarte **Funktionen**.
-   Wählen Sie die Gerätefunktionen aus, die Ihre App verwendet. Wenn die gewünschte Funktion nicht im Manifest-Designer angezeigt wird, fügen Sie sie manuell hinzu. Weitere Informationen finden Sie unter [So wird's gemacht: Angeben von Gerätefunktionen in einem Paketmanifest](/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest).

| Gerätefunktion | Manifest-Designer | BESCHREIBUNG |
|-------------------|-------------------|-------------|    
| AllJoyn | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht es AllJoyn-fähigen Apps und Geräten in einem Netzwerk, sich gegenseitig zu erkennen und miteinander zu interagieren. Alle Apps, die auf APIs im [**Windows.Devices.AllJoyn**](/uwp/api/Windows.Devices.AllJoyn)-Namespace zugreifen, müssen diese Funktion verwenden. |
| Blockierte Chatnachrichten | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht Apps das Lesen von SMS- und MMS-Nachrichten, die von der Spamfilter-App blockiert wurden. |
| Chatnachrichtzugriff | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht Apps das Lesen und Löschen von Textnachrichten. Ermöglicht Apps darüber hinaus das Speichern von Chatnachrichten im Systemdatenspeicher. |
| Codeerzeugung | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht Apps das dynamische Generieren von Code. |
| Unternehmensauthentifizierung | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Diese Funktion unterliegt der Microsoft Store Richtlinie. Sie bietet die Möglichkeit zum Herstellen einer Verbindung mit Intranetressourcen im Unternehmen, die Domänenanmeldeinformationen erfordern. Diese Funktion ist in der Regel für die meisten Apps nicht erforderlich. | 
| Internet (Client) | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet ausgehenden Zugriff auf das Internet und auf Netzwerke an öffentlichen Orten wie Flughäfen und Cafés. Beispielsweise Intranetnetzwerke, für die der Benutzer das Netzwerk als „öffentlich“ festgelegt hat. Die Funktion sollte von den meisten Apps verwendet werden, die den Internetzugriff benötigen. |
| Internet (Client &amp; Server) | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet ein- und ausgehenden Zugriff auf das Internet und auf Netzwerke an öffentlichen Orten wie Flughäfen und Cafés. Diese Funktion ist eine Obermenge von **Internet (Client)**. **Internet (Client)** muss nicht aktiviert sein, wenn diese Funktion ebenfalls aktiviert ist. Der eingehende Zugriff auf kritische Ports ist immer gesperrt. |
| Ort| ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet Zugriff auf die aktuelle Position. Die Position wird von spezieller Hardware (z. B. einem GPS-Sensor im PC) abgerufen oder aus verfügbaren Netzwerkinformationen abgeleitet. | 
| Mikrofon | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet Zugriff auf den Audiofeed des Mikrofons. Mit dieser Funktion kann die App Audio von angeschlossenen Mikrofonen aufzeichnen. | 
| Musikbibliothek | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht das Hinzufügen, Ändern oder Löschen von Dateien in der **Musikbibliothek** für den lokalen PC und die PCs der **Heimnetzgruppe**. | 
| 3D-Objekte | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet programmgesteuerten Zugriff auf die **3D-Objekte** des Benutzers, wodurch die App alle Dateien in der Bibliothek auflisten und ohne Eingreifen des Benutzers darauf zugreifen kann. Diese Funktion wird in der Regel in 3D-Apps und -Spielen verwendet, die auf die gesamte **3D-Objektbibliothek** zugreifen müssen. | 
| Telefonanruf | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht Apps den Zugriff auf alle Telefonleitungen auf dem Gerät sowie das Ausführen der folgenden Funktionen: Tätigen eines Anrufs über die Telefonleitung und Anzeigen der systemeigenen Wählhilfe ohne Benutzeraufforderung; Zugreifen auf Leitungsmetadaten; Zugreifen auf Leitungstrigger. Festlegen und Überprüfen der Liste „Blockieren“ und der Informationen zum Anrufursprung durch die vom Benutzer ausgewählte Spamfilter-App. | 
| Bildbibliothek | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht das Hinzufügen, Ändern oder Löschen von Dateien in der **Bildbibliothek** für den lokalen PC und die PCs der **Heimnetzgruppe**. | 
| Point of Service | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht den Zugriff auf Peripherie Dienst-Peripheriegeräte. Diese Funktion ist für den Zugriff auf APIs im [**Windows. Devices. poindefservice**](/uwp/api/Windows.Devices.PointOfService) -Namespace erforderlich. | 
| Private Netzwerke (Client &amp; Server) | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet ein- und ausgehenden Zugriff auf Intranetnetzwerke, die über einen authentifizierten Domänencontroller verfügen oder vom Benutzer als Heim- oder Firmennetzwerke festgelegt wurden. Der eingehende Zugriff auf kritische Ports ist immer gesperrt. | 
| Nähe | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet die Möglichkeit, über Nahfeldkommunikation (Near-Field Communication, NFC) eine Verbindung mit Geräten in unmittelbarer Nähe zum PC herzustellen. Die Nahfeldnäherung kann verwendet werden, um Dateien zu senden oder mit einer App auf dem anderen Gerät in der Nähe zu kommunizieren. | 
| Wechselmedien | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet die Möglichkeit zum Hinzufügen, Ändern oder Löschen von Dateien auf Wechselmedien. Die App kann nur auf die Dateitypen auf Wechselmedien zugreifen, die mithilfe der Deklaration für **Dateitypzuordnungen** im Manifest definiert sind. Die App kann nicht auf Wechselmedien auf PCs der **Heimnetzgruppe** zugreifen. | 
| Freigegebene Benutzerzertifikate | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Diese Funktion unterliegt der Microsoft Store Richtlinie. Sie bietet die Möglichkeit, zum Überprüfen der Identität des Benutzers auf Software- und Hardwarezertifikate (wie Smartcardzertifikate) zuzugreifen. Wenn verwandte APIs zur Laufzeit aufgerufen werden, muss der Benutzer eine Maßnahme ergreifen (Karte einfügen, Zertifikat auswählen usw.). Diese Funktion ist nicht erforderlich, wenn Ihre App über die **Certificates**-Deklaration ein privates Zertifikat enthält. | 
| Benutzerkontoinformationen | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet Apps die Möglichkeit, auf den Namen und das Bild des Benutzers zuzugreifen. Diese Funktion ist für den Zugriff auf einige APIs im [**Windows.System.UserProfile**](/uwp/api/Windows.System.UserProfile)-Namespace erforderlich. | 
| Videobibliothek | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht das Hinzufügen, Ändern oder Löschen von Dateien in der **Videobibliothek** für den lokalen PC und die PCs der **Heimnetzgruppe**. | 
| VOIP-Anruf | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht Apps den Zugriff auf die VOIP-Anruf-APIs im [**Windows.ApplicationModel.Calls**](/uwp/api/Windows.ApplicationModel.Calls)-Namespace. | 
| Webcam | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet Zugriff auf den Videofeed der integrierten Kamera oder der angeschlossenen Webcam. Mit dieser Funktion kann die App Schnappschüsse und Filme aufnehmen. | 
| USB | | Bietet Zugriff auf benutzerdefinierte USB-Geräte. Diese Funktion erfordert untergeordnete Elemente. Dieses Feature wird für Windows Phone nicht unterstützt. | 
| Eingabegerät (Human Interface Device, HID) | | Bietet Zugriff auf Eingabegeräte. Diese Funktion erfordert untergeordnete Elemente. Weitere Informationen finden Sie unter [So wird's gemacht: Angeben von Gerätefunktionen für HID](/uwp/schemas/appxpackage/how-to-specify-device-capabilities-for-hid). | 
| Bluetooth GATT | | Bietet über eine Sammlung von primären Diensten, enthaltenen Diensten, Merkmalen und Deskriptoren Zugriff auf Bluetooth LE-Geräte. Diese Funktion erfordert untergeordnete Elemente. Weitere Informationen finden Sie unter [So wird's gemacht: Angeben von Gerätefunktionen für Bluetooth](/uwp/schemas/appxpackage/how-to-specify-device-capabilities-for-bluetooth). | 
| Bluetooth RFCOMM |  | Bietet Zugriff auf APIs, die den Tarif "Basic Rate/erweiterte Daten Rate (BR/EDR)" unterstützen, und ermöglicht der UWP-App außerdem das Zugreifen auf ein Gerät, das das PWP (Serial Port Profile) implementiert. Diese Funktion erfordert untergeordnete Elemente. Weitere Informationen finden Sie unter [So wird's gemacht: Angeben von Gerätefunktionen für Bluetooth](/uwp/schemas/appxpackage/how-to-specify-device-capabilities-for-bluetooth). |

## <a name="use-the-windows-runtime-api-for-communicating-with-your-device"></a>Verwenden der Windows-Runtime-API für die Kommunikation mit dem Gerät

In der folgende Tabelle werden einige der Funktionen mit Windows-Runtime-APIs verbunden.

| Gerätefunktion        | API             | 
|--------------------------|-----------------|
| AllJoyn                  | [**Windows.Devices.AllJoyn**](/uwp/api/Windows.Devices.AllJoyn) | 
| Blockierte Chatnachrichten    | [**Windows.ApplicationModel.CommunicationBlocking**](/uwp/api/Windows.ApplicationModel.CommunicationBlocking) | 
| Ort                 | Weitere Informationen finden Sie unter [Übersicht über Karten und Position](../maps-and-location/index.md). | 
| Telefonanruf               | [**Windows.ApplicationModel.Calls**](/uwp/api/Windows.ApplicationModel.Calls) | 
| Benutzerkontoinformationen | [**Windows.System.UserProfile**](/uwp/api/Windows.System.UserProfile) | 
| VOIP-Anruf             | [**Windows.ApplicationModel.Calls**](/uwp/api/Windows.ApplicationModel.Calls) | 
| USB                      | [**Windows.Devices.Usb**](/uwp/api/Windows.Devices.Usb) | 
| HID                      | [**Windows.Devices.HumanInterfaceDevice**](/uwp/api/Windows.Devices.HumanInterfaceDevice) | 
| Bluetooth GATT           | [**Windows. Devices. Bluetooth. generiertributeprofile**](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile) | 
| Bluetooth RFCOMM         | [**Windows.Devices.Bluetooth.Rfcomm**](/uwp/api/Windows.Devices.Bluetooth.Rfcomm) | 
| Point of Service         | [**Windows.Devices.PointOfService**](/uwp/api/Windows.Devices.PointOfService) |