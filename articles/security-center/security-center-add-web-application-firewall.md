---
title: "Hinzufügen einer Web Application Firewall in Azure Security Center | Microsoft Docs"
description: "In diesem Dokument wird erläutert, wie Sie die Azure Security Center-Empfehlungen **Web Application Firewall hinzufügen** und **Finalize application protection** (Anwendungsschutz abschließen) implementieren."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 8f56139a-4466-48ac-90fb-86d002cf8242
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 2286437f4ab13384f895e906ccda48ac1b4c553d
ms.openlocfilehash: b44a0373ceca84b423984e01eee1e57a67d97cdd


---
# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Hinzufügen einer Web Application Firewall in Azure Security Center
Zum Schutz Ihrer Webanwendungen empfiehlt Azure Security Center unter Umständen das Hinzufügen einer Web Application Firewall (WAF) von einem Microsoft-Partner. Die Umsetzung dieser Empfehlung wird in diesem Dokument anhand eines Beispiels veranschaulicht.

Eine WAF-Empfehlung wird für jede öffentlich zugängliche IP-Adresse (sowohl auf Instanzebene als auch mit Lastenausgleich) angezeigt, die über eine zugeordnete Netzwerksicherheitsgruppe mit offenen eingehenden Webports (80, 443) verfügt.

Security Center empfiehlt die Bereitstellung einer WAF zum Schutz vor Angriffen auf Ihre Webanwendungen auf virtuellen Computern und in der App Service-Umgebung. Eine App Service-Umgebung ist eine Option des [Premium](https://azure.microsoft.com/pricing/details/app-service/)-Tarifs von Azure App Service, die eine vollständig isolierte und dedizierte Umgebung zur sicheren Ausführung von Azure App Service-Apps bereitstellt. Weitere Informationen zu ASE finden Sie unter [Dokumentation zur App Service-Umgebung](../app-service/app-service-app-service-environments-readme.md).

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt.  Dieses Dokument ist keine Schritt-für-Schritt-Anleitung.
>
>

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung
1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Webanwendung mit Web Application Firewall sichern** aus.
   ![Sichere Webanwendung][1]
2. Wählen Sie auf dem Blatt **Webanwendungen mit Web Application Firewall sichern** eine Webanwendung aus. Das Blatt **Web Application Firewall hinzufügen** wird geöffnet.
   ![Add a web application firewall][2]
3. Sie können eine vorhandene Web Application Firewall verwenden oder eine neue erstellen. Da in diesem Beispiel noch keine WAFs vorhanden sind, erstellen wir eine neue WAF.
4. Wählen Sie hierzu in der Liste mit den integrierten Partnern eine Lösung aus. In diesem Beispiel verwenden wir **Barracuda Web Application Firewall**.
5. Auf dem dann geöffneten Blatt **Barracuda Web Application Firewall** werden Informationen zu dieser Partnerlösung angezeigt. Klicken Sie auf dem Informationsblatt auf **Erstellen** .
   ![Blatt mit Firewallinformationen][3]
6. Das Blatt **Neue Web Application Firewall** wird geöffnet, auf dem Sie Schritte zur **VM-Konfiguration** ausführen und **WAF-Informationen** bereitstellen können. Wählen Sie **VM-Konfiguration**aus.
7. Auf dem Blatt **VM-Konfiguration** können Sie Informationen zum Einrichten des virtuellen Computers eingeben, auf dem die WAF ausgeführt wird.
   ![VM configuration][4]
8. Kehren Sie zum Blatt **Neue Web Application Firewall** zurück, und wählen Sie **WAF-Informationen** aus. Auf dem Blatt **WAF-Informationen** können Sie die eigentliche WAF konfigurieren. In Schritt 7 können Sie den virtuellen Computer konfigurieren, auf dem die WAF ausgeführt wird, und in Schritt 8 können Sie die eigentliche WAF bereitstellen.

## <a name="finalize-application-protection"></a>Finalize application protection (Anwendungsschutz abschließen)
1. Kehren Sie zum Blatt **Empfehlungen** zurück. Ein neuer Eintrag wurde generiert, nachdem Sie die WAF erstellt haben: **Finalize application protection**(Anwendungsschutz abschließen). Aufgrund dieses Eintrags wissen Sie, dass Sie den Prozess der eigentlichen „Verdrahtung“ der WAF in Azure Virtual Network abschließen müssen, damit die Anwendung geschützt werden kann.
   ![Finalize application protection (Anwendungsschutz abschließen) (Anwendungsschutz abschließen)][5]
2. Wählen Sie **Finalize application protection**(Anwendungsschutz abschließen). Ein neues Blatt wird geöffnet. Sie sehen, dass eine Webanwendung vorhanden ist, für die der Datenverkehr umgeleitet muss.
3. Wählen Sie die Webanwendung aus. Es wird ein Blatt geöffnet, in dem Sie Schritte zum Abschließen des Web Application Firewall-Setups ausführen können. Führen Sie die Schritte aus, und wählen Sie dann **Datenverkehr einschränken**aus. Security Center kümmert sich daraufhin um die Details.
   ![Datenverkehr einschränken][6]

> [!NOTE]
> Sie können mehrere Webanwendungen in Security Center schützen, indem Sie diese Anwendungen Ihren vorhandenen WAF-Bereitstellungen hinzufügen.
>
>

Die Protokolle dieser WAF sind nun vollständig integriert. Security Center kann automatisch beginnen, die Protokolle zu sammeln und zu analysieren, damit Sie wichtige Sicherheitswarnungen erhalten können.

## <a name="see-also"></a>Weitere Informationen
In diesem Dokument wurde gezeigt, wie Sie die Security Center-Empfehlung „Web Application Firewall hinzufügen“ implementieren. Weitere Informationen zum Konfigurieren einer Web Application Firewall finden Sie im folgenden Artikel:

* [Konfigurieren einer Web Application Firewall (WAF) für eine App Service-Umgebung](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md) : Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und auf diese reagieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md) : Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) : Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png



<!--HONumber=Feb17_HO3-->


