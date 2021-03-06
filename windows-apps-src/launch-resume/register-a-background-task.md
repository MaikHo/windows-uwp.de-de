---
title: Registrieren einer Hintergrundaufgabe
description: Hier erfahren Sie, wie eine Funktion erstellt wird, die zum sicheren Registrieren der meisten Hintergrundaufgaben wiederverwendet werden kann.
ms.assetid: 8B1CADC5-F630-48B8-B3CE-5AB62E3DFB0D
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Hintergrundaufgabe
ms.localizationpriority: medium
ms.openlocfilehash: 06d9fdfe57ead1e5405a21658654a8992343bb95
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172964"
---
# <a name="register-a-background-task"></a>Registrieren einer Hintergrundaufgabe


**Wichtige APIs**

-   [**BackgroundTaskRegistration-Klasse**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration)
-   [**BackgroundTaskBuilder-Klasse**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
-   [**SystemCondition-Klasse**](/uwp/api/Windows.ApplicationModel.Background.SystemCondition)

Hier erfahren Sie, wie eine Funktion erstellt wird, die zum sicheren Registrieren der meisten Hintergrundaufgaben wiederverwendet werden kann.

Dieses Thema gilt für In-Process-Hintergrundaufgaben sowie für Out-of-Process-Hintergrundaufgaben. In diesem Thema wird davon ausgegangen, dass Sie bereits über eine Hintergrundaufgabe verfügen, die registriert werden muss. (Informationen zum Erstellen einer Hintergrundaufgabe finden Sie unter [Erstellen und Registrieren einer außerhalb des Prozesses ausgeführten Hintergrundaufgabe](create-and-register-a-background-task.md) oder [Erstellen und Registrieren einer In-Process-Hintergrundaufgabe](create-and-register-an-inproc-background-task.md)).

Dieses Thema behandelt schrittweise eine Hilfsfunktion, die Hintergrundaufgaben registriert. Diese Hilfsfunktion sucht zuerst nach vorhandenen Registrierungen, um Probleme mit mehrfachen Registrierungen zu vermeiden. Sie kann außerdem eine Systembedingung auf die Hintergrundaufgabe anwenden. Die exemplarische Vorgehensweise enthält ein vollständiges lauffähiges Beispiel für diese Hilfsfunktion.

**Hinweis**  

Universelle Windows-Apps müssen jedoch [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) aufrufen, bevor Hintergrundtrigger-Typen registriert werden.

Rufen Sie [**RemoveAccess**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess) und anschließend [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) auf, wenn die App nach der Aktualisierung gestartet wird, um sicherzustellen, dass Ihre universelle Windows-App nach der Veröffentlichung eines Updates weiterhin ordnungsgemäß ausgeführt wird. Weitere Informationen finden Sie unter [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md).

## <a name="define-the-method-signature-and-return-type"></a>Definieren von Methodensignatur und Rückgabetyp

Diese Methode übernimmt den Einstiegspunkt der Aufgabe, den Namen der Aufgabe, einen vorgefertigten Trigger für die Hintergrundaufgabe und (optional) eine [**SystemCondition**](/uwp/api/Windows.ApplicationModel.Background.SystemCondition) für die Hintergrundaufgabe. Diese Methode gibt ein [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration)-Objekt zurück.

> [!Important]
> `taskEntryPoint` -bei Hintergrundaufgaben, die in außerhalb des Prozesses ausgeführt werden, muss dieser als Namespace Name, "." und der Name der Klasse erstellt werden, die ihre Hintergrund Klasse enthält. Die Zeichenfolge beachtet die Groß-/Kleinschreibung.  Wenn Sie z. b. den Namespace "mybackgroundtasks" und die Klasse "BackgroundTask1" mit dem Code der Hintergrund Klasse haben, lautet die Zeichenfolge `taskEntryPoint` "mybackgroundtasks. BackgroundTask1".
> Wenn Ihre Hintergrundaufgabe im gleichen Prozess wie Ihre App (d. h. als In-Prozess-Hintergrundaufgabe) ausgeführt wird, sollte `taskEntryPoint` nicht festgelegt werden.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     
>     // We'll add code to this function in subsequent steps.
>
> }
> ```
> ``` cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>     
>     // We'll add code to this function in subsequent steps.
>
> }
> ```

## <a name="check-for-existing-registrations"></a>Überprüfung von vorhandenen Registrierungen

Überprüfen Sie, ob die Aufgabe bereits registriert ist. Diese Überprüfung ist wichtig, denn eine mehrfach registrierte Hintergrundaufgabe wird mehrfach aufgerufen, wenn sie ausgelöst wird. Dies kann die CPU überlasten und zu unerwartetem Verhalten führen.

Sie können die vorhandenen Registrierungen durch Abfrage der [**BackgroundTaskRegistration.AllTasks**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks)-Eigenschaft und Iteration über das Ergebnis überprüfen. Überprüfen Sie die Namen aller Instanzen: wenn ein Name mit dem Namen der Aufgabe übereinstimmt, die registriert werden soll, verlassen Sie die Schleife, und legen Sie eine Flag-Variable fest, damit Ihr Code im nächsten Schritt einen anderen Pfad wählen kann.

> **Hinweis**    Verwenden Sie Namen von Hintergrundaufgaben, die für Ihre APP eindeutig sind. Stellen Sie sicher, dass jede Hintergrundaufgabe einen eindeutigen Namen hat.

Der folgende Code registriert eine Hintergrundaufgabe mit dem im vorigen Schritt erstellten [**SystemTrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTrigger):

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == name)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>     
>     // We'll register the task in the next step.
> }
> ```
> ``` cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>     
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>     
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>         
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>             
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>         
>         hascur = iter->MoveNext();
>     }
>     
>     // We'll register the task in the next step.
> }
> ```

