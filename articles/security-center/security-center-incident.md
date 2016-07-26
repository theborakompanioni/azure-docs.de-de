<properties
   pageTitle="Behandeln von Sicherheitswarnungen in Azure Security Center | Microsoft Azure"
   description="Dieses Dokument enthält hilfreiche Informationen zur Verwendung der Azure Security Center-Funktionen zum Behandeln von Sicherheitsvorfällen."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/20/2016"
   ms.author="yurid"/>

# Behandeln von Sicherheitswarnungen in Azure Security Center 
Das Selektieren und Untersuchen von Sicherheitswarnungen kann auch für sehr erfahrene Sicherheitsanalysten zeitaufwändig sein, und häufig ist nur schwer zu erkennen, wo überhaupt begonnen werden soll. Mithilfe von [Analysen](security-center-detection-capabilities.md) zum Verknüpfen der Informationen der einzelnen [Sicherheitswarnungen](security-center-managing-and-responding-alerts.md) kann Security Center Ihnen eine zentrale Übersicht über einen Angriffsversuch und alle dazugehörigen Warnungen bieten. So können Sie schnell überblicken, welche Aktionen der Angreifer unternommen hat und welche Ressourcen betroffen sind.

In diesem Dokument wird beschrieben, wie Sie die Sicherheitswarnungsfunktionen in Security Center verwenden, um die Behandlung von Sicherheitsvorfällen zu unterstützen.


## Was ist ein Sicherheitsvorfall?

In Security Center ist ein Sicherheitsvorfall eine Aggregation aller Warnungen für eine Ressource, die [Kill Chain](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/)-Mustern entsprechen. Eine „Kill Chain“ ist die Schrittfolge eines Angreifers. Vorfälle werden auf der Kachel bzw. auf dem Blatt [Sicherheitswarnungen](security-center-managing-and-responding-alerts.md) angezeigt. Bei einem Vorfall wird die Liste mit den dazugehörigen Warnungen eingeblendet, damit Sie mehr Informationen zu den einzelnen Fällen einsehen können.

## Verwalten von Sicherheitsvorfällen

Sie können die aktuellen Sicherheitsvorfälle prüfen, indem Sie sich die Kachel „Sicherheitswarnungen“ ansehen. Greifen Sie auf das Azure-Portal zu, und führen Sie die unten angegebenen Schritte aus, um jeweils weitere Details zu einem Sicherheitsvorfall anzuzeigen:

1. Auf dem Security Center-Dashboard sehen Sie die Kachel **Sicherheitswarnungen**.

    ![Kachel „Sicherheitswarnungen“ in Security Center](./media/security-center-incident/security-center-incident-fig1.png)

2.  Klicken Sie auf diese Kachel, um sie zu erweitern. Wenn ein Sicherheitsvorfall erkannt wird, wird er im Graphen für die Sicherheitswarnungen eingeblendet. Dies ist in der folgenden Abbildung dargestellt.

    ![Sicherheitsvorfall](./media/security-center-incident/security-center-incident-fig2.png)

3.	Beachten Sie, dass die Beschreibung des Sicherheitsvorfalls im Gegensatz zu anderen Warnungen ein anderes Symbol enthält. Klicken Sie darauf, um weitere Details zu diesem Vorfall anzuzeigen.

	![Sicherheitsvorfall](./media/security-center-incident/security-center-incident-fig3.png)

4. 	Auf dem Blatt **Security incident detected** (Sicherheitsvorfall erkannt) sind weitere Details zum Sicherheitsvorfall angegeben. Dies sind beispielsweise die vollständige Beschreibung des Vorfalls, der Schweregrad (in diesem Fall „Hoch“), der aktuelle Status des Vorfalls (hier noch *aktiv*), die angegriffene Ressource (hier *VM1*) und die Lösungsschritte. Unten sind die Warnungen aufgeführt, die in diesem Vorfall enthalten sind. Wenn Sie weitere Informationen zu jeder Warnung erhalten möchten, klicken Sie einfach auf die Warnung. Ein weiteres Blatt wird wie unten dargestellt geöffnet.

	![Sicherheitsvorfall](./media/security-center-incident/security-center-incident-fig4.png)

Die Informationen auf diesem Blatt variieren je nach Warnung. Weitere Informationen zur Verwaltung dieser Warnungen finden Sie unter [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md). Wichtige Aspekte zu dieser Funktion:

- Mit einem neuen Filter können Sie Ihre Ansicht wie gewünscht anpassen (Nur Vorfall, Nur Warnungen oder beides).
- Die gleiche Warnung kann als Teil eines Vorfalls vorhanden sein (falls zutreffend) und als eigenständige Warnung angezeigt werden.
- Wenn ein Vorfall verworfen wird, werden die dazugehörigen Warnungen nicht verworfen.

## Weitere Informationen

In diesem Dokument haben Sie erfahren, wie Sie die Sicherheitsvorfallfunktion in Security Center verwenden. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

- [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md)
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md)
- [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
- [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md): Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
- [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

<!---HONumber=AcomDC_0720_2016-->