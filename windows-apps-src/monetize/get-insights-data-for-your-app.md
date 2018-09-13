---
author: mcleanbyron
description: Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um Einblicke Daten für Ihre app zu erhalten.
title: Abrufen von Daten Einblicke
ms.author: mcleans
ms.date: 07/31/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, Uwp, Store-Dienste, Microsoft Store-Analyse-API, Einblicke
ms.localizationpriority: medium
ms.openlocfilehash: 53fbd91437e5dc702f8672c6cbadeea32a8a96bf
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "3963620"
---
# <a name="get-insights-data"></a>Abrufen von Daten Einblicke

Verwenden Sie diese Methode in der Microsoft Store-Analyse-API zum Abrufen von Daten Einblicke Käufe, Integrität und Nutzung Metrik für eine app während eines bestimmten Zeitraums und andere optionale Filter im Zusammenhang mit. Diese Informationen sind auch im [Bericht](../publish/insights-report.md) in Windows Dev Center-Dashboard verfügbar.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | string | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) der app, für die Sie Einblicke Daten abrufen möchten. Wenn Sie diesen Parameter nicht angeben, enthält der Antworttext Einblicke Daten für alle apps, die für Ihr Konto registriert wurden.  |  Nein  |
| startDate | date | Das Startdatum im Datumsbereich der Einblicke Daten, die abgerufen werden sollen. Der Standardwert ist 30Tage vor dem aktuellen Datum. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der Einblicke Daten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| filter | string  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält einen Feldnamen aus dem Antworttext und einen Wert, die mit den Operatoren **eq** oder **ne** verknüpft sind. Anweisungen können mit **and** oder **or** kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Beispielsweise *Filter = DataType Eq 'Kauf"*. <p/><p/>Sie können die folgenden Filterfelder angeben:<p/><ul><li><strong>Erwerb</strong></li><li><strong>Gesundheit</strong></li><li><strong>Nutzung</strong></li></ul> | Nein   |

### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt eine Anforderung zum Abrufen von Daten Einblicke. Ersetzen Sie den Wert *applicationId* durch die Store-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights?applicationId=9NBLGGGZ5QDR&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'acquisition' or dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von Objekten, die Einblicke Daten für die app enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie weiter unten im Abschnitt [Insight Werte](#insight-values) .                                                                                                                      |
| TotalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.                 |


### <a name="insight-values"></a>Insight-Werte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | Beschreibung                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | Die Store-ID der app, für die Sie Einblicke Daten abrufen.     |
| insightDate                | string | Das Datum, an dem wir die Änderung in einer bestimmten Metrik identifiziert. Dieses Datum stellt das Ende der Woche, in dem wir eine erhebliche Erhöhung erkannt, oder in einer Metrik im Vergleich zur vorherigen Woche, verringern. |
| Datentyp     | string | Eine der folgenden Zeichenfolgen, die den Bereich für die allgemeine Analysen angibt, den diese Insight beschreibt:<p/><ul><li><strong>Erwerb</strong></li><li><strong>Gesundheit</strong></li><li><strong>Nutzung</strong></li></ul>   |
| insightDetail          | array | Eine oder mehrere [InsightDetail Werte](#insightdetail-values) , die die Details für aktuelle Insight darstellen.    |


### <a name="insightdetail-values"></a>InsightDetail Werte

| Wert               | Typ   | Beschreibung                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | Eine der folgenden Werte, der die Metrik gibt an, die den aktuellen Insight oder die aktuelle Dimension beschreibt, basierend auf dem **DataType** -Wert.<ul><li>Für die **Integrität**ist dieser Wert immer **HitCount**.</li><li>Für den **Kauf**ist dieser Wert immer **AcquisitionQuantity**.</li><li>Für die **Nutzung**kann der Wert eine der folgenden Zeichenfolgen sein:<ul><li><strong>DailyActiveUsers</strong></li><li><strong>EngagementDurationMinutes</strong></li><li><strong>DailyActiveDevices</strong></li><li><strong>DailyNewUsers</strong></li><li><strong>DailySessionCount</strong></li></ul></ul>  |
| SubDimensions         | array |  Ein oder mehrere Objekte, die eine einzelne Metrik für die Einblicke zu beschreiben.   |
| Prozent            | string |  Der Prozentsatz, den die Metrik befindet sich über Ihre gesamte Kundenbasis geändert.  |
| DimensionName           | string |  Der Name des die Metrik befindet sich in der aktuellen Dimension beschrieben. Beispiele sind **EventType**, **Markt**, **Gerätetyp**, **PackageVersion**, **AcquisitionType**, **AgeGroup** und **Geschlecht**.   |
| DimensionValue              | string | Der Wert der Metrik, die in der aktuellen Dimension beschrieben wird. Beispielsweise ist **DimensionName** **EventType**, **DimensionValue** **Absturz** oder **Blockade**möglicherweise.   |
| FactValue     | string | Den absoluten Wert der Metrik auf das Datum, an das die Einblicke erkannt wurde.  |
| Richtung | string |  Die Richtung der Änderung (**positiv** oder **negativ**).   |
| Date              | String |  Das Datum, an dem wir die Änderung im Zusammenhang mit der aktuellen Insight oder die aktuelle Dimension identifiziert.   |

### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "insightDate": "2018-06-03T00:00:00",
      "dataType": "health",
      "insightDetail": [
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "21",
              "DimensionValue:": "DE",
              "FactValue": "109",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "crash",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "71",
              "DimensionValue:": "JP",
              "FactValue": "112",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "hang",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
      ],
      "insightId": "9CY0F3VBT1AS942AFQaeyO0k2zUKfyOhrOHc0036Iwc="
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Geschäftsinformationen-Bericht](../publish/insights-report.md)
* [Zugreifen auf Analysedaten mit MicrosoftStore-Diensten](access-analytics-data-using-windows-store-services.md)