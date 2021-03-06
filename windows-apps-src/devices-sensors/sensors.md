---
ms.assetid: 415F4107-0612-4235-9722-0F5E4E26F957
title: Sensoren
description: Mithilfe von Sensoren können Apps die Beziehung zwischen einem Gerät und der physischen Umgebung ermitteln. Sensoren können für die App die Richtung, Ausrichtung und Bewegung des Geräts erfassen.
ms.date: 06/06/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8c76893dd56c2c3d207444b5c6331c0cd4445ed1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159654"
---
# <a name="sensors"></a>Sensoren



Mithilfe von Sensoren können Apps die Beziehung zwischen einem Gerät und der physischen Umgebung ermitteln. Sensoren können für die App die Richtung, Ausrichtung und Bewegung des Geräts erfassen. Diese Sensoren können Ihre Spiel-, Augmented Reality- oder Dienstprogramm-App hilfreicher und interaktiver machen, indem sie eine spezielle Eingabeform bereitstellen. So können z. B. durch Bewegen des Geräts die Zeichen auf dem Bildschirm angepasst werden, oder das Gerät kann als virtuelles Lenkrad verwendet werden.

Im Allgemeinen sollten Sie im Vorfeld entscheiden, ob die App ausschließlich auf Sensorsteuerung beruhen soll oder ob mit den Sensoren lediglich ein zusätzlicher Steuermechanismus bereitgestellt wird. Ein Rennspiel, bei dem ein Gerät als virtuelles Lenkrad genutzt wird, kann beispielsweise auch über eine GUI auf dem Bildschirm gesteuert werden. Die App funktioniert dann unabhängig von den Sensoren, die für das System zur Verfügung stehen. Andererseits könnte ein Murmel-Kipplabyrinth ausschließlich für die Funktion mit Systemen mit entsprechenden Sensoren geschrieben werden. Sie müssen die strategische Entscheidung treffen, ob ausschließlich Sensoren verwendet werden sollen. Dabei ist zu beachten, dass ein Ansatz mit Steuerung per Maus oder Touchfunktion eine bessere Kontrolle ermöglicht, was jedoch zu Lasten des Immersionseffekts geht.

| Thema                                                       | BESCHREIBUNG  |
|-------------------------------------------------------------|--------------|
| [Kalibrieren von Sensoren](calibrate-sensors.md)                   | Für Sensoren in einem auf dem Magnetometer – Kompass, Neigungsmesser und Ausrichtungssensor – basierenden Gerät kann aufgrund von Umweltfaktoren eine Kalibrierung erforderlich sein. Falls für Ihr Gerät eine Kalibrierung erforderlich ist, kann die [<strong>MagnetometerAccuracy</strong>](/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy)-Aufzählung helfen, die nächsten Schritte festzulegen. |
| [Sensorausrichtung](sensor-orientation.md)                 | Sensordaten der [<strong>OrientationSensor</strong>](/uwp/api/Windows.Devices.Sensors.OrientationSensor)-Klassen sind durch ihre Referenzachsen definiert. Diese Achsen werden durch das Querformat des Geräts bestimmt und drehen sich mit dem Gerät, wenn es vom Benutzer gedreht wird. |
| [Verwenden des Beschleunigungsmessers](use-the-accelerometer.md)           | Hier erfahren Sie, wie Sie mithilfe des Beschleunigungsmessers auf Benutzerbewegungen reagieren können. |
| [Verwenden des Kompasses](use-the-compass.md)                       | Hier erfahren Sie, wie Sie mithilfe des Kompasses die aktuelle Richtung ermitteln. |
| [Verwenden des Gyrometers](use-the-gyrometer.md)                   | Hier erfahren Sie, wie Sie mithilfe des Gyrometers Bewegungsänderungen des Benutzers erkennen. | 
| [Verwenden des Neigungsmessers](use-the-inclinometer.md)             | Hier erfahren Sie, wie Sie mithilfe des Neigungsmessers Werte für Nick-, Roll- und Gierwinkel ermitteln. |
| [Verwenden des Lichtsensors](use-the-light-sensor.md)             | Hier erfahren Sie, wie Sie mithilfe des Umgebungslichtsensors veränderte Lichtverhältnisse erkennen. |
| [Verwenden des Ausrichtungssensors](use-the-orientation-sensor.md) | Hier erfahren Sie, wie Sie mithilfe der Ausrichtungssensoren die Ausrichtung des Geräts ermitteln.|

