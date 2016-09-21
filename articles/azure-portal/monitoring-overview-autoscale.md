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
	ms.date="08/30/2016"
	ms.author="robb"/>

# Übersicht über die automatische Skalierung in Microsoft Azure Virtual Machines, Cloud Services und Web-Apps

In diesem Artikel wird beschrieben, was die automatische Skalierung von Microsoft Azure ist, welche Vorteile sie hat und wie Sie am besten mit der Nutzung beginnen.

Die automatische Azure Insights-Skalierung gilt nur für:

* [Skalierungsgruppen für virtuelle Computer](https://azure.microsoft.com/services/virtual-machine-scale-sets/)
* [Cloud Services](https://azure.microsoft.com/services/cloud-services/)
* [App Service – Web-Apps](https://azure.microsoft.com/services/app-service/web/)
  
   
>[AZURE.NOTE] Azure verfügt über zwei Methoden für die automatische Skalierung. Eine ältere Version der automatischen Skalierung gilt für virtuelle Computer (Verfügbarkeitsgruppen). Diese Funktion verfügt über eingeschränkten Support, und wir empfehlen die Migration zu VM-Skalierungsgruppen, um für einen schnelleren und zuverlässigeren Support der automatischen Skalierung zu sorgen. Dieser Artikel enthält einen Link zur Verwendung der älteren Technologie.


## Grundlagen der automatischen Skalierung 

Bei der automatischen Skalierung können Sie die richtige Menge von Ressourcen ausführen, um die Anwendungslast zu bewältigen, ohne dass aufgrund von ungenutzten Ressourcen unnötige Kosten entstehen. Sie können Computeressourcen automatisch basierend auf einer Gruppe von Regeln hinzufügen oder entfernen. Dieses Konzept ist in Abbildung 1 dargestellt.

![Beschreibung des Konzepts der automatischen Skalierung](./media/monitoring-autoscale-overview/AutoscaleConcept.png)

**Abbildung 1: Beschreibung des Konzepts der automatischen Skalierung**

Bei der automatischen Skalierung wird nur horizontal skaliert. Dies bedeutet, dass die Anzahl von VM-Instanzen erhöht („horizontal hochskalieren“) oder verringert wird („horizontal herunterskalieren“). Die horizontale Skalierung ist in einer Cloudsituation flexibler, da Sie bei Bedarf Tausende von VMs ausführen können, um die Last zu bewältigen. Die andere Art der Skalierung ist die vertikale Skalierung. Bei der vertikalen Skalierung wird die gleiche Anzahl von VMs beibehalten, aber die Leistungsfähigkeit der VMs wird erhöht („zentral hochskalieren“) oder verringert („zentral herunterskalieren“). Die Leistungsfähigkeit wird in Bezug auf den Arbeitsspeicher, die CPU-Geschwindigkeit, Speicherplatz auf dem Datenträger usw. gemessen. Für die vertikale Skalierung gelten mehr Einschränkungen. Sie ist von der Verfügbarkeit größerer Hardware abhängig, die je nach Region variieren kann, und erreicht relativ schnell das obere Limit. Außerdem ist für die vertikale Skalierung auch das Beenden und Starten einer VM erforderlich. Weitere Informationen finden Sie unter [Vertikales Skalieren von virtuellen Azure-Computern mit Azure Automation](../virtual-machines/virtual-machines-linux-vertical-scaling-automation.md).


Bei der automatischen Skalierung müssen Sie Regeln dafür erstellen, wann Ressourcen horizontal hoch- oder herunterskaliert werden sollen. Kriterien, die Sie zum Steuern von Skalierungsaktionen festlegen können

* **Minimale** und **maximale** Anzahl von auszuführenden Instanzen. Mit dem Minimum wird sichergestellt, dass die Anwendung immer ausgeführt wird, und das Maximum dient zum Kontrollieren Ihrer Kosten.
* **Regel oder Bedingung** für die automatische Skalierung. Dies kann eine Skalierung sein, die auf einer Metrik oder einem Zeitplan basiert.
* Mit **Abkühldauer** wird angegeben, wie lange nach einem Ereignis zur automatischen Skalierung gewartet wird, bevor ein weiteres Ereignis zur automatischen Skalierung ausgeführt werden kann. Dieser Zeitraum dient dem Schutz vor einem Zustand, der als „Flapping“ (Fluktuation) bezeichnet wird. Er tritt ein, wenn innerhalb weniger Minuten VMs in schneller Folge hinzugefügt und entfernt werden. Das Starten oder Beenden einer VM ist mit Kosten verbunden. Mit dem „Flapping“ werden keine Kosten eingespart, und mit der gestarteten und beendeten VM kann keine nützliche Verarbeitung durchgeführt werden. Dies führt also zu mehr Nachteilen, als wenn die VM einfach weiter ausgeführt wird.

   
Die vollständige Liste mit den konfigurierbaren Werten finden Sie unter den Informationen zur [REST-API für die automatische Skalierung](https://msdn.microsoft.com/library/dn931928.aspx).


## Konzeptionelles Diagramm  
Abbildung 2 enthält eine Konzeptübersicht über die automatische Skalierung, gefolgt von einer Erklärung der Diagrammelemente.

![Warnung hinzufügen](./media/monitoring-autoscale-overview/AutoscaleOverview3.png)

**Abbildung 2: Übersicht über den Prozess der automatischen Skalierung**

## Ressourcenmetriken 
Ressourcen geben Metriken aus, die später von Regeln verarbeitet werden. Metriken sind über verschiedene Methoden verfügbar. Für VM-Skalierungsgruppen werden Telemetriedaten von Azure-Diagnose-Agents verwendet, während die Telemetriedaten für Web-Apps und Clouddienste direkt aus der Azure-Infrastruktur stammen. Einige häufig verwendete Statistiken sind CPU-Nutzung, Arbeitsspeichernutzung, Threadanzahl, Warteschlangenlänge und Datenträgerverwendung. Eine Liste mit den Telemetriedaten, die Sie verwenden können, finden Sie unter [Allgemeine Metriken für die automatische Skalierung](insights-autoscale-common-metrics.md).

 
## Zeit
Für zeitplanbasierte Regeln gilt die UTC-Zeit. Sie müssen die Zeitzone richtig einrichten, wenn Sie Ihre Regeln festlegen.

## Regeln für die automatische Skalierung
Im Diagramm wird nur eine Regel für die automatische Skalierung angezeigt, aber Sie können auch eine höhere Zahl von Regeln verwenden. Sie können je nach Ihren Anforderungen komplexe überlappende Regeln erstellen. Regeltypen
 
 - **Metrikbasiert**: Führen Sie diese Aktion beispielsweise aus, wenn die CPU-Auslastung oberhalb von 50% liegt.
 - **Zeitbasiert**: Lösen Sie einen Webhook beispielsweise jeweils samstags um 8 Uhr in einer bestimmten Zeitzone aus.

 
## Aktionen und Automation der automatischen Skalierung

Mit Regeln können eine oder mehrere Arten von Aktionen ausgelöst werden.

- **Skalieren**: Horizontales Herunter- oder Hochskalieren von VMs
- **E-Mail**: Senden einer E-Mail an Administratoren und Co-Admins eines Abonnements und an weitere von Ihnen angegebene E-Mail-Adressen
- **Über Webhooks automatisieren**: Aufrufen von Webhooks, mit denen mehrere komplexe Aktionen innerhalb oder außerhalb von Azure ausgelöst werden können. In Azure können Sie ein Azure Automation-Runbook, eine Azure-Funktion oder eine Azure-Logik-App starten. Beispiele für Drittanbieter-URLs außerhalb von Azure sind Dienste wie Slack und Twilio.


## Details von Resource Manager-Regeln

Regeln für die automatische Skalierung haben in einer Azure Resource Manager-Vorlage die folgende Struktur.

![Azure Resource Manager – Struktur der Regeln für die automatische Skalierung](./media/monitoring-autoscale-overview/AzureResourceManagerRuleStructure2.png)

Sie können mehrere Profile auflisten. Jedes Profil kann mehrere Regeln enthalten. Benachrichtigungsmethoden und Speicherorte werden nach Ihren Profilen angegeben. Beispiele für die Benachrichtigung sind ein Webhook mit dem URI oder E-Mails mit den E-Mail-Adressen.

Codebeispiele finden Sie unter:

* [Erweiterte Konfiguration der automatischen Skalierung mithilfe von Resource Manager-Vorlagen für VM-Skalierungsgruppen](insights-advanced-autoscale-virtual-machine-scale-sets.md)
* [REST-API für die automatische Skalierung](https://msdn.microsoft.com/library/dn931953.aspx)

## Zugriffsmethoden 
Zum Einrichten von Regeln für die automatische Skalierung können Sie Folgendes verwenden:

- Azure-Portal
- PowerShell
- Befehlszeilenschnittstelle
- Insights-REST-API

## Exemplarische Vorgehensweisen für die automatische Skalierung

- [Skalieren von Clouddiensten](../cloud-services/cloud-services-how-to-scale-portal.md)
- [Skalieren von Web-Apps](insights-how-to-scale.md)
- [Scaling Classic Virtual Machine Availability Sets](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) (Skalieren von klassischen VM-Verfügbarkeitsgruppen)
- [Skalieren von VM-Skalierungsgruppen in Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md)
- [Skalieren von VM-Skalierungsgruppen in Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md)
- [Erweiterte Konfiguration der automatischen Skalierung mithilfe von Resource Manager-Vorlagen für VM-Skalierungsgruppen](insights-advanced-autoscale-virtual-machine-scale-sets.md)

## Nächste Schritte

Weitere Informationen zur automatischen Skalierung können Sie den obigen exemplarischen Vorgehensweisen für die automatische Skalierung oder den folgenden Ressourcen entnehmen:

- [Allgemeine Metriken für die automatische Skalierung in Azure Insights](insights-autoscale-common-metrics.md)
- [Empfohlene Methoden für die automatische Skalierung in Azure Insights](insights-autoscale-best-practices.md)
- [Verwenden von automatischen Skalierungsvorgängen zum Senden von E-Mail- und Webhook-Warnbenachrichtigungen](insights-autoscale-to-webhook-email.md)
- [REST-API für die automatische Skalierung](https://msdn.microsoft.com/library/dn931953.aspx): Hier finden Sie die gesamte API und Informationen zu den Feldern und Werten.
- [Beheben von Problemen bei der automatischen Skalierung von VM-Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)

<!---HONumber=AcomDC_0907_2016-->