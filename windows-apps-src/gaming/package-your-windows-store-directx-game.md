---
title: Packen Ihres UWP-DirectX-Spiels (DirectX-Spiels für die universelle Windows-Plattform)
description: Umfangreichere UWP-Spiele (Universelle Windows-Plattform) können leicht relativ groß werden. Dies gilt besonders für Spiele, bei denen mehrere Sprachen mit regionsspezifischen Ressourcen unterstützt werden oder die über optionale HD-Ressourcen verfügen.
ms.assetid: 68254203-c43c-684f-010a-9cfa13a32a77
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, DirectX, Package
ms.localizationpriority: medium
ms.openlocfilehash: 8a1e255a5219e39d866915bce25778dc8f1dfdba
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175244"
---
#  <a name="package-your-universal-windows-platform-uwp-directx-game"></a>Packen Ihres UWP-DirectX-Spiels (DirectX-Spiels für die universelle Windows-Plattform)

Umfangreichere UWP-Spiele (Universelle Windows-Plattform) können leicht relativ groß werden. Dies gilt besonders für Spiele, bei denen mehrere Sprachen mit regionsspezifischen Ressourcen unterstützt werden oder die über optionale HD-Ressourcen verfügen. In diesem Thema erfahren Sie, wie Sie App-Pakete und App-Bündel zum Anpassen der App verwenden, damit Kunden nur die wirklich benötigten Ressourcen erhalten.

Zusätzlich zum App-Paketmodell bietet Windows 10 Unterstützung für App-Bündel, bei denen zwei Arten von Paketen gruppiert werden:

-   App-Pakete enthalten plattformspezifische ausführbare Dateien und Bibliotheken. Häufig verfügen UWP-Spiele über bis zu drei App-Pakete, und zwar jeweils ein Paket für die x86-, x64- und ARM-CPU-Architekturen. Der Code und die Daten für die entsprechende Hardwareplattform müssen im dazugehörigen App-Paket enthalten sein. Ein App-Paket sollte außerdem alle wichtigen Ressourcen für das Spiel enthalten, damit die Voraussetzungen für eine Ausführung in guter Qualität und mit guter Leistung gegeben sind.
-   Ressourcenpakete enthalten optionale oder erweiterte plattformagnostische Daten, z. B. Spielressourcen (Texturen, Gitter, Sound, Text). Ein UWP-Spiel kann über ein oder mehrere Ressourcenpakete verfügen, z. B. Ressourcenpakete für HD-Ressourcen oder -Texturen, Ressourcen der DirectX-Featureebene 11+ oder sprachspezifische Ressourcen.

Weitere Informationen zu App-Bündeln und App-Paketen finden Sie unter [Definieren von App-Ressourcen](/previous-versions/windows/apps/hh965321(v=win.10)).

Sie können zwar alle Inhalte in Ihre App-Pakete integrieren, aber diese Vorgehensweise ist ineffizient und redundant. Warum soll eine große Texturdatei für jede Plattform einzeln vorhanden sein (also dreimal), wenn sie für ARM-Plattformen meist gar nicht genutzt wird? Eine gute Zielsetzung besteht darin, die von Kunden herunterzuladende Datenmenge möglichst gering zu halten. Die Kunden können dann schneller mit dem Spielen beginnen, Speicherplatz auf dem Gerät sparen und potenzielle Kosten für Bandbreite in getakteten Netzwerken vermeiden.

Bei der Nutzung dieses Features des UWP-App-Installers ist es wichtig, zu einem frühen Zeitpunkt des Entwicklungsprozesses das Verzeichnislayout und die Benennungskonventionen für Dateien zu beachten. So ermöglichen Sie Tools und Quellen die korrekte Ausgabe und somit einen einfachen Verpackungsvorgang. Befolgen Sie die in diesem Dokument beschriebenen Regeln, wenn Sie Spiele entwickeln, die Ressourcenentwicklung konfigurieren, Tools und Skripts verwalten und Code erstellen, mit dem Ressourcen geladen werden oder darauf verwiesen wird.

## <a name="why-create-resource-packs"></a>Warum sollten Ressourcenpakete erstellt werden?


Beim Erstellen einer App – besonders einer Spiele-App, die in vielen Regionen bzw. für viele verschiedene UWP-Hardwareplattformen verkauft werden kann – müssen Sie viele Dateien häufig in mehreren Versionen bereitstellen, um die Unterstützung für diese Gebietsschemas und Plattformen sicherzustellen. Wenn Sie das Spiel beispielsweise sowohl in den USA als auch in Japan veröffentlichen, benötigen Sie ggf. einen Satz Sprachdateien in Englisch für das Gebietsschema "en-us" und einen zweiten Satz in Japanisch für "jp-jp". Falls Sie in Ihrem Spiel ein Bild für ARM-Geräte sowie für x86- und x64-Plattformen verwenden möchten, müssen Sie dieselbe Bildressourcen einmal für jede CPU-Architektur hochladen, also insgesamt dreimal.

Falls im Spiel viele HD-Ressourcen enthalten sind, die für Plattformen mit niedrigeren DirectX-Featureebenen nicht geeignet sind, muss die folgende Frage gestellt werden: Warum sollen diese Ressourcen in das Hauptpaket der App eingebunden werden, sodass Benutzer eine große Menge von Komponenten herunterladen müssen, die vom Gerät gar nicht genutzt werden können? Das Abtrennen dieser HD-Ressourcen als optionales Ressourcenpaket bedeutet, dass Kunden mit Geräten, von denen diese HD-Ressourcen unterstützt werden, Zugriff darauf haben (und bei getakteter Netzwerkbandbreite möglicherweise zusätzliche Kosten anfallen). Kunden, die nicht über diese Geräte verfügen, erhalten das Spiel dann schneller und zu geringeren Netzwerkkosten.

Inhaltskandidaten für Pakete mit Spielressourcen sind:

-   Ressourcen für internationale Gebietsschemas (lokalisierter Text, Audiodaten oder Bilder)
-   Hochaufgelöste Ressourcen für unterschiedliche Geräteskalierungsfaktoren (1,0x, 1,4x und 1,8x)
-   HD-Ressourcen für höhere DirectX-Featureebenen (9, 10 und 11)

All diese Ressourcen werden im "package.appxmanifest" definiert, das Bestandteil des UWP-Projekts ist, sowie in der Verzeichnisstruktur des fertigen Pakets. Wenn Sie sich an den in diesem Dokument beschriebenen Prozess halten, ist aufgrund der neuen Visual Studio-UI normalerweise keine manuelle Bearbeitung erforderlich.

> **Wichtig**    Das Laden und die Verwaltung dieser Ressourcen erfolgt über die **Windows. applicationmodel. Resources** - \* APIs. Wenn Sie diese App-Modellressourcen-APIs zum Laden der richtigen Datei für ein Gebietsschema, einen Skalierungsfaktor oder eine DirectX-Featureebene verwenden, müssen Sie die Ressourcen nicht mithilfe expliziter Dateipfade laden. Stattdessen stellen Sie für die Ressourcen-APIs nur den generalisierten Dateinamen der gewünschten Ressource bereit und überlassen es dem Ressourcenverwaltungssystem, die richtige Variante der Ressource für die aktuelle Plattform und Gebietsschemakonfiguration (direkte Angabe ebenfalls mit diesen APIs) des Benutzers abzurufen.

 

Es gibt zwei grundlegende Möglichkeiten, Ressourcen für das Verpacken anzugeben:

