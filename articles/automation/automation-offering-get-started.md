--- 
title: Erste Schritte mit Azure Automation | Microsoft-Dokumentation
description: "Dieser Artikel enthält eine Übersicht über den Azure Automation-Dienst, und es werden die wichtigsten Konzepte und Implementierungsdetails beschrieben, die als Vorbereitung für die Nutzung des Angebots über Azure Marketplace erforderlich sind."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/02/2017
ms.author: magoedte
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 8a04fda8eaf6e14a278941e7bb55b23012f67850
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017

---

## <a name="getting-started-with-azure-automation"></a>Erste Schritte mit Azure Automation

In dieser Anleitung zu den ersten Schritten werden wichtige Konzepte zur Bereitstellung von Azure Automation vorgestellt. Wenn Automation in Azure neu für Sie ist oder wenn Sie Erfahrung mit Software zur Automatisierung von Workflows haben (z.B. System Center Orchestrator), ist diese Anleitung eine gute Hilfe bei den ersten Schritten in Bezug auf die Konzepte und Bereitstellungsdetails. 

## <a name="key-concepts"></a>Wichtige Begriffe

### <a name="automation-service"></a>Automation-Dienst
Automation ist ein Azure-Dienst, bei dem Windows PowerShell- und Azure-Technologie eingesetzt wird, um Sie beim Steuern der Verwaltung Ihrer Infrastruktur und Anwendungen zu unterstützen – in Azure, in der Cloud und lokal.  Azure Automation dient Ihnen als Hilfe für folgende Vorgänge: Verwalten des gesamten Lebenszyklus Ihrer Dienste und Anwendungen mit automatisierter Bereitstellung per Prozessautomatisierung, Betriebssystemkonfiguration per PowerShell-Dienst zum Konfigurieren des gewünschten Zustands, fortlaufende Aktualisierung und Überwachung mit Updateverwaltung und Änderungsnachverfolgung und automatisierter Problembehandlung und -lösung.

### <a name="automation-account"></a>Automation-Konto
Ein Automation-Konto ist eine Azure-Ressource, die Sie erstellen.  Sie können Ihre gesamten Azure-, Cloud und lokalen Ressourcen mit nur einem Automation-Konto verwalten.  Ein Automation-Konto ist ein Container für die Elemente, die Sie für die Automatisierung benötigen: Runbooks, Module, Assets, z.B. Anmeldeinformationen und Zeitpläne, und Konfigurationen. Sie können mehrere Automation-Konten verwenden, um Ressourcen in separate logische Umgebungen zu unterteilen, z.B. Entwicklung, Test und Produktion, oder auf geografische Regionen zu verteilen.  

### <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker
Für die Ausführung von Runbooks auf physischen oder virtuellen Systemen in Ihrem lokalen Rechenzentrum, in Azure oder bei einem anderen Cloudanbieter ermöglicht es Ihnen das Hybrid Runbook Worker-Feature, auf diese lokalen Ressourcen zuzugreifen und sie zu verwalten.     

### <a name="automation-desired-state-configuration"></a>Automation-Konfiguration für den gewünschten Zustand
Die Automation-Konfiguration für den gewünschten Zustand (Desired State Configuration, DSC), die auf PowerShell DSC basiert, dient zum Konfigurieren, Überwachen und automatischen Aktualisieren des gewünschten Zustands Ihres Betriebssystems, das in Azure, lokal oder in einer anderen Cloud gehostet wird.  

### <a name="management-solutions"></a>Verwaltungslösungen
Verwaltungslösungen wie die Aktualisierungsverwaltung und Änderungsnachverfolgung erweitern die Funktionalität von Azure Automation und werden mit Log Analytics verwendet.  Sie können mehrere Ressourcen enthalten, die ein bestimmtes Verwaltungsszenario unterstützen, z.B. Automation-Runbooks, vordefinierte Log Analytics-Suchabfragen und -Warnungen und Visualisierungen.  

## <a name="architecture-overview"></a>Übersicht über die Architektur

Azure Automation ist eine SaaS-Anwendung (Software as a Service), bei der eine skalierbare und zuverlässige mehrinstanzenfähige Umgebung bereitgestellt wird, um Prozesse mit Runbooks zu automatisieren und Konfigurationsänderungen für Windows- und Linux-Systeme zu verwalten, indem die Konfiguration des gewünschten Zustands in Azure, anderen Clouddiensten oder lokal verwendet wird. In Ihrem Automation-Konto enthaltene Entitäten, z.B. Runbooks, Assets und ausführende Konten, sind von anderen Automation-Konten in Ihrem Abonnement und anderen Abonnements isoliert.  

Runbooks, die Sie in Azure ausführen, werden in Automation-Sandboxes ausgeführt, die auf virtuellen Azure-Computern vom Typ „PaaS“ (Platform as a Service) gehostet werden.  Automation-Sandboxes ermöglichen die Mandantenisolation für alle Aspekte der Runbookausführung: Module, Speicher, Arbeitsspeicher, Netzwerkkommunikation, Auftragsdatenströme usw. Diese Rolle wird vom Dienst verwaltet und ist zu Steuerungszwecken nicht über Ihr Azure- oder Azure Automation-Konto zugänglich.         

Um die Bereitstellung und Verwaltung von Ressourcen in Ihrem lokalen Rechenzentrum oder anderen Clouddiensten zu automatisieren, können Sie einen oder mehrere Computer für die Ausführung der HRW-Rolle (Hybrid Runbook Worker) festlegen.  Für jeden HRW ist der Microsoft-Verwaltungs-Agent (Microsoft Management Agent, MMA) mit einer Verbindung mit einem Log Analytics-Arbeitsbereich und einem Automation-Konto erforderlich.  Log Analytics wird verwendet, um den Bootstrap-Vorgang für die Installation durchzuführen, den Microsoft-Verwaltungs-Agent zu verwalten und die Funktionalität des HRW zu überwachen.  Die Übermittlung von Runbooks und die Anweisung zu ihrer Ausführung erfolgt durch Azure Automation.

Sie können mehrere HRW-Instanzen bereitstellen, um eine hohe Verfügbarkeit für Ihre Runbooks zu erzielen, einen Lastenausgleich für Runbookaufträge durchzuführen und diese in einigen Fällen fest bestimmten Workloads oder Umgebungen zuzuordnen.  Der HRW kommuniziert mit dem Automation-Dienst über den ausgehenden TCP-Port 443.  Wenn ein Runbook auf einem HRW in Ihrem Datencenter ausgeführt wird und damit Verwaltungsaufgaben für andere Computer oder Dienste im Datencenter durchgeführt werden sollen, ist für das Runbook unter Umständen der Zugriff auf weitere Ports erforderlich.  Falls es gemäß Ihren IT-Sicherheitsrichtlinien unzulässig ist, dass Computer in Ihrem Netzwerk eine Internetverbindung herstellen, hilft Ihnen der Artikel zum [OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md) weiter. Das OMS-Gateway fungiert als Proxy für den HRW, um den Auftragsstatus zu erfassen und Konfigurationsinformationen für Ihr Automation-Konto zu empfangen.

Runbooks, die auf einem HRW ausgeführt werden, werden im Kontext des lokalen Systemkontos auf dem Computer ausgeführt. Dies ist der empfohlene Sicherheitskontext für die Durchführung von administrativen Aktionen auf dem lokalen Windows-Computer. Falls mit dem Runbook Aufgaben für Ressourcen außerhalb des lokalen Computers ausgeführt werden sollen, müssen Sie unter Umständen sichere Anmeldeinformationen für das Automation-Konto definieren, auf das Sie vom Runbook aus zugreifen können und das Sie zum Authentifizieren gegenüber der externen Ressource verwenden. Sie können in Ihrem Runbook Assets vom Typ [Anmeldeinformationen](automation-credentials.md), [Zertifikat](automation-certificates.md) und [Verbindung](automation-connections.md) mit Cmdlets verwenden, die Ihnen das Angeben von Anmeldeinformationen zur Authentifizierung ermöglichen.