## <a name="sensor-batching"></a>Sensorbatchverarbeitung

Einige Sensoren unterstützen das Konzept der Batchverarbeitung. Dies variiert je nach verfügbaren Sensoren. Wenn ein Sensor die Batchverarbeitung implementiert, sammelt er mehrere Datenpunkte in einem bestimmten Zeitintervall und überträgt dann alle Daten auf einmal. Dies unterscheidet sich vom normalen Verhalten, bei dem ein Sensor die Ergebnisse sofort beim Erfassen meldet. Im folgenden Diagramm sehen Sie, wie Daten erfasst und anschließend übermittelt werden. Zuerst ist die normale Übermittlung und anschließend die Batchübermittlung dargestellt.

![Sensorbatchsammlung](images/batchsample.png)

Der wichtigste Vorteil der Sensorbatchverarbeitung besteht in der längeren Akkulaufzeit. Wenn die Daten nicht sofort gesendet werden, wird dadurch Prozessorleistung gespart und die umgehende Verarbeitung der Daten verhindert. Teile des Systems können deaktiviert werden, bis sie benötigt werden. Dadurch wird Energie gespart.

Sie können durch Anpassen der Wartezeit beeinflussen, wie oft der Sensor Batches sendet. Beispiel: Der [**Beschleunigungsmesser**](/uwp/api/Windows.Devices.Sensors.Accelerometer)-Sensor verfügt über die [**ReportLatency**](/uwp/api/windows.devices.sensors.accelerometer.reportlatency)-Eigenschaft. Wenn diese Eigenschaft für eine Anwendung festgelegt ist, sendet der Sensor Daten nach Ablauf der angegebenen Zeit. Sie können mithilfe der [**ReportInterval**](/uwp/api/windows.devices.sensors.accelerometer.reportinterval)-Eigenschaft steuern, wie viele Daten über einen bestimmten Zeitraum gesammelt werden.

Beim Festlegen der Wartezeit müssen mehrere Punkte berücksichtigt werden. Beispielsweise gilt für jeden Sensor eine [**MaxBatchSize**](/uwp/api/windows.devices.sensors.accelerometer.maxbatchsize), die basierend auf dem Sensor unterstützt wird. Dabei handelt es sich um die Anzahl der Ereignisse, die der Sensor zwischenspeichern kann, bevor er gezwungen ist, sie zu senden. Wenn Sie **MaxBatchSize** mit [**ReportInterval**](/uwp/api/windows.devices.sensors.accelerometer.reportinterval) multiplizieren, ergibt das den Höchstwert für [**ReportLatency**](/uwp/api/windows.devices.sensors.accelerometer.reportlatency). Wenn Sie einen höheren Wert als diesen angeben, wird die maximale Wartezeit verwendet, damit keine Daten verloren gehen. Darüber hinaus können mehrere Anwendungen eine gewünschte Wartezeit festlegen. Um die Anforderungen aller Anwendungen zu erfüllen, wird die kürzeste Wartezeit verwendet. Aufgrund dieser Tatsachen kann die in Ihrer Anwendung festgelegte Wartezeit von der beobachteten Wartezeit abweichen.

Falls ein Sensor die Batchberichterstellung verwendet, wird durch Aufruf von [**GetCurrentReading**](/uwp/api/windows.devices.sensors.accelerometer.getcurrentreading) der aktuelle Datenbatch gelöscht und eine neue Wartezeit gestartet.

## <a name="accelerometer"></a>Beschleunigungsmesser

Mit dem [**Beschleunigungsmesser**](/uwp/api/Windows.Devices.Sensors.Accelerometer) -Sensor werden Schwerkraftwerte entlang der X-, Y- und Z-Achse des Geräts gemessen. Er eignet sich gut für einfache Anwendungen, die auf Bewegung basieren. Beachten Sie, dass für die Beschleunigungskraftwerte die Schwerkraft berücksichtigt wird. Wenn das Gerät für das **FaceUp**-Element den Wert [**SimpleOrientation**](/uwp/api/Windows.Devices.Sensors.SimpleOrientation) auf einem Tisch aufweist, ermittelt der Beschleunigungsmesser für die Z-Achse den Wert -1 G. Mit Beschleunigungsmessern wird also nicht unbedingt nur die Koordinatenbeschleunigung (Veränderung der Geschwindigkeit) gemessen. Bei Verwendung eines Beschleunigungsmessers sollten Sie zwischen dem schwerkraftbezogenen Vektor der Schwerkraft und dem linearen Beschleunigungsvektor für die Bewegung unterscheiden. Beachten Sie, dass der Gravitationsvektor für ein stationäres Gerät auf den Wert 1 normalisiert werden sollte.

