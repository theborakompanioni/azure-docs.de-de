---
title: Überwachen von Ressourcen in der Sicherheits- und Überwachungslösung von Operations Management Suite | Microsoft Docs
description: In diesem Dokument ist die Verwendung der Sicherheits- und Überwachungslösung von OMS zum Überwachen Ihrer Ressourcen und Identifizieren von Sicherheitsproblemen beschrieben.
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: ''

ms.service: operations-management-suite
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2016
ms.author: yurid

---
# <a name="monitoring-resources-in-operations-management-suite-security-and-audit-solution"></a>Überwachen von Ressourcen in der Sicherheits- und Überwachungslösung von Operations Management Suite
In diesem Dokument ist die Verwendung der Sicherheits- und Überwachungslösung von OMS zum Überwachen Ihrer Ressourcen und Identifizieren von Sicherheitsproblemen beschrieben.

## <a name="what-is-oms?"></a>Was ist OMS?
Microsoft Operations Management Suite (OMS) ist die cloudbasierte IT-Verwaltungslösung von Microsoft, die Ihnen das Verwalten und Schützen Ihrer lokalen und Cloudinfrastruktur erleichtert. Weitere Informationen zu OMS finden Sie im Artikel [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="monitoring-resources"></a>Überwachen von Ressourcen
Es ist natürlich wünschenswert, Sicherheitsvorfälle von vornherein zu verhindern. Allerdings ist es unmöglich, das Auftreten aller Sicherheitsvorfälle zu verhindern. Wenn ein Sicherheitsvorfall eintritt, müssen Sie sicherstellen, dass die Auswirkungen minimiert werden.  Es gibt drei wichtige Empfehlungen, an die Sie sich halten können, um die Anzahl und die Auswirkungen von Sicherheitsvorfällen zu verringern:

* Bewerten Sie regelmäßig Sicherheitsrisiken in Ihrer Umgebung.
* Prüfen Sie regelmäßig alle Computersysteme und Netzwerkgeräte, um sicherzustellen, dass darauf die neuesten Patches installiert sind.
* Prüfen Sie regelmäßig alle Protokolle und Protokollierverfahren, z.B. Betriebssystem-Ereignisprotokolle, anwendungsspezifische Protokolle und Protokolle von Angriffserkennungssystemen.

Mit der Sicherheits- und Überwachungslösung von OMS kann die IT-Abteilung aktiv alle Ressourcen überwachen. Dies kann zur Minimierung der Auswirkungen von Sicherheitsvorfällen beitragen. Die Sicherheits- und Überwachungslösung von OMS verfügt über Sicherheitsdomänen, die zum Überwachen von Ressourcen verwendet werden können. Die Sicherheitsdomänen ermöglichen den schnellen Zugriff auf Optionen. Für die Sicherheitsüberwachung werden die folgenden Domänen ausführlicher beschrieben:

* Bewertung von Schadsoftware
* Updatebewertung
* Identität und Zugriff

> [!NOTE]
> Eine Übersicht über alle Optionen können Sie sich unter [Erste Schritte mit der Sicherheits- und Überwachungslösung von Operations Management Suite](oms-security-getting-started.md)verschaffen.
> 
> 

### <a name="monitoring-system-protection"></a>Überwachen des Systemschutzes
Bei einem tiefgreifenden Verteidigungsansatz ist jede Schutzschicht für den allgemeinen Sicherheitszustand Ihrer Daten wichtig. Computer mit erkannten Bedrohungen und Computer mit unzureichendem Schutz werden auf der Kachel „Malware Assessment“ (Bewertung von Schadsoftware) unter „Security Domains“ (Sicherheitsdomänen) angezeigt. Anhand der Informationen unter „Malware Assessment“ (Bewertung von Schadsoftware) können Sie einen Plan zum Anwenden von Schutzmaßnahmen auf die betreffenden Server erstellen. Führen Sie die folgenden Schritte aus, um auf diese Option zuzugreifen:

1. Klicken Sie im Hauptdashboard der **Microsoft Operations Management Suite** auf die Kachel **Sicherheit und Überwachung**.
   
    ![Security and Audit](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. Klicken Sie im Dashboard **Sicherheit und Überwachung** unter **Sicherheitsdomänen** auf **Antischadsoftwarebewertung**. Das Dashboard **Antischadsoftwarebewertung** wird wie unten dargestellt angezeigt:

![Bewertung von Schadsoftware](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig2-ga.png)

Sie können das Dashboard **Malware Assessment** (Bewertung von Schadsoftware) verwenden, um die folgenden Sicherheitsprobleme zu identifizieren:

* **Aktive Bedrohungen**: Computer, die kompromittiert wurden und im System über aktive Bedrohungen verfügen.
* **Bereinigte Bedrohungen**: Computer, die kompromittiert wurden, deren Bedrohungen aber bereinigt wurden.
* **Veraltete Signatur**: Computer, für die der Schutz vor Schadsoftware aktiviert ist, deren Signatur aber veraltet ist.
* **Kein Echtzeitschutz**: Computer, auf denen keine Antischadsoftware installiert ist.

### <a name="monitoring-updates"></a>Überwachen von Updates
Das Anwenden der aktuellen Sicherheitsupdates ist eine bewährte Methode im Bereich der Sicherheit, die Sie bei Ihrer Strategie für die Updateverwaltung berücksichtigen sollten. Der Microsoft Monitoring Agent-Dienst (HealthService.exe) liest Updateinformationen von überwachten Computern und sendet diese aktualisierten Informationen dann zur Verarbeitung an den OMS-Dienst in der Cloud. Der Microsoft Monitoring Agent-Dienst ist als automatischer Dienst konfiguriert und sollte auf dem Zielcomputer immer ausgeführt werden.

![Überwachen von Updates](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig3.png)

Auf die Updatedaten wird Logik angewendet, und der Cloud-Dienst zeichnet die Daten auf. Wenn ermittelt wird, dass Updates fehlen, werden diese im Dashboard **Updates** angezeigt. Mithilfe des Dashboards **Updates** können Sie mit fehlenden Updates arbeiten und einen Plan für deren Anwendung auf die betreffenden Server entwickeln. Führen Sie die unten angegebenen Schritte aus, um auf das Dashboard für die **Updates** zuzugreifen:

1. Klicken Sie im Hauptdashboard der **Microsoft Operations Management Suite** auf die Kachel **Sicherheit und Überwachung**.
2. Klicken Sie im Dashboard **Sicherheit und Überwachung** unter **Sicherheitsdomänen** auf **Updatebewertung**. Das Dashboard für Updates wird wie unten dargestellt angezeigt:

![Updatebewertung](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig4.png)

In diesem Dashboard können Sie eine Updatebewertung durchführen, um den aktuellen Zustand Ihrer Computer zu ermitteln und den wichtigsten Bedrohungen zu begegnen. Über die Kachel **Kritische Updates oder Sicherheitsupdates** können IT-Administratoren auf ausführliche Informationen zu den fehlenden Updates zugreifen. Dies ist unten dargestellt:

![Suchergebnis](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig5.png)

Dieser Bericht enthält wichtige Informationen, die Sie zum Identifizieren der Art von Bedrohungen verwenden können, für die das System anfällig ist. Dies umfasst auch die Artikel der Microsoft KB, die dem Sicherheitsupdate zugeordnet sind, und das MS Bulletin, das weitere Details zum Sicherheitsrisiko enthält.

### <a name="monitoring-identity-and-access"></a>Überwachen der Identität und des Zugriffs
Da Benutzer an den unterschiedlichsten Orten arbeiten und dabei unterschiedliche Geräte verwenden und auf viele Cloud-Apps und lokale Apps zugreifen, ist es zwingend erforderlich, ihre Anmeldeinformationen zu schützen. Bei Angriffen mit gestohlenen Anmeldeinformationen verschafft sich ein Angreifer zuerst Zugang zu den Anmeldeinformationen eines regulären Benutzers, um damit im Netzwerk dann auf ein System zuzugreifen. Häufig ist dieser erste Angriff lediglich das Mittel, um Zugang zum Netzwerk zu erhalten. Das Hauptziel ist der Zugriff auf Konten mit höheren Rechten. 

Angreifer bewegen sich im Netzwerk, indem sie frei verfügbare Tools nutzen, um Anmeldeinformationen aus den Sitzungen von anderen angemeldeten Konten zu extrahieren. Je nach Systemkonfiguration können diese Anmeldeinformationen in Form von Hashes, Tickets oder auch Klartext-Kennwörtern extrahiert werden.  

> [!NOTE]
> Für Computer, auf die über das Internet direkt zugegriffen werden kann, kommt es zu vielen fehlgeschlagenen Anmeldeversuchen mit allen Arten von gängigen Benutzernamen (z.B. „Administrator“). Dies stellt in den meisten Fällen keine größere Gefahr dar, solange keine gängigen Benutzernamen verwendet werden und das Kennwort sicher genug ist.
> 
> 

Es ist möglich, diese Eindringlinge zu identifizieren, bevor sie ein Konto mit erhöhten Rechten kompromittieren. Sie können die **Sicherheits- und Überwachungslösung von OMS** nutzen, um die Identität und den Zugriff zu überwachen. Führen Sie die unten angegebenen Schritte aus, um auf das Dashboard **Identität und Zugriff** zuzugreifen:

1. Klicken Sie im Hauptdashboard von **Microsoft Operations Management Suite** auf die Kachel „Security and Audit“ (Sicherheit und Überwachung).
2. Klicken Sie im Dashboard **Sicherheit und Überwachung** unter **Sicherheitsdomänen** auf **Identität und Zugriff**. Das Dashboard **Identität und Zugriff** wird wie hier dargestellt angezeigt:

![Identität und Zugriff](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig6-ga.png)

Im Rahmen Ihrer normalen Überwachungsstrategie sollten Sie auch die Überwachung der Identität berücksichtigen. Von den IT-Administratoren sollte überprüft werden, ob es einen gültigen Benutzernamen gibt, für den viele Anmeldeversuche vorliegen. Dies kann entweder auf einen Angreifer hinweisen, der sich einen richtigen Benutzernamen beschafft hat und einen Brute-Force-Angriff durchführt, oder auf ein automatisches Tool, für das ein abgelaufenes hartcodiertes Kennwort verwendet wird.

Mit diesem Dashboard kann das IT-Personal potenzielle Bedrohungen, die sich auf die Identität und den Zugriff beziehen, und den Zugriff auf die Ressourcen des Unternehmens schnell identifizieren. Es ist besonders wichtig, auch potenzielle Bedrohungen zu identifizieren. Auf der Kachel „Logons Over Time“ (Anmeldungen nach Zeit) sehen Sie beispielsweise für einen bestimmten Zeitraum, wie häufig ein nicht erfolgreicher Anmeldeversuch durchgeführt wurde. In diesem Fall sind für den Computer **FileServer** 35 Anmeldeversuche angegeben. Sie können weitere Details zu diesem Computer anzeigen, indem Sie darauf klicken. 

![Weitere Informationen](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig7-new.png)

Der für diesen Computer generierte Bericht liefert wertvolle Details zu diesem Muster. Beachten Sie, dass in der Spalte **ACCOUNT** das Benutzerkonto angegeben ist, mit dem versucht wurde, auf das System zuzugreifen. In der Spalte **TIMEGENERATED** ist das Zeitintervall angegeben, in das der Versuch gefallen ist, und in der Spalte **LOGONTYPENAME** sehen Sie den Standort des Anmeldeversuchs. Wenn diese Versuche lokal im System von einem Programm durchgeführt worden wären, würde in der Spalte **PROCESS** der Name des Prozesses angezeigt werden. In Fällen, in denen der Anmeldeversuch von einem Programm stammt, ist der Prozessname bereits angegeben, sodass Sie den Vorgang im Zielsystem genauer untersuchen können.

## <a name="see-also"></a>Weitere Informationen
In diesem Dokument haben Sie erfahren, wie Sie die Sicherheits- und Überwachungslösung von OMS zum Überwachen Ihrer Ressourcen verwenden. Weitere Informationen zur Sicherheitslösung von OMS finden Sie in den folgenden Artikeln:

* [Operations Management Suite (OMS) – Übersicht](operations-management-suite-overview.md)
* [Erste Schritte mit der Sicherheits- und Überwachungslösung von Operations Management Suite](oms-security-getting-started.md)
* [Überwachen von und Reagieren auf Sicherheitswarnungen in der Sicherheits- und Überwachungslösung von Operations Management Suite](oms-security-responding-alerts.md)

<!--HONumber=Oct16_HO2-->