In Azure Automation gespeicherte DSC-Konfigurationen können direkt auf virtuelle Azure-Computer angewendet werden. Andere physische und virtuelle Computer können Konfigurationen vom DSC-Pullserver von Azure Automation anfordern.  Zum Verwalten von Konfigurationen Ihrer lokalen physischen oder virtuellen Windows- und Linux-Systeme müssen Sie keine Infrastruktur für die Unterstützung des Automation DSC-Pullservers bereitstellen, sondern nur den ausgehenden Internetzugriff der einzelnen Systeme, die mit Automation DSC verwaltet werden sollen, und die Kommunikation mit dem OMS-Dienst über TCP-Port 443.   

![Azure Automation – Übersicht](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

## <a name="prerequisites"></a>Voraussetzungen

### <a name="automation-dsc"></a>Automation DSC
Azure Automation DSC kann verwendet werden, um verschiedene Computer zu verwalten:

* Virtuelle Azure-Computer (klassisch) mit Windows oder Linux
* Virtuelle Azure-Computer mit Windows oder Linux
* Virtuelle AWS-Computer (Amazon Web Services) mit Windows oder Linux
* Physische/virtuelle Windows-Computer, lokal oder in einer anderen Cloud als Azure oder AWS
* Physische/virtuelle Linux-Computer, lokal oder in einer anderen Cloud als Azure oder AWS

Für den PowerShell DSC-Agent für Windows muss die neueste Version von WMF 5 installiert werden, um eine Kommunikation mit Azure Automation zu ermöglichen. Die neueste Version des [PowerShell DSC-Agents für Linux](https://www.microsoft.com/en-us/download/details.aspx?id=49150) muss installiert sein, um unter Linux eine Kommunikation mit Azure Automation zu ermöglichen.

### <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker  
Ein Computer, der für die Ausführung von Hybrid Runbook-Aufträgen vorgesehen ist, muss über Folgendes verfügen:

* Windows Server 2012 oder höher
* Windows PowerShell 4.0 oder höher.  Aufgrund der höheren Zuverlässigkeit wird die Installation von Windows PowerShell 5.0 auf dem Computer empfohlen. Sie können die neue Version aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=50395) herunterladen.
* Mindestens zwei Kerne
* Mindestens 4 GB RAM

## <a name="security"></a>Sicherheit
Mit Azure Automation können Sie Aufgaben für Ressourcen in Azure, lokal und mit anderen Cloudanbietern automatisieren.  Damit ein Runbook die erforderlichen Aktionen ausführen kann, muss es über Berechtigungen für den sicheren Zugriff auf die Ressourcen mit den geringstmöglichen Rechten im Abonnement verfügen.  

### <a name="automation-account"></a>Automation-Konto 
Alle Automatisierungsaufgaben, die Sie für Ressourcen mit den Azure-Cmdlets in Azure Automation durchführen, werden gegenüber Azure mit Azure Active Directory basierend auf den Anmeldeinformationen für die Organisationsidentität authentifiziert.  Ein Automation-Konto wird separat von dem Konto verwendet, das Sie zum Anmelden am Portal zur Konfiguration und Verwendung von Azure-Ressourcen nutzen.  

Die Automation-Ressourcen für jedes Automation-Konto sind mit einer einzelnen Azure-Region verknüpft, aber die Automation-Konten können alle Ressourcen in Ihrem Abonnement verwalten. Erstellen Sie Automation-Konten in unterschiedlichen Regionen, wenn Ihre Richtlinien eine Isolierung von Daten und Ressourcen innerhalb einer spezifischen Region vorsehen.

> [!NOTE]
> Auf Automation-Konten und die darin enthaltenen Ressourcen, die mit dem Azure-Portal erstellt werden, kann im klassischen Azure-Portal nicht zugegriffen werden. Wenn Sie diese Konten oder die enthaltenen Ressourcen mit Windows PowerShell verwalten möchten, müssen Sie die Azure-Ressourcen-Manager-Module verwenden.
> 

Wenn Sie ein Automation-Konto im Azure-Portal erstellen, werden automatisch zwei Authentifizierungsentitäten erstellt:

* Ein ausführendes Konto. Für dieses Konto werden ein Dienstprinzipal in Azure Active Directory (Azure AD) und ein Zertifikat erstellt. Außerdem wird die Rolle „Mitwirkender“ der rollenbasierten Zugriffssteuerung (RBAC) zugewiesen, die zum Verwalten von Resource Manager-Ressourcen mit Runbooks verwendet wird.
* Ein klassisches ausführendes Konto. Für dieses Konto wird ein Verwaltungszertifikat hochgeladen, das zum Verwalten von klassischen Ressourcen mit Runbooks verwendet wird.

Die rollenbasierte Zugriffssteuerung ist für Azure Resource Manager verfügbar, um einem Azure AD-Benutzerkonto und ausführenden Konto zulässige Aktionen zu gewähren und den Dienstprinzipal zu authentifizieren.  Weitere Informationen zur Entwicklung Ihres Modells zum Verwalten von Automation-Berechtigungen finden Sie im Artikel [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md) .  

#### <a name="authentication-methods"></a>Authentifizierungsmethoden
In der folgenden Tabelle sind die unterschiedlichen Authentifizierungsmethoden für die einzelnen Umgebungen zusammengefasst, die von Azure Automation unterstützt werden.

| Methode | Environment 
| --- | --- | 
| Ausführendes Azure-Konto und klassisches ausführendes Konto |Azure Resource Manager- und klassische Azure-Bereitstellung |  
| Azure AD-Benutzerkonto |Azure Resource Manager- und klassische Azure-Bereitstellung |  
| Windows-Authentifizierung |Lokales Rechenzentrum oder anderer Cloudanbieter mit Hybrid Runbook Worker-Nutzung |  
| AWS-Anmeldeinformationen |Amazon Web Services |  

Der Abschnitt **How to\Authentication and Security** (Vorgehensweise/Authentifizierung und Sicherheit) enthält hilfreiche Artikel mit einer Übersicht und Implementierungsschritten zum Konfigurieren der Authentifizierung für diese Umgebungen – mit einem vorhandenen oder neuen Konto, das Sie für diese Umgebung festlegen.  Für das ausführende Azure-Konto und das klassische ausführende Konto wird im Thema [Update Automation Run As account using PowerShell](automation-update-account-powershell.md) (Aktualisieren des ausführenden Automation-Kontos mit PowerShell) beschrieben, wie Sie Ihr vorhandenes Automation-Konto mit den ausführenden Konten per PowerShell aktualisieren, falls die ursprüngliche Konfiguration nicht mit einem ausführenden bzw. klassischen ausführenden Konto durchgeführt wurde.   
 
## <a name="network"></a>Netzwerk
Damit der Hybrid Runbook Worker eine Verbindung mit Microsoft Operations Management Suite (OMS) herstellen und sich bei dem Dienst registrieren kann, benötigt er Zugriff auf die unten angegebene Portnummer und die angegebenen URLs.  Dies ist zusätzlich zu den [für Microsoft Monitoring Agent benötigten Ports und URLs](../log-analytics/log-analytics-proxy-firewall.md#configure-settings-with-the-microsoft-monitoring-agent) für die Herstellung einer Verbindung mit OMS erforderlich. Wenn Sie einen Proxyserver für die Kommunikation zwischen dem Agent und dem OMS-Dienst verwenden, müssen Sie sicherstellen, dass auf die entsprechenden Ressourcen zugegriffen werden kann. Wenn Sie eine Firewall verwenden, um den Zugriff auf das Internet einzuschränken, müssen Sie die Firewall so konfigurieren, dass der Zugriff möglich ist.

Nachfolgend sind der Port und die URLs aufgeführt, die für die Kommunikation zwischen Hybrid Runbook Worker und Automation erforderlich sind.

* Port: Für ausgehenden Zugriff auf das Internet ist nur TCP 443 erforderlich.
* Globale URL: *.azure-automation.net

Wenn Sie ein Automation-Konto für eine bestimmte Region festgelegt haben und die Kommunikation mit diesem regionalen Rechenzentrum beschränken möchten, sehen Sie sich die folgende Tabelle mit dem DNS-Eintrag für jede Region an:

| **Region** | **DNS-Einträge** |
| --- | --- |
| USA (Mitte/Süden) |scus-jobruntimedata-prod-su1.azure-automation.net |
| USA (Ost) 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| USA, Westen-Mitte | wcus-jobruntimedata-prod-su1.azure-automation.net |
| Westeuropa |we-jobruntimedata-prod-su1.azure-automation.net |
| Nordeuropa |ne-jobruntimedata-prod-su1.azure-automation.net |
| Kanada, Mitte |cc-jobruntimedata-prod-su1.azure-automation.net |
| Südostasien |sea-jobruntimedata-prod-su1.azure-automation.net |
| Indien (Mitte) |cid-jobruntimedata-prod-su1.azure-automation.net |
| Japan Ost |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Australien, Südosten |ase-jobruntimedata-prod-su1.azure-automation.net |
| UK, Süden | uks-jobruntimedata-prod-su1.azure-automation.net |
| US Government, Virginia | usge-jobruntimedata-prod-su1.azure-automation.us |

Wenn Sie eine Liste mit IP-Adressen statt Namen benötigen, können Sie die XML-Datei mit [Azure Datacenter-IP-Adressen](https://www.microsoft.com/download/details.aspx?id=41653) aus dem Microsoft Download Center herunterladen und überprüfen. 

> [!NOTE]
> Diese Datei enthält die IP-Adressbereiche (einschließlich Compute-, SQL- und Speicherbereiche), die in Microsoft Azure-Datencentern verwendet werden. Eine aktualisierte Datei mit den derzeit bereitgestellten Bereichen und alle anstehenden Änderungen an den IP-Adressbereichen wird wöchentlich veröffentlicht. In der Datei enthaltene neue Bereiche werden frühestens nach einer Woche in den Datencentern verwendet. Laden Sie die neue XML-Datei jede Woche herunter, und nehmen Sie die erforderlichen Änderungen an Ihrer Website vor, um in Azure ausgeführte Dienste ordnungsgemäß zu ermitteln. Diese Datei zum Aktualisieren der BGP-Ankündigung von Azure-Bereichen wird ExpressRoute-Benutzern jeweils in der ersten Woche des Monats angezeigt. 
> 


## <a name="implementation"></a>Implementierung

### <a name="creating-an-automation-account"></a>Erstellen eines Automation-Kontos

Es gibt verschiedene Möglichkeiten, wie Sie im Azure-Portal ein Automation-Konto erstellen können.  In der folgenden Tabelle sind die einzelnen Bereitstellungstypen und die Unterschiede aufgeführt.  

|Methode | Beschreibung |
|-------|-------------|
| Auswählen von Automation + Control über den Marketplace | Bei diesem Angebot werden ein Automation-Konto und ein OMS-Arbeitsbereich, die miteinander verknüpft sind, in derselben Ressourcengruppe und Region erstellt.  Außerdem werden die Lösungen für die Änderungsnachverfolgung und Aktualisierungsverwaltung bereitgestellt, die standardmäßig aktiviert sind. |
| Auswählen von Automation über den Marketplace | Es wird ein Automation-Konto in einer neuen oder vorhandenen Ressourcengruppe erstellt, die nicht mit einem OMS-Arbeitsbereich verknüpft ist und keine verfügbaren Lösungen des Angebots „Automation + Control“ enthält. Dies ist eine einfache Konfiguration zur Einführung in Automation, damit Sie lernen können, wie Sie Runbooks schreiben, DSC-Konfigurationen einrichten und die Funktionen des Diensts nutzen. |
| Ausgewählte Verwaltungslösungen | Wenn Sie eine Lösung auswählen – **[Aktualisierungsverwaltung](../operations-management-suite/oms-solution-update-management.md)**, **[VMs außerhalb der Geschäftszeiten starten/beenden](automation-solution-vm-management.md)** oder **[Änderungsnachverfolgung](../log-analytics/log-analytics-change-tracking.md)** –, werden Sie zum Auswählen eines vorhandenen Automation- und OMS-Arbeitsbereichs aufgefordert. Unter Umständen können Sie auch beides wählen, je nachdem, was für die Lösung erforderlich ist, die in Ihrem Abonnement bereitgestellt werden soll. |

In diesem Thema wird die Vorgehensweise bei der Erstellung eines Automation-Kontos und OMS-Arbeitsbereichs unter Verwendung des Angebots „Automation + Control“ beschrieben.  Lesen Sie sich den folgenden Artikel [Erstellen eines eigenständigen Automation-Kontos](automation-create-standalone-account.md) durch, um ein eigenständiges Automation-Konto zum Testen oder Anzeigen einer Vorschau des Diensts zu erstellen.  

### <a name="create-automation-account-integrated-with-log-analytics"></a>Erstellen eines in Log Analytics integrierten Automation-Kontos
Die empfohlene Methode zum Integrieren von Automation ist die Auswahl des Angebots „Automation + Control“ über den Marketplace.  Bei diesem Vorgang wird nicht nur ein Automation-Konto erstellt, sondern es wird auch die Integration in einen OMS-Arbeitsbereich durchgeführt, einschließlich der Option zum Installieren der Verwaltungslösungen, die für das Angebot verfügbar sind.  

>[!NOTE]
>Zur Erstellung eines Automation-Kontos müssen Sie Mitglied der Rolle „Dienstadministratoren“ oder Co-Administrator des Abonnements sein, worüber Zugriff auf das Abonnement gewährt wird. Außerdem müssen Sie der Active Directory-Standardinstanz dieses Abonnements als Benutzer hinzugefügt worden sein. Dem Konto muss dabei keine privilegierte Rolle zugewiesen sein.
>
>Wenn Sie kein Mitglied der Active Directory-Instanz des Abonnements sind, bevor Sie der Rolle „Co-Administrator“ des Abonnements hinzugefügt werden, werden Sie Active Directory als Gast hinzugefügt. In diesem Fall erhalten Sie eine Warnung der Art „Sie haben keine Berechtigungen zum Erstellen von…“ auf dem Blatt **Automation-Konto hinzufügen**.
>
>Benutzer, die zuerst der Rolle des Co-Administrators hinzugefügt wurden, können aus der Active Directory-Instanz des Abonnements entfernt und erneut hinzugefügt werden, um sie als Vollbenutzer in Active Directory einzurichten. Sie können dies im Azure-Portal im Bereich **Azure Active Directory** überprüfen. Wählen Sie hierzu **Benutzer und Gruppen**, **Alle Benutzer** und nach Auswahl des jeweiligen Benutzers die Option **Profil**. Als Wert des Attributs **Benutzertyp** im Benutzerprofil darf nicht **Gast** angegeben sein.

1. Melden Sie sich mit einem Konto, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist, beim Azure-Portal an.

2. Klicken Sie auf **Neu**.<br><br> ![Auswählen der Option „Neu“ im Azure-Portal](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  

3. Suchen Sie nach **Automation**, und wählen Sie dann in den Suchergebnissen die Option **Automation + Control**.<br><br> ![Suchen nach und Auswählen von „Automation + Control“ über den Marketplace](media/automation-offering-get-started/automation-portal-martketplace-select-automationandcontrol.png)<br>   

4. Klicken Sie nach dem Durchlesen der Beschreibung für das Angebot auf **Erstellen**.  

5. Wählen Sie auf dem Blatt mit den Einstellungen für **Automation + Control** die Option **OMS-Arbeitsbereich**.  Wählen Sie auf dem Blatt **OMS-Arbeitsbereich** einen OMS-Arbeitsbereich aus, der mit demselben Azure-Abonnement verknüpft ist, in dem sich das Automation-Konto befindet, oder erstellen Sie einen OMS-Arbeitsbereich.  Gehen Sie wie folgt vor, wenn Sie noch nicht über einen OMS-Arbeitsbereich verfügen: Wählen Sie die Option **Neuen Arbeitsbereich erstellen**, und führen Sie auf dem Blatt für den **OMS-Arbeitsbereich** die folgenden Schritte aus: 
   - Geben Sie einen Namen für den neuen **OMS-Arbeitsbereich** an.
   - Wählen Sie ein **Abonnement** aus, mit dem eine Verknüpfung erstellt werden soll, indem Sie in der Dropdownliste einen anderen Eintrag auswählen, falls der Standardeintrag nicht geeignet ist.
   - Unter **Ressourcengruppe** können Sie eine Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe auswählen.  
   - Wählen Sie einen **Speicherort**aus.  Derzeit sind nur die Standorte **Australien, Südosten**, **USA, Osten**, **Asien, Südosten**, **USA, Westen-Mitte** und **Europa, Westen** verfügbar.
   - Wählen Sie einen **Tarif**aus.  Für die Lösung sind zwei Tarife verfügbar: „Free“ und „Pro Knoten (OMS)“.  Beim Free-Tarif gelten Beschränkungen für die Datenmenge, die pro Tag erfasst werden kann, die Aufbewahrungsdauer und die Laufzeit von Runbookaufträgen in Minuten.  Für den Tarif „Pro Knoten (OMS)“ gilt keine Beschränkung für die täglich erfasste Datenmenge.  
   - Wählen Sie die Option **Automation-Konto**.  Bei der Erstellung eines neuen OMS-Arbeitsbereichs müssen Sie auch ein Automation-Konto erstellen, das dem zuvor angegebenen neuen OMS-Arbeitsbereich zugeordnet wird, einschließlich Azure-Abonnement, -Ressourcengruppe und -Region.  Sie können die Option **Automation-Konto erstellen** wählen und auf dem Blatt **Automation-Konto** Folgendes angeben: 
  - Geben Sie im Feld **Name** den Namen des Automation-Kontos ein.

    Alle anderen Optionen werden basierend auf dem ausgewählten OMS-Arbeitsbereich automatisch aufgefüllt, und diese Optionen können nicht geändert werden.  Ein ausführendes Azure-Konto ist die standardmäßig verwendete Authentifizierungsmethode für das Angebot.  Nach dem Klicken auf **OK** werden die Konfigurationsoptionen überprüft, und das Automation-Konto wird erstellt.  Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen. 

    Wählen Sie andernfalls ein vorhandenes ausführendes Automation-Konto aus.  Das ausgewählte Konto darf nicht bereits mit einem anderen OMS-Arbeitsbereich verknüpft sein. Andernfalls wird auf dem Blatt eine Benachrichtigungsmeldung angezeigt.  Wenn bereits eine Verknüpfung besteht, müssen Sie ein anderes ausführendes Automation-Konto wählen oder ein Konto erstellen.

    Klicken Sie auf **Erstellen**, nachdem Sie die erforderlichen Informationen angegeben haben.  Die Informationen werden überprüft, und das Automation-Konto und die ausführenden Konten werden erstellt.  Sie gelangen automatisch wieder zurück zum Blatt **OMS-Arbeitsbereich**.  

6. Klicken Sie nach dem Bereitstellen der erforderlichen Informationen auf dem Blatt **OMS-Arbeitsbereich** auf **Erstellen**.  Die Informationen werden überprüft, und der Arbeitsbereich wird erstellt. Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen.  Sie gelangen zurück zum Blatt **Lösung hinzufügen**.  

7. Bestätigen Sie auf dem Blatt mit den Einstellungen für **Automation + Control**, dass Sie die empfohlenen vorab ausgewählten Lösungen installieren möchten. Falls Sie einige Lösungen deaktivieren, können Sie sie im Nachhinein einzeln installieren.  

8. Klicken Sie auf **Erstellen**, um mit der Integration von Automation und eines OMS-Arbeitsbereichs zu beginnen. Alle Einstellungen werden überprüft, und anschließend wird versucht, das Angebot in Ihrem Abonnement bereitzustellen.  Dieser Vorgang kann einige Sekunden dauern, und Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen. 

Nach der Integration des Angebots können Sie mit der Erstellung von Runbooks beginnen, die aktivierten Verwaltungslösungen einsetzen oder [Log Analytics](https://docs.microsoft.com/azure/log-analytics) verwenden, um Daten zu erfassen, die von den Ressourcen in Ihrer Cloud- oder lokalen Umgebung generiert werden.   

## <a name="next-steps"></a>Nächste Schritte
* Sie können überprüfen, ob die Authentifizierung Ihres neuen Automation-Kontos für Azure-Ressourcen funktioniert, indem Sie die Informationen zum [Testen der Authentifizierung für ausführende Azure Automation-Konten](automation-verify-runas-authentication.md) verwenden.
* Erste Schritte mit PowerShell-Runbooks werden unter [Mein erstes PowerShell-Runbook](automation-first-runbook-textual-powershell.md) beschrieben.
* Weitere Informationen zur grafischen Inhaltserstellung finden Sie unter [Grafische Erstellung in Azure Automation](automation-graphical-authoring-intro.md).


