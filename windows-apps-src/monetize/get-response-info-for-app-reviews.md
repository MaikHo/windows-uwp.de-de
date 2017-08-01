---
author: mcleanbyron
ms.assetid: fb6bb856-7a1b-4312-a602-f500646a3119
description: "Verwenden Sie diese Methode der Windows Store-API für Rezensionen, um zu bestimmen, ob Sie auf eine bestimmte Rezension für eine bestimmte App oder auf alle Rezensionen für diese App antworten können."
title: "Antwortinformationen für Rezensionen abrufen"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows10, UWP, Store-Dienste, Windows Store-API für Rezensionen, Antwortinformationen"
ms.openlocfilehash: 46f5cb04fcea1b06205999743205396a875ebbce
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="get-response-info-for-reviews"></a>Antwortinformationen für Rezensionen abrufen

Wenn Sie programmgesteuert auf eine Kundenrezension zu Ihrer App antworten möchten, verwenden Sie diese Methode der Windows Store-API für Rezensionen, um zunächst zu ermitteln, ob Sie berechtigt sind, auf die Rezension zu antworten. Sie können nicht auf Rezensionen von Kunden antworten, die keine Antwort auf ihre Rezension erhalten möchten. Nachdem sichergestellt ist, dass Sie auf eine Rezension antworten können, verwenden Sie die Methode [Antworten auf App-Rezensionen senden](submit-responses-to-app-reviews.md), um programmgesteuert zu antworten.


## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](respond-to-reviews-using-windows-store-services.md#prerequisites) für die Windows Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Rufen Sie die ID der Rezension ab, für die Sie überprüfen möchten, ob Sie darauf antworten können. Rezensions-IDs finden Sie in den Antwortdaten der Methode [Abrufen von App-Rezensionen](get-app-reviews.md) der Windows Store-Analyse-API und unter [Offlinedownload](../publish/download-analytic-reports.md) im Bericht [Rezensionen](../publish/reviews-report.md).

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/{reviewId}/apps/{applicationId}/responses/info``` |

<span/> 

### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/> 

### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   | Beschreibung                                     |  Erforderlich  |
|---------------|--------|--------------------------------------------------|--------------|
| applicationId | string | Die Store-ID der App, welche die Rezension enthält, für die Sie bestimmen möchten, ob Sie antworten können. Die Store-ID ist auf der [Seite mit der App-Identität](../publish/view-app-identity-details.md) des DevCenter-Dashboards verfügbar. Beispiel für eine Store-ID: 9WZDNCRFJ3Q8. |  Ja  |
| reviewId | string | Die ID der Rezension, auf die Sie antworten möchten (dies ist eine GUID). Rezensions-IDs finden Sie in den Antwortdaten der Methode [Abrufen von App-Rezensionen](get-app-reviews.md) der Windows Store-Analyse-API und unter [Offlinedownload](../publish/download-analytic-reports.md) im Bericht [Rezensionen](../publish/reviews-report.md). <br/>Wenn Sie diesen Parameter nicht angeben, wird im Antworttext für diese Methode stehen, ob Sie über Berechtigungen zum Beantworten von Rezensionen für die angegebene App verfügen. |  Nein  |

<span/>

### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt, wie Sie diese Methode verwenden, um festzustellen, ob auf eine bestimmte Rezension antworten können.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/reviews/6be543ff-1c9c-4534-aced-af8b4fbe0316/apps/9WZDNCRFJ3Q8/responses/info HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung    |  
|------------|--------|-----------------------|
| CanRespond      | Boolean  | Der Wert **true** gibt an, dass Sie auf die angegebene Rezension antworten können oder dass Sie berechtigt sind, auf eine beliebige Rezensionen für die angegebene App zu antworten. Andernfalls ist dieser Wert **false**.       |
| DefaultSupportEmail  | string |  Die von Ihrer App [unterstütze E-Mail-Adresse](../publish/create-app-store-listings.md#support-contact-info) finden Sie im Store-Eintrag Ihrer App. Wenn Sie keine unterstützte E-Mail-Adresse angegeben haben, ist dieses Feld leer.    |

<span/>
 
### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "CanRespond": true,
  "DefaultSupportEmail": "support@contoso.com"
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Übermitteln von Antworten auf Rezensionen mithilfe der Windows Store-Analyse-API](submit-responses-to-app-reviews.md)
* [Reagieren Sie auf Kundenrezensionen über das Dev Center-Dashboard](../publish/respond-to-customer-reviews.md)
* [Antworten auf Rezensionen mithilfe von Windows Store-Diensten](respond-to-reviews-using-windows-store-services.md)
* [Abrufen von App-Rezensionen](get-app-reviews.md)