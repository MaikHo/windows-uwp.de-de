---
title: So wird's gemacht - Aktivieren einer App (DirectX und C++)
description: In diesem Thema erfahren Sie, wie Sie die Aktivierungsbenutzeroberfläche für eine DirectX-App für die Universelle Windows-Plattform (UWP) definieren.
ms.assetid: b07c7da1-8a5e-5b57-6f77-6439bf653a53
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, DirectX, Activation
ms.localizationpriority: medium
ms.openlocfilehash: 839cfc718e6225beb14df535bc48f9ba6f3f6dc5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173144"
---
# <a name="how-to-activate-an-app-directx-and-c"></a>So wird's gemacht - Aktivieren einer App (DirectX und C++)



In diesem Thema erfahren Sie, wie Sie die Aktivierungsbenutzeroberfläche für eine DirectX-App für die Universelle Windows-Plattform (UWP) definieren.

## <a name="register-the-app-activation-event-handler"></a>Registrieren des Ereignishandlers für die Aktivierung der App


Registrieren Sie zuerst die Behandlung des [**CoreApplicationView::Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated)-Ereignisses, das ausgelöst wird, wenn die App vom Betriebssystem gestartet und initialisiert wird.

Fügen Sie Ihrer Implementierung der [**IFrameworkView::Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)-Methode Ihres Ansichtsanbieters (in diesem Beispiel **MyViewProvider**) folgenden Code hinzu:

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
    // Register event handlers for the app lifecycle. This example includes Activated, so that we
    // can make the CoreWindow active and start rendering on the window.
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);
  
  //...

}
```

## <a name="activate-the-corewindow-instance-for-the-app"></a>Aktivieren der CoreWindow-Instanz für die App


Beim Start der App müssen Sie einen Verweis auf die [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)-Instanz für Ihre App abrufen. **CoreWindow** enthält den Meldungsverteiler für Fensterereignisse, mit dem die App Fensterereignisse verarbeitet. Rufen Sie diesen Verweis im Rückruf für das App-Aktivierungsereignis durch Aufrufen von [**CoreWindow::GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) ab. Wenn Sie diesen Verweis abgerufen haben, aktivieren Sie das Hauptfenster der App durch Aufrufen von [**CoreWindow::Activate**](/uwp/api/windows.ui.core.corewindow.activate).

```cpp
void App::OnActivated(CoreApplicationView^ applicationView, IActivatedEventArgs^ args)
{
    // Run() won't start until the CoreWindow is activated.
    CoreWindow::GetForCurrentThread()->Activate();
}
```

## <a name="start-processing-event-message-for-the-main-app-window"></a>Beginn der Verarbeitung von Ereignismeldungen für das Hauptfenster der App


Ihre Rückrufe treten als Ereignismeldungen auf, die vom [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)-Element des [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)-Elements der App verarbeitet werden. Dieser Rückruf wird nicht aufgerufen, wenn Sie in der Hauptschleife der App (in der [**IFrameworkView::Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run)-Methode des Ansichtsanbieters implementiert) [**CoreDispatcher::ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) nicht aufrufen.

``` syntax
// This method is called after the window becomes active.
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

## <a name="related-topics"></a>Zugehörige Themen


* [So wird's gemacht: Anhalten einer App (DirectX und C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [So wird's gemacht - Reaktivieren einer App (DirectX und C++)](how-to-resume-an-app-directx-and-cpp.md)

 

 