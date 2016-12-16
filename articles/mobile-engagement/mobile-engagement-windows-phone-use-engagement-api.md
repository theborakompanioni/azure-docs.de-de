---
title: Verwendung der Engagement-API auf Windows Phone
description: Verwendung der Engagement-API auf Windows Phone
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: dwrede
editor: 
ms.assetid: ae2ba2e8-f75b-4dee-a164-a7dd65d35a23
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: f8ed91048ecfb1e7d19bb000216618b4e0c4b381


---
# <a name="how-to-use-the-engagement-api-on-windows-phone-silverlight"></a>Verwendung der Engagement-API auf Windows Phone
Dieses Dokument ist ein Zusatz zum Dokument [Integration von Mobile Engagement in Ihrer Windows Phone Silverlight-App](mobile-engagement-windows-phone-integrate-engagement.md). Es bietet tiefergehende Details zur Verwendung der Engagement-API, um Ihre Anwendungsstatistik zu melden.

Wenn Engagement nur die Sitzungsaktivitäten der Anwendung, Abstürze und technische Informationen melden soll, ist es am einfachsten, wenn alle Ihre `PhoneApplicationPage`-Unterklassen von der `EngagementPage`-Klasse erben.

Wenn Sie darüber hinaus noch mehr Meldungen wünschen, z. B. wenn Sie anwendungsspezifische Ereignisse, Fehler und Aufträge melden möchten, oder wenn die Aktivitäten Ihrer Anwendung anders als in den `EngagementPage`-Klassen implementiert gemeldet werden sollen, dann müssen Sie die Engagement-API verwenden.

Die Engagement-API wird von der `EngagementAgent` -Klasse zur Verfügung gestellt. Sie können über `EngagementAgent.Instance`auf diese Methoden zugreifen.

Auch wenn das Agent-Modul nicht initialisiert wurde, wird jeder Aufruf zur API verzögert und erneut ausgeführt, sobald der Agent verfügbar ist.

## <a name="engagement-concepts"></a>Engagement-Konzepte
In den folgenden Abschnitten werden die Mobile Engagement-Konzepte für die Windows Phone-Plattform genauer dargestellt.

### <a name="session-and-activity"></a>`Session` und `Activity`
Eine *Aktivität* ist normalerweise einer Seite der Anwendung zugeordnet, d.h. die *Aktivität* beginnt, wenn die Seite angezeigt wird, und wird beendet, wenn die Seite geschlossen wird: Dies ist der Fall, wenn das Engagement-SDK unter Verwendung der `EngagementPage`-Klasse integriert wurde.

*Aktivitäten* können aber auch manuell mithilfe der Engagement-API gesteuert werden. Auf diese Weise kann eine bestimmte Seite in mehrere Teile unterteilt werden, um weitere Informationen über die Verwendung dieser Seite zu erhalten (z. B. um zu erfahren, wie oft und wie lange Dialoge innerhalb dieser Seite verwendet werden).

## <a name="reporting-activities"></a>Berichterstellung für Aktivitäten
### <a name="user-starts-a-new-activity"></a>Benutzer startet eine neue Aktivität
#### <a name="reference"></a>Referenz
            void StartActivity(string name, Dictionary<object, object> extras = null)

Sie müssen jedes Mal `StartActivity()` aufrufen, wenn sich die Benutzeraktivität ändert. Der erste Aufruf dieser Funktion startet eine neue Benutzersitzung.

> [!IMPORTANT]
> Das SDK ruft automatisch die EndActivity-Methode auf, wenn die Anwendung geschlossen wird. Daher wird DRINGEND empfohlen, die StartActivity-Methode aufzurufen, sobald sich die Aktivität des Benutzers ändert, und die EndActivity-Methode NIE aufzurufen, weil dadurch ein Beenden der aktuellen Sitzung erzwungen wird.
> 
> 

#### <a name="example"></a>Beispiel
            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Der Benutzer beendet seine aktuelle Aktivität
#### <a name="reference"></a>Referenz
            void EndActivity()

Sie müssen `EndActivity()` mindestens einmal aufrufen, wenn der Benutzer seine letzte Aktivität beendet. Dadurch wird das Engagement-SDK darüber informiert, dass sich der Benutzer derzeit im Leerlauf befindet und die Benutzersitzung geschlossen werden muss, sobald das Sitzungszeitlimit abläuft (wenn Ihr Aufruf `StartActivity()` von vor dem Sitzungszeitlimit abläuft, wird die Sitzung einfach fortgesetzt).

#### <a name="example"></a>Beispiel
            EngagementAgent.Instance.EndActivity();

## <a name="reporting-jobs"></a>Berichterstellung für Aufträge
### <a name="start-a-job"></a>Starten eines Auftrags
#### <a name="reference"></a>Referenz
            void StartJob(string name, Dictionary<object, object> extras = null)

