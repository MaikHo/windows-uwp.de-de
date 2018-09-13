---
author: stevewhims
description: Eine Eigenschaft, die effektiv an ein XAML-Steuerelement gebunden werden kann, wird als *Observable*-Eigenschaft bezeichnet. Dieses Thema zeigt, wie man eine Observable-Eigenschaft implementiert und nutzt und wie man ein XAML-Steuerelement daran bindet.
title: XAML-Steuerelemente; Binden an eine C++/WinRT-Eigenschaft
ms.author: stwhi
ms.date: 08/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, standard, c++, cpp, winrt, projizierung, XAML, steuerelement, binden, eigenschaft
ms.localizationpriority: medium
ms.openlocfilehash: bdf4d3ff17dcdf51dba2e37929228560e2e58fb5
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "3959812"
---
# <a name="xaml-controls-bind-to-a-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-property"></a>XAML-Steuerelemente; Binden an eine [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)-Eigenschaft
Eine Eigenschaft, die effektiv an ein XAML-Steuerelement gebunden werden kann, wird als *observable*-Eigenschaft bezeichnet. Dieses Konzept basiert auf dem Software-Design-Muster, das als *Observer-Pattern* bekannt ist. Dieses Thema zeigt, wie man Observable-Eigenschaften in C++/WinRT implementiert und wie man XAML-Steuerelemente an diese bindet.

> [!IMPORTANT]
> Wichtige Konzepte und Begriffe, die Ihr Verständnis für die Verwendung von Laufzeitklassen mit C++/WinRT unterstützen, finden Sie unter [Verwenden von APIs mit C++/WinRT](consume-apis.md) und [Erstellen von APIs mit C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-property"></a>Was bedeutet *observable* für eine Eigenschaft?
Angenommen, eine Laufzeitklasse namens **BookSku** hat eine Eigenschaft namens **Title**. Wenn **BookSku** das Ereignis [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) auslöst, wenn sich der Wert von **Title** ändert, dann ist **Title** eine Observable-Eigenschaft. Es ist das Verhalten von **BookSku** (Auslösen oder nicht Auslösen des Ereignisses), das bestimmt, welche seiner Eigenschaften Observable-Eigenschaften sind.

Ein XAML-Textelement oder -Steuerelement kann sich an diese Ereignisse binden und sie verarbeiten, indem es den/die aktualisierten Wert(e) abruft und dann eine Aktualisierung durchführt, um den neuen Wert anzuzeigen.

> [!NOTE]
> Informationen zur Installation und Verwendung der C++/WinRT Visual Studio Extension (VSIX) (die Projektvorlagenunterstützung sowie C++/WinRT MSBuild-Eigenschaften und -Ziele bietet) finden Sie unter [Visual Studio-Unterstützung für C++/WinRT und VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="create-a-blank-app-bookstore"></a>Erstellen einer leeren App (Bookstore)
Erstellen Sie zunächst ein neues Projekt in Microsoft Visual Studio. Erstellen Sie eine **Visual C++** > **Universelle Windows-** > **leere App (C++ / WinRT)** Projekt und nennen Sie es *Bookstore*.

Wir werden eine neue Klasse schreiben, um ein Buch mit einer Observable-Eigenschaft namens „Titel” darzustellen. Wir erstellen und nutzen die Klasse innerhalb derselben Kompilierungseinheit. Aber wir wollen in der Lage sein, aus XAML eine Bindung an diese Klasse zu nutzen. Daher wird es eine Laufzeitklasse sein. Und wir werden C++/WinRT verwenden, um diese zu schreiben und zu nutzen.

Der erste Schritt beim Erstellen einer neuen Laufzeitklasse besteht darin, dem Projekt ein neues **Midl-Datei (.idl)**-Element hinzuzufügen. Nennen Sie es `BookSku.idl`. Löschen Sie den Standardinhalt von `BookSku.idl` und fügen Sie diese Laufzeitklassendeklaration ein.

```idl
// BookSku.idl
namespace Bookstore
{
    runtimeclass BookSku : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        String Title;
    }
}
```

> [!NOTE]
> Ihre Ansichtsmodell-Klassen&mdash;letzten Endes beliebigen Laufzeitklasse, die Sie in Ihrer Anwendung deklarieren&mdash;müssen nicht von einer Basisklasse abgeleitet. Oben deklarierte **BookSku** -Klasse ist ein Beispiel dafür. Eine Schnittstelle implementiert, aber sie nicht von einer Basisklasse abgeleitet werden.
>
> Beliebigen Laufzeitklasse, die Sie in der Anwendung deklarieren, *wird* von einer Basis abgeleitet Klasse wird als bezeichnet ein *zusammensetzbaren* Klasse. Und es gibt Einschränkungen um zusammensetzbaren Klassen. Damit eine Anwendung die [Zertifizierungskit für Windows-App](../debug-test-perf/windows-app-certification-kit.md) -Tests von Visual Studio und vom Microsoft Store verwendet, um Übermittlungen zu überprüfen (und daher für die Anwendung erfolgreich in den Microsoft Store aufgenommen werden), müssen Sie eine zusammensetzbare-Klasse letztlich von einem Windows-Basisklasse abgeleitet. Was bedeutet, dass die Klasse im sehr Stamm der Vererbungshierarchie ein Typ sein, in einem Windows-Namespace sein muss. Wenn Sie eine Laufzeitklasse von einer Basisklasse abgeleitet benötigen&mdash;z. B. implementieren eine Klasse **BindableBase** für alle Ihre Ansichtsmodelle Ableiten von&mdash;dann [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)abgeleitet werden können.
>
> Ein Ansichtsmodell ist eine Abstraktion einer Ansicht und ist daher direkt an die Ansicht (das XAML-Markup) gebunden. Ein Datenmodell ist eine Abstraktion von Daten, und er nur in Ihren Ansichtsmodellen genutzt, und nicht direkt mit XAML gebunden. Daher können Sie Ihre Datenmodelle nicht als-Runtime-Klassen, sondern als C++ Strukturen oder Klassen deklarieren. Sie müssen nicht in MIDL deklariert werden, und Sie können beliebige Vererbungshierarchie verwenden Sie, wie.

Speichern Sie die Datei, und erstellen Sie das Projekt. Während des Buildprozesses wird das `midl.exe`-Tool ausgeführt, um eine Windows-Runtime-Metadaten-Datei (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`) zu erstellen, die die Laufzeitklasse beschreibt. Dann wird das `cppwinrt.exe`-Tool ausgeführt, um Quellcodedateien zu erzeugen, die Sie bei der Erstellung und Nutzung Ihrer Laufzeitklasse unterstützen. Diese Dateien enthalten Stubs, um mit der Implementierung der **BookSku**-Laufzeitklasse zu beginnen, die Sie in Ihrer IDL deklariert haben. Diese Stubs sind `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` und `BookSku.cpp`.

Kopieren Sie die Stub-Dateien `BookSku.h` und `BookSku.cpp` von `\Bookstore\Bookstore\Generated Files\sources\` in den Projektordner `\Bookstore\Bookstore\`. Stellen Sie im **Projektmappen-Explorer** sicher, dass **Alle Dateien anzeigen** aktiviert ist. Klicken Sie mit der rechten Maustaste auf die kopierten Stub-Dateien und klicken Sie auf **In Projekt aufnehmen**.

## <a name="implement-booksku"></a>Implementieren von **BookSku**
Nun öffnen wir `\Bookstore\Bookstore\BookSku.h` und `BookSku.cpp` und implementieren unsere Laufzeitklasse. Fügen Sie in `BookSku.h` einen Konstruktor hinzu, der ein [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)-Argument (ein privates Mitglied zum Speichern des Titels) und eine weiteres für das bei einer Titeländerung ausgelöste Ereignis entgegen nimmt. Nachdem Sie diese geändert, Ihre `BookSku.h` sieht wie folgt aus.

```cppwinrt
// BookSku.h
#pragma once

#include "BookSku.g.h"

namespace winrt::Bookstore::implementation
{
    struct BookSku : BookSkuT<BookSku>
    {
        BookSku() = delete;
        BookSku(winrt::hstring const& title);

        winrt::hstring Title();
        void Title(winrt::hstring const& value);
        winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& value);
        void PropertyChanged(winrt::event_token const& token);
    
    private:
        winrt::hstring m_title;
        winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
    };
}
```

Implementieren Sie in `BookSku.cpp` die Funktionen wie folgt.

```cppwinrt
// BookSku.cpp
#include "pch.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    BookSku::BookSku(winrt::hstring const& title) : m_title{ title }
    {
    }

    winrt::hstring BookSku::Title()
    {
        return m_title;
    }

    void BookSku::Title(winrt::hstring const& value)
    {
        if (m_title != value)
        {
            m_title = value;
            m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"Title" });
        }
    }

    winrt::event_token BookSku::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
    {
        return m_propertyChanged.add(handler);
    }

    void BookSku::PropertyChanged(winrt::event_token const& token)
    {
        m_propertyChanged.remove(token);
    }
}
```

In der **Titelleiste** Zugriffsfunktion überprüfen wir, ob ein Wert festgelegt wird, die nicht den aktuellen Wert entspricht. Und wenn dies der Fall ist, wir aktualisieren den Titel und auch Auslösen des Ereignisses [**INotifyPropertyChanged:: PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) mit einem Argument gleich dem Namen der Eigenschaft, die geändert hat. Dies dient dazu, dass die Benutzeroberfläche (UI) erkennt, welcher Eigenschaftswert erneut abgefragt werden muss.

## <a name="declare-and-implement-bookstoreviewmodel"></a>**BookstoreViewModel** deklarieren und implementieren
Unsere XAML-Hauptseite wird sich an ein Hauptansichtsmodell gebunden. Dieses Ansichtsmodell wird mehrere Eigenschaften haben, darunter eine vom Typ **BookSku**. In diesem Schritt deklarieren und implementieren wir unsere Hauptansichtsmodell-Laufzeitklasse.

Fügen Sie eine neue **Midl-Datei (.idl)** mit dem Namen `BookstoreViewModel.idl` hinzu.

```idl
// BookstoreViewModel.idl
import "BookSku.idl";