-   Objektdateien weisen den gleichen Dateinamen auf, und die einzelnen Ressourcenpaketversionen werden in Verzeichnisse mit bestimmten Namen eingefügt. Diese Verzeichnisnamen werden vom System reserviert. Beispielsweise " \\ en-US", " \\ Scale-140", " \\ dxfl-DX11".
-   Objektdateien werden in Ordnern mit beliebigen Namen gespeichert. Die Dateien werden jedoch mit einer gemeinsamen Bezeichnung benannt, die mit Zeichenfolgen angehängt wird, die das System zum Angeben von Sprach- und anderen Qualifizierern reserviert. Insbesondere werden die Qualifiziererzeichenfolgen nach einem Unterstrich ("") dem verallgemeinerten Dateinamen angehängt \_ . Beispiel: \\ Assets- \\ Menü \_ Option1 \_lang-en-us.png, \\ Menü "Assets" \\ \_ Option1 \_scale-140.png, \\ Assets \\ coolsign \_ dxfl-DX11. DDS. Sie können diese Zeichenfolgen auch kombinieren. Beispielsweise das \\ Assets \\ \_ -Menü Option1 \_ Scale-140 \_lang-en-us.png.
    > **Hinweis**    Wenn ein sprach Qualifizierer in einem Dateinamen anstelle allein eines Verzeichnis namens verwendet wird, muss er <tag> wie unter [Anpassen der Ressourcen für Sprache, Skalierung und andere Qualifizierer](../app-resources/tailor-resources-lang-scale-contrast.md)das Format "lang" aufweisen, z. b. "lang-en-US".

     

Verzeichnisnamen können beim Packen von Ressourcen kombiniert werden, um die Genauigkeit zu erhöhen. Sie dürfen jedoch nicht redundant sein. Beispielsweise \\ ist das Menü "en-US" \\ \_ Option1 \_lang-en-us.png redundant.

Sie können unter einem Ressourcenverzeichnis einen benötigten nicht reservierten Namen eines Unterverzeichnisses angeben, solange die Verzeichnisstruktur in jedem Ressourcenverzeichnis identisch ist. \\Dxfl-DX10 \\ Assets z. b \\ \\ . Texturen coolsign. DDS. Wenn Sie eine Ressource laden oder darauf verweisen, muss der Pfadname generalisiert werden. Dabei müssen alle Qualifizierer für Sprache, Skalierung und DirectX-Featureebene unabhängig davon entfernt werden, ob sie sich in Ordnerknoten oder in den Dateinamen befinden. Um z. b. im Code auf ein Medienobjekt zu verweisen, für das eine der Varianten \\ dxfl-DX10 \\ Assets \\ Texturen \\ coolsign. DDS ist, verwenden Sie \\ Assets \\ Texturen \\ coolsign. DDS. Verwenden Sie zum Verweisen auf ein Medienobjekt mit einem Variant- \\ Bild \\ Hintergrund \_scale-140.png \\ Bilder \\background.png.

In der folgenden Tabelle sind die reservierten Verzeichnisnamen und die per Unterstrich angefügten Dateinamenpräfixe aufgeführt:

| Ressourcentyp                   | Verzeichnisname für Ressourcenpaket                                                                                                                  | Dateinamensuffix für Ressourcenpaket                                                                                                    |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| Lokalisierte Ressourcen             | Alle möglichen Sprachen bzw. Kombinationen aus Sprache und Gebietsschema für Windows 10. (Das Qualifiziererpräfix „lang-“ ist in Ordnernamen nicht erforderlich.) | Ein " \_ ", gefolgt von der Sprache, dem Gebiets Schema oder dem sprach Gebiets Schema Bezeichner. Beispiel: " \_ en", " \_ US" \_ bzw. "en-US". |
| Ressourcen für Skalierungsfaktor        | scale-100, scale-140, scale-180. Gilt für die UI-Skalierungsfaktoren 1,0x, 1,4x bzw. 1,8x.                                     | Ein " \_ ", gefolgt von "Scale-100", "Scale-140" oder "Scale-180".                                                                    |
| Ressourcen für die DirectX-Featureebene | dxfl-dx9, dxfl-dx10 und dxfl-dx11. Gilt für die DirectX-Featureebenen 9, 10 bzw. 11.                                     | Ein " \_ ", gefolgt von "dxfl-DX9", "dxfl-DX10" oder "dxfl-DX11".                                                                     |

 

## <a name="defining-localized-language-resource-packs"></a>Definieren von Paketen mit lokalisierten Sprachressourcen


Gebietsschemaspezifische Dateien werden in Projektverzeichnisse eingefügt, die nach der Sprache benannt sind (z. B. "en").

Gehen Sie beim Konfigurieren der App für die Unterstützung lokalisierter Ressourcen für mehrere Sprachen wie folgt vor:

-   Erstellen Sie ein App-Unterverzeichnis (bzw. eine Dateiversion) für jede Sprache bzw. jedes Gebietsschema, die bzw. das unterstützt werden soll (z. B. en-us, jp-jp, zh-cn oder fr-fr).
-   Fügen Sie während der Entwicklung Kopien ALLER Ressourcen (z. B. lokalisierte Audiodateien, Texturen und Menügrafiken) auch dann in das Unterverzeichnis für die entsprechende Sprache bzw. das Gebietsschema ein, wenn diese sich über die Sprachen/Gebietsschemas hinweg nicht unterscheiden. Stellen Sie mit Blick auf die bestmögliche Benutzerfreundlichkeit sicher, dass Benutzer darauf hingewiesen werden, falls diese ein verfügbares Sprachressourcenpaket für ihr Gebietsschema nicht abgerufen haben (oder es nach dem Herunterladen oder der Installation versehentlich gelöscht haben).
-   Achten Sie darauf, dass die Ressourcen- und Zeichenfolgendateien (.resw) in jedem Verzeichnis gleich benannt sind. Beispielsweise sollte Menü \_option1.png in den \\ Verzeichnissen "en-US" und "JP-JP" denselben Namen haben, \\ auch wenn der Inhalt der Datei für eine andere Sprache gilt. In diesem Fall werden Sie als \\ Menü "en-US" \\ \_option1.png und im \\ JP-JP- \\ Menüoption1.png angezeigt \_ .
    > **Hinweis**    Optional können Sie das Gebiets Schema an den Dateinamen anfügen und im gleichen Verzeichnis speichern. Beispiel: \\ Menü "Assets" \\ \_ Option1 \_lang-en-us.png, \\ Menü "Assets" \\ \_ Option1 \_lang-jp-jp.png.

     

-   Verwenden Sie die APIs in [**Windows.ApplicationModel.Resources**](/uwp/api/Windows.ApplicationModel.Resources) und [**Windows.ApplicationModel.Resources.Core**](/uwp/api/Windows.ApplicationModel.Resources.Core), um die gebietsschemaspezifischen Ressourcen für die App anzugeben und zu laden. Verwenden Sie auch Verweise auf Ressourcen, die kein bestimmtes Gebietsschema enthalten. Diese APIs bestimmen das richtige Gebietsschema basierend auf den Einstellungen des jeweiligen Benutzers und rufen somit die richtige Ressource für den Benutzer ab.
-   Wählen Sie in Microsoft Visual Studio 2015 die Option **PROJEKT -> Store -> Anwendungspaket erstellen...**, und erstellen Sie das Paket.

## <a name="defining-scaling-factor-resource-packs"></a>Definieren von Ressourcenpaketen für den Skalierungsfaktor


Windows 10 verfügt über drei Skalierungsfaktoren für Benutzeroberflächen: 1,0x, 1,4x und 1,8x. Die Skalierungswerte für jede Anzeige werden während der Installation basierend auf verschiedenen kombinierten Faktoren festgelegt: Größe des Bildschirms, Auflösung des Bildschirms und angenommener durchschnittlicher Abstand der Benutzer vom Bildschirm. Der Benutzer kann Skalierungsfaktoren auch anpassen, um die Lesbarkeit zu verbessern. Um die besten Ergebnisse zu erzielen, sollte das Spiel sowohl mit DPI-Werten kompatibel sein als auch den Skalierungsfaktor beachten können. Dies bedeutet, dass Sie Versionen wichtiger Grafikressourcen für alle drei Skalierungsfaktoren erstellen sollten. Dies betrifft auch die Zeigerinteraktion und Treffererkennung!

Konfigurieren Sie eine App, für die Ressourcenpakete für unterschiedliche Skalierungsfaktoren von UWP-Apps unterstützt werden sollen, wie folgt:

-   Erstellen Sie ein Unterverzeichnis (bzw. eine Dateiversion) für jeden zu unterstützenden Skalierungsfaktor (scale-100, scale-140 und scale-180).
-   Fügen Sie während der Entwicklung auf den Skalierungsfaktor zugeschnittene Kopien ALLER Ressourcen in jedes Skalierungsfaktor-Ressourcenverzeichnis ein, auch wenn sich diese für die einzelnen Skalierungsfaktoren nicht unterscheiden.
-   Achten Sie darauf, dass die Ressourcen in jedem Verzeichnis gleich benannt sind. Beispielsweise sollte Menü \_option1.png in den \\ Verzeichnissen "Scale-100" und "Scale-180" denselben Namen haben, \\ auch wenn der Inhalt der Datei anders ist. In diesem Fall werden Sie als Menüoption1.png " \\ Skala-100 \\ " \_ und " \\ Scale-140"- \\ Menü \_option1.png angezeigt.
    > **Hinweis**    Auch hier können Sie den Skalierungsfaktor Suffix an den Dateinamen anfügen und im gleichen Verzeichnis speichern. Beispiel: \\ Menü "Assets" \\ \_ Option1 \_scale-100.png, \\ Menü "Assets" \\ \_ Option1 \_scale-140.png.

     

-   Verwenden Sie die APIs in [**Windows.ApplicationModel.Resources.Core**](/uwp/api/Windows.ApplicationModel.Resources.Core) zum Laden der Ressourcen. Verweise auf Ressourcen sollten generalisiert werden (kein Suffix), wobei die spezifische Skalierungsvariante weggelassen wird. Das System ruft die geeignete Skalierungsressource für die Anzeige und die Einstellungen des Benutzers ab.
-   Wählen Sie in Visual Studio 2015 die Option **PROJEKT -> Store -> Anwendungspaket erstellen...**, und erstellen Sie das Paket.

## <a name="defining-directx-feature-level-resource-packs"></a>Definieren von Ressourcenpaketen für DirectX-Featureebenen


Die DirectX-Featureebenen entsprechen den GPU-Featuresätzen für ältere und aktuelle Versionen von DirectX (speziell Direct3D). Dies beinhaltet Spezifikationen und Funktionen von Shadermodellen, Sprachunterstützung für Shader, Unterstützung für Texturkomprimierung und allgemeine Features der Grafikpipeline.

Für Ihr grundlegendes App-Paket sollten Sie eines der grundlegenden Formate für die Texturkomprimierung verwenden: BC1, BC2 oder BC3. Diese Formate können von allen UWP-Geräten genutzt werden, von normalen ARM-Plattformen bis zu dedizierten Arbeitsstationen mit mehreren GPUs und Mediencomputern.

Es ist ratsam, einem Ressourcenpaket Texturformatunterstützung für DirectX-Featureebene 10 oder höher hinzuzufügen, um lokalen Festplattenspeicher und Downloadbandbreite zu sparen. Dies ermöglicht die Verwendung moderner Komprimierungsschemas für Ebene 11, z. B. BC6H und BC7. (Weitere Informationen finden Sie unter [Texturblockkomprimierung in Direct3D 11](/windows/desktop/direct3d11/texture-block-compression-in-direct3d-11).) Diese Formate sind für die hochaufgelösten Texturressourcen effizienter, die von modernen GPUs unterstützt werden. Zudem verbessern sie auf Highend-Plattformen das Erscheinungsbild, die Leistung und die Speicheranforderungen Ihres Spiels.

| DirectX-Featureebene | Unterstützte Texturkomprimierung |
|-----------------------|-------------------------------|
| 9                     | BC1, BC2, BC3                 |
| 10                    | BC4, BC5                      |
| 11                    | BC6H, BC7                     |

 

Von allen DirectX-Featureebenen werden außerdem unterschiedliche Shadermodellversionen unterstützt. Kompilierte Shaderressourcen können pro Featureebene erstellt werden und in Ressourcenpakete für DirectX-Featureebenen eingebunden werden. In Verbindung mit einigen neueren Shadermodellen können auch Ressourcen genutzt werden, z. B. Normalmaps, die für frühere Versionen von Shadermodellen nicht geeignet sind. Diese für Shadermodelle spezifischen Ressourcen sollten ebenfalls in ein Ressourcenpaket für DirectX-Featureebenen eingefügt werden.

Der Ressourcenmechanismus ist vorrangig auf die für Ressourcen unterstützten Texturformate ausgerichtet, sodass nur die drei allgemeinen Featureebenen unterstützt werden. Wenn Sie separate Shader für untergeordnete Ebenen (Punkt Versionen) wie DX9 \_ 1 vs DX9 \_ 3 benötigen, müssen Sie von Ihrem Asset Management-und Renderingcode explizit behandelt werden.

Gehen Sie wie folgt vor, wenn Sie eine App konfigurieren, für die Ressourcenpakete für unterschiedliche DirectX-Featureebenen unterstützt werden sollen:

-   Erstellen Sie ein Unterverzeichnis (bzw. eine Dateiversion) für jede zu unterstützende DirectX-Featureebene (dxfl-dx9, dxfl-dx10 und dxfl-dx11).
-   Fügen Sie für die Featureebene spezifische Ressourcen während der Entwicklung in jedes Ressourcenverzeichnis für Featureebenen ein. Im Gegensatz zu Gebietsschemas und Skalierungsfaktoren können Sie im Spiel für jede Featureebene unterschiedliche Rendercodeverzweigungen verwenden. Falls Sie über Texturen, kompilierte Shader oder andere Ressourcen verfügen, die nur für eine Featureebene oder eine Teilmenge aller unterstützten Featureebenen verwendet werden, fügen Sie die entsprechenden Ressourcen nur in die Verzeichnisse für die Featureebenen ein, von denen sie genutzt werden. Beachten Sie für Ressourcen, die über alle Featureebenen hinweg geladen werden, dass für jedes Featureebenen-Ressourcenverzeichnis davon eine Version gleichen Namens vorhanden ist. Legen Sie beispielsweise für eine unabhängige Textur auf Featureebene mit dem Namen "coolsign. DDS" die BC3-komprimierte Version im Verzeichnis " \\ dxfl-DX9" und die bzw bc7-komprimierte Version im \\ Verzeichnis "dxfl-DX11" ab.
-   Stellen Sie sicher, dass alle Ressourcen (falls sie für mehrere Featureebenen verfügbar sind) in jedem Verzeichnis denselben Namen haben. Beispielsweise sollte coolsign. DDS in den \\ Verzeichnissen dxfl-DX9 und \\ dxfl-DX11 denselben Namen haben, auch wenn der Inhalt der Datei anders ist. In diesem Fall werden Sie als " \\ dxfl-DX9 \\ coolsign. DDS" und " \\ dxfl-DX11 \\ coolsign. DDS" angezeigt.
    > **Hinweis**    Auch hier können Sie optional das Suffix auf Featureebene an den Dateinamen anfügen und im gleichen Verzeichnis speichern. beispielsweise \\ Texturen \\ coolsign \_ dxfl-DX9. DDS, \\ Texturen \\ coolsign \_ dxfl-DX11. DDS.

     

-   Deklarieren Sie die unterstützten DirectX-Featureebenen beim Konfigurieren der Grafikressourcen.
    ```cpp
    D3D_FEATURE_LEVEL featureLevels[] = 
    {
      D3D_FEATURE_LEVEL_11_1,
      D3D_FEATURE_LEVEL_11_0,
      D3D_FEATURE_LEVEL_10_1,
      D3D_FEATURE_LEVEL_10_0,
      D3D_FEATURE_LEVEL_9_3,
      D3D_FEATURE_LEVEL_9_1
    };
    ```

    ```cpp
    ComPtr<ID3D11Device> device;
    ComPtr<ID3D11DeviceContext> context;
    D3D11CreateDevice(
        nullptr,                    // Use the default adapter.
        D3D_DRIVER_TYPE_HARDWARE,
        0,                      // Use 0 unless it is a software device.
        creationFlags,          // defined above
        featureLevels,          // What the app will support.
        ARRAYSIZE(featureLevels),
        D3D11_SDK_VERSION,      // This should always be D3D11_SDK_VERSION.
        &device,                    // created device
        &m_featureLevel,            // The feature level of the device.
        &context                    // The corresponding immediate context.
    );
    ```

-   Verwenden Sie die APIs in [**Windows.ApplicationModel.Resources.Core**](/uwp/api/Windows.ApplicationModel.Resources.Core) zum Laden der Ressourcen. Verweise auf Ressourcen sollten generalisiert werden (kein Suffix), wobei die Featureebene weggelassen wird. Im Gegensatz zu Sprache und Skalierung bestimmt das System jedoch nicht automatisch, welche Featureebene für eine Anzeige optimal ist; dies bestimmen Sie selbst basierend auf Codelogik. Sobald Sie die richtige Ebene bestimmt haben, informieren Sie das Betriebssystem mithilfe der APIs über die bevorzugte Featureebene. Anschließend kann das System basierend auf dieser Einstellung die richtige Ressource abrufen. Dies ist ein Codebeispiel dafür, wie Ihre App über die aktuelle DirectX-Featureebene für die Plattform informiert wird:
    
    ```cpp
    // Set the current UI thread's MRT ResourceContext's DXFeatureLevel with the right DXFL. 

    Platform::String^ dxFeatureLevel;
        switch (m_featureLevel)
        {
        case D3D_FEATURE_LEVEL_9_1:
        case D3D_FEATURE_LEVEL_9_2:
        case D3D_FEATURE_LEVEL_9_3:
            dxFeatureLevel = L"DX9";
            break;
        case D3D_FEATURE_LEVEL_10_0:
        case D3D_FEATURE_LEVEL_10_1:
            dxFeatureLevel = L"DX10";
            break;
        default:
            dxFeatureLevel = L"DX11";
        }

        ResourceContext::SetGlobalQualifierValue(L"DXFeatureLevel", dxFeatureLevel);
    ```

    > **Hinweis**    Laden Sie die Textur in Ihrem Code direkt nach dem Namen (oder dem Pfad unterhalb des Verzeichnisses auf Featureebene). Fügen Sie weder den Featureebenen-Verzeichnisnamen noch das Suffix ein. Laden Sie z. b. "Texturen \\ coolsign. DDS", nicht "dxfl-DX11 \\ Texturen \\ coolsign. DDS" oder "Texturen \\ coolsign \_ dxfl-DX11. DDS".

     

-   Suchen Sie jetzt mithilfe der [**ResourceManager**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager)-Klasse nach der passenden Datei für die aktuelle DirectX-Featureebene. Die **ResourceManager**-Klasse gibt ein [**ResourceMap**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap)-Objekt zurück, das Sie mit [**ResourceMap::GetValue**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getvalue) (oder [**ResourceMap::TryGetValue**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.trygetvalue)) und einem bereitgestellten [**ResourceContext**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext)-Objekt abfragen können. Daraufhin wird ein [**ResourceCandidate**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceCandidate)-Objekt zurückgegeben, das der DirectX-Featureebene am ehesten entspricht, die durch den Aufruf von [**SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue) angegeben wurde.
    
    ```cpp
    // An explicit ResourceContext is needed to match the DirectX feature level for the display on which the current view is presented.

    auto resourceContext = ResourceContext::GetForCurrentView();
    auto mainResourceMap = ResourceManager::Current->MainResourceMap;

    // For this code example, loader is a custom ref class used to load resources.
    // You can use the BasicLoader class from any of the 8.1 DirectX samples similarly.


    auto possibleResource = mainResourceMap->GetValue(
        L"Files/BumpPixelShader.cso",
        resourceContext
    );
    Platform::String^ resourceName = possibleResource->ValueAsString;
    ```

-   Wählen Sie in Visual Studio 2015 die Option **PROJEKT -> Store -> Anwendungspaket erstellen...**, und erstellen Sie das Paket.
-   Achten Sie darauf, dass Sie App-Bündel in den Einstellungen des „package.appxmanifest“-Manifests aktivieren.

## <a name="related-topics"></a>Zugehörige Themen


* [Definition der App-Ressourcen](/previous-versions/windows/apps/hh965321(v=win.10))
* [Verpacken von Apps](../packaging/index.md)
* [App-Packager (MakeAppx.exe)](/windows/desktop/appxpkg/make-appx-package--makeappx-exe-)

 

 