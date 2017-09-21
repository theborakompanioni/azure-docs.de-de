---
title: Informationen zu Bedrohungen in Azure Security Center | Microsoft-Dokumentation
description: "In diesem Dokument wird die Verwendung der Funktion für Informationen zu Bedrohungen in Azure Security Center beschrieben, mit der potenzielle Bedrohungen auf Ihren VMs und Computern identifiziert werden können."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: a771a3a1-2925-46ca-8e27-6f6a0746f58b
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: c492662aa3ee444347c55d9771790573ad969150
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="threat-intelligence-in-azure-security-center"></a>Informationen zu Bedrohungen in Azure Security Center
In diesem Dokument wird die Verwendung der Funktion für Informationen zu Bedrohungen in Azure Security Center beschrieben, mit der sicherheitsbezogene Probleme gelöst werden können.

## <a name="what-is-threat-intelligence"></a>Was sind Informationen zu Bedrohungen?
Mithilfe der Option „Informationen zu Bedrohungen“ (Threat Intelligence) in Security Center können IT-Administratoren Sicherheitsrisiken für die Umgebung erkennen. So kann beispielsweise ermittelt werden, ob ein bestimmter Computer Teil eines Botnets ist. Computer können Knoten in einem Botnet werden, wenn Angreifer illegal Schadsoftware installieren, die diesen Computer heimlich mit einem Steuerknoten verbindet. Die Option ermöglicht auch die Erkennung potenzieller Bedrohungen aus verdeckten Kommunikationskanälen wie dem Darknet.

Für den Aufbau dieser Threat Intelligence werden in Security Center Daten aus mehreren Quellen von Microsoft genutzt. Security Center setzt diese Daten zum Erkennen potenzieller Bedrohungen für Ihre Umgebung ein. Der Bereich Threat Intelligence umfasst drei wichtige Optionen:

- Arten der erkannten Bedrohungen
- Ursprung der Bedrohung
- Threat Intelligence-Karte


## <a name="when-should-i-use-threat-intelligence"></a>Wann sollte ich Threat Intelligence verwenden?
Einer der Schritte bei der [Reaktion auf Sicherheitsvorfälle](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide#incident-response) ist die Ermittlung des Schweregrads der Gefährdung für die Systeme. In dieser Phase sollten Sie die folgenden Aufgaben durchführen:

- Bestimmen der Art des Angriffs
- Bestimmen des ersten Angriffspunkts
- Bestimmen der Absicht des Angriffs. Ist der Angriff speziell auf Ihre Organisation ausgerichtet, um bestimmte Informationen zu erlangen, oder erfolgte er zufällig?
- Ermitteln der betroffenen Systeme
- Identifizieren der Dateien, auf die zugegriffen wurde, und Bestimmen der Vertraulichkeit dieser Dateien. Sie können die Informationen zu Bedrohungen in Security Center als Hilfe für diese Aufgaben nutzen. 

## <a name="how-to-access-the-threat-intelligence"></a>Wie greife ich auf die Informationen zu Bedrohungen zu?
Sie müssen zuerst den Arbeitsbereich auswählen, in dem sich Ihre Informationen befinden, um die aktuellen Informationen zu Bedrohungen für Ihre Umgebung zu visualisieren. Wenn Sie nicht über mehrere Arbeitsbereiche verfügen, wird die Arbeitsbereichsauswahl nicht angezeigt, und Sie gelangen direkt zum Dashboard **Informationen zu Bedrohungen**. Führen Sie die folgenden Schritte aus, um auf das Dashboard mit den Informationen zu Bedrohungen zuzugreifen:

1.  Öffnen Sie das Dashboard **Security Center**.
2.  Klicken Sie im linken Bereich unter **Erkennung** auf **Informationen zu Bedrohungen**. Das Dashboard **Informationen zu Bedrohungen** wird angezeigt.

    ![Threat Intelligence](./media/security-center-threat-intel/security-center-threat-intel-fig1.png)

    > [!NOTE]
    > Wenn in der letzten Spalte **UPGRADEPLAN** angezeigt wird, liegt dies daran, dass für diesen Arbeitsbereich das kostenlose Abonnement verwendet wird und Sie ein Upgrade auf ein Standard-Abonnement durchführen müssen, um dieses Feature nutzen zu können. Bei REQUIRES UPDATE (UPDATE ERFORDERLICH) müssen Sie [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) aktualisieren, um das Feature nutzen zu können. Weitere Informationen zum Tarif finden Sie unter „Azure Security Center-Preise“. 
    > 
3. Wenn Sie mehr als einen Arbeitsbereich untersuchen möchten, können Sie die Untersuchung gemäß der Spalte **SCHÄDLICHE IP** priorisieren. In dieser Spalte wird die aktuelle Anzahl von schädlichen IP-Adressen des Arbeitsbereichs angezeigt. Wählen Sie den gewünschten Arbeitsbereich aus. Das Dashboard **Informationen zu Bedrohungen** wird angezeigt.

    ![Threat Intelligence](./media/security-center-threat-intel/security-center-threat-intel-fig5.png)

4. Dieses Dashboard ist in vier Kacheln unterteilt:
    * **Bedrohungstypen**: Hier sind die Typen von Bedrohungen zusammengefasst, die im ausgewählten Arbeitsbereich erkannt wurden.
    * **Ursprungsland**: Aggregiert die Datenverkehrsmenge gemäß dem Quellort.
    * **Bedrohungsposition**: Dient als Hilfe beim Identifizieren der aktuellen Standorte weltweit, die mit Ihrer Umgebung kommunizieren. Die Karte gibt die Richtung des Datenverkehrs mit orangefarbenen (eingehend) und roten (ausgehend) Pfeilen an. Wenn Sie auf einen dieser Pfeile klicken, werden die Art der Bedrohung und die Datenrichtung angezeigt.
    * **Bedrohungsdetails**: Hier werden weitere Details zu der Bedrohung angezeigt, die Sie in der Karte ausgewählt haben.

Unabhängig von der ausgewählten Optionskachel basiert das angezeigte Dashboard auf der [Protokollsuche](https://docs.microsoft.com/azure/security-center/security-center-search)-Abfrage. Die einzigen Unterschiede sind der Typ der Abfrage und das Ergebnis.

### <a name="threat-types"></a>Bedrohungstypen
Nach dem Klicken auf die Kachel **Bedrohungstypen** wird das Dashboard **Protokollsuche** angezeigt. Darin befinden sich die Filteroptionen auf der linken Seite und die Abfrageergebnisse auf der rechten Seite.

![Threat Intelligence-Suche](./media/security-center-threat-intel/security-center-threat-intel-fig3.png)

Im Abfrageergebnis werden Aggregate der Bedrohungen nach Name angezeigt. Im linken Bereich können Sie das Attribut auswählen, nach dem Sie filtern möchten. Beispiel: Wenn Sie nur die Bedrohungen anzeigen möchten, für die derzeit Verbindungen mit den Computern bestehen, können Sie unter **SESSIONSTATE** die Option **Verbunden** auswählen und auf die Schaltfläche **Übernehmen** klicken.

![Sitzungsstatus](./media/security-center-threat-intel/security-center-threat-intel-fig4.png)

Für Azure-VMs werden im Dashboard „Informationen zu Bedrohungen“ nur die Netzwerkdaten angezeigt, die den Agent durchlaufen. Außerdem werden für die Informationen zu Bedrohungen die folgenden Datentypen verwendet:

- CEF-Data (Type=CommonSecurityLog)
- WireData (Type=WireData)
- IIS-Protokolle (Type=W3CIISLog)
- Windows-Firewall (Type=WindowsFirewall)
- DNS-Ereignisse (Type=DnsEvents)


## <a name="see-also"></a>Weitere Informationen
In diesem Dokument wurde beschrieben, wie Sie Informationen zu Bedrohungen in Security Center als Hilfe beim Identifizieren von verdächtigen Aktivitäten nutzen. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Quellen:

* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Hier erfahren Sie, wie Sie Warnungen verwalten und auf Sicherheitsvorfälle in Security Center reagieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md). Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verstehen der Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Hier finden Sie Informationen zu den unterschiedlichen Arten von Sicherheitswarnungen.
* [Azure Security Center – Handbuch zur Problembehandlung](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Hier erfahren Sie, wie Sie allgemeine Probleme in Security Center behandeln. 
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md)festgelegt ist. Enthält häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security-Blog](http://blogs.msdn.com/b/azuresecurity/). Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.


