---
title: Integration von Service Map in System Center Operations Manager | Microsoft Docs
description: "Service Map ist eine Lösung in der Operations Management Suite, die automatisch Anwendungskomponenten auf Windows- und Linux-Systemen ermittelt und die Kommunikation zwischen Diensten abbildet. Dieser Artikel erläutert die Verwendung von Service Map zum automatischen Erstellen von Diagrammen der verteilten Anwendungen in Operations Manager."
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: e8614a5a-9cf8-4c81-8931-896d358ad2cb
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: bwren;dairwin
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 0b710c338be3a2c2fde6bba43173f7c5f480e357
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---

# <a name="service-map-integration-with-system-center-operations-manager"></a>Integration von Service Map in System Center Operations Manager
  > [!NOTE]
  > Weil dieses Feature in der privaten Vorschau enthalten ist, sollte es nicht auf Produktionssystemen verwendet werden.
  > 
  
Service Map in der Operations Management Suite ermittelt automatisch Anwendungskomponenten auf Windows- und Linux-Systemen und bildet die Kommunikation zwischen Diensten ab. In Service Map können Sie die Server Ihrer Vorstellung gemäß anzeigen – als verbundene Systeme, die wichtige Dienste bereitstellen. Service Map zeigt Verbindungen zwischen Servern, Prozessen und Ports über die gesamte TCP-Verbindungsarchitektur an. Außer der Installation eines Agents ist keine weitere Konfiguration erforderlich. Weitere Informationen finden Sie in der [Dokumentation zu Service Map](operations-management-suite-service-map.md).

Mit dieser Integration zwischen Service Map und System Center Operations Manager können Sie basierend auf dynamischen Abhängigkeitszuordnungen in Service Map automatisch Diagramme der verteilten Anwendungen in Operations Manager erstellen.

## <a name="prerequisites"></a>Voraussetzungen
* Eine Operations Manager-Verwaltungsgruppe, die eine Gruppe von Servern verwaltet.
* Ein Operations Management Suite-Arbeitsbereich mit aktivierter Service Map-Lösung
* Eine Gruppe von Servern (mindestens eine), die über Operations Manager verwaltet werden und Daten an Service Map senden. Es werden Windows- und Linux-Server unterstützt.
* Ein Dienstprinzipal mit Zugriff auf das Azure-Abonnement, das dem Operations Management Suite-Arbeitsbereich zugeordnet ist. Weitere Informationen finden Sie unter [Erstellen eines Dienstprinzipals](#creating-a-service-principal).

## <a name="install-the-service-map-management-pack"></a>Installieren des Service Map Management Pack
Sie aktivieren die Integration zwischen Operations Manager und Service Map durch Importieren des Management Pack-Pakets Microsoft.SystemCenter.ServiceMap (Microsoft.SystemCenter.ServiceMap.mpb). Das Paket enthält die folgenden Management Packs:
* Microsoft Service Map Application Views
* Microsoft System Center Service Map Internal
* Microsoft System Center Service Map Overrides
* Microsoft System Center Service Map

## <a name="configure-the-service-map-integration"></a>Konfigurieren der Integration von Service Map
Nach der Installation des Service Map Management Packs wird im Bereich **Verwaltung** unter **Operations Management Suite** der neue Knoten **Service Map** angezeigt. 

Um die Service Map-Integration zu konfigurieren, führen Sie folgende Schritte aus:

1. Um den Konfigurations-Assistenten zu öffnen, klicken Sie im Bereich **Service Map Overview** (Service Map-Übersicht) auf **Arbeitsbereich hinzufügen**.  

    ![Bereich „Service Map Overview“ (Service Map-Übersicht)](media/oms-service-map/scom-configuration.png)

2. Geben Sie im Fenster **Verbindungskonfiguration** den Mandantennamen oder die Mandanten-ID, die Anwendungs-ID (auch als Benutzername bzw. clientID bezeichnet) und das Kennwort des Dienstprinzipals ein, und klicken Sie auf **Weiter**. Weitere Informationen finden Sie unter [Erstellen eines Dienstprinzipals](#creating-a-service-principal).

    ![Das Fenster „Verbindungskonfiguration“](media/oms-service-map/scom-config-spn.png)

3. Wählen Sie im Fenster **Subscription Selection** (Abonnementauswahl) das Azure-Abonnement, die Azure-Ressourcengruppe (diejenige, die den Operations Management Suite-Arbeitsbereich enthält) und den Operations Management Suite-Arbeitsbereich, und klicken Sie dann auf **Weiter**.

    ![Der Operations Manager-Konfigurationsarbeitsbereich](media/oms-service-map/scom-config-workspace.png)

4. Konfigurieren Sie im Fenster **Serverauswahl** die „Service Map-Servergruppe“ mit den Servern, die Sie zwischen Operations Manager und Service Map synchronisieren möchten. Klicken Sie auf **Server hinzufügen/entfernen**.   
    
    Damit die Integration ein Diagramm der verteilten Anwendung für einen Server erstellen kann, muss der Server folgende Voraussetzungen erfüllen:

    * Verwaltet von Operations Manager.
    * Verwaltet von Service Map.
    * In der „Service Map-Servergruppe“ aufgelistet.

    ![Die Operations Manager-Konfigurationsgruppe](media/oms-service-map/scom-config-group.png)

5. Optional: Wählen Sie den Ressourcenpool „Verwaltungsserver“ für die Kommunikation mit Operations Management Suite aus, und klicken Sie auf **Arbeitsbereich hinzufügen**.

    ![Der Operations Manager-Konfigurationsressourcenpool](media/oms-service-map/scom-config-pool.png)

    Es kann eine Minute dauern, den Operations Management Suite-Arbeitsbereich zu konfigurieren und registrieren. Sobald er konfiguriert ist, wird in Operations Manager die erste Service Map-Synchronisierung von der Operations Management Suite aus initiiert.

    ![Der Operations Manager-Konfigurationsressourcenpool](media/oms-service-map/scom-config-success.png)

    >[!NOTE]
    >Das Standardsynchronisierungsintervall ist auf 60 Minuten festgelegt. Sie können Außerkraftsetzungen konfigurieren, um das Synchronisierungsintervall zu ändern. Sie können Server auch der „Service Map-Servergruppe“ über den Bereich **Erstellung** manuell hinzufügen. Wählen Sie zu diesem Zweck **Gruppen**, und suchen Sie nach **Service Map-Servergruppe**. Die Serverzuordnungen für diese Server werden dann bei der nächsten Synchronisierung synchronisiert, die auf dem konfigurierten Synchronisierungsintervall basiert.

## <a name="monitor-service-map"></a>Überwachen von Service Map
Nachdem die Verbindung des Operations Management Suite-Arbeitsbereichs hergestellt ist, wird ein neuer Ordner „Service Map“ im Bereich **Überwachung** der Operations Manager-Konsole angezeigt.

![Operations Manager-Bereich „Überwachung“](media/oms-service-map/scom-monitoring.png)

Der Ordner „Service Map“ umfasst drei Knoten:
* **Aktive Warnungen**: Listet alle aktiven Warnungen zur Kommunikation zwischen Operations Manager und Service Map-Lösung in der Operations Management Suite auf.

    >[!NOTE]
    >Diese Warnungen sind nicht die Operations Management Suite-Warnungen, die im Operations Manager eingeblendet werden.

* **Server**: Hier wird die Liste der überwachten Server angezeigt, die für die Synchronisierung mit Service Map konfiguriert sind.

    ![Der Serverüberwachungsbereich von Operations Manager](media/oms-service-map/scom-monitoring-servers.png)

* **Server Dependency Views** (Serverabhängigkeitsansichten): Listet alle Server auf, die von Service Map aus synchronisiert werden. Sie können auf einen beliebigen Server klicken, um das entsprechende Diagramm der verteilten Anwendungen anzuzeigen.

    ![Das Diagramm der verteilten Anwendungen im Operations Manager](media/oms-service-map/scom-dad.png)

## <a name="edit-or-delete-the-workspace"></a>Bearbeiten oder Löschen des Arbeitsbereichs
Sie können den konfigurierten Arbeitsbereich im Bereich der **Service Map-Übersicht** (Bereich **Verwaltung** > **Operations Management Suite** > **Service Map**) bearbeiten oder löschen. Sie können zurzeit nur einen Operations Management Suite-Arbeitsbereich konfigurieren.

![Der Bereich zum Bearbeiten des Arbeitsbereichs im Operations Manager ](media/oms-service-map/scom-edit-workspace.png)

## <a name="configure-rules-and-overrides"></a>Konfigurieren von Regeln und Außerkraftsetzungen
Die Regel _Microsoft.SystemCenter.ServiceMapImport.Rule_ wird erstellt, um regelmäßig Informationen von Service Map abzurufen. Um die Zeitsteuerungen der Synchronisierung zu ändern, können Sie Außerkraftsetzungen der Regel (Bereich **Erstellung** > **Regeln** > **Microsoft.SystemCenter.ServiceMapImport.Rule**) konfigurieren.

![Das Operations Manager-Fenster für Eigenschaften von Außerkraftsetzungen](media/oms-service-map/scom-overrides.png)

* **Enabled**: Aktivieren oder Deaktivieren der automatischen Updates. 
* **IntervalMinutes**: Zurücksetzen der Zeitspanne zwischen Updates. Das Standardintervall beträgt eine Stunde. Wenn Sie Serverzuordnungen in kürzeren Abständen synchronisieren möchten, können Sie den Wert ändern.
* **TimeoutSeconds**: Zurücksetzen der Zeitspanne bis zum Timeout der Anforderung. 
* **TimeWindowMinutes**: Zurücksetzen des Zeitfensters zum Abfragen von Daten. Standardmäßig ist ein Fenster von 60 Minuten festgelegt. Der von Service Map zugelassene Maximalwert beträgt 60 Minuten.

## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen

Das aktuelle Design weist folgende Probleme und Einschränkungen auf:
* Sie können im Bereich **Erstellen** der „Service Map-Servergruppe“ zwar manuell Server hinzufügen, aber die Synchronisierung der Zuordnungen für diese Server mit Service Map erfolgt erst während des nächsten Synchronisierungszyklus. Die Standardeinstellung ist 60 Minuten, aber Sie können die zeitliche Steuerung außer Kraft setzen. 
* Sie können eine Verbindung mit einem einzelnen Operations Management Suite-Arbeitsbereich herstellen.

## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals
Die offizielle Azure-Dokumentation zum Erstellen eines Dienstprinzipals finden Sie in den folgenden Dokumenten:
* [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)
* [Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal)

### <a name="feedback"></a>Feedback
Haben Sie Feedback für uns zu Service Map oder dieser Dokumentation? Besuchen Sie unsere [UserVoice-Webseite](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), auf der Sie Features vorschlagen oder vorhandene Vorschläge unterstützen können.

