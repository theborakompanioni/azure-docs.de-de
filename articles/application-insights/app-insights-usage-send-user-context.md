Titel: Senden von Benutzerkontext-IDs zur Nutzung in Azure Application Insights | Microsoft-Dokumentation Beschreibung: Verfolgen Sie nach, welche Vorgänge Benutzer in Ihrem Dienst durchführen, indem Sie ihnen eine eindeutige, persistente ID-Zeichenfolge in Application Insights zuweisen.
services: application-insights documentationcenter: '' author: abgreg manager: carmonm

ms.service: application-insights ms.workload: tbd ms.tgt_pltfrm: ibiza ms.devlang: csharp ms.topic: article ms.date: 08/02/2017 ms.author: bwren

---
#  <a name="send-user-context-ids-to-enable-usage-experiences-in-azure-application-insights"></a>Senden von Benutzerkontext-IDs zur Nutzung in Azure Application Insights

## <a name="tracking-users"></a>Nachverfolgen von Benutzern

Mithilfe von Application Insights können Sie Ihre Benutzer durch eine Reihe von Produktnutzungstools überwachen und nachverfolgen: 
* [Benutzer, Sitzungen, Ereignisse](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
* [Trichter](https://docs.microsoft.com/azure/application-insights/usage-funnels)
* [Aufbewahrung](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention)
* Kohorten
* [Arbeitsmappen](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

Um nachzuverfolgen, welche Vorgänge ein Benutzer im Laufe der Zeit durchführt, erfordert Application Insights eine ID für jeden Benutzer bzw. jede Sitzung. Schließen Sie die folgenden IDs in jedem benutzerdefinierten Ereignis bzw. in jeder Seitenansicht ein.
- Benutzer, Trichter, Vermerkdauer und Kohorten: Benutzer-ID einschließen
- Sitzungen: Sitzungs-ID einschließen

Wenn Ihre App in das [JavaScript-SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page) integriert ist, wird automatisch die Benutzer-ID nachverfolgt.

## <a name="choosing-user-ids"></a>Auswählen von Benutzer-IDs

Benutzer-IDs sollten benutzersitzungsübergreifend beibehalten werden, um das Benutzerverhalten im Laufe der Zeit nachzuverfolgen. Es gibt verschiedene Methoden zur Beibehaltung der ID.
- Behalten Sie sie als Definition eines Benutzers bei, der bereits in Ihrem Dienst vorhanden ist.
- Wenn der Dienst Zugriff auf einen Browser hat, kann ein Cookie mit einer ID an den Browser übergeben werden. Die ID wird solange beibehalten wie das Cookie im Browser des Benutzers.
- Bei Bedarf können Sie für jede Sitzung eine neue ID verwenden, die Ergebnisse bezüglich der Benutzer sind jedoch begrenzt. Beispielsweise können Sie nicht feststellen, inwiefern sich das Verhalten eines Benutzers im Laufe der Zeit ändert.

Bei der ID muss es sich um eine GUID oder eine andere komplexe Zeichenfolge handeln, die den Benutzer eindeutig identifiziert. Verwenden Sie beispielsweise eine lange Zufallszahl.

Eine ID, die personenbezogene Informationen über den Benutzer enthält, ist kein geeigneter Wert, der als Benutzer-ID an Application Insights gesendet werden kann. Sie können diese ID zwar als [authentifizierte Benutzer-ID](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users) senden, sie erfüllt jedoch nicht die Anforderung an Benutzer-ID für Verwendungsszenarien.

## <a name="aspnet-apps-setting-the-user-context-in-an-itelemetryinitializer"></a>ASP.NET-Apps: Festlegen des Benutzerkontexts in einer ITelemetryInitializer-Eigenschaft

Erstellen Sie einen Telemetrieinitialisierer, wie [hier](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer) detailliert beschrieben wird, und legen Sie die Eigenschaften „Context.User.Id“ und „Context.Session.Id“ fest.

In diesem Beispiel wird die Benutzer-ID auf einen Bezeichner festgelegt, der nach der Sitzung abläuft. Wenn möglich, verwenden Sie eine Benutzer-ID, die sitzungsübergreifend beibehalten wird.

```C#

    using System;
    using System.Web;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that sets the user ID.
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            // For a full experience, track each user across sessions. For an incomplete view of user 
            // behavior within a session, store user ID on the HttpContext Session.
            // Set the user ID if we haven't done so yet.
            if (HttpContext.Current.Session["UserId"] == null)
            {
                HttpContext.Current.Session["UserId"] = Guid.NewGuid();
            }

            // Set the user id on the Application Insights telemetry item.
            telemetry.Context.User.Id = (string)HttpContext.Current.Session["UserId"];

            // Set the session id on the Application Insights telemetry item.
            telemetry.Context.Session.Id = HttpContext.Current.Session.SessionID;
        }
      }
    }
```

## <a name="next-steps"></a>Nächste Schritte
- Um mit der Nutzung zu beginnen, senden Sie [benutzerdefinierte Ereignisse](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) oder [Seitenansichten](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Wenn Sie bereits benutzerdefinierte Ereignisse oder Seitenansichten senden, finden Sie mithilfe der Nutzungstools heraus, wie Benutzer den Dienst verwenden.
    * [Nutzungsübersicht](app-insights-usage-overview.md)
    * [Benutzer-, Sitzungs- und Ereignisanalyse in Azure Application Insights](app-insights-usage-segmentation.md)
    * [Trichter](usage-funnels.md)
    * [Aufbewahrung](app-insights-usage-retention.md)
    * [Arbeitsmappen](app-insights-usage-workbooks.md)
