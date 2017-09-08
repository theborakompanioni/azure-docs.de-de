---
title: "Application Insights für die Problembehandlung von benutzerdefinierten Richtlinien: Azure Active Directory B2C | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie Application Insights zum Nachverfolgen der Ausführung von benutzerdefinierten Richtlinien einrichten."
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: saeda
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: 8c79df33cd5f04f490e2cc6372f7e8ac1c4d9bbe
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---

# <a name="azure-active-directory-b2c-collecting-logs"></a>Azure Active Directory B2C: Erfassen von Protokollen

Dieser Artikel behandelt Schritte zum Sammeln von Protokollen von Azure AD B2C, mit denen Sie Probleme mit Ihren benutzerdefinierten Richtlinien diagnostizieren können.

>[!NOTE]
>Aktuell dienen die hier detailliert beschriebenen Aktivitätsprotokolle **NUR** zur Entwicklung von benutzerdefinierten Richtlinien. Verwenden Sie den Entwicklungsmodus nicht in der Produktion.  Protokolle erfassen alle Ansprüche, die während der Entwicklung an die Identitätsanbieter und von Ihnen gesendet werden.  Bei Verwendung in der Produktion übernehmen Entwickler die Verantwortung für PII (Privately Identifiable Information, privat identifizierbare Informationen), die in dem App Insights-Protokoll gesammelt werden, das sie besitzen.  Diese detaillierten Protokolle werden nur gesammelt, wenn die Richtlinie sich im **ENTWICKLUNGSMODUS** befindet.


## <a name="use-application-insights"></a>Verwenden von Application Insights

Azure AD B2C unterstützt eine Funktion zum Senden von Daten an Application Insights.  Application Insights bietet eine Möglichkeit zum Diagnostizieren von Ausnahmen und Visualisieren von Leistungsproblemen der Anwendung.

### <a name="setup-application-insights"></a>Einrichten von Application Insights

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Stellen Sie sicher, dass Sie sich im Mandanten mit Ihrem Azure-Abonnement (nicht Ihrem Azure AD B2C-Mandanten) befinden.
1. Klicken Sie im linken Navigationsbereich auf **+ Neu**.
1. Suchen Sie nach **Application Insights**, wählen Sie die Option aus, und klicken Sie dann auf **Erstellen**.
1. Füllen Sie das Formular aus, und klicken Sie auf **Erstellen**. Wählen Sie als **Anwendungstyp** die Option **Allgemein** aus.
1. Nachdem die Ressource erstellt wurde, öffnen Sie die Application Insights-Ressource.
1. Suchen Sie im linken Menü nach **Eigenschaften**, und klicken Sie darauf.
1. Kopieren Sie den **Instrumentierungsschlüssel**, und speichern Sie ihn für die nächste Aktion.

### <a name="set-up-the-custom-policy"></a>Einrichten der benutzerdefinierten Richtlinie

1. Öffnen Sie die RP-Datei (z.B. „SignUpOrSignin.xml“).
1. Fügen Sie dem `<TrustFrameworkPolicy>`-Element die folgenden Attribute hinzu:

  ```XML
  DeploymentMode="Development"
  UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  ```

1. Wenn er noch nicht vorhanden ist, fügen Sie dem Knoten `<RelyingParty>` den untergeordneten Knoten `<UserJourneyBehaviors>` hinzu. Er muss direkt nach `<DefaultUserJourney ReferenceId="YourPolicyName" />` angeordnet werden.
2. Fügen Sie den folgenden Knoten als untergeordnetes Element des `<UserJourneyBehaviors>`-Elements hinzu. Achten Sie darauf, `{Your Application Insights Key}` durch den **Instrumentierungsschlüssel** zu ersetzen, den Sie im vorherigen Abschnitt von Application Insights erhalten haben.

  ```XML
  <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
  ```

  * `DeveloperMode="true"` weist Application Insights an, die Telemetrie über die Verarbeitungspipeline zu beschleunigen. Dies ist gut für die Entwicklung, bei hohem Datenvolumen jedoch nicht zu empfehlen.
  * `ClientEnabled="true"` sendet das clientseitige Application Insights-Skript zum Nachverfolgen der Seitenansicht und von clientseitigen Fehlern (nicht erforderlich).
  * `ServerEnabled="true"` sendet die vorhandene Datei „UserJourneyRecorder.json“ als benutzerdefiniertes Ereignis an Application Insights.
