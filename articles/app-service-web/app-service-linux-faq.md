---
title: "Häufig gestellte Fragen zu Azure App Service-Web-Apps unter Linux | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zu Azure App Service-Web-Apps unter Linux"
keywords: "Azure App Service, Web-App, häufig gestellte Fragen, Linux, OSS"
services: app-service
documentationCenter: 
authors: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2017
ms.author: aelnably
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: d9410448952438d6b9d437b7ca8823d4f196a2d6
ms.lasthandoff: 04/22/2017


---

# <a name="azure-app-service-web-apps-on-linux-faq"></a>Häufig gestellte Fragen zu Azure App Service-Web-Apps unter Linux

Mit der Veröffentlichung von Azure App Service unter Linux (derzeit als Vorschau verfügbar) erweitern wir unsere Plattform um Funktionen und Verbesserungen. Im Folgenden finden Sie einige häufig gestellte Fragen (FAQ) von unseren Kunden aus den letzten Monaten.
Wenn Sie eine Frage haben, kommentieren Sie den Artikel, und wir werden so bald wie möglich antworten.

## <a name="built-in-images"></a>Integrierte Images

**F:** Ich möchte die integrierten Docker-Container nutzen, die von der Plattform bereitgestellt werden. Wo finde ich die Dateien?

**A:** Sie finden alle Docker-Dateien auf [GitHub](https://github.com/azure-app-service). Sie finden alle Docker-Container auf [Docker Hub](https://hub.docker.com/u/appsvc/).

**F:** Welche Werte sind beim Konfigurieren des Laufzeitstapels im Abschnitt „Startdatei“ anzugeben?

**A:** Für Node.js geben Sie die PM2-Konfigurationsdatei oder Ihre Skriptdatei an. Für .NET Core geben Sie den Namen der kompilierten DLL-Datei an. Für Ruby können Sie das Ruby-Skript angeben, mit dem Ihre App initialisiert werden soll.

## <a name="management"></a>Verwaltung

**F:** Ich habe im Azure-Portal auf „Neu starten“ geklickt, doch meine Web-App wurde nicht neu gestartet. Woran liegt das?

**A:** Wir arbeiten daran, die Schaltfläche „Neu starten“ in der nahen Zukunft zu aktivieren. In der Zwischenzeit haben Sie zwei Möglichkeiten:
- Fügen Sie der Anwendung eine Dummyeinstellung hinzu, oder ändern Sie eine solche Einstellung. Dadurch wird der Neustart der Web-App erzwungen.
- Beenden und starten Sie Ihre Web-App.

**F:** Kann ich über Secure Shell (SSH) eine Verbindung mit dem virtuellen Computer (VM) des App-Containers herstellen?

**A:** Nein. Wir werden in einer zukünftigen Version eine Möglichkeit bereitstellen, über SSH eine Verbindung mit Ihrem App-Container herzustellen.

## <a name="continuous-integrationdeployment"></a>Continuous Integration/Bereitstellung

**F:** Meine Web-App verwendet nach dem Aktualisieren des Images auf DockerHub noch ein altes Docker-Containerimage. Unterstützen Sie Continuous Integration/Bereitstellung von benutzerdefinierten Containern?

**A:** Sie können den Container aktualisieren, indem Sie Ihre Web-App beenden und dann wieder starten. Sie können auch eine Dummyeinstellung in der Anwendung ändern oder hinzufügen, um die Aktualisierung Ihres Containers zu erzwingen. In einer zukünftigen Version werden wir eine Funktion für die Continous Integration/Bereitstellung anbieten.

## <a name="language-support"></a>Sprachunterstützung

**F:** Unterstützen Sie nicht kompilierte .NET Core-Apps?

**A:** Nein. Nein, Sie müssen kompilierte .NET Core-Apps mit allen Abhängigkeiten bereitstellen. Vollständige Funktionen für Bereitstellungen und Builds werden in einer zukünftigen Version verfügbar sein.

**F:** Unterstützen Sie Composer als Abhängigkeits-Manager für PHP-Apps?

**A:** Nein. Sie müssen Ihre PHP-Apps mit allen Abhängigkeiten bereitstellen. Vollständige Funktionen für Bereitstellungen werden in einer zukünftigen Version verfügbar sein.

## <a name="custom-containers"></a>Benutzerdefinierte Container

**F:** Ich verwende meinen eigenen benutzerdefinierten Container. Meine App befindet sich im Verzeichnis „\home\“, aber ich kann meine Dateien nicht finden, wenn ich mit der [SCM](https://github.com/projectkudu/kudu)-Website oder mit einem FTP-Client den Inhalt durchsuche. Wo sind meine Dateien?

**A:** Wir stellen eine SMB-Freigabe im Verzeichnis „\home\“ bereit. Dadurch werden alle vorhandenen Inhalte überschrieben.

**F:** Welches Format hat die Server-URL der privaten Registrierung?

**A:** Sie müssen die vollständige Registrierungs-URL samt „http://“ oder „https://“ eingeben.

**F:** Welches Format hat der Bildname in der Option zur privaten Registrierung?

**A:** Sie müssen den vollständigen Bildnamen samt URL der privaten Registrierung hinzufügen (z.B. myacr.azurecr.io/dotnet:latest)

**F:** Ich möchte mehr als einen Port in meinem benutzerdefinierten Containerimage verfügbar machen. Ist das möglich?

**A:** Dies wird derzeit nicht unterstützt.

**F:** Kann ich meinen eigenen Speicher verwenden?

**A:** Dies wird derzeit nicht unterstützt.

**F:** Ich kann über die SCM-Website das Dateisystem meines benutzerdefinierten Containers nicht durchsuchen und keine Prozesse ausführen. Warum?

**A:** Die SCM-Website wird in einem separaten Container ausgeführt. Sie können das Dateisystem oder aktive Prozesse des App-Containers nicht überprüfen.

**F:** Mein benutzerdefinierter Container lauscht auf einen anderen Port als Port 80. Wie kann ich meine App so konfigurieren, dass die Anforderungen an diesen Port weitergeleitet werden?

**A:** Sie können eine Anwendungseinstellung mit dem Namen **PORT** und für diese den Wert der erwarteten Portnummer angeben.

**F:** Muss ich HTTPS in meinem benutzerdefinierten Container implementieren?

**A:** Die Plattform handhabt die HTTPS-Beendigung an den freigegebenen Front-Ends.

## <a name="pricing-and-sla"></a>Preise und SLA

**F:** Welche Preise gelten für die öffentliche Vorschau?

**A:** Ihnen wird die Hälfte der Stunden, die Ihre App ausgeführt wird, zu den normalen Azure App Service-Preisen berechnet. Sie erhalten also effektiv einen Rabatt von 50 % auf den normalen Azure App Service-Preis.

## <a name="other"></a>Sonstige

**F:** Welche Zeichen werden in den Namen von Anwendungseinstellungen unterstützt?

**A:** Sie können nur A-Z, a-z, 0-9 und Unterstriche für Anwendungseinstellungen verwenden.

**F:** Wo kann ich neue Funktionen beantragen?

**A:** Sie können Ihre Idee im [Web-Apps-Feedbackforum](https://aka.ms/webapps-uservoice) einreichen. Bitte fügen Sie im Titel Ihrer Idee [Linux] hinzu.

## <a name="next-steps"></a>Nächste Schritte
* [Was ist App Service unter Linux?](app-service-linux-intro.md)
* [Erstellen von Web-Apps in App Service unter Linux](app-service-linux-how-to-create-a-web-app.md)

