---
description: Ein Einzelwert muss in ein Referenzklassenobjekt gepackt werden, bevor er an eine Funktion übergeben wird, die **IInspectable** erwartet. Dieser Wrapping-Prozess wird als *Boxing* des Werts bezeichnet.
title: Boxing und Unboxing von Skalarwerten für „IInspectable“ mit C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, XAML, Steuerelement, Boxing, Skalarwert
ms.localizationpriority: medium
ms.openlocfilehash: 3c1a64b97b40608e877f18b764ae92835d2bc4a7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154374"
---
# <a name="boxing-and-unboxing-scalar-values-to-iinspectable-with-cwinrt"></a>Boxing und Unboxing von Skalarwerten für „IInspectable“ mit C++/WinRT
 
Die [**Schnittstelle „IInspectable“** ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) ist die Stammschnittstelle jeder Laufzeitklasse in der Windows-Runtime (WinRT). Dieses Konzept kommt auch bei der [**Schnittstelle „IUnknown“** ](/windows/desktop/api/unknwn/nn-unknwn-iunknown) zur Anwendung, die sich am Stamm jeder COM-Schnittstelle und -Klasse befindet, sowie bei **System.Object** am Stamm jeder Klasse des [allgemeinen Typsystems](/dotnet/standard/base-types/common-type-system).

Mit anderen Worten: An eine Funktion, die **IInspectable** erwartet, kann eine Instanz einer beliebigen Laufzeitklasse übergeben werden. Du kannst einer solchen Funktion aber nicht direkt einen Skalarwert (etwa einen Zahl- oder Textwert) übergeben. Skalarwerte müssen innerhalb eines Referenzklassenobjekts platziert werden. Dieser Wrapping-Prozess wird als *Boxing* des Werts bezeichnet.

> [!IMPORTANT]
> Du kannst jeden an eine Windows-Runtime API übergebenen Typ konvertieren (Boxing und Unboxing). Anders ausgedrückt: ein Windows-Runtime-Typ. Zahlen- und Textwerte (Zeichenfolgen) sind die oben genannten Beispiele. Ein weiteres Beispiel ist ein `struct`, das du in IDL definierst. Wenn du versuchst, ein reguläres C++ `struct` (eines, das nicht in IDL definiert ist) per Boxing zu konvertieren, erinnert der Compiler dich daran, dass du nur einen Windows-Runtime-Typ konvertieren darfst. Eine Laufzeitklasse ist ein Windows-Runtime-Typ, aber du kannst natürlich Laufzeitklassen an Windows-Runtime-APIs übergeben, ohne sie zu per Boxing zu konvertieren.

In [C++/WinRT](./intro-to-using-cpp-with-winrt.md) akzeptiert die Funktion [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) einen Skalarwert und gibt ihn als in **IInspectable** geschachtelten Wert zurück. Wenn du ein **IInspectable**-Element wieder in einen Skalarwert konvertieren möchtest (Unboxing), kannst du die Funktionen [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) und [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) verwenden.

## <a name="examples-of-boxing-a-value"></a>Beispiele für das Boxing eines Werts
Die Accessorfunktion [**LaunchActivatedEventArgs::Arguments**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.Arguments) gibt eine [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)-Struktur zurück. Dabei handelt es sich um einen Skalarwert. Wir können diesen **hstring**-Wert mittels Boxing verpacken und an eine Funktion übergeben, die **IInspectable** erwartet:

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    ...
    rootFrame.Navigate(winrt::xaml_typename<BlankApp1::MainPage>(), winrt::box_value(e.Arguments()));
    ...
}
```

Zum Festlegen der Inhaltseigenschaft einer XAML-Schaltfläche ([**Button**](/uwp/api/windows.ui.xaml.controls.button)) muss die Mutatorfunktion [**Button::Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content?) aufgerufen werden. Die Inhaltseigenschaft kannst du mithilfe des folgenden Codes auf einen Zeichenfolgenwert festlegen:

```cppwinrt
Button().Content(winrt::box_value(L"Clicked"));
```

Zuerst wandelt der [**hstring**](/uwp/cpp-ref-for-winrt/hstring)-Konvertierungskonstruktor das Zeichenfolgenliteral in eine **hstring**-Struktur um. Anschließend wird die Überladung von **winrt::box_value** aufgerufen, die eine **hstring**-Struktur akzeptiert.

## <a name="examples-of-unboxing-an-iinspectable"></a>Beispiele für das Unboxing von „IInspectable“
In deinen eigenen Funktionen, die **IInspectable** erwarten, kannst du [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) zum Unboxing und [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) zum Unboxing mit einem Standardwert verwenden.

```cppwinrt
void Unbox(winrt::Windows::Foundation::IInspectable const& object)
{
    hstring hstringValue = unbox_value<hstring>(object); // Throws if object is not a boxed string.
    hstringValue = unbox_value_or<hstring>(object, L"Default"); // Returns L"Default" if object is not a boxed string.
    float floatValue = unbox_value_or<float>(object, 0.f); // Returns 0.0 if object is not a boxed float.
}
```

## <a name="determine-the-type-of-a-boxed-value"></a>Ermitteln der Art eines geschachtelten Werts
Wenn du einen geschachtelten Wert erhältst und nicht sicher bist, welchen Typ er enthält, kannst du die [**IPropertyValue**](/uwp/api/windows.foundation.ipropertyvalue)-Schnittstelle des geschachtelten Werts abfragen und anschließend **Type** dafür aufrufen. (Der Typ muss bekannt sein, um das Unboxing durchführen zu können.) Hier sehen Sie ein Codebeispiel.

`WINRT_ASSERT` ist eine Makrodefinition, die auf [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros) erweitert wird.

```cppwinrt
float pi = 3.14f;
auto piInspectable = winrt::box_value(pi);
auto piPropertyValue = piInspectable.as<winrt::Windows::Foundation::IPropertyValue>();
WINRT_ASSERT(piPropertyValue.Type() == winrt::Windows::Foundation::PropertyType::Single);
```

## <a name="important-apis"></a>Wichtige APIs
* [Schnittstelle „IInspectable“](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [Funktionsvorlage „winrt::box_value“](/uwp/cpp-ref-for-winrt/box-value)
* [Struktur „winrt::hstring“](/uwp/cpp-ref-for-winrt/hstring)
* [Funktionsvorlage „winrt::unbox_value“](/uwp/cpp-ref-for-winrt/unbox-value)
* [Funktionsvorlage „winrt::unbox_value_or“](/uwp/cpp-ref-for-winrt/unbox-value-or)