## <a name="register-the-background-task-or-return-the-existing-registration"></a>Registrieren der Hintergrundaufgabe (oder Rückgabe der bereits vorhandenen Registrierung)


Überprüfen Sie, ob sich die Aufgabe in der Liste der bereits registrierten Hintergrundaufgaben befindet. Geben Sie in diesem Fall diese Instanz der Aufgabe zurück.

Registrieren Sie die Aufgabe dann mithilfe eines neuen [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)-Objekts. Der Code sollte überprüfen, ob der Parameter für die Bedingung null ist, und andernfalls die Bedingung an das Registrierungsobjekt anfügen. Geben Sie die von der [**BackgroundTaskBuilder.Register**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register)-Methode zurückgegebene [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) zurück.

> **Hinweis**    Die Registrierungs Parameter für die Hintergrundaufgabe werden zum Zeitpunkt der Registrierung überprüft. Bei ungültigen Registrierungsparametern wird ein Fehler zurückgegeben. Stellen Sie sicher, dass Ihre App problemlos mit Szenarien ohne erfolgreiche Registrierung von Hintergrundaufgaben zurechtkommt. Andernfalls stürzt die App unter Umständen ab, wenn sie so konzipiert ist, dass nach dem Versuch, eine Aufgabe zu registrieren, ein gültiges Registrierungsobjekt vorhanden sein muss.
> **Hinweis** Wenn Sie eine Hintergrundaufgabe registrieren, die im gleichen Prozess wie Ihre-App ausgeführt wird, senden Sie `String.Empty` oder `null` als `taskEntryPoint`-Parameter.

Im folgenden Beispiel wird entweder die vorhandene Aufgabe zurückgegeben, oder es wird Code hinzugefügt, mit dem die Hintergrundaufgabe registriert wird (ggf. einschließlich der Systembedingung):

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == taskName)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>
>     //
>     // Register the background task.
>     //
>
>     var builder = new BackgroundTaskBuilder();
>
>     builder.Name = name;
>
>     // in-process background tasks don't set TaskEntryPoint
>     if ( taskEntryPoint != null && taskEntryPoint != String.Empty)
>     {
>         builder.TaskEntryPoint = taskEntryPoint;
>     }
>     builder.SetTrigger(trigger);
>
>     if (condition != null)
>     {
>         builder.AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration task = builder.Register();
>
>     return task;
> }
> ```
> ``` cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>
>         hascur = iter->MoveNext();
>     }
>
>     //
>     // Register the background task.
>     //
>
>     auto builder = ref new BackgroundTaskBuilder();
>
>     builder->Name = name;
>     builder->TaskEntryPoint = taskEntryPoint;
>     builder->SetTrigger(trigger);
>
>     if (condition != nullptr) {
>         
>         builder->AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration ^ task = builder->Register();
>
>     return task;
> }
> ```

## <a name="complete-background-task-registration-utility-function"></a>Hilfsfunktion zur Registrierung der abgeschlossenen Hintergrundaufgabe

Dieses Beispiel zeigt die Hilfsfunktion zur Registrierung der abgeschlossenen Hintergrundaufgabe. Diese Funktion kann zum Registrieren der meisten Hintergrundaufgaben mit Ausnahme von Hintergrundaufgaben im Netzwerk verwendet werden.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> //
> // Register a background task with the specified taskEntryPoint, name, trigger,
> // and condition (optional).
> //
> // taskEntryPoint: Task entry point for the background task.
> // taskName: A name for the background task.
> // trigger: The trigger for the background task.
> // condition: Optional parameter. A conditional event that must be true for the task to fire.
> //
> public static BackgroundTaskRegistration RegisterBackgroundTask(string taskEntryPoint,
>                                                                 string taskName,
>                                                                 IBackgroundTrigger trigger,
>                                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == taskName)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>
>     //
>     // Register the background task.
>     //
>
>     var builder = new BackgroundTaskBuilder();
>
>     builder.Name = taskName;
>     builder.TaskEntryPoint = taskEntryPoint;
>     builder.SetTrigger(trigger);
>
>     if (condition != null)
>     {
>
>         builder.AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration task = builder.Register();
>
>     return task;
> }
> ```
> ``` cpp
> //
> // Register a background task with the specified taskEntryPoint, name, trigger,
> // and condition (optional).
> //
> // taskEntryPoint: Task entry point for the background task.
> // taskName: A name for the background task.
> // trigger: The trigger for the background task.
> // condition: Optional parameter. A conditional event that must be true for the task to fire.
> //
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(Platform::String ^ taskEntryPoint,
>                                                              Platform::String ^ taskName,
>                                                              IBackgroundTrigger ^ trigger,
>                                                              IBackgroundCondition ^ condition)
> {
>
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>
>         hascur = iter->MoveNext();
>     }
>
>
>     //
>     // Register the background task.
>     //
>
>     auto builder = ref new BackgroundTaskBuilder();
>
>     builder->Name = name;
>     builder->TaskEntryPoint = taskEntryPoint;
>     builder->SetTrigger(trigger);
>
>     if (condition != nullptr) {
>
>         builder->AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration ^ task = builder->Register();
>
>     return task;
> }
> ```

## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md)
* [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb von Prozessen](create-and-register-an-inproc-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)
* [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [Gewusst wie: Starten von Suspend-, Resume-und Background-Ereignissen in UWP-Apps (beim Debuggen)](/previous-versions/hh974425(v=vs.110))