Sie können den Auftrag verwenden, um bestimmte Aufgaben eine Zeit lang nachzuverfolgen.

#### <a name="example"></a>Beispiel
            // An upload begins...

            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");

            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a>Beenden eines Auftrags
#### <a name="reference"></a>Referenz
            void EndJob(string name)

Sobald eine Aufgabe, die von einem Auftrag nachverfolgt wird, beendet ist, sollten Sie die EndJob-Methode für diesen Auftrag aufrufen, indem Sie den Auftragsnamen angeben.

#### <a name="example"></a>Beispiel
            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends

            EngagementAgent.Instance.EndJob("uploadData");

## <a name="reporting-events"></a>Berichterstellung für Ereignisse
Es gibt drei Arten von Ereignissen:

* Eigenständige Ereignisse
* Sitzungsereignisse
* Auftragsereignisse

### <a name="standalone-events"></a>Eigenständige Ereignisse
#### <a name="reference"></a>Referenz
            void SendEvent(string name, Dictionary<object, object> extras = null)

Eigenständige Ereignisse können außerhalb des Kontexts einer Sitzung auftreten.

#### <a name="example"></a>Beispiel
            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a>Sitzungsereignisse
#### <a name="reference"></a>Referenz
            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

Sitzungsereignisse werden normalerweise verwendet, um die Aktionen eines Benutzers während seiner Sitzung zu melden.

#### <a name="example"></a>Beispiel
**Ohne Daten:**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");

            // or

            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**Mit Daten:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a>Auftragsereignisse
#### <a name="reference"></a>Referenz
            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

Auftragsereignisse werden normalerweise verwendet, um die Aktionen eines Benutzers während eines Auftrags zu melden.

#### <a name="example"></a>Beispiel
            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

## <a name="reporting-errors"></a>Melden von Fehlern
Es gibt drei Arten von Fehlern:

* Eigenständige Fehler
* Sitzungsfehler
* Auftragsfehler

### <a name="standalone-errors"></a>Eigenständige Fehler
#### <a name="reference"></a>Referenz
            void SendError(string name, Dictionary<object, object> extras = null)

Im Gegensatz zu Sitzungsfehlern können eigenständige Fehler außerhalb des Kontexts einer Sitzung auftreten.

#### <a name="example"></a>Beispiel
            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a>Sitzungsfehler
#### <a name="reference"></a>Referenz
            void SendSessionError(string name, Dictionary<object, object> extras = null)

Sitzungsfehler werden normalerweise zum Melden der Fehler verwendet, die Auswirkungen auf den Benutzer während seiner Sitzung haben.

#### <a name="example"></a>Beispiel
            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a>Auftragsfehler
#### <a name="reference"></a>Referenz
            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

Fehler können mit einem ausgeführten Auftrag in Zusammenhang stehen anstatt mit der aktuellen Benutzersitzung.

#### <a name="example"></a>Beispiel
            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

## <a name="reporting-crashes"></a>Berichterstellung von Abstürzen
Der Agent stellt zwei Methoden für den Umgang mit Abstürzen zur Verfügung.

### <a name="send-an-exception"></a>Senden einer Ausnahme
#### <a name="reference"></a>Referenz
            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a>Beispiel
Sie können eine Ausnahme jederzeit durch folgenden Aufruf senden:

            EngagementAgent.Instance.SendCrash(aCatchedException);

Sie können auch einen optionalen Parameter zum Beenden der Engagement-Sitzung zur gleichen Zeit wie bei der Übermittlung des Absturzes verwenden. Hierzu rufen Sie Folgendes auf:

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Wenn Sie dies tun, werden die Sitzung und die Aufträge direkt nach dem Senden des Absturzes geschlossen.

### <a name="send-an-unhandled-exception"></a>Senden Sie eine nicht behandelte Ausnahme
#### <a name="reference"></a>Referenz
            void SendCrash(ApplicationUnhandledExceptionEventArgs e)

Engagement bietet auch die Möglichkeit, unbehandelte Ausnahmen zu senden. Dies ist besonders bei Verwendung im Silverlight-UnhandledException-Ereignishandler nützlich.

Durch diese Methode werden **IMMER** die Engagement-Sitzungen und -Aufträge nach dem Aufrufen beendet.

#### <a name="example"></a>Beispiel
Sie können sie verwenden, um Ihren eigenen UnhandledException-Handler zu implementieren (insbesondere, wenn Sie die automatische Absturzberichtsfunktion von Engagement deaktiviert haben). Beispielsweise in der Methode `Application_UnhandledException` der Datei `App.xaml.cs`:

            // In your App.xaml.cs file

            // Code to execute on Unhandled Exceptions
            private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
            {
              // your own code

              EngagementAgent.Instance.SendCrash(e);
            }

## <a name="onactivated"></a>OnActivated
### <a name="reference"></a>Referenz
            void OnActivated(ActivatedEventArgs e)

Wenn der Benutzer weiter navigiert, weg von einer Anwendung, nachdem das Deaktiviert-Ereignis aufgetreten ist, versucht das Betriebssystem, die Anwendung in den Ruhezustand zu versetzen. Dann wird die Anwendung als veraltet markiert. In diesem Prozess wird eine Anwendung beendet, aber einige Daten zum Zustand der Anwendung und die einzelnen Seiten innerhalb der Anwendung bleiben erhalten.

Sie müssen in die `Application_Activated`-Methode aus der App.xaml.cs-Datei `EngagementAgent.Instance.OnActivated(e)` eingeben, um den Engagement-Agent zurückzusetzen, wenn die Anwendung als veraltet markiert wurde.

### <a name="example"></a>Beispiel
            // Inside your App.xaml.cs file

            // Code to execute when the application is activated (brought to foreground)
            // This code will not execute when the application is first launched
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
              EngagementAgent.Instance.OnActivated(e);
            }

## <a name="device-id"></a>Geräte-ID
            String GetDeviceId()

Sie können die Engagement-Geräte-ID durch Aufrufen dieser Methode abrufen.

## <a name="extras-parameters"></a>Extras-Parameter
Einem Ereignis, einem Fehler, einer Aktivität oder einem Auftrag können beliebige Daten zugeordnet werden. Diese Daten können unter Verwendung eines Wörterbuchs strukturiert werden. Schlüssel und Werte können einen beliebigen Typ aufweisen.

Extras-Daten werden serialisiert; wenn Sie also einen eigenen Typ in Extras einfügen möchten, müssen Sie einen Datenvertrag für diesen Typ hinzufügen.

### <a name="example"></a>Beispiel
Erstellen Sie die neue Klasse "Person".

            using System.Runtime.Serialization;

            namespace Engagement.Agent
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }

                // Properties

                [DataMember]
                public int Age
                {
                  get;
                  set;
                }

                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

Fügen Sie anschließend eine `Person` -Instanz zu einem Extra hinzu.

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);

            EngagementAgent.Instance.SendEvent("Event", extras);

> [!WARNING]
> Wenn Sie andere Objekttypen einfügen, stellen Sie sicher, dass deren ToString()-Methode implementiert wird, sodass eine visuell lesbare Zeichenfolge zurückgegeben wird.
> 
> 

### <a name="limits"></a>Grenzen
#### <a name="keys"></a>Schlüssel
Jeder Schlüssel in dem Objekt muss mit dem folgenden regulären Ausdruck übereinstimmen:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Das bedeutet, dass Schlüssel mit mindestens einem Buchstaben beginnen müssen, gefolgt von Buchstaben, Ziffern oder Unterstrichen (\_).

#### <a name="size"></a>Größe
Extras sind beschränkt auf **1024** Zeichen pro Aufruf.

## <a name="reporting-application-information"></a>Informationen zur Berichterstellung für Anwendungen
### <a name="reference"></a>Referenz
            void SendAppInfo(Dictionary<object, object> appInfos)

Sie können Nachverfolgungsinformationen (oder beliebige andere anwendungsspezifische Informationen) manuell mithilfe der SendAppInfo()-Funktion melden.

Beachten Sie, dass diese Informationen inkrementell gesendet werden können: Nur der letzte Wert für einen bestimmten Schlüssel wird für ein bestimmtes Gerät gespeichert. Verwenden Sie wie bei Ereignisextras ein Dictionary\<Objekt, Objekt\>-Element, um Informationen anzufügen.

### <a name="example"></a>Beispiel
            Dictionary<object, object> appInfo = new Dictionary<object, object>()
            {
               {"subscription", "2013-12-07"},
               {"premium", "true"}
            };

            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Grenzen
#### <a name="keys"></a>Schlüssel
Jeder Schlüssel in dem Objekt muss mit dem folgenden regulären Ausdruck übereinstimmen:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Das bedeutet, dass Schlüssel mit mindestens einem Buchstaben beginnen müssen, gefolgt von Buchstaben, Ziffern oder Unterstrichen (\_).

#### <a name="size"></a>Größe
Anwendungsinformationen sind beschränkt auf **1024** Zeichen pro Aufruf.

Im vorherigen Beispiel enthält die an den Server gesendete JSON 44 Zeichen:

            {"subscription":"2013-12-07","premium":"true"}

## <a name="logging"></a>Protokollierung
### <a name="enable-logging"></a>Aktivieren der Protokollierung
Das SDK kann zum Erzeugen von Testprotokollen in der IDE-Konsole konfiguriert werden.
Diese Protokolle sind nicht standardmäßig aktiviert. Um diese anzupassen, aktualisieren Sie die Eigenschaft `EngagementAgent.Instance.TestLogEnabled` auf einen der aus der `EngagementTestLogLevel`-Enumeration verfügbaren Werte, beispielsweise:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();



<!--HONumber=Nov16_HO3-->