Die folgenden Diagramme veranschaulichen Folgendes:

-   V1 = Vektor 1 = Anziehungskraft aufgrund der Gravitation
-   V2 = Vektor 2 = -Z-Achse der Geräte-Chassis (zeigt aus der Bildschirmrückseite)
-   Θi = Neigungswinkel (Neigung) = Winkel zwischen –Z-Achse des Geräte-Chassis und Schwerkraftvektor

![Beschleunigungsmesser](images/accelerometer1.png)![Beschleunigungsmessung](images/accelerometer2.png)

Zu den Apps, für die der Beschleunigungsmessersensor verwendet werden kann, zählt beispielsweise ein Spiel, bei dem Sie durch Kippen des Geräts eine Murmel bewegen (Schwerkraftvektor). Die Funktionalität entspricht in etwa der [**Inclinometer**](/uwp/api/Windows.Devices.Sensors.Inclinometer)-Funktionalität und kann auch mit dem Sensor mithilfe einer Kombination aus Nick- und Rollwinkel erzielt werden. Die Nutzung des Schwerkraftvektors eines Beschleunigungsmessers vereinfacht dies, da ein Vektor für das Kippen des Geräts vorhanden ist, der auf einfache Weise mathematisch manipuliert werden kann. Ein anderes Beispiel ist eine App, die das Geräusch eines Peitschenschlags generiert, wenn der Benutzer das Gerät schnell durch die Luft bewegt (linearer Beschleunigungsvektor).

Eine Beispiel Implementierung finden Sie im [Beschleunigungsmesser](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Accelerometer)Beispiel.

## <a name="activity-sensor"></a>Aktivitätssensor

Der [**Activity**](/uwp/api/Windows.Devices.Sensors.ActivitySensor)-Sensor ermittelt den aktuellen Status des an den Sensor angeschlossenen Geräts. Dieser Sensor wird häufig in Fitness-Apps verwendet, um zu verfolgen, wenn ein Benutzer mit einem Gerät läuft oder geht. Eine Liste der möglichen Aktivitäten, die von dieser Sensor-API erkannt werden kann, finden Sie unter [**ActivityType**](/uwp/api/Windows.Devices.Sensors.ActivityType).

Eine Beispiel Implementierung finden Sie unter dem Beispiel für den [Aktivitätssensor](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ActivitySensor).

## <a name="altimeter"></a>Höhenmesser

Mit dem [**Höhenmesser**](/uwp/api/Windows.Devices.Sensors.Altimeter) -Sensor gibt einen Wert zurück, der die Höhe des Sensors angibt. Dadurch können Sie Änderungen der Höhe in Metern über dem Meeresspiegel nachverfolgen. Ein Beispiel für eine App, die dies nutzen kann, ist eine Lauf-App, die Höhenänderungen verfolgt, um den Kalorienverbrauch zu berechnen. In diesem Fall können diese Sensordaten mit dem [**Activity**](/uwp/api/Windows.Devices.Sensors.ActivitySensor)-Sensor kombiniert werden, um genauere Informationen bereitzustellen.

Ein Beispiel für eine Implementierung finden Sie unter dem Beispiel für einen [Höhenmeter](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Altimeter).

## <a name="barometer"></a>Barometer

Mit dem [**Barometer**](/uwp/api/Windows.Devices.Sensors.Barometer) -Sensor ermöglicht einer Anwendung das Abrufen barometrischer Messwerte. Eine Wetter-App kann diese Informationen verwenden, um den aktuellen Luftdruck anzugeben. Dies kann zur Bereitstellung ausführlicher Informationen und zur Vorhersage potenzieller Wetteränderungen genutzt werden.

Eine Beispiel Implementierung finden Sie im Beispiel für das [Barometer](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Barometer).

## <a name="compass"></a>Kompass

