<properties
	pageTitle="Übersicht über die automatische Skalierung in Microsoft Azure Virtual Machines, Cloud Services und Web-Apps | Microsoft Azure"
	description="Enthält eine Übersicht über die automatische Skalierung in Microsoft Azure. Gilt für Virtual Machines, Cloud Services und Web-Apps."
	authors="rboucher"
	manager=""
	editor=""
	services="monitoring-and-diagnostics"
	documentationCenter="monitoring-and-diagnostics"/>

<tags
	ms.service="monitoring-and-diagnostics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/06/2016"
	ms.author="robb"/>

# Übersicht über die automatische Skalierung in Microsoft Azure Virtual Machines, Cloud Services und Web-Apps

In diesem Artikel wird beschrieben, was die automatische Skalierung von Microsoft Azure ist, welche Vorteile sie hat und wie Sie mit der Nutzung beginnen.

Die automatische Skalierung von Azure Insights gilt nur für [Skalierungsgruppen für virtuelle Computer](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloud Services](https://azure.microsoft.com/services/cloud-services/) und [App Service – Web-Apps](https://azure.microsoft.com/services/app-service/web/).

>[AZURE.NOTE] Azure verfügt über zwei Methoden für die automatische Skalierung. Eine ältere Version der automatischen Skalierung gilt für virtuelle Computer (Verfügbarkeitsgruppen). Diese Funktion verfügt über eingeschränkten Support. Wir empfehlen die Migration zu VM-Skalierungsgruppen, um für einen schnelleren und zuverlässigeren Support der automatischen Skalierung zu sorgen. Dieser Artikel enthält einen Link zur Verwendung der älteren Technologie.


## Was ist die automatische Skalierung?

Bei der automatischen Skalierung können Sie jeweils die richtige Menge an Ressourcen ausführen, um die Lasten für Ihre Anwendung zu bewältigen. Sie können Ressourcen hinzufügen, um auf einen Anstieg der Last zu reagieren, und Kosten sparen, indem Sie ungenutzte Ressourcen entfernen. Sie geben eine minimale und maximale Anzahl von auszuführenden Instanzen an und können VMs basierend auf einer Reihe von Regeln automatisch hinzufügen oder entfernen. Eine minimale Anzahl sorgt dafür, dass Ihre Anwendung auch dann immer ausgeführt wird, wenn keine Last vorhanden ist. Mit einer maximalen Anzahl können Sie eine Obergrenze für die Gesamtkosten pro Stunde festlegen. Sie bestimmen die automatische Skalierung zwischen diesen beiden Grenzwerten, indem Sie Regeln erstellen.

 ![Automatische Skalierung – Beschreibung VMs hinzufügen und entfernen](./media/monitoring-autoscale-overview/AutoscaleConcept.png)

Wenn die Bedingungen der Regel erfüllt sind, werden eine oder mehrere Aktionen der automatischen Skalierung ausgelöst. Sie können VMs hinzufügen und entfernen oder andere Aktionen durchführen. Im folgenden konzeptionellen Diagramm ist dieser Prozess dargestellt.

 ![Konzeptionelles Flussdiagramm – Automatische Skalierung](./media/monitoring-autoscale-overview/AutoscaleOverview3.png)
 

## Prozess der automatischen Skalierung
Die folgende Beschreibung bezieht sich auf die einzelnen Elemente des obigen Diagramms.

### Ressourcenmetriken 
Ressourcen geben Metriken aus, die später von Regeln verarbeitet werden. Metriken sind über verschiedene Methoden verfügbar. Für VM-Skalierungsgruppen werden Telemetriedaten von Azure-Diagnose-Agents verwendet, während die Telemetriedaten für Web-Apps und Clouddienste direkt aus der Azure-Infrastruktur stammen. Einige häufig verwendete Statistiken sind CPU-Nutzung, Arbeitsspeichernutzung, Threadanzahl, Warteschlangenlänge und Datenträgerverwendung. Eine Liste mit den Telemetriedaten, die Sie verwenden können, finden Sie unter [Allgemeine Metriken für die automatische Skalierung](insights-autoscale-common-metrics.md).

### Zeit
Für zeitplanbasierte Regeln gilt die UTC-Zeit. Sie müssen die Zeitzone richtig einrichten, wenn Sie Ihre Regeln festlegen.

### Regeln
Im Diagramm wird nur eine Regel für die automatische Skalierung angezeigt, aber Sie können auch eine höhere Zahl von Regeln verwenden. Sie können je nach Ihren Anforderungen komplexe überlappende Regeln erstellen. Regeltypen
 
 - **Metrikbasiert**: Führen Sie diese Aktion beispielsweise aus, wenn die CPU-Auslastung oberhalb von 50% liegt.
 - **Zeitbasiert**: Lösen Sie einen Webhook beispielsweise jeweils samstags um 8 Uhr in einer bestimmten Zeitzone aus.

Mit metrikbasierten Regeln wird die Anwendungslast gemessen, und basierend auf dieser Last werden VMs hinzugefügt oder entfernt. Zeitplanbasierte Regeln ermöglichen die Skalierung, wenn für die Last Zeitmuster erkennbar sind und Sie die Skalierung durchführen möchten, bevor eine mögliche Zu- oder Abnahme der Last eintritt.

 
### Aktionen und Automation

Mit Regeln können eine oder mehrere Arten von Aktionen ausgelöst werden.

- **Skalieren**: Horizontales Herunter- oder Hochskalieren von VMs
- **E-Mail**: Senden einer E-Mail an Abonnementadministratoren, Co-Admins und weitere von Ihnen angegebene E-Mail-Adressen
- **Über Webhooks automatisieren**: Aufrufen von Webhooks, mit denen mehrere komplexe Aktionen innerhalb oder außerhalb von Azure ausgelöst werden können. In Azure können Sie ein Azure Automation-Runbook, eine Azure-Funktion oder eine Azure-Logik-App starten. Beispiele für Drittanbieter-URLs außerhalb von Azure sind Dienste wie Slack und Twilio.


## Einstellungen für die automatische Skalierung
Für die automatische Skalierung wird die folgende Terminologie bzw. Struktur verwendet.

- Eine **Autoskalierungseinstellung** wird vom Modul für die automatische Skalierung gelesen, um zu ermitteln, ob zentral hoch- oder herunterskaliert werden soll. Sie enthält mindestens ein Profil, Informationen zur Zielressource und Benachrichtigungseinstellungen.
    - Ein **Autoskalierungsprofil** ist eine Kombination aus einer Kapazitätseinstellung, einem Regelsatz für die Trigger, Skalierungsaktionen für das Profil und einer Wiederholung. Sie können mehrere Profile verwenden, um unterschiedliche Anforderungen abzudecken, die sich ggf. überschneiden.
        - Mit einer **Kapazitätseinstellung** werden die minimalen Werte, maximalen Werte und Standardwerte für die Anzahl von Instanzen angegeben. [Platz für Abbildung 1]
        - Eine **Regel** enthält einen Trigger – entweder einen metrischen Trigger oder einen zeitlichen Trigger – und eine Skalierungsaktion, mit der angegeben wird, ob automatisch zentral hoch- oder herunterskaliert werden soll, wenn die Bedingung der Regel erfüllt ist.
        - Mit einer **Wiederholung** wird angegeben, wann die automatische Skalierung dieses Profil anwenden soll. Sie können beispielsweise unterschiedliche Autoskalierungsprofile für verschiedene Tageszeiten oder Wochentage verwenden.
- Mit einer **Benachrichtigungseinstellung** wird definiert, welche Benachrichtigungen gesendet werden sollen, wenn ein Autoskalierungsereignis eintritt. Hierfür müssen jeweils die entsprechenden Kriterien eines Profils der Autoskalierungseinstellung erfüllt sein. Bei der automatischen Skalierung können Benachrichtigungen an eine oder mehrere E-Mail-Adressen gesendet oder Webhooks aufgerufen werden.
 
![Azure-Autoskalierungseinstellung, Profil und Regelstruktur](./media/monitoring-autoscale-overview/AzureResourceManagerRuleStructure3.png)

Die vollständige Liste mit den konfigurierbaren Feldern und Beschreibungen finden Sie unter den Informationen zur [REST-API für die automatische Skalierung](https://msdn.microsoft.com/library/dn931928.aspx).

Codebeispiele finden Sie unter:

* [Erweiterte Konfiguration der automatischen Skalierung mithilfe von Resource Manager-Vorlagen für VM-Skalierungsgruppen](insights-advanced-autoscale-virtual-machine-scale-sets.md)
* [REST-API für die automatische Skalierung](https://msdn.microsoft.com/library/dn931953.aspx)



## Horizontale und vertikale Skalierung
  
Bei der automatischen Skalierung wird nur horizontal skaliert, und Ressourcen werden entsprechend bereitgestellt. Dies bedeutet, dass die Anzahl von VM-Instanzen erhöht („horizontal hochskalieren“) oder verringert wird („horizontal herunterskalieren“). Die horizontale Skalierung ist in einer Cloudsituation flexibler, da Sie bei Bedarf Tausende von VMs ausführen können, um die Last zu bewältigen. Bei der vertikalen Skalierung ist dies anders. Hierbei wird die gleiche Anzahl von VMs beibehalten, aber die Leistungsfähigkeit der VMs wird erhöht („zentral hochskalieren“) oder verringert („zentral herunterskalieren“). Die Leistungsfähigkeit wird in Bezug auf den Arbeitsspeicher, die CPU-Geschwindigkeit, Speicherplatz auf dem Datenträger usw. gemessen. Für die vertikale Skalierung gelten mehr Einschränkungen. Sie ist von der Verfügbarkeit größerer Hardware abhängig, die je nach Region variieren kann, und erreicht relativ schnell das obere Limit. Außerdem ist für die vertikale Skalierung auch das Beenden und Starten einer VM erforderlich. Weitere Informationen finden Sie unter [Vertikales Skalieren von virtuellen Azure-Computern mit Azure Automation](../virtual-machines/virtual-machines-linux-vertical-scaling-automation.md).


## Zugriffsmethoden 
Zum Einrichten der automatischen Skalierung können Sie Folgendes verwenden:

- [Azure-Portal](insights-how-to-scale.md)
- [PowerShell](insights-powershell-samples.md#create-and-manage-autoscale-settings)
- [Plattformübergreifende Befehlszeilenschnittstelle](insights-cli-samples.md#autoscale)
- [Insights-REST-API](https://msdn.microsoft.com/library/azure/dn931953.aspx)

## Unterstützte Dienste für die automatische Skalierung


| Dienst | Schema und Dokumente |
|--------------------------------------|-----------------------------------------------------|
| Web-Apps | [Skalieren von Web-Apps](insights-how-to-scale.md) |
| Cloud Services | [Automatisches Skalieren eines Clouddiensts](../cloud-services/cloud-services-how-to-scale.md) |
| Virtuelle Computer: Klassisch | [Scaling Classic Virtual Machine Availability Sets (Skalieren von klassischen VM-Verfügbarkeitsgruppen)](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) |
| Virtuelle Computer: Windows-Skalierungsgruppen| [Skalieren von VM-Skalierungsgruppen in Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) |
| Virtuelle Computer: Linux-Skalierungsgruppen | [Skalieren von VM-Skalierungsgruppen in Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) |
| Virtuelle Computer: Windows-Beispiel | [Erweiterte Konfiguration der automatischen Skalierung mithilfe von Resource Manager-Vorlagen für VM-Skalierungsgruppen](insights-advanced-autoscale-virtual-machine-scale-sets.md) |

## Nächste Schritte

Weitere Informationen zur automatischen Skalierung können Sie den obigen exemplarischen Vorgehensweisen für die automatische Skalierung oder den folgenden Ressourcen entnehmen:

- [Allgemeine Metriken für die automatische Skalierung in Azure Insights](insights-autoscale-common-metrics.md)
- [Empfohlene Methoden für die automatische Skalierung in Azure Insights](insights-autoscale-best-practices.md)
- [Verwenden von automatischen Skalierungsvorgängen zum Senden von E-Mail- und Webhook-Warnbenachrichtigungen](insights-autoscale-to-webhook-email.md)
- [REST-API für die automatische Skalierung](https://msdn.microsoft.com/library/dn931953.aspx)
- [Beheben von Problemen bei der automatischen Skalierung von VM-Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)

<!---HONumber=AcomDC_0914_2016-->