namespace Bookstore
{
    runtimeclass BookstoreViewModel
    {
        BookSku BookSku{ get; };
    }
}
```

Speichern und erstellen Sie das Projekt. Kopieren Sie `BookstoreViewModel.h` und `BookstoreViewModel.cpp` aus dem `Generated Files`-Ordner in den Projektordner und nehmen Sie sie in das Projekt auf. Öffnen Sie diese Dateien und implementieren Sie die Laufzeitklasse aus, wie unten dargestellt. Hinweis: wie im `BookstoreViewModel.h`, wir sind einschließlich `BookSku.h`, der Deklaration des Implementierungstyps (**Winrt::Bookstore::implementation::BookSku**). Und wir sind den Standardkonstruktor wiederherstellen, indem Sie entfernen `= delete`.

```cppwinrt
// BookstoreViewModel.h
#pragma once

#include "BookstoreViewModel.g.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    struct BookstoreViewModel final : BookstoreViewModelT<BookstoreViewModel>
    {
        BookstoreViewModel();

        Bookstore::BookSku BookSku();

    private:
        Bookstore::BookSku m_bookSku{ nullptr };
    };
}
```

```cppwinrt
// BookstoreViewModel.cpp
#include "pch.h"
#include "BookstoreViewModel.h"

namespace winrt::Bookstore::implementation
{
    BookstoreViewModel::BookstoreViewModel()
    {
        m_bookSku = make<Bookstore::implementation::BookSku>(L"Atticus");
    }

    Bookstore::BookSku BookstoreViewModel::BookSku()
    {
        return m_bookSku;
    }
}
```

> [!NOTE]
> Der Typ von `m_bookSku` ist der projizierte Typ (**winrt::Bookstore::BookSku**), und der Template-Parameter, den Sie mit **make** verwenden, ist der Implementierungstyp (**winrt::Bookstore::implementation::BookSku**). Dennoch gibt **make** eine Instanz des projizierten Typs zurück.

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>Hinzufügen einer Eigenschaft vom Typ **BookstoreViewModel** zu **MainPage**
Öffnen Sie `MainPage.idl` (unsere Haupt-UI-Seite) mit der Deklaration der Laufzeitklasse. Fügen Sie eine Import-Anweisung zum Import von `BookstoreViewModel.idl` hinzu und fügen Sie eine schreibgeschützte Eigenschaft namens MainViewModel vom Typ **BookstoreViewModel** hinzu. Entfernen Sie auch die **MyProperty** -Eigenschaft. Beachten Sie außerdem die `import` -Direktive in den folgenden Eintrag.

```idl
// MainPage.idl
import "BookstoreViewModel.idl";

namespace Bookstore
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

Speichern Sie die Datei. Erstellen des Projekts wird nicht bis zum Abschluss im Moment, aber nun erstellen ist hilfreich, etwas zu tun, da es die Quellcodedateien generiert, in denen der **"MainPage"** implementiert ist (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` und `MainPage.cpp`). So einfach, und erstellen Sie jetzt. Der Buildfehler kann angezeigt werden in dieser Phase ist **'MainViewModel': ist kein Mitglied von 'Winrt::Bookstore::implementation::MainPage'**.

Wenn Sie das Hinzufügen von weglassen `BookstoreViewModel.idl` (finden Sie unter den Eintrag der `MainPage.idl` oben), erscheint den Fehler **erwartet \ < in der Nähe "MainViewModel"**. Ein weiterer Tipp ist, um sicherzustellen, dass Sie alle Typen im Namespace lassen: der Namespace, der in der angezeigt wird.

Um den Fehler zu beheben, die wir angezeigt werden, jetzt müssen Sie die Zugriffs-Stubs für die **MainViewModel** -Eigenschaft aus der generierten Dateien kopieren (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` und `MainPage.cpp`) und in `\Bookstore\Bookstore\MainPage.h` und `MainPage.cpp`.

In `\Bookstore\Bookstore\MainPage.h`, gehören `BookstoreViewModel.h`, der Deklaration des Implementierungstyps (**Winrt::Bookstore::implementation::BookstoreViewModel**). Fügen Sie ein privates Mitglied zum Ansichtsmodell zu speichern. Beachten Sie, dass die Zugriffsfunktion für die Eigenschaft (und das Mitglied m_mainViewModel) als **Bookstore::BookstoreViewModel** (dem projizierten Typ) implementiert ist. Der Implementierungstyp im selben Projekt (Kompilierungseinheit) wie die Anwendung ist, damit wir M_mainViewModel über der Konstruktorüberladung erstellt, die akzeptiert `nullptr_t`. Entfernen Sie auch die **MyProperty** -Eigenschaft.

```cppwinrt
// MainPage.h
...
#include "BookstoreViewModel.h"
...
namespace winrt::Bookstore::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Bookstore::BookstoreViewModel MainViewModel();

        void ClickHandler(Windows::Foundation::IInspectable const&, Windows::UI::Xaml::RoutedEventArgs const&);

    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
    };
}
...
```

In `\Bookstore\Bookstore\MainPage.cpp`, rufen Sie [**WinRT:: Make**](/uwp/cpp-ref-for-winrt/make) (mit dem Implementierungstyp), um M_mainViewModel eine neue Instanz des projizierten Typs zuzuweisen. Weisen Sie einen Initialwert für den Titel des Buches zu. Implementieren Sie die Zugriffsfunktion für die MainViewModel-Eigenschaft. Aktualisieren Sie zum Schluss den Titel des Buches im Event-Handler der Schaltfläche. Entfernen Sie auch die **MyProperty** -Eigenschaft.

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"

using namespace winrt;
using namespace Windows::UI::Xaml;

namespace winrt::Bookstore::implementation
{
    MainPage::MainPage()
    {
        m_mainViewModel = make<Bookstore::implementation::BookstoreViewModel>();
        InitializeComponent();
    }

    void MainPage::ClickHandler(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* args */)
    {
        MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    }

    Bookstore::BookstoreViewModel MainPage::MainViewModel()
    {
        return m_mainViewModel;
    }
}
```

## <a name="bind-the-button-to-the-title-property"></a>Binden Sie die Schaltfläche an die **Title**-Eigenschaft.
Öffnen Sie `MainPage.xaml` mit dem XAML-Markup für unsere UI-Hauptseite. Wie in den Eintrag unten gezeigt, entfernen Sie den Namen von der Schaltfläche, und ändern Sie den **Content** -Eigenschaft-Wert von einem Literal in einen Bindungsausdruck. Beachten Sie die Eigenschaft `Mode=OneWay` des Bindungsausdrucks (einseitig vom Ansichtsmodell zum UI). Ohne diese Eigenschaft reagiert die Benutzeroberfläche nicht auf Ereignisse zu Eigenschaftsänderungen.

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

Erstellen Sie nun das Projekt und führen Sie es aus. Klicken Sie auf die Schaltfläche, um den **Click**-Ereignis-Handler auszuführen. Dieser Handler ruft die Zugriffsfunktion des Buches auf; diese Zugriffsfunktion löst ein Ereignis aus, um die Benutzeroberfläche wissen zu lassen, dass sich die **Title**-Eigenschaft geändert hat; die Schaltfläche fragt den Wert dieser Eigenschaft erneut ab, um ihren eigenen **Content**-Wert zu aktualisieren.

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>Mithilfe der {Binding}-Markuperweiterung mit C++ / WinRT
Für die derzeit veröffentlichte Version von C++ / WinRT, um eventuell die {Binding}-Markuperweiterung verwenden, Sie die [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) und [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) -Schnittstellen implementieren müssen.

## <a name="important-apis"></a>Wichtige APIs
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [winrt::make Funktionsvorlage](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Verwandte Themen
* [Verwenden von APIs mit C++/WinRT](consume-apis.md)
* [Erstellen von APIs mit C++/WinRT](author-apis.md)