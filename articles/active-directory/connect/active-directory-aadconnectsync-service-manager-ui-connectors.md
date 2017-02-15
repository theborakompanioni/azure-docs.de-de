---
title: "Azure AD Connect-Synchronisierung: Synchronization Service Manager-Benutzeroberfläche | Microsoft Docs"
description: "Grundlegende Informationen zur Registerkarte „Connectors“ in Synchronization Service Manager für Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 60f1d979-8e6d-4460-aaab-747fffedfc1e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: aa20b20c86763791eb579883b5273ea79cc714b5
ms.openlocfilehash: 4d248fedea7c3b05b5bf4a28cd99923302c43a9b


---
# <a name="azure-ad-connect-sync-synchronization-service-manager"></a>Azure AD Connect-Synchronisierung: Synchronization Service Manager
| [Vorgänge](active-directory-aadconnectsync-service-manager-ui-operations.md) | [Connectors](active-directory-aadconnectsync-service-manager-ui-connectors.md) | [Metaverse Designer](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md) | [Metaverse Search](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) |
| --- | --- | --- | --- |
|  | | | |

![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

Die Registerkarte „Connectors“ wird verwendet, um alle Systeme zu verwalten, mit denen das Synchronisierungsmodul verbunden ist.

## <a name="connector-actions"></a>Connectoraktionen
| Aktion | Kommentar |
| --- | --- |
| Erstellen |Nicht verwenden. Verwenden Sie den Installations-Assistenten zum Verbinden mit weiteren Active Directory-Gesamtstrukturen. |
| Eigenschaften |Wird zum Filtern von Domänen und Organisationseinheiten verwendet. |
| [Löschen](#delete) |Wird zum Löschen der Daten im Connectorbereich oder zum Löschen der Verbindung mit einer Gesamtstruktur verwendet. |
| [Ausführungsprofile konfigurieren](#configure-run-profiles) |Hier muss nur das Filtern von Domänen konfiguriert werden. Sie können diese Aktion verwenden, um bereits konfigurierte Ausführungsprofile anzuzeigen. |
| Ausführen |Wird zum Starten einer einmaligen Ausführung eines Profils verwendet. |
| Beenden |Beendet einen Connector, der gerade ein Profil ausführt. |
| Connector exportieren |Nicht verwenden. |
| Connector importieren |Nicht verwenden. |
| Connector aktualisieren |Nicht verwenden. |
| Schema aktualisieren |Aktualisiert das zwischengespeicherte Schema. Es wird empfohlen, die Option im Installations-Assistenten zu verwenden, da damit auch die Synchronisierungsregeln aktualisiert werden. |
| [Connectorbereich durchsuchen](#search-connector-space) |Wird verwendet, um Objekte zu finden und um [ein Objekt und dessen Daten durch das System zu verfolgen](#follow-an-object-and-its-data-through-the-system). |

### <a name="delete"></a>Löschen
Die Löschaktion wird für zwei verschiedene Dinge verwendet.
![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

Mit der Option **Delete connector space only** (Nur Connectorbereich löschen) werden alle Daten entfernt, die Konfiguration bleibt aber erhalten.

Mit der Option **Delete Connector and connector space** (Connector und Connectorbereich löschen) werden sowohl die Daten als auch die Konfiguration entfernt. Diese Option wird verwendet, wenn Sie mit einer Gesamtstruktur keine Verbindung mehr herstellen möchten.

Mit beiden Optionen werden alle Objekte synchronisiert und die Metaverse-Objekte aktualisiert. Bei dieser Aktion handelt es sich um einen Vorgang mit langer Ausführungsdauer.

### <a name="configure-run-profiles"></a>Ausführungsprofile konfigurieren
Mit dieser Option können Sie die für einen Connector konfigurierten Ausführungsprofile anzeigen.

![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a>Connectorbereich durchsuchen
Die Aktion zum Durchsuchen des Connectorbereichs ist nützlich, um nach Objekten zu suchen und Probleme zu behandeln.

![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

Wählen Sie zuerst einen **Bereich**aus. Sie können basierend auf Daten (RDN, DN, Anker, Teilstruktur) oder des Zustands des Objekts (alle anderen Optionen) suchen.  
![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)  
 Wenn Sie beispielsweise eine Unterstruktur durchsuchen, erhalten Sie alle Objekte in einer Organisationseinheit.
![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png): In diesem Raster können Sie ein Objekt auswählen, **properties**auswählen und das Objekt aus dem Quellconnectorbereich durch das Metaverse bis zum Zielconnectorbereich [verfolgen](#follow-an-object-and-its-data-through-the-system).

## <a name="follow-an-object-and-its-data-through-the-system"></a>ein Objekt und dessen Daten durch das System zu verfolgen
Wenn Sie ein Problem mit Daten behandeln möchten, ist das Verfolgen eines Objekts vom Quellconnectorbereich zum Metaverse und zum Zielconnectorbereich ein wichtiges Verfahren, um zu verstehen, warum die Daten nicht die erwarteten Werte aufweisen.

### <a name="connector-space-object-properties"></a>Eigenschaften der Objekte des Connectorbereichs
**Importieren**  
 Beim Öffnen eines Connectorbereichsobjekts befinden sich oben mehrere Registerkarten. Auf der Registerkarte **Importieren** werden die Daten angezeigt, die nach einem Import bereitgestellt werden.
![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/csimport.png): Unter **Old Value** (Alter Wert) wird dargestellt, was derzeit im System gespeichert ist, und unter **New Value ** (Neuer Wert) wird angezeigt, welcher Wert aus dem Quellsystem empfangen, aber noch nicht angewendet wurde. Da in diesem Fall ein Synchronisierungsfehler vorliegt, kann die Änderung nicht angewendet werden.

**Fehler**  
 Die Fehlerseite wird nur angezeigt, wenn ein Problem mit dem Objekt besteht. Unter den Details auf der Seite mit Vorgängen finden Sie weitere Informationen zur [Problembehandlung bei Synchronisierungsfehlern](active-directory-aadconnectsync-service-manager-ui-operations.md#troubleshoot-errors-in-operations-tab).

**Herkunft**  
 Auf der Registerkarte für die Herkunft wird gezeigt, wie das Objekt des Connectorbereichs mit dem Metaverseobjekt verknüpft ist. Sie sehen, wann der Connector zuletzt eine Änderung aus dem verbundenen System importiert hat und welche Regeln zum Auffüllen der Daten im Metaverse angewendet wurden.
![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cslineage.png): In der Spalte **Action ** (Aktion) sehen Sie eine einzelne Synchronisierungsregel vom Typ **Inbound** (Eingehend) mit der Aktion **Provision** (Bereitstellen). Damit wird angegeben, dass das Metaverse-Objekt erhalten bleibt, solange dieses Objekt des Connectorbereichs vorhanden ist. Wenn die Liste mit den Synchronisierungsregeln hingegen eine Synchronisierungsregel mit der Richtung **Outbound** (Ausgehend) und der Aktion **Provision** (Bereitstellen) enthält, wird beim Löschen des Metaverseobjekts auch dieses Objekt gelöscht.
![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cslineageout.png): Darüber hinaus sehen Sie in der Spalte für die **PasswordSync** (Kennwortsynchronisierung), dass der eingehende Connectorbereich Kennwortänderungen beitragen kann, da eine Synchronisierungsregel den Wert **True** (Wahr) aufweist. Dieses Kennwort wird anschließend über die ausgehende Regel an Azure AD gesendet.

Auf der Registerkarte für die Herkunft gelangen Sie zum Metaverse, indem Sie auf [Metaverse Object Properties](#metaverse-object-properties)(Metaverse-Objekteigenschaften) klicken.

Unterhalb der Registerkarten befinden sich zwei Schaltflächen: **Preview** (Vorschau) und **Log** (Protokoll).

**Preview**  
 Die Vorschauseite wird verwendet, um ein einzelnes Objekt zu synchronisieren. Sie ist nützlich, wenn Sie Probleme mit den Synchronisierungsregeln einiger Kunden behandeln und die Auswirkungen einer Änderung auf ein einzelnes Objekt sehen möchten. Sie können zwischen **Full Sync** (Vollständige Synchronisierung) und **Delta Sync** (Deltasynchronisierung) auswählen. Außerdem haben Sie die Wahl zwischen **Generate Preview** (Vorschau generieren), um die Änderung nur im Arbeitsspeicher beizubehalten, und **Commit Preview** (Commitvorschau), um alle Änderungen im Zielconnectorbereich bereitzustellen.
![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/preview1.png) Sie können das Objekt und die für einen bestimmten Attributfluss angewendete Regel prüfen.
![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/preview2.png)

**Protokoll**  
Die Protokollseite wird verwendet, um den Kennwortsynchronisierungsstatus und den Verlauf anzuzeigen.

### <a name="metaverse-object-properties"></a>Metaverse Object Properties
**Attribute**  
: Auf der Registerkarte „Attribute“ sehen Sie die Werte und den Connector, von dem sie stammen.
![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/mvattributes.png)
: **Connectors**  
: Auf der Registerkarte „Connectors“ werden alle Connectorbereiche angezeigt, die über eine Darstellung des Objekts verfügen.
![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/mvconnectors.png) Auf dieser Registerkarte können Sie auch zum [Connectorbereichsobjekt](#connector-space-object-properties).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Konfiguration der [Azure AD Connect-Synchronisierung](active-directory-aadconnectsync-whatis.md) .

Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).



<!--HONumber=Nov16_HO3-->


