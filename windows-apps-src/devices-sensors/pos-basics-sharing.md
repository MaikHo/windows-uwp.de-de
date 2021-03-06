---
title: Geräte Freigabe des pointservice-Dienstanbieter
description: Erfahren Sie, wie Sie Netzwerk-oder Bluetooth-verbundene Peripheriegeräte für andere Computer in einer Umgebung freigeben, in der mehrere PCs auf freigegebenen Peripheriegeräten basieren.
ms.date: 06/14/2018
ms.topic: article
keywords: Windows 10, UWP, Point of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 4fad9bc75ed0ff79be1596a3c99445c7e9f97b1f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163264"
---
# <a name="pointofservice-device-sharing"></a>Geräte Freigabe des pointservice-Dienstanbieter

Erfahren Sie, wie Sie Netzwerk-oder Bluetooth-verbundene Peripheriegeräte für andere Computer in einer Umgebung freigeben, in der mehrere PCs auf freigegebenen Peripheriegeräten anstatt auf dedizierte Peripheriegeräte basieren, die an jeden Computer angeschlossen sind.

## <a name="device-sharing"></a>Geräte Freigabe

Netzwerk-und Bluetooth-verbundene pointservice-Peripheriegeräte werden in der Regel in Umgebungen verwendet, in denen mehrere Client Geräte die gleichen Peripheriegeräte im ganzen Tag gemeinsam nutzen.  In einer ausgelasteten Einzelhandelsumgebung oder in einer lebensmittelumgebung wirkt sich jede Verzögerung der Fähigkeit eines Client Geräts an eine Peripherie auf die Effizienz aus, bei der ein Mitarbeiter eine Transaktion mit dem Kunden schließen und mit der nächsten fortfahren kann. In einem Quick Service Restaurant-Szenario, in dem ein Empfangsdrucker als Küchen Drucker verwendet wird, um die Details der Bestellung eines Kunden zur Vorbereitung an die Küche zu übertragen, werden mehrere Client Geräte Bestellungen von Kunden übernehmen.  Sobald die Bestellung fertiggestellt ist, sollte jedes Client Gerät in der Lage sein, den freigegebenen Drucker zu beanspruchen und die Bestellung für die Küche sofort auszugeben.

In diesen Umgebungen ist es wichtig, dass die Anwendung das Geräte **Objekt vollständig** freigibt, damit ein anderes das gleiche Gerät beanspruchen kann.

Verwerfen eines posprinter am Ende eines using-Blocks

```Csharp 
using Windows.Devices.PointOfService;
using(PosPrinter printer = await PosPrinter.FromIdAsync("Device ID"))
{
    if (printer != null)
    {
        // Exercise the printer.
    }

    // When leaving this scope, printer.Dispose() is automatically invoked, 
    // releasing the session we have with the printer.
}
```


Löschen eines posprinter durch explizites Aufrufen von "verwerfen ()"

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>Verwendete API-Methoden 

+ [Barcodescanner. verwerfen](/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [Cashschublade. verwerfen](/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay. verwerfen](/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ ["Magneticstripereader. verwerfen"](/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [Posprinter. verwerfen](/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]