---
title: Netzwerk für Spiele
description: Erfahren Sie, wie Sie Netzwerkfeatures entwickeln und in ein DirectX-Spiel integrieren.
ms.assetid: 212eee15-045c-8ba1-e274-4532b2120c55
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, Networking, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 6d6d9d927c60cb74f1b19de607480e0811f47cbb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162934"
---
# <a name="networking-for-games"></a>Netzwerk für Spiele

Erfahren Sie, wie Sie Netzwerkfeatures entwickeln und in ein DirectX-Spiel integrieren.

## <a name="concepts-at-a-glance"></a>Konzepte auf einen Blick

Unabhängig davon, ob es sich um ein einfaches eigenständiges Spiel oder ein umfangreiches Multiplayer-Spiel handelt, können für DirectX-Spiele verschiedene Netzwerkfeatures verwendet werden. Das einfachste Netzwerkfeature dient zum Speichern von Benutzernamen und Spielständen auf einem zentralen Netzwerkserver.

Netzwerk-APIs werden für Multiplayer-Spiele mit Infrastrukturmodell (Client-Server oder Peer-zu-Peer über das Internet) sowie für Ad-hoc-Spiele (lokales Peer-zu-Peer) benötigt. Bei serverbasierten Multiplayer-Spielen erfolgen die meisten Spielvorgänge in der Regel auf einem zentralen Spielserver, und die Client-Spiel-App wird für Eingaben, die Grafikanzeige, die Audiowiedergabe und weitere Features verwendet. Die Geschwindigkeit und Latenz von Netzwerkübertragungen sind entscheidend für ein ansprechendes Spielerlebnis.

Bei Peer-zu-Peer-Spielen werden die Eingaben und Grafiken in der App jedes Spielers verarbeitet. In den meisten Fällen sind die Spieler nicht weit voneinander entfernt, sodass die Netzwerklatenz zwar niedriger ist, aber dennoch eine Rolle spielt. In diesem Fall ist es wichtig, wie Mitspieler gefunden werden und eine Verbindung mit ihnen hergestellt wird.

Bei Spielen mit einem Spieler wird oft ein zentraler Webserver oder -service zum Speichern von Benutzernamen, Spielständen und weiteren Informationen verwendet. Bei diesen Spielen stellt die Geschwindigkeit und Latenz von Netzwerkübertragungen in der Regel kein Problem dar, da die Spielvorgänge nicht unmittelbar damit zusammenhängen.

Netzwerkbedingungen können sich jederzeit ändern, sodass jedes Spiel, für das Netzwerk-APIs verwendet werden, eventuell auftretende Netzwerkausnahmen behandeln muss. Weitere Informationen zum Behandeln von Netzwerkausnahmen finden Sie unter [Grundlagen zum Netzwerk](../networking/networking-basics.md).

Firewalls und Webproxys werden häufig verwendet und können die Verwendung von Netzwerkfeatures beeinträchtigen. In einem Spiel über ein Netzwerk müssen Firewalls und Proxys ordnungsgemäß verarbeitet werden können.

Bei mobilen Geräten ist es wichtig, verfügbare Netzwerkressourcen zu überwachen und in getakteten Netzwerken entsprechend zu reagieren, da Roaming-oder Datenkosten anfallen können.

Die Netzwerkisolation ist Teil des von Windows verwendeten App-Sicherheitsmodells. Unter Windows werden Netzwerkgrenzen aktiv erkannt und Netzwerkzugriffsbeschränkungen für die Netzwerkisolation erzwungen. Apps müssen Netzwerkisolationsfunktionen deklarieren, um den Umfang des Netzwerkzugriffs festzulegen. Ohne Deklaration dieser Funktionen hat Ihre App keinen Zugriff auf Netzwerkressourcen. Unter [Konfigurieren von Netzwerkisolationsfunktionen](/previous-versions/windows/apps/hh770532(v=win.10)) finden Sie weitere Informationen zur Erzwingung der Netzwerkisolation für Apps durch Windows.

## <a name="design-considerations"></a>Überlegungen zum Entwurf