Mit dem [**Kompass**](/uwp/api/Windows.Devices.Sensors.Compass) -Sensor gibt eine 2D-Richtung unter Berücksichtigung des magnetischen Nordpols auf Grundlage der horizontalen Ebene der Erde zurück. Der Kompasssensor sollte nicht für die Bestimmung spezifischer Geräteausrichtung oder die Darstellung von Objekten im 3D-Raum verwendet werden. Geografische Objekte können eine natürliche Abweichung der Richtung verursachen, daher unterstützen einige Systeme sowohl [**HeadingMagneticNorth**](/uwp/api/windows.devices.sensors.compassreading.headingmagneticnorth) als auch [**HeadingTrueNorth**](/uwp/api/windows.devices.sensors.compassreading.headingtruenorth). Entscheiden Sie, welcher Pol von der App verwendet werden soll, aber bedenken Sie, dass nicht alle Systeme den korrekten Nordwert ausgeben. Gyrometer- und Magnetometer (ein Gerät zur Messung der magnetischen Stärke)-Sensoren vereinen ihre Daten, um die Kompassrichtung zu bestimmen, wodurch die Daten stabilisiert werden (Die magnetische Feldstärke ist aufgrund der Komponenten elektrischer Systeme sehr instabil).

![Kompasswerte im Hinblick auf den magnetischen Nordpol](images/compass.png)

Für Apps, in denen eine Kompassrose angezeigt oder nach einer Karte navigiert werden soll, wird dafür in der Regel der Kompasssensor eingesetzt.

Eine Beispiel Implementierung finden Sie im Beispiel für das [Kompass](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Compass).

## <a name="gyrometer"></a>Gyrometer

Mit dem [**Gyrometer**](/uwp/api/Windows.Devices.Sensors.Gyrometer) -Sensor misst Winkelgeschwindigkeiten entlang der X-, Y- und Z-Achse. Dies ist sehr nützlich bei einfachen bewegungsabhängigen Apps, bei denen es nicht um die Ausrichtung des Geräts geht, sondern darum, mit welchen unterschiedlichen Geschwindigkeiten sich das Gerät dreht. Die Leistung von Gyrometern kann durch ein Rauschen in den Daten oder einen systematischen Fehler entlang einer oder mehrerer Achsen beeinträchtigt werden. Sie sollten den Beschleunigungsmesser abfragen, um zu überprüfen, ob sich das Gerät bewegt. So können Sie ermitteln, ob für das Gyrometer ein Fehler vorliegt, und dies in der App dann entsprechend berücksichtigen.

![Gyrometer mit Neigen, Rollen und Schwenken](images/gyrometer.png)

Ein Beispiel für eine App, in der der Gyrometersensor verwendet werden kann, ist ein Spiel, bei dem ein Rouletterad der Drehbewegung des Geräts entsprechend schnell gedreht wird.

Eine Beispiel Implementierung finden Sie im Beispiel für das [Gyrometer](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Gyrometer)-Beispiel.

## <a name="inclinometer"></a>Neigungsmesser

Mit dem [**Neigungssensor**](/uwp/api/Windows.Devices.Sensors.Inclinometer) -Sensor gibt Gier-, Nick- und Rollwinkel des Geräts an und funktioniert optimal mit Apps, die die Stellung des Geräts im dreidimensionalen Raum berücksichtigen. Neige- und Rollwinkel werden anhand der Daten des Schwerkraftvektors des Beschleunigungsmessers und der Daten des Gyrometers berechnet. Der Schwenkwert wird durch die Daten des Magnetometers und des Gyrometers (entsprechend der Kompassrichtung) bestimmt. Neigungssensoren bieten erweiterte Ausrichtungsdaten auf eine leicht verständliche Art und Weise. Verwenden Sie Neigungsmesser, wenn Sie Geräteausrichtungswerte benötigen, aber die Sensordaten nicht verarbeiten möchten.

![Neigungsmesser mit Neigungs-, Roll- und Schwenkdaten](images/inclinometer.png)

Für Apps, bei denen die Ansicht der Ausrichtung des Geräts entsprechend geändert werden soll, kann der Neigungssensor verwendet werden. Eine App, bei der Nick-, Gier- und Rollwinkel eines Flugzeugs angezeigt werden, würde ebenfalls Neigungsmesserdaten verwenden.

Eine Beispiel Implementierung finden Sie im Beispiel "Neigungsmesser" [https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer) .

## <a name="light-sensor"></a>Belichtungssensor

Der [**Light**](/uwp/api/Windows.Devices.Sensors.LightSensor)-Sensor kann das Umgebungslicht bestimmen, in dem sich der Sensor befindet. Dadurch kann die App ermitteln, wann sich die Einstellung des Umgebunglichts des Geräts geändert hat. Angenommen, ein Benutzer mit einem Slate-Gerät geht an einem sonnigen Tag aus einem geschlossenen Raum nach draußen. Eine intelligente Anwendung könnte diesen Wert verwenden, um den Kontrast zwischen dem Hintergrund und der dargestellten Schriftart zu erhöhen. Dadurch wäre der Inhalt auch in der helleren Umgebung draußen noch lesbar.