Beispiel:

  ```XML
  <TrustFrameworkPolicy
    ...
    TenantId="fabrikamb2c.onmicrosoft.com"
    PolicyId="SignUpOrSignInWithAAD"
    DeploymentMode="Development"
    UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  >
    ...
    <RelyingParty>
      <DefaultUserJourney ReferenceId="YourPolicyName" />
      <UserJourneyBehaviors>
        <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
      </UserJourneyBehaviors>
      ...
  </TrustFrameworkPolicy>
  ```

3. Laden Sie die Richtlinie hoch.

### <a name="see-the-logs-in-application-insights"></a>Anzeigen von Protokollen in Application Insights

>[!NOTE]
> Es gibt eine kurze Verzögerung (weniger als fünf Minuten), bevor Sie neue Protokolle in Application Insights anzeigen können.

1. Öffnen Sie die zuvor erstellte Application Insights-Ressource im [Azure-Portal](https://portal.azure.com).
1. Klicken Sie im Menü **Übersicht** auf **Analytics**.
1. Öffnen Sie eine neue Registerkarte in Application Insights.
1. Es folgt eine Liste der Abfragen, mit deren Hilfe Sie die Protokolle anzeigen können.

| Abfrage | Beschreibung |
|---------------------|--------------------|
traces | Anzeigen aller von Azure AD B2C generierten Protokolle |
traces \| where timestamp > ago(1d) | Anzeigen aller am letzten Tag von Azure AD B2C generierten Protokolle

Diese Einträge können ggf. lang sein.  Führen Sie einen Export in eine CSV-Datei durch, um sich dies genauer anzusehen.

Weitere Informationen zum Tool Analytics finden Sie [hier](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).

>[!NOTE]
>In der Community wurde ein User Journey-Viewer entwickelt, der als Hilfe für Identitätsentwickler dient.  Er wird von Microsoft nicht unterstützt und unverändert zur Verfügung gestellt.  Mit dem Viewer werden Daten aus Ihrer Application Insights-Instanz gelesen, und es wird eine gut strukturierte Ansicht der User Journey-Ereignisse bereitgestellt.  Sie können den Quellcode abrufen und in Ihrer eigenen Lösung bereitstellen.

>[!NOTE]
>Aktuell dienen die hier detailliert beschriebenen Aktivitätsprotokolle **NUR** zur Entwicklung von benutzerdefinierten Richtlinien. Verwenden Sie den Entwicklungsmodus nicht in der Produktion.  Protokolle erfassen alle Ansprüche, die während der Entwicklung an die Identitätsanbieter und von Ihnen gesendet werden.  Bei Verwendung in der Produktion übernehmen Entwickler die Verantwortung für PII (Privately Identifiable Information, privat identifizierbare Informationen), die in dem App Insights-Protokoll gesammelt werden, das sie besitzen.  Diese detaillierten Protokolle werden nur gesammelt, wenn die Richtlinie sich im **ENTWICKLUNGSMODUS** befindet.

[GitHub-Repository mit Beispielen für nicht unterstützte benutzerdefinierte Richtlinien und verwandte Tools](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies)



## <a name="next-steps"></a>Nächste Schritte

Untersuchen Sie die Daten in Application Insights, um besser zu verstehen, wie die zugrunde liegende B2C-Funktion arbeitet, um Ihre eigenen Identitätserfahrungen zu vermitteln.