In DirectX-Spielen können verschiedene Netzwerk-APIs verwendet werden. Daher ist es wichtig, die richtige API auszuwählen. Windows unterstützt verschiedene Netzwerk-APIs, die Ihre App für die Kommunikation mit anderen Computern und Geräten entweder über das Internet oder private Netzwerke nutzen kann. Sie müssen zunächst ermitteln, welche Netzwerkfeatures Ihre App erfordert.

Dies sind die beliebtesten Netzwerk-APIs für Spiele.

-   TCP und Sockets – sorgen für eine zuverlässige Verbindung. Verwenden Sie TCP für Spielvorgänge, die keine Sicherheit erfordern. TCP ermöglicht eine einfache Skalierung des Servers, daher wird es häufig in Spielen mit dem Infrastruktur (Client-Server oder Peer-zu-Peer über das Internet)-Modell verwendet. TCP kann auch für Ad-hoc-Spiele (lokales Peer-zu-Peer) über Wi-Fi Direct und Bluetooth verwendet werden. TCP wird in der Regel für die Bewegung von Spielobjekten, die Interaktion zwischen Spielfiguren, Textchat und andere Vorgänge verwendet. Die [**Streamsocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) -Klasse stellt einen TCP-Socket bereit, der in Microsoft Store spielen verwendet werden kann. Die **StreamSocket**-Klasse wird zusammen mit verwandten Klassen im [**Windows::Networking::Sockets**](/uwp/api/Windows.Networking.Sockets)-Namespace verwendet.
-   TCP und Sockets mit SSL – sorgen für zuverlässige Verbindung und verhindern Mitverfolgung. Verwenden Sie TCP-Verbindungen mit SSL für Spielvorgänge, die Sicherheit erfordern. Die Verschlüsselung und der Mehraufwand von SSL erhöht die Latenz- und Leistungskosten, daher wird SSL nur verwendet, wenn Sicherheit erforderlich ist. TCP mit SSL wird in der Regel für Anmelde-, Kauf- und Handelsressourcen sowie die Erstellung und Verwaltung von Spielfiguren verwendet. Die [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket)-Klasse bietet einen TCP-Socket, der SSL unterstützt.
-   UDP und Sockets – bieten unzuverlässige Netzwerkübertragungen mit wenig Mehraufwand. UDP wird für Spielvorgänge verwendet, die niedrige Latenz erfordern und Paketverluste tolerieren können. Dies wird häufig für Kampfspiele, Schießen mit Leuchtspur, Netzwerkaudio und Sprachchat verwendet. Die [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket) -Klasse stellt einen UDP-Socket bereit, der in Microsoft Store spielen verwendet werden kann. Die **DatagramSocket**-Klasse wird zusammen mit verwandten Klassen im [**Windows::Networking::Sockets**](/uwp/api/Windows.Networking.Sockets)-Namespace verwendet.
-   HTTP-Client – stellt eine zuverlässige Verbindung mit HTTP-Servern bereit. Das gängigste Netzwerkszenario ist der Zugriff auf eine Website, um Informationen abzurufen oder zu speichern. Ein einfaches Beispiel dafür ist ein Spiel, das eine Website zum Speichern von Benutzerinformationen und Spielständen verwendet. Wenn ein HTTP-Client zusammen mit SSL für die Sicherheit verwendet wird, kann er für Anmelde-, Kauf- und Handelsressourcen sowie die Erstellung und Verwaltung von Spielfiguren verwendet werden. Die [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) -Klasse stellt eine moderne HTTP-Client-API für die Verwendung in Microsoft Store Spielen bereit. Die **HttpClient**-Klasse wird zusammen mit verwandten Klassen im [**Windows::Web::Http**](/uwp/api/Windows.Web.Http)-Namespace verwendet.

## <a name="handling-network-exceptions-in-your-directx-game"></a>Behandeln von Netzwerkausnahmen in einem DirectX-Spiel

Wenn in Ihrem DirectX-Spiel eine Netzwerkausnahme auftritt, weist dies auf ein erhebliches Problem oder einen Fehler hin. Ausnahmen können bei der Nutzung von Netzwerk-APIs viele Gründe haben. Häufig ergeben sich Ausnahmen aus Änderungen bei der Netzwerkverbindung oder anderen Netzwerkproblemen mit dem Remotehost oder Server.

Bei der Verwendung von Netzwerk-APIs können beispielsweise aus folgenden Gründen Ausnahmen auftreten:

-   Die Eingabe des Benutzers für einen Hostnamen oder einen URI enthält Fehler und ist ungültig.
-   Fehler bei der Namensauflösung beim Suchen nach einem Hostnamen oder URI
-   Verlust oder Änderung der Netzwerkverbindung
-   Netzwerkverbindungsfehler mit Sockets oder HTTP-Client-APIs
-   Netzwerkserver- oder Remoteendpunktfehler
-   Andere Netzwerkfehler

Ausnahmen aufgrund von Netzwerkfehlern (z. B. Unterbrechung oder Änderung der Netzwerkverbindung, Verbindungsfehler und Serverfehler) können jederzeit auftreten. Diese Fehler haben zur Folge, dass Ausnahmen ausgelöst werden. Wenn eine Ausnahme nicht von Ihrer App behandelt wird, kann sie dazu führen, dass die gesamte App von der Runtime beendet wird.

Beim Aufrufen der meisten asynchronen Netzwerkmethoden müssen Sie Code zum Behandeln von Ausnahmen schreiben. In manchen Fällen kann versucht werden, eine Netzwerkmethode beim Auftreten einer Ausnahme zu wiederholen, um das Problem zu beheben. In anderen Fällen muss die App ohne Netzwerkverbindung mit zuvor zwischengespeicherten Daten weiter ausgeführt werden.

UWP-Apps (Universelle Windows-Plattform) lösen im Allgemeinen nur eine Ausnahme aus. Ihr Ausnahmehandler kann detailliertere Informationen zur Ursache abrufen, um die Ausnahme besser verstehen und entsprechende Entscheidungen treffen zu können.

Wenn eine Ausnahme in einem DirectX-Spiel auftritt, bei dem es sich um eine UWP-App handelt, kann der **HRESULT**-Wert zur Ermittlung der Fehlerursache abgerufen werden. Die *Winerror.h*-Includedatei enthält eine umfangreiche Liste möglicher **HRESULT**-Werte, die Netzwerkfehler umfasst.

Die Netzwerk-APIs unterstützen verschiedene Methoden zum Abrufen der detaillierten Informationen zur Ursache der Ausnahme.

-   Eine Methode zum Abrufen des **HRESULT**-Werts des Fehlers, der die Ausnahme verursacht hat. Die mögliche Liste potenzieller **HRESULT**-Werte ist lang und nicht spezifiziert. Der **HRESULT**-Wert kann abgerufen werden, wenn eine der Netzwerk-APIs verwendet wird.
-   Eine Hilfsmethode, die den **HRESULT**-Wert in einen Enumerationswert konvertiert. Die Liste der möglichen Enumerationswerte ist spezifiziert und relativ kurz. Eine Hilfsmethode für die Socketklassen ist in [**Windows::Networking::Sockets**](/uwp/api/Windows.Networking.Sockets) verfügbar.

### <a name="exceptions-in-windowsnetworkingsockets"></a>Ausnahmen in „Windows.Networking.Sockets“

Der Konstruktor für die [**HostName**](/uwp/api/Windows.Networking.HostName)-Klasse in Verbindung mit Sockets kann eine Ausnahme auslösen, wenn die übergebene Zeichenfolge kein gültiger Hostname ist (enthält Zeichen, die in einem Hostnamen nicht zulässig sind). Wenn eine App vom Benutzer eine Eingabe für das **HostName**-Element einer Peerverbindung zum Spielen erhält, sollte sich der Konstruktor innerhalb eines try/catch-Blocks befinden. Wenn eine Ausnahme ausgelöst wird, kann die App den Benutzer benachrichtigen und einen neuen Hostnamen anfordern.

Hinzufügen von Code zum Überprüfen einer Zeichenfolge für einen Hostnamen vom Benutzer

```cppcx
// Define some variables at the class level.
Windows::Networking::HostName^ remoteHost;

bool isHostnameFromUser = false;
bool isHostnameValid = false;

///...

// If the value of 'remoteHostname' is set by the user in a control as input 
// and is therefore untrusted input and could contain errors. 
// If we can't create a valid hostname, we notify the user in statusText 
// about the incorrect input.

String ^hostString = remoteHostname;

try 
{
    remoteHost = ref new Windows::Networking:Host(hostString);
    isHostnameValid = true;
}
catch (InvalidArgumentException ^ex)
{
    statusText->Text = "You entered a bad hostname, please re-enter a valid hostname.";
    return;
}

isHostnameFromUser = true;

// ... Continue with code to execute with a valid hostname.
```