Eine Beispiel Implementierung finden Sie im Beispiel [Light Sensor](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LightSensor).

## <a name="orientation-sensor"></a>Ausrichtungssensor

Die Geräteausrichtung wird durch die Quaternionen- und Drehungsmatrizes ausgedrückt. Der [**OrientationSensor**](/uwp/api/Windows.Devices.Sensors.OrientationSensor) bietet einen hohen Genauigkeitsgrad bei der Bestimmung der Position des Geräts im dreidimensionalen Raum unter Berücksichtigung der absoluten Richtung. Die **OrientationSensor**-Daten werden vom Beschleunigungsmesser, Gyrometer und Magnetometer abgeleitet. Auch die Neigungsmesser- und die Kompasssensorendaten können von den Quaternionenwerten abgeleitet werden. Quaternion- und Rotationsmatrizes sind für erweiterte mathematische Manipulationen geeignet und werden oft bei der grafischen Programmierung verwendet. Für Apps, die komplexe Manipulation verwenden, sollten die Ausrichtungssensoren verwendet werden, da viele Transformationen auf Quaternion- und Rotationsmatrizes beruhen.

![Ausrichtungssensordaten](images/orientation-sensor.png)

Der Ausrichtungssensor wird häufig in Augmented Reality-Apps verwendet, in denen auf Grundlage der Ausrichtung der Geräterückseite das Bild der Umgebung mit einer Grafik überlagert wird.

Eine Beispiel Implementierung finden Sie im Beispiel für den [Orientation-Sensor](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/OrientationSensor).

## <a name="pedometer"></a>Schrittzähler

Mit dem [**Schrittzähler**](/uwp/api/Windows.Devices.Sensors.Pedometer) -Sensor verfolgt die Anzahl der Schritte des Benutzers, der das verbundene Gerät trägt. Der Sensor ist so konfiguriert, dass die Anzahl der Schritte über einen bestimmten Zeitraum hinweg verfolgt wird. Einige Fitness-Apps verfolgen die Anzahl der ausgeführten Schritte, um dem Benutzer das Festlegen und Erreichen verschiedener Ziele zu erleichtern. Diese Informationen können dann erfasst und gespeichert werden, um den Fortschritt über einen Zeitraum anzuzeigen.

Ein Beispiel für eine Implementierung finden Sie im Beispiel für das Beispiel " [Peer Domain](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Pedometer)".

## <a name="proximity-sensor"></a>Näherungssensor

Der [**Proximity**](/uwp/api/Windows.Devices.Sensors.ProximitySensor)-Sensor kann verwendet werden, um anzugeben, ob Objekte vom Sensor erkannt werden. Neben der Bestimmung, ob sich ein Objekt innerhalb des Erkennungsbereichs des Geräts befindet, kann der Näherungssensor auch den Abstand zum erkannten Objekt ermitteln. Ein Verwendungsbeispiel wäre eine Anwendung, die aus einem Standbyzustand aktiviert werden soll, wenn sich ein Benutzer bis zu einer bestimmten Entfernung nähert. Das Gerät könnte sich in einem Ruhezustand befinden, bis der Näherungssensor ein Objekt erkennt, woraufhin es in einen aktiveren Zustand versetzt wird.

Eine Beispiel Implementierung finden Sie im Beispiel für den [near-Sensor](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ProximitySensor).

## <a name="simple-orientation"></a>Einfache Ausrichtung

Der [**SimpleOrientationSensor**](/uwp/api/windows.devices.sensors.simpleorientationsensor) erkennt die aktuelle Quadrantenausrichtung des angegebenen Geräts, bzw. ob die Oberseite nach oben oder unten zeigt. Er verfügt über sechs mögliche [**SimpleOrientation**](/uwp/api/Windows.Devices.Sensors.SimpleOrientation)-Zustände (**NotRotated**, **Rotated90**, **Rotated180**, **Rotated270**, **FaceUp**, **FaceDown**).

Eine Reader-App, bei der die Anzeige abhängig von der Ausrichtung des Geräts in Relation zum Boden geändert wird, würde die Ausrichtung des Geräts anhand von SimpleOrientationSensor-Werten bestimmen.

Eine Beispiel Implementierung finden Sie im Beispiel [Simple Orientation Sensor](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleOrientationSensor).