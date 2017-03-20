---
title: "Connectors der Benutzeroberfläche von Azure AD Synchronization Service Manager | Microsoft Docs"
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
ms.date: 03/02/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: bd2240678fed44db748ae062bdf91e457159b4a2
ms.lasthandoff: 03/04/2017

---
# <a name="using-connectors-with-the-azure-ad-connect-sync-service-manager"></a>Verwenden von Connectors mit dem Azure AD Connect Synchronization Service Manager

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
![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png)  
In dieser Tabelle können Sie ein Objekt auswählen, **Eigenschaften** auswählen und das Objekt aus dem Quellconnectorbereich durch das Metaverse bis zum Zielconnectorbereich [verfolgen](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Konfiguration der [Azure AD Connect-Synchronisierung](active-directory-aadconnectsync-whatis.md) .

Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