Der [**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets)-Namespace enthält praktische Hilfsmethoden und Enumerationen für die Behandlung von Fehlern bei der Verwendung von Sockets. Mit ihnen lassen sich spezifische Netzwerkausnahmen in der App unterschiedlich behandeln.

Ein Fehler, der für einen [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket)-, [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket)- oder [**StreamSocketListener**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener)-Vorgang auftritt, führt zur Auslösung einer Ausnahme. Die Ursache der Ausnahme ist ein Fehlerwert, der als **HRESULT**-Wert dargestellt wird. Mit der [**SocketError.GetStatus**](/uwp/api/windows.networking.sockets.socketerror.getstatus)-Methode wird ein Netzwerkfehler aus einem Socketvorgang in einen [**SocketErrorStatus**](/uwp/api/Windows.Networking.Sockets.SocketErrorStatus)-Enumerationswert konvertiert. Die meisten **SocketErrorStatus**-Enumerationswerte entsprechen einem vom systemeigenen Windows Sockets-Vorgang zurückgegebenen Fehler. Eine App kann nach bestimmten **SocketErrorStatus**-Enumerationswerten filtern, um das App-Verhalten je nach Ausnahmeursache zu ändern.

Bei Parameterprüfungsfehlern kann eine App den **HRESULT**-Wert aus der Ausnahme auch verwenden, um ausführlichere Informationen zum zugehörigen Fehler zu erhalten. Mögliche **HRESULT**-Werte sind in der Headerdatei *Winerror.h* aufgeführt. Bei den meisten Parameter Validierungs Fehlern lautet der zurückgegebene **HRESULT** **E \_ invalidArg**.

Hinzufügen von Code zum Behandeln von Ausnahmen beim Herstellen einer Streamsocketverbindung

```cppcx
using namespace Windows::Networking;
using namespace Windows::Networking::Sockets;
    
    // Define some more variables at the class level.

    bool isSocketConnected = false
    bool retrySocketConnect = false;

    // The number of times we have tried to connect the socket.
    unsigned int retryConnectCount = 0;

    // The maximum number of times to retry a connect operation.
    unsigned int maxRetryConnectCount = 5; 
    ///...

    // We pass in a valid remoteHost and serviceName parameter.
    // The hostname can contain a name or an IP address.
    // The servicename can contain a string or a TCP port number.

    StreamSocket ^ socket = ref new StreamSocket();
    SocketErrorStatus errorStatus; 
    HResult hr;

    // Save the socket, so any subsequent steps can use it.
    CoreApplication::Properties->Insert("clientSocket", socket);

    // Connect to the remote server. 
    create_task(socket->ConnectAsync(
            remoteHost,
            serviceName,
            SocketProtectionLevel::PlainSocket)).then([this] (task<void> previousTask)
    {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.get();

            isSocketConnected = true;
            // Mark the socket as connected. We do not really care about the value of the property, but the mere 
            // existence of  it means that we are connected.
            CoreApplication::Properties->Insert("connected", nullptr);
        }
        catch (Exception^ ex)
        {
            hr = ex.HResult;
            errorStatus = SocketStatus::GetStatus(hr); 
            if (errorStatus != Unknown)
            {
                                                                switch (errorStatus) 
                   {
                    case HostNotFound:
                        // If the hostname is from the user, this may indicate a bad input.
                        // Set a flag to ask the user to re-enter the hostname.
                        isHostnameValid = false;
                        return;
                        break;
                    case ConnectionRefused:
                        // The server might be temporarily busy.
                        retrySocketConnect = true;
                        return;
                        break; 
                    case NetworkIsUnreachable: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    case UnreachableHost: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    case NetworkIsDown: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    // Handle other errors. 
                    default: 
                        // The connection failed and no options are available.
                        // Try to use cached data if it is available. 
                        // You may want to tell the user that the connect failed.
                        break;
                }
                }
                else 
                {
                    // Received an Hresult that is not mapped to an enum.
                    // This could be a connectivity issue.
                    retrySocketConnect = true;
                }
            }
        });
    }

```

### <a name="exceptions-in-windowswebhttp"></a>Ausnahmen in „Windows.Web.Http“

Der Konstruktor für die [**Windows::Foundation::Uri**](/uwp/api/Windows.Foundation.Uri)-Klasse in Verbindung mit [**Windows::Web::Http::HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) kann eine Ausnahme auslösen, wenn die übergebene Zeichenfolge kein gültiger URI ist (enthält Zeichen, die in einem URI nicht zulässig sind). In C++ gibt es keine Methode zum Analysieren einer Zeichenfolge für einen URI. Wenn eine APP Eingaben vom Benutzer für den **Windows:: Foundation:: URI**erhält, sollte sich der Konstruktor in einem try/catch-Block befinden. Wenn eine Ausnahme ausgelöst wird, kann die App den Benutzer benachrichtigen und einen neuen URI anfordern.

Von der App sollte auch überprüft werden, ob das Schema im URI HTTP oder HTTPS lautet, da dies die einzigen von [**Windows::Web::Http::HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) unterstützten Schemas sind.

Hinzufügen von Code zum Überprüfen einer Zeichenfolge für einen URI vom Benutzer

```cppcx
    // Define some variables at the class level.
    Windows::Foundation::Uri^ resourceUri;

    bool isUriFromUser = false;
    bool isUriValid = false;

    ///...

    // If the value of 'inputUri' is set by the user in a control as input 
    // and is therefore untrusted input and could contain errors. 
    // If we can't create a valid hostname, we notify the user in statusText 
    // about the incorrect input.

    String ^uriString = inputUri;

    try 
    {
        isUriValid = false;
        resourceUri = ref new Windows::Foundation:Uri(uriString);

        if (resourceUri->SchemeName != "http" && resourceUri->SchemeName != "https")
        {
            statusText->Text = "Only 'http' and 'https' schemes supported. Please re-enter URI";
            return;
        }
        isUriValid = true;
    }
    catch (InvalidArgumentException ^ex)
    {
        statusText->Text = "You entered a bad URI, please re-enter Uri to continue.";
        return;
    }

    isUriFromUser = true;


    // ... Continue with code to execute with a valid URI.
```

Der [**Windows::Web::Http**](/uwp/api/windows.web.http)-Namespace bietet keine Funktion, die die Behandlung von Ausnahmen erleichtert. Eine APP, die [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) und andere Klassen in diesem Namespace verwendet, muss also den **HRESULT** -Wert verwenden.

In Apps mit C++ stellt das [**Platform::Exception**](/cpp/cppcx/platform-exception-class)-Objekt einen Fehler während der App-Ausführung dar, wenn eine Ausnahme auftritt. Die [**Platform::Exception::HResult**](/cpp/cppcx/platform-exception-class#hresult)-Eigenschaft gibt den **HRESULT**-Wert zurück, der der jeweiligen Ausnahme zugewiesen ist. Die [**Platform::Exception::Message**](/cpp/cppcx/platform-exception-class#message)-Eigenschaft gibt die vom System bereitgestellte Zeichenfolge zurück, die dem **HRESULT**-Wert zugeordnet ist. Mögliche **HRESULT**-Werte sind in der Headerdatei *Winerror.h* aufgeführt. Eine App kann nach bestimmten **HRESULT**-Werten filtern, um das App-Verhalten je nach Ausnahmeursache zu ändern.

Bei den meisten Parameter Validierungs Fehlern lautet der zurückgegebene **HRESULT** **E \_ invalidArg**. Bei manchen unzulässigen Methodenaufrufen wird der **HRESULT**-Wert **E\_ILLEGAL\_METHOD\_CALL** zurückgegeben.

Hinzufügen von Code zum Behandeln von Ausnahmen beim Verwenden von [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) zum Herstellen einer Verbindung mit einem HTTP-Server

```cppcx
using namespace Windows::Foundation;
using namespace Windows::Web::Http;
    
    // Define some more variables at the class level.

    bool isHttpClientConnected = false
    bool retryHttpClient = false;

    // The number of times we have tried to connect the socket
    unsigned int retryConnectCount = 0;

    // The maximum number of times to retry a connect operation.
    unsigned int maxRetryConnectCount = 5; 
    ///...

    // We pass in a valid resourceUri parameter.
    // The URI must contain a scheme and a name or an IP address.

    HttpClient ^ httpClient = ref new HttpClient();
    HResult hr;

    // Save the httpClient, so any subsequent steps can use it.
    CoreApplication::Properties->Insert("httpClient", httpClient);

    // Send a GET request to the HTTP server. 
    create_task(httpClient->GetAsync(resourceUri)).then([this] (task<void> previousTask)
    {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.get();

            isHttpClientConnected = true;
            // Mark the HttClient as connected. We do not really care about the value of the property, but the mere 
            // existence of  it means that we are connected.
            CoreApplication::Properties->Insert("connected", nullptr);
        }
        catch (Exception^ ex)
        {
            hr = ex.HResult;
                                                switch (errorStatus) 
               {
                case WININET_E_NAME_NOT_RESOLVED:
                    // If the Uri is from the user, this may indicate a bad input.
                    // Set a flag to ask user to re-enter the Uri.
                    isUriValid = false;
                    return;
                    break;
                case WININET_E_CANNOT_CONNECT:
                    // The server might be temporarily busy.
                    retryHttpClientConnect = true;
                    return;
                    break; 
                case WININET_E_CONNECTION_ABORTED: 
                    // This could be a connectivity issue.
                    retryHttpClientConnect = true;
                    break;
                case WININET_E_CONNECTION_RESET: 
                    // This could be a connectivity issue.
                    retryHttpClientConnect = true;
                    break;
                case INET_E_RESOURCE_NOT_FOUND: 
                    // The server cannot locate the resource specified in the uri.
                    // If the Uri is from user, this may indicate a bad input.
                    // Set a flag to ask the user to re-enter the Uri
                    isUriValid = false;
                    return;
                    break;
                // Handle other errors. 
                default: 
                    // The connection failed and no options are available.
                    // Try to use cached data if it is available. 
                    // You may want to tell the user that the connect failed.
                    break;
            }
            else 
            {
                // Received an Hresult that is not mapped to an enum.
                // This could be a connectivity issue.
                retrySocketConnect = true;
            }
        }
    });
```

## <a name="related-topics"></a>Zugehörige Themen

### <a name="other-resources"></a>Weitere Ressourcen

* [Herstellen einer Verbindung mit einem Datagrammsocket](/previous-versions/windows/apps/jj635238(v=win.10))
* [Herstellen einer Verbindung mit einer Netzwerkressource mit einem Streamsocket](/previous-versions/windows/apps/jj150599(v=win.10))
* [Herstellen einer Verbindung mit Netzwerkdiensten](/previous-versions/windows/apps/hh452976(v=win.10))
* [Herstellen von Verbindungen mit Webdiensten](/previous-versions/windows/apps/hh761504(v=win.10))
* [Netzwerkgrundlagen](../networking/networking-basics.md)
* [So wird's gemacht: Konfigurieren von Netzwerkisolationsfunktionen](/previous-versions/windows/apps/hh770532(v=win.10))
* [Aktivieren von Loopback und Debuggen der Netzwerkisolation](/previous-versions/windows/apps/hh780593(v=win.10))

### <a name="reference"></a>Verweis

* [DatagramSocket](/uwp/api/Windows.Networking.Sockets.DatagramSocket)
* [HttpClient](/uwp/api/Windows.Web.Http.HttpClient)
* [StreamSocket](/uwp/api/Windows.Networking.Sockets.StreamSocket)
* [Windows:: Web:: http-Namespace](/uwp/api/Windows.Web.Http)
* [Windows:: Networking:: Sockets-Namespace](/uwp/api/Windows.Networking.Sockets)

### <a name="sample-apps"></a>Beispiel-Apps

* [DatagramSocket-Beispiel](/samples/microsoft/windows-universal-samples/datagramsocket/)
* [HttpClient-Beispiel](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/HttpClient%20sample)
* [Näherungsbeispiel](https://github.com/microsoft/VCSamples/tree/master/VC2012Samples/Windows%208%20samples/C%2B%2B/Windows%208%20app%20samples/Proximity%20sample%20(Windows%208))
* [Beispiel für StreamSocket](/samples/microsoft/windows-universal-samples/streamsocket/)