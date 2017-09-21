---
title: "Azure App Service-Web-App für Container – FAQs | Microsoft-Dokumentation"
description: "Azure App Service-Web-App für Container – FAQs"
keywords: "Azure App Service, Web-App, häufig gestellte Fragen, Linux, OSS"
services: app-service
documentationCenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: aelnably;wesmc
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 146c598f5cb62612327ce679a6bfaadc8312b149
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="azure-app-service-web-app-for-containers-faq"></a>Azure App Service-Web-App für Container – FAQs

Mit der Veröffentlichung von Web-App für Container erweitern wir unsere Plattform um Funktionen und Verbesserungen. Im Folgenden finden Sie einige häufig gestellte Fragen (FAQ) von unseren Kunden aus den letzten Monaten.
Wenn Sie eine Frage haben, kommentieren Sie den Artikel, und wir werden so bald wie möglich antworten.

## <a name="built-in-images"></a>Integrierte Images

**F:** Ich möchte die integrierten Docker-Container nutzen, die von der Plattform bereitgestellt werden. Wo finde ich die Dateien?

**A:** Sie finden alle Docker-Dateien auf [GitHub](https://github.com/azure-app-service). Sie finden alle Docker-Container auf [Docker Hub](https://hub.docker.com/u/appsvc/).

**F:** Welche Werte sind beim Konfigurieren des Laufzeitstapels im Abschnitt „Startdatei“ anzugeben?

**A:** Für Node.js geben Sie die PM2-Konfigurationsdatei oder Ihre Skriptdatei an. Für .NET Core geben Sie den Namen der kompilierten DLL-Datei an. Für Ruby können Sie das Ruby-Skript angeben, mit dem Ihre App initialisiert werden soll.

## <a name="management"></a>Verwaltung

**F:** Was geschieht, wenn ich auf die Schaltfläche „Neu starten“ im Azure-Portal klicke?

**A:** Dies entspricht dem Docker-Neustart.

**F:** Kann ich über Secure Shell (SSH) eine Verbindung mit dem virtuellen Computer (VM) des App-Containers herstellen?

**A:** Ja, Sie können dies über die SCM-Website tun.

**F:** Ich möchte einen App Service-Plan für Linux über das SDK oder eine ARM-Vorlage erstellen. Wie kann ich dies erreichen?

**A:** Sie müssen das Feld `reserved` von App Service auf `true` festlegen.

## <a name="continuous-integrationdeployment"></a>Continuous Integration/Bereitstellung

**F:** Meine Web-App verwendet nach dem Aktualisieren des Images auf DockerHub noch ein altes Docker-Containerimage. Unterstützen Sie Continuous Integration/Bereitstellung von benutzerdefinierten Containern?

**A:** Informationen zum Einrichten von Continuous Integration/Deployment für Azure Container Registry- oder DockerHub-Images finden Sie im Artikel [Continuous Deployment mit Azure-Web-Apps für Container](./app-service-linux-ci-cd.md). Bei privaten Registrierungen können Sie den Container aktualisieren, indem Sie Ihre Web-App beenden und dann wieder starten. Sie können auch eine Dummyeinstellung in der Anwendung ändern oder hinzufügen, um die Aktualisierung Ihres Containers zu erzwingen.

**Q:** Werden Stagingumgebungen unterstützt?

**A:** Ja.

**F:** Kann ich **Web Deploy** zur Bereitstellung meiner Web-App verwenden?

**A:** Ja, Sie müssen die App-Einstellung `WEBSITE_WEBDEPLOY_USE_SCM` auf `false` festlegen.

## <a name="language-support"></a>Sprachunterstützung

**F:** Unterstützen Sie nicht kompilierte .NET Core-Apps?

**A:** Ja.

**F:** Unterstützen Sie Composer als Abhängigkeits-Manager für PHP-Apps?

**A:** Ja. Während einer Git-Bereitstellung sollte Kudu erkennen, dass Sie eine PHP-Anwendung bereitstellen (durch das Vorhandensein einer Datei „composer.lock“), und eine Composer-Installation auslösen.

## <a name="custom-containers"></a>Benutzerdefinierte Container

**F:** Ich verwende meinen eigenen benutzerdefinierten Container. Meine App befindet sich im Verzeichnis `/home/`, aber ich kann meine Dateien nicht finden, wenn ich mit der [SCM](https://github.com/projectkudu/kudu)-Website oder mit einem FTP-Client den Inhalt durchsuche. Wo sind meine Dateien?

**A:** Wir stellen eine SMB-Freigabe im Verzeichnis `/home/` bereit. Dadurch werden alle vorhandenen Inhalte überschrieben.

**F:** Ich verwende meinen eigenen benutzerdefinierten Container. Ich möchte die Plattform in eine SMB-Freigabe im Verzeichnis `/home/` einbinden.

**A:** Dies können Sie tun, indem Sie die App-Einstellung `WEBSITES_ENABLE_APP_SERVICE_STORAGE` auf `true` festlegen oder die App-Einstellung vollständig entfernen. Denken Sie daran, dass der Container bei einer Änderung des Plattformspeichers neu gestartet wird. 

Beachten Sie, dass das Verzeichnis „/home/“ nicht über Skalierungsinstanzen freigegeben wird und dort geschriebene Dateien nicht über Neustarts hinweg beibehalten werden, wenn „WEBSITES_ENABLE_APP_SERVICE_STORAGE“ auf „False“ festgelegt ist.

**F:** Mein benutzerdefinierter Container benötigt für den Start sehr lange, und die Plattform startet den Container neu, bevor er den Startvorgang abgeschlossen hat.

**A:** Sie können den Zeitraum konfigurieren, den die Plattform vor dem Neustarten Ihres Containers wartet. Dies können Sie durch Festlegen der App-Einstellung `WEBSITES_CONTAINER_START_TIME_LIMIT` auf den gewünschten Wert in Sekunden erreichen. Der Standardwert ist 230 Sekunden, und der maximale Wert beträgt 600 Sekunden.

**F:** Welches Format hat die Server-URL der privaten Registrierung?

**A:** Sie müssen die vollständige Registrierungs-URL samt `http://` oder `https://` eingeben.

**F:** Welches Format hat der Bildname in der Option zur privaten Registrierung?

**A:** Sie müssen den vollständigen Bildnamen samt URL der privaten Registrierung hinzufügen (z.B. „myacr.azurecr.io/dotnet:latest“). Namen von Images mit einem benutzerdefinierten Port [können nicht über das Portal eingegeben werden](https://feedback.azure.com/forums/169385-web-apps/suggestions/31304650). Verwenden Sie das Befehlszeilentool [`az` ](https://docs.microsoft.com/en-us/cli/azure/webapp/config/container?view=azure-cli-latest#az_webapp_config_container_set), um `docker-custom-image-name` festzulegen.

**F:** Ich möchte mehr als einen Port in meinem benutzerdefinierten Containerimage verfügbar machen. Ist das möglich?

**A:** Dies wird derzeit nicht unterstützt.

**F:** Kann ich meinen eigenen Speicher verwenden?

**A:** Dies wird derzeit nicht unterstützt.

**F:** Ich kann über die SCM-Website das Dateisystem meines benutzerdefinierten Containers nicht durchsuchen und keine Prozesse ausführen. Warum?

**A:** Die SCM-Website wird in einem separaten Container ausgeführt. Sie können das Dateisystem oder aktive Prozesse des App-Containers nicht überprüfen.

**F:** Mein benutzerdefinierter Container lauscht auf einen anderen Port als Port 80. Wie kann ich meine App so konfigurieren, dass die Anforderungen an diesen Port weitergeleitet werden?

**A:** Ports werden automatisch erkannt. Zudem können Sie eine Anwendungseinstellung mit dem Namen **WEBSITES_PORT** mit dem Wert der erwarteten Portnummer angeben. Bei der Plattform wurde in der Vergangenheit die App-Einstellung `PORT` verwendet. Es ist geplant, die Verwendung dieser App-Einstellung als veraltet zu markieren und ausschließlich `WEBSITES_PORT` zu verwenden.

**F:** Muss ich HTTPS in meinem benutzerdefinierten Container implementieren?

**A:** Die Plattform handhabt die HTTPS-Beendigung an den freigegebenen Front-Ends.

## <a name="pricing-and-sla"></a>Preise und SLA

**F:** Wie erfolgt die Abrechnung für den nun allgemein verfügbaren Dienst?

**A:** Ihnen wird die Hälfte der Stunden, die Ihre App ausgeführt wird, zu den normalen Azure App Service-Preisen berechnet.

## <a name="other"></a>Sonstige

**F:** Welche Zeichen werden in den Namen von Anwendungseinstellungen unterstützt?

**A:** Sie können nur A-Z, a-z, 0-9 und Unterstriche für Anwendungseinstellungen verwenden.

**F:** Wo kann ich neue Funktionen beantragen?

**A:** Sie können Ihre Idee im [Web-Apps-Feedbackforum](https://aka.ms/webapps-uservoice) einreichen. Fügen Sie im Titel Ihrer Idee „[Linux]“ hinzu.

## <a name="next-steps"></a>Nächste Schritte

* [Was sind Azure-Web-Apps für Container?](app-service-linux-intro.md)
* [Einrichten von Stagingumgebungen in Azure App Service](../../app-service-web/web-sites-staged-publishing.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Continuous Deployment mit Azure-Web-Apps für Container](./app-service-linux-ci-cd.md)

