---
author: Xansky
description: Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um detaillierte Daten zu einem spezifischen Fehler für Ihre Xbox One Spiel abzurufen.
title: Abrufen von Details zu einem Fehler in Ihrer Xbox One-Spiele
ms.author: mhopkins
ms.date: 11/06/2018
ms.topic: article
keywords: Windows10, UWP, Store-Dienste, Microsoft Store-Analyse-API, Fehler, Details
ms.localizationpriority: medium
ms.openlocfilehash: 6f665b99b7f64e41597a5767d58b86ac8e85c6bd
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2018
ms.locfileid: "7155536"
---
# <a name="get-details-for-an-error-in-your-xbox-one-game"></a>Abrufen von Details zu einem Fehler in Ihrer Xbox One-Spiele

Verwenden Sie diese Methode in der Microsoft Store-Analyse-API um detaillierte Daten zu einem spezifischen Fehler für Ihre Xbox One Spiel abzurufen, die das über das Xbox-Portal (XDP) und im XDP Analytics Partner Center-Dashboard verfügbar ist. Diese Methode kann nur Details zu Fehlern abrufen, die in den letzten 30Tagen aufgetreten sind.

Bevor Sie diese Methode verwenden können, müssen Sie zuerst die Methode zum [Abrufen von Fehlerberichtsdaten für Ihre Xbox One Spiel](get-error-reporting-data-for-your-xbox-one-game.md) verwenden, um die ID des Fehlers abzurufen, zu dem Sie detaillierte Informationen erhalten möchten.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Rufen Sie die ID des Fehlers ab, zu dem Sie detaillierte Informationen erhalten möchten. Um diese ID zu erhalten, verwenden Sie die Methode zum [Abrufen von Fehlerberichtsdaten für Ihre Xbox One Spiel](get-error-reporting-data-for-your-xbox-one-game.md) , und verwenden Sie den Wert **FailureHash** im Antworttext dieser Methode.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | string | Die Produkt-ID des Xbox One Spiels, für die Fehlerdetails abgerufen werden. Um die Produkt-ID Ihres Spiels zu erhalten, wechseln Sie zu Ihrem Spiel in der Xbox-Entwickler-Portal (XDP) und rufen Sie die Produkt-ID von der URL ab. Alternativ können ist Sie Ihre integritätsdaten vom Windows Partner Center-Analysebericht herunterladen, die Produkt-ID in der TSV-Datei enthalten. |  Ja  |
| failureHash | string | Die eindeutige ID des Fehlers, zu dem Sie detaillierte Informationen erhalten möchten. Um diesen Wert für den Fehler zu erhalten, die Sie interessiert sind, verwenden Sie die Methode zum [Abrufen von Fehlerberichtsdaten für Ihre Xbox One Spiel](get-error-reporting-data-for-your-xbox-one-game.md) , und verwenden Sie den Wert **FailureHash** im Antworttext dieser Methode. |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der detaillierten Fehlerdaten, die abgerufen werden sollen. Der Standardwert ist 30Tage vor dem aktuellen Datum. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der detaillierten Fehlerdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| top | int | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Wenn die Abfrage keine weiteren Zeilen enthält, entält der Antworttext den Link „Weiter“, den Sie verwenden können, um die nächste Seite mit Daten anzufordern. |  Nein  |
| skip | int | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10“ und „skip=0“ die ersten 10 Datenzeilen ab, „top=10“ und „skip=10“ die nächsten 10Datenzeilen usw. |  Nein  |
| filter |string  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält einen Feldnamen aus dem Antworttext und einen Wert, die mit den Operatoren **eq** oder **ne** verknüpft sind. Anweisungen können mit **and** oder **or** kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul> | Nein   |
| orderby | string | Eine Anweisung, die die Ergebnisdatenwerte anfordert. Die Syntax lautet <em>orderby=field [order],field [order],...</em>, wobei der Parameter <em>field</em> eine der folgenden Zeichenfolgen sein kann:<ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standard ist <strong>asc</strong>.</p><p>Dies ist eine Beispielzeichenfolge für <em>orderby</em>: <em>orderby=date,market</em></p> |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Die folgenden Beispiele zeigen verschiedene Anforderungen für das Abrufen detaillierter Fehlerdaten für ein Xbox One-Spiele. Ersetzen Sie den Wert *ApplicationId* durch die Produkt-ID für Ihr Spiel.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails?applicationId=BRRT4NJ9B3D1&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails?applicationId=BRRT4NJ9B3D1&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'Windows.Desktop' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ    | Beschreibung    |
|------------|---------|------------|
| Wert      | array   | Ein Array von Objekten, die detaillierte Fehlerdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Fehlerdetailwerte](#error-detail-values).          |
| @nextLink  | String  | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10 festgelegt ist, es jedoch mehr als 10 Zeilen mit Fehlern für die Abfrage gibt. |
| TotalCount | Ganzzahl | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.        |


<span id="error-detail-values"/>

### <a name="error-detail-values"></a>Fehlerdetailwerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert           | Typ    | Beschreibung     |
|-----------------|---------|----------------------------|
| applicationId   | string  | Die Produkt-ID des Xbox One Spiels, für die detaillierte Fehlerdaten abgerufen wurden.      |
| failureHash     | string  | Der eindeutige Bezeichner des Fehlers.     |
| failureName     | string  | Der Name des Fehlers, der aus vier Teilen besteht: eine oder mehrere Problemklassen, ein Ausnahme/Fehlerprüfcode, der Name des Image, in dem der Fehler aufgetreten ist, und der zugehörige Funktionsname.           |
| date            | string  | Das erste Datum im Datumsbereich für die Fehlerdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| cabId           | string  | Die eindeutige ID der CAB-Datei, die mit diesem Fehler verknüpft ist.   |
| cabExpirationTime  | string  | Datum und Uhrzeit im Format ISO 8601, an dem/der die CAB-Datei abgelaufen ist und nicht mehr heruntergeladen werden kann.   |
| market          | string  | Der ISO3166-Ländercode des Gerätemarkts.     |
| osBuild         | string  | Die Buildnummer des Betriebssystems, auf dem der Fehler aufgetreten ist.       |
| packageVersion  | string  | Die Version des Spiels Pakets, das mit diesem Fehler verknüpft ist.    |
| deviceModel           | string  | Einer der folgenden Zeichenfolgen, die Xbox One-Konsole angibt, auf der das Spiel ausgeführt wurde, als der Fehler aufgetreten ist.<p/><ul><li><strong>Microsoft-Xbox eine</strong></li><li><strong>Microsoft-Xbox One S</strong></li><li><strong>Microsoft-Xbox One X</strong></li></ul>  |
| osVersion       | string  | Die Version des Betriebssystems, auf dem der Fehler aufgetreten ist. Dies ist immer den Wert **Windows 10**.    |
| osRelease       | string  |  Einer der folgenden Zeichenfolgen, die die Windows 10-Betriebssystemversion oder verteilungsring (als ein Subpopulation innerhalb der Betriebssystemversion) angibt, auf dem der Fehler aufgetreten ist.<p/><ul><li><strong>Version1507</strong></li><li><strong>Version1511</strong></li><li><strong>Version1607</strong></li><li><strong>Version1703</strong></li><li><strong>Version1709</strong></li><li><strong>Version1803</strong></li><li><strong>Release Preview</strong></li><li><strong>Insider Fast</strong></li><li><strong>Insider Slow</strong></li></ul><p>Wenn die Betriebssystemversion oder Verteilungsring unbekannt ist, hat dieses Feld den Wert <strong>Unknown</strong>.</p>    |
| deviceType      | string  | Der Typ des Geräts, auf dem der Fehler aufgetreten ist. Dies ist immer den Wert **Konsole**.     |
| cabDownloadable           | Boolean  | Gibt an, ob die CAB-Datei durch den Benutzer heruntergeladen werden kann.   |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "applicationId": "BRRT4NJ9B3D1",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "STOWED_EXCEPTION_System.UriFormatException_exe!ContosoSports.GroupedItems+_ItemView_ItemClick_d__9.MoveNext",
      "date": "2018-02-05 09:11:25",
      "cabId": "133637331323",
      "cabExpirationTime": "2016-12-05 09:11:25",
      "market": "US",
      "osBuild": "10.0.17134",
      "packageVersion": "1.0.2.6",
      "deviceModel": "Microsoft-Xbox One",
      "osVersion": "Windows 10",
      "osRelease": "Version 1803",
      "deviceType": "Console",
      "cabDownloadable": false
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Zugreifen auf Analysedaten mit MicrosoftStore-Diensten](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Fehlerberichtsdaten für Ihre Xbox One Spiel](get-error-reporting-data-for-your-xbox-one-game.md)
* [Abrufen der stapelüberwachung für einen Fehler in Ihrer Xbox One-Spiele](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
* [Herunterladen der CAB-Datei für einen Fehler in Ihrer Xbox One Spiel](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)