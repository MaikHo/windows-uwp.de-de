---
author: stevewhims
description: Antworten auf Fragen zur Erstellung und Nutzung von Windows-Runtime-APIs mit C++/WinRT.
title: Häufig gestellte Fragen zu C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, standard, c++, cpp, winrt, projektion, häufig, gestellte, fragen, faq
ms.localizationpriority: medium
ms.openlocfilehash: 9316a29a50970bdaa288a4744f3aab7d873cbe4e
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "3962904"
---
# <a name="frequently-asked-questions-about-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Häufig gestellte Fragen zu [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Antworten auf Fragen zur Erstellung und Nutzung von Windows-Runtime-APIs mit C++/WinRT.

> [!NOTE]
> Wenn Ihre Frage eine Fehlermeldung betrifft, die Sie gesehen haben, lesen Sie auch das Thema [Problembehandlung bei C++/WinRT](troubleshooting.md).

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>Warum kompiliert nicht Mein neue Projekt? Ich verwende Visual Studio 2017 (Version 15.8.0 oder höher), und SDK-Version 17134

Wenn Sie Visual Studio 2017 verwenden (Version 15.8.0 oder höher), und für das Windows SDK-Version 10.0.17134.0 (Windows 10, Version 1803), klicken Sie dann eine neu erstellte C++ / WinRT-Projekt möglicherweise nicht mit dem Fehler kompilieren "*Fehler C3861: 'From_abi': Bezeichner nicht gefunden*", und mit anderen sein, der aus *base.h*Fehlern. Die Lösung besteht darin, entweder Ziel höher (größere Übereinstimmung) Version des Windows SDK oder der Set-Projekteigenschaft **C/C++-** > **Sprache** > **Konformitätsmodus: Nein** (auch, wenn **/ PERMISSIVE--** wird im Projekteigenschaft ** C/C++** > **Sprache** > **Befehlszeile** unter **Zusätzliche Optionen**, löschen Sie es).

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsixhttpsakamscppwinrtvsix"></a>Was sind die Voraussetzungen für die [C++/WinRT Visual Studio Extension (VSIX)](https://aka.ms/cppwinrt/vsix)?
Das [VSIX](https://aka.ms/cppwinrt/vsix) erzwingt eine minimale Windows SDK-Zielversion von 10.0.17134.0 (Windows 10, Version 1803). Sie benötigen außerdem Visual Studio2017 (mindestens Version 15.6– wir empfehlen mindestens 15.7). Sie können ein Projekt, das VSIX verwendet, durch das Vorhandensein von `<CppWinRTEnabled>true</CppWinRTEnabled>` in `<PropertyGroup Label="Globals">` in der Datei `.vcxproj` erkennen. Weitere Informationen finden Sie unter [Visual Studio-Unterstützung für C++/WinRT und VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="whats-a-runtime-class"></a>Was ist eine *Laufzeitklasse*?
Eine Laufzeitklasse ist ein Typ, der über moderne COM-Schnittstellen aktiviert und genutzt werden kann, typischerweise über Ausführungsgrenzen hinweg. Eine Laufzeitklasse kann aber auch innerhalb der Kompiliereinheit verwendet werden, die sie implementiert. Sie deklarieren eine Laufzeitklasse in der Interface Definition Language (IDL) und können sie in Standard C++ mit C++/WinRT implementieren.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>Was bedeuten *der projizierte Typ* und *der Implementierungstyp*?
Wenn Sie eine Windows-Runtime-Klasse (Laufzeitklasse) nur *verwenden*, dann arbeiten Sie ausschließlich mit *projizierten Typen*. C++/WinRT ist eine *Sprachprojektion*. Projizierte Typen sind Teil der Oberfläche von Windows-Runtime, die über C++/WinRT auf C++ *projiziert* werden. Weitere Details finden Sie unter [APIs mit C++/WinRT nutzen](consume-apis.md).

Der *Implementierungstyp* enthält die Implementierung einer Laufzeitklasse. Er ist also nur im Projekt verfügbar, das die Laufzeitklasse implementiert. Wenn Sie in einem Projekt arbeiten, das Laufzeitklassen implementiert (ein Projekt für eine Komponente für Windows-Runtime oder ein Projekt, das XAML-UI verwendet), ist es wichtig, sich mit der Unterscheidung zwischen Ihrem Implementierungstyp für eine Laufzeitklasse und dem projizierten Typ, der die in C++/WinRT projizierte Laufzeitklasse repräsentiert, vertraut zu machen. Weitere Details finden Sie unter [APIs mit C++/WinRT erstellen](author-apis.md).

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>Muss ich einen Konstruktor in der IDL meiner Laufzeitklasse deklarieren?
Nur wenn die Laufzeitklasse so konzipiert ist, dass sie von außerhalb ihrer implementierenden Kompiliereinheit verwendet werden kann (es handelt sich um eine Komponente für Windows-Runtime, die für den allgemeinen Gebrauch durch Windows-Runtime-Clientanwendungen bestimmt ist). Ausführliche Informationen über den Zweck und die Konsequenzen der Deklaration von Konstruktoren in IDL finden Sie unter [Runtime-Klassenkonstruktoren](author-apis.md#runtime-class-constructors).

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>Warum gibt der Linker den Fehler „LNK2019: Nicht aufgelöstes externes Symbol“ aus?
Wenn es sich bei dem nicht aufgelösten Symbol um eine API aus den Windows-Namespace-Headern für die C++/WinRT-Projektion (im **Winrt**-Namespace) handelt, ist die API in einem Header forward-deklariert, den Sie eingeschlossen haben, ihre Definition befindet sich aber in einem Header, der noch nicht hinzugefügt wurde. Binden Sie den Header ein, der für den Namespace der API benannt ist, und führen Sie eine erneute Erstellung durch. Weitere Informationen finden Sie unter [C++/WinRT-Projektionsheader](consume-apis.md#cwinrt-projection-headers).

Wenn es sich bei dem nicht aufgelösten Symbol um eine freie Windows-Runtime-Funktion handelt, z.B. [RoInitialize](https://msdn.microsoft.com/library/br224650), müssen Sie explizit die Überbibliothek [WindowsApp.lib](/uwp/win32-and-com/win32-apis) zu Ihrem Projekt hinzufügen. Die C++/WinRT-Projektion hängt von einigen dieser freien (Nicht-Mitglieds)-Funktionen und Einstiegspunkten ab. Wenn Sie eine der [C++/WinRT Visual Studio Extension (VSIX)](https://aka.ms/cppwinrt/vsix)-Projektvorlagen für Ihre Anwendung verwenden, wird `WindowsApp.lib` automatisch für Sie verknüpft. Andernfalls können Sie die Projektlinkeinstellungen verwenden, um sie einzuschließen, oder dies im Quellcode erledigen.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>Sollte ich [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable) implementieren und wenn ja, wie?
Wenn Sie eine Laufzeitklasse haben, die Ressourcen in ihrem Destruktor freigibt, und diese Laufzeitklasse dafür ausgelegt ist außerhalb ihrer implementierenden Kompiliereinheit genutzt zu werden (eine Komponente für Windows-Runtime, die für die allgemeinen Nutzung durch Windows-Runtime-Clientanwendungen bestimmt ist), dann empfehlen wir Ihnen,** IClosable** zu implementieren, um die Nutzung Ihrer Laufzeitklasse durch Sprachen ohne deterministische Finalisierung zu unterstützen. Stellen Sie sicher, dass Ihre Ressourcen freigegeben werden – egal ob der Destruktor, [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.Close) oder beide aufgerufen werden. **IClosable::Close** kann beliebig oft aufgerufen werden.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosablewindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>Muss ich [**IClosable::Close**](/uwp/api/windows.foundation.iclosable#Windows_Foundation_IClosable_Close_) bei Laufzeitklassen aufrufen, die ich nutze?
**IClosable** existiert, um Sprachen zu unterstützen, die keine deterministische Finalisierung haben. Daher sollten Sie **IClosable::Close** nicht von C++/WinRT aus aufrufen, außer in sehr seltenen Fällen, in denen es um Shutdown-Races oder halb Semi-Deadocks geht. Wenn Sie z. B.** Windows.UI.Composition**-Typen verwenden, können Sie auf Fälle stoßen, in denen Sie Objekte in einer festgelegten Reihenfolge verwerfen möchten (statt dies durch den C++/WinRT-Wrapper erledigen zu lassen).

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>Kann ich LLVM/Clang verwenden, um mit C++/WinRT zu kompilieren?
Die Toolkette LLVM und Clang wird für C++/WinRT nicht unterstützt, aber wir verwenden sie intern, um die Standardkonformität von C++/WinRT zu überprüfen. Wenn Sie z.B. emulieren möchten, was wir intern tun, könnten Sie ein Experiment wie das unten beschriebene ausprobieren.

Wechseln Sie zur [LLVM-Download-Seite](https://releases.llvm.org/download.html), suchen Sie nach **Download LLVM 6.0.0** > **Pre-Built Binaries**, und laden Sie **Clang for Windows (64-bit)** herunter. Geben Sie während der Installation an, dass LLVM zur Systemvariablen PATH hinzugefügt werden soll, sodass Sie in der Lage sind, sie von einer Befehlszeile aufzurufen. Für die Zwecke dieses Experiments können Sie die Meldungen, dass das Verzeichnis für die MSBuild-Toolsets nicht gefunden werden konnte und/oder dass die MSVC-Integrationsinstallation fehlgeschlagen ist, ignorieren (sollten diese angezeigt werden). Es gibt verschiedene Möglichkeiten zum Aufrufen von LLVM/Clang. Das folgende Beispiel zeigt nur eine davon.

```
C:\ExperimentWithLLVMClang>type main.cpp
// main.cpp
#pragma comment(lib, "windowsapp")
#pragma comment(lib, "ole32")

#include "winrt/Windows.Foundation.h"
#include <stdio.h>
#include <iostream>

using namespace winrt;

int main()
{
    winrt::init_apartment();
    Windows::Foundation::Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    std::wcout << rssFeedUri.Domain().c_str() << std::endl;
}

C:\ExperimentWithLLVMClang>clang-cl main.cpp /EHsc /I ..\.. -Xclang -std=c++17 -Xclang -Wno-delete-non-virtual-dtor -o app.exe

C:\ExperimentWithLLVMClang>app
windows.com
```

Da C++/WinRT Features vom C++17-Standard verwendet, müssen Sie alle Compiler-Flags verwenden, die erforderlich sind, um diese Unterstützung zu erhalten. Diese Flags sind je nach Compiler unterschiedlich.

Visual Studio ist das Entwicklungstool, das wir unterstützen und für C++/WinRT empfehlen. Weitere Informationen finden Sie unter [Visual Studio-Unterstützung für C++/WinRT und VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>Warum nicht die Funktion generierten Implementierung für eine schreibgeschützte Eigenschaft hat die `const` Qualifizierer?

Wenn Sie eine schreibgeschützte Eigenschaft in [MIDL 3.0](/uwp/midl-3/)deklarieren, können Sie davon ausgehen, dass die `cppwinrt.exe` Tool zum Generieren einer Implementierung-Funktion für Sie `const`-qualifizierte (eine const Funktion behandelt den *diesem* Zeiger als const).

Sicherlich empfohlen, Const nach Möglichkeit, aber die `cppwinrt.exe` Tool selbst nicht versuchen, Grund, über welche Implementierung Funktionen u. u. tatsächlich const werden und die möglicherweise nicht. Sie können festlegen, dass Ihre Implementierung Funktionen const, wie im folgenden Beispiel.

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

Sie können entfernen, die `const` Qualifizierer für **ToString** sollten Sie entscheiden, dass Sie einige Objektzustand in der Implementierung ändern müssen. Es sollte jedoch darauf aller Ihrer Member Funktionen const oder nicht-Const, nicht beides. Anders ausgedrückt, nicht überladen eine Implementierung Funktion auf `const`.

Abgesehen von der Ihrer Implementierungsfunktionen einer anderen anderen Standort const wird das Bild ist in Windows-Runtime-Funktion Projektionen. Betrachten Sie diesen Code.

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

Für den Aufruf von **ToString** oben, der Befehl **Gehe zu Deklaration** in Visual Studio angezeigt, die die Projektion der Windows-Runtime- **istringable:: ToString** in C++ / WinRT sieht wie folgt aus.

```
winrt::hstring ToString() const;
```

Funktionen in der Projektion sind const unabhängig davon, wie Sie Ihre Implementierung von ihnen zu qualifizieren. Im Hintergrund Ruft die Projektion auf die binären Anwendungsschnittstelle (ABI), welche Beträge auf einen Aufruf über einen COM-Schnittstellenzeiger. Der einzige Zustand, mit dem interagiert projizierten **ToString** ist die COM-Interface-Zeiger. und natürlich keine Notwendigkeit, das diesem Zeiger zu ändern, so dass die Funktion konstant ist verfügt. Mit dieser erhalten Sie die Sicherstellung, dass es sich nicht über die **IStringable** -Referenz, der Sie ändert durch Aufrufen, und es wird sichergestellt, dass Sie auch mit einer Const **ToString** aufrufen können, ein **IStringable**verweisen.

Zu verstehen, die diese Beispiele für `const` sind Details zur Implementierung von C++ / WinRT Projektionen und Implementierungen; Sie bilden Code Hygiene Ihrem nutzen. Es gibt keine als `const` auf der COM- noch die Windows-Runtime-ABI (für Memberfunktion).

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>Haben Sie alle Empfehlungen für das Verringern der Codegröße für C++ / WinRT-Binärdateien?

Bei der Arbeit mit Windows-Runtime-Objekten vermeiden Sie das Codierungsmuster unten angezeigt werden, da es sich negativ auf Ihre Anwendung aufweisen kann, indem verursacht Weitere Binärcode als nötig, generiert werden.

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

In der Windows-Runtime-Welt, kann der Compiler nicht entweder den Wert der cache `c()` oder die Schnittstellen für jede Methode, die durch eine Dereferenzierung aufgerufen wird ('. '). Es sei denn, Sie eingreifen, führt, die weitere virtuelle Aufrufe und referenzzählung Aufwand. Das Muster, die oben genannten konnte leicht doppelt so viel Code, der als streng benötigt generiert werden. Bevorzugen Sie stattdessen das Muster unten Sie überall können. Es wird viel weniger Code generiert, und es kann auch erheblich verbessern der Leistung Ihrer Laufzeit.

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

Das empfohlene oben gezeigte Muster gilt nicht nur für C++ / WinRT jedoch auf alle Windows-Runtime-sprachprojektionen.

> [!NOTE]
> Wenn Ihre Frage in diesem Thema nicht beantwortet werden konnte, finden Sie ggf. hier Unterstützung: [`c++-winrt`-Tag auf Stack Overflow ](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt).