---
title: Integration von Service Map in System Center Operations Manager | Microsoft Docs
description: "Service Map ist eine Lösung in der Operations Management Suite (OMS), die automatisch Anwendungskomponenten auf Windows- und Linux-Systemen ermittelt und die Kommunikation zwischen Diensten abbildet.  Dieser Artikel enthält Informationen zur Verwendung von Service Map zum automatischen Erstellen von Diagrammen der verteilten Anwendungen in SCOM."
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
translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: 389c01234acff068dc90f3cdfdc4916a9d76d244
ms.lasthandoff: 03/09/2017


---

# <a name="service-map-integration-with-system-center-operations-manager-integration"></a>Integration von Service Map in System Center Operations Manager
  > [!NOTE]
  > Dieses Feature ist in der privaten Vorschau enthalten und sollte deshalb nicht auf Produktionssystemen verwendet werden.
  > 
  
Service Map in der Operations Management Suite (OMS) ermittelt automatisch Anwendungskomponenten auf Windows- und Linux-Systemen und bildet die Kommunikation zwischen Diensten ab. In dieser Lösung können Sie die Server ihrer Funktion gemäß anzeigen – als verbundene Systeme, die wichtige Dienste bereitstellen. Service Map zeigt Verbindungen zwischen Servern, Prozessen und Ports über die gesamte TCP-Verbindungsarchitektur an. Außer der Installation eines Agents ist keine weitere Konfiguration erforderlich.  Weitere Informationen finden Sie in der [Dokumentation zu Service Map](operations-management-suite-service-map.md).

Mit dieser Integration zwischen Service Map und System Center Operations Manager (SCOM) können Sie basierend auf dynamischen Abhängigkeitszuordnungen in Service Map automatisch Diagramme der verteilten Anwendungen in SCOM erstellen.

## <a name="prerequisites"></a>Voraussetzungen
1.    Eine SCOM-Verwaltungsgruppe zur Verwaltung einer Gruppe von Servern
2.    Ein OMS-Arbeitsbereich mit aktivierter Service Map-Lösung
3.    Eine Gruppe von Servern (mindestens eine), die über SCOM verwaltet werden und Daten an Service Map senden.  Es werden Windows- und Linux-Server unterstützt.
4.    Ein Dienstprinzipal mit Zugriff auf das Azure-Abonnement, das dem OMS-Arbeitsbereich zugeordnet ist.  [Weitere Informationen zum Erstellen eines Dienstprinzipals](#creating-a-service-principal).

## <a name="installing-service-map-management-pack"></a>Installieren des Service Map Management Pack
Die Integration zwischen SCOM und Service Map wird durch Importieren des Management Pack-Pakets Microsoft.SystemCenter.ServiceMap (Microsoft.SystemCenter.ServiceMap.mpb) aktiviert.  Das Paket enthält die folgenden Management Packs:
* Microsoft ServiceMap Application Views
* Microsoft System Center ServiceMap Internal
* Microsoft System Center ServiceMap Overrides
* Microsoft System Center ServiceMap

## <a name="configuring-the-service-map-integration"></a>Konfigurieren der Integration von Service Map
1. Nach der Installation des ServiceMap Management Packs wird im Bereich „Verwaltung“ unter „Operations Management Suite“ der neue Knoten „Service Map“ angezeigt.
2. Klicken Sie im Bereich mit der Service Map-Übersicht auf „Arbeitsbereich hinzufügen“, um den Konfigurations-Assistenten zu öffnen.

    ![SCOM-Konfigurations-Assistent](media/oms-service-map/scom-configuration.png)

3. Der erste Schritt im Assistenten ist die Verbindungskonfiguration, bei der Sie die Informationen für Ihren Azure-Dienstprinzipal eingeben. Geben Sie den Mandantennamen, die Mandanten-ID, die Anwendungs-ID (oder den Benutzernamen bzw. die ClientID) und das Kennwort des Dienstprinzipals ein.  [Weitere Informationen zum Erstellen eines Dienstprinzipals](#creating-a-service-principal).

    ![SCOM-Konfiguration – Dienstprinzipalname](media/oms-service-map/scom-config-spn.png)

4. Im nächsten Schritt werden das Azure-Abonnement, die Azure-Ressourcengruppe (die Gruppe mit dem OMS-Arbeitsbereich) und der OMS-Arbeitsbereich ausgewählt.

    ![SCOM-Konfiguration – Arbeitsbereich](media/oms-service-map/scom-config-workspace.png)

5. Im nächsten Schritt wird die Service Map-Servergruppe mit den Servern konfiguriert, die zwischen SCOM und Service Map synchronisiert werden sollen.  Klicken Sie auf die Schaltfläche „Server hinzufügen/entfernen“ . Beachten Sie, dass für die Integration zum Erstellen eines Diagramms der verteilten Anwendungen für einen Server Folgendes für diesen Server gilt: 1) Der Server muss über SCOM verwaltet werden. 2) Er muss über Service Map verwaltet werden, und er muss 3) in der Service Map-Servergruppe aufgeführt sein.

    ![SCOM-Konfiguration – Gruppe](media/oms-service-map/scom-config-group.png)

6. Optional: Wählen Sie den Ressourcenpool „Verwaltungsserver“ für die Kommunikation mit OMS aus, und klicken Sie auf „Arbeitsbereich hinzufügen“.

    ![SCOM-Konfiguration – Ressourcenpool](media/oms-service-map/scom-config-pool.png)

7. Es dauert einige Minuten, bis der OMS-Arbeitsbereich konfiguriert und registriert ist. Nachdem der Arbeitsbereich konfiguriert wurde, wird in SCOM die erste Service Map-Synchronisierung mit OMS initiiert.

    ![SCOM-Konfiguration – Ressourcenpool](media/oms-service-map/scom-config-success.png)

**Hinweis:** Das Standardsynchronisierungsintervall ist auf 60 Minuten festgelegt. Benutzer können Außerkraftsetzungen konfigurieren, um das Synchronisierungsintervall zu ändern. Benutzer können der Service Map-Servergruppe zudem im Bereich „Konfiguration“ (Bereich „Erstellen“ > „Gruppen“, dann nach „Service Map-Servergruppe“ suchen) manuell Server hinzufügen. Die Serverzuordnungen für diese Server werden dann bei der nächsten Synchronisierung synchronisiert (basierend auf dem konfigurierten Synchronisierungsintervall).

## <a name="monitoring-service-map"></a>Überwachen von Service Map
Wenn der OMS-Arbeitsbereich verbunden ist, wird im Bereich „Überwachung“ der SCOM-Konsole der neue Ordner „Service Map“ angezeigt.
![SCOM-Überwachung](media/oms-service-map/scom-monitoring.png)

Der Ordner „Service Map“ umfasst drei Knoten:
### <a name="all-alerts"></a>Alle Warnungen:
Hier werden alle Warnungen zur Kommunikation zwischen SCOM und der Service Map-Lösung in OMS angezeigt.

**Hinweis:** Dabei handelt es sich nicht um OMS-Warnungen, die in SCOM angezeigt werden.
### <a name="servers"></a>Server:
Hier wird die Liste der überwachten Server angezeigt, die für die Synchronisierung mit Service Map konfiguriert sind.

![SCOM-Überwachung – Server](media/oms-service-map/scom-monitoring-servers.png)

### <a name="server-dependency-views"></a>Ansichten der Serverabhängigkeiten:
In dieser Ansicht wird die Liste aller mit Service Map synchronisierten Server angezeigt. Benutzer können auf einen Server klicken, um das entsprechende Diagramm der verteilten Anwendungen anzuzeigen.

![SCOM – Diagramm der verteilten Anwendungen](media/oms-service-map/scom-dad.png)

## <a name="editdelete-workspace"></a>Bearbeiten und Löschen von Arbeitsbereichen
Benutzer können den konfigurierten Arbeitsbereich im Bereich der Service Map-Übersicht (Bereich „Verwaltung“ > „Operations Management Suite“ > „Service Map“) bearbeiten oder löschen.  Beachten Sie, dass vorerst nur ein OMS-Arbeitsbereich konfiguriert werden kann.

![SCOM-Bearbeitung – Arbeitsbereich](media/oms-service-map/scom-edit-workspace.png)

## <a name="configuring-rules-and-overrides"></a>Konfigurieren von Regeln und Außerkraftsetzungen
Die Regel **Microsoft.SystemCenter.ServiceMap.Import.Rule** wird erstellt, damit regelmäßig Informationen von Service Map abgerufen werden.  Benutzer können Außerkraftsetzungen für diese Regel konfigurieren, um die Zeitvorgaben für die Synchronisierung zu ändern.
Bereich „Erstellen“ > „Regeln“ > „Microsoft.SystemCenter.ServiceMapImport.Rule“

![SCOM-Außerkraftsetzungen](media/oms-service-map/scom-overrides.png)
* **Enabled:** Aktivieren/Deaktivieren der automatischen Updates 
* **IntervalSeconds:** Zeitraum zwischen den Updates.  Der Standardwert ist 1 Stunde. Benutzer können den Wert hier ändern, wenn Serverzuordnungen in kürzeren Abständen synchronisiert werden sollen.
* **TimeoutSeconds:** Zeitspanne vor dem Timeout der Anforderung 
* **TimeWindowMinutes:** Zeitfenster für die Abfrage von Daten.  Standardmäßig ist ein Fenster von 60 Minuten festgelegt. Der Maximalwert ist 1 Stunde (maximal zulässiger Wert in Service Map).

## <a name="known-issueslimitations"></a>Bekannte Probleme und Einschränkungen
Im aktuellen Entwurf:
1. Benutzer können im Bereich „Erstellen“ in „Service Map-Servergruppe“ zwar manuell Server hinzufügen, die Synchronisierung der Zuordnungen für diese Server mit Service Map erfolgt aber erst während des nächsten Synchronisierungszyklus (Standardmäßig&60; Minuten. Benutzer können die Zeitvorgabe für die Synchronisierung ändern.). 
2. Benutzer können nur eine Verbindung mit einem einzigen OMS-Arbeitsbereich herstellen.

## <a name="creating-a-service-principal"></a>Erstellen eines Dienstprinzipals
Über die folgenden Links gelangen Sie zur offiziellen Azure-Dokumentation für die drei verschiedenen Möglichkeiten zum Erstellen eines Dienstprinzipals.
* [Erstellen eines Dienstprinzipals mit PowerShell](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Erstellen eines Dienstprinzipals über die Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)
* [Erstellen eines Dienstprinzipals über das Azure-Portal](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal)

### <a name="feedback"></a>Feedback
Haben Sie Feedback für uns zu Service Map oder dieser Dokumentation?  Besuchen Sie unsere [User Voice-Webseite](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), auf der Sie Features vorschlagen oder vorhandene Vorschläge unterstützen können.

