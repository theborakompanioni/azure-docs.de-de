---
title: "Konfigurieren von MSI auf einem virtuellen Azure-Computer über das Azure-Portal"
description: "Schrittweise Anweisungen zum Konfigurieren einer verwalteten Dienstidentität (Managed Service Identity, MSI) auf einem virtuellen Azure-Computer über das Azure-Portal."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: bryanla
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: 9406ba2bbbea41f4677cd0d5aaddf16b0f4f26c8
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="configure-a-vm-managed-service-identity-msi-using-the-azure-portal"></a>Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) über das Azure-Portal

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie MSI für einen virtuellen Azure-Windows-Computer über das Azure-Portal aktivieren und entfernen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/msi-qs-configure-prereqs.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Aktivieren von MSI beim Erstellen eines virtuellen Azure-Computers

Zum Zeitpunkt der Erstellung dieses Dokuments wird das Aktivieren von MSI während der Erstellung eines virtuellen Computers im Azure-Portal nicht unterstützt. Informationen zum Erstellen eines virtuellen Computers finden Sie stattdessen im Schnellstart [Erstellen einer Windows-VM im Azure-Portal](../virtual-machines/windows/quick-create-portal.md#create-virtual-machine). Fahren Sie dann mit dem nächsten Abschnitt fort, um weitere Details zum Aktivieren von MSI zu erhalten.

## <a name="enable-msi-on-an-existing-azure-vm"></a>Aktivieren von MSI auf einem vorhandenen virtuellen Azure-Computer

Gehen Sie für einen virtuellen Computer, der ursprünglich ohne MSI bereitgestellt wurde, wie folgt vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, unter dem Sie den virtuellen Computer bereitstellen möchten.

2. Navigieren Sie zum gewünschten virtuellen Computer.

2. Klicken Sie auf die Seite „Konfiguration“, aktivieren Sie MSI für den virtuellen Computer, indem Sie unter „Verwaltete Dienstidenität“ die Option „Ja“ auswählen, und klicken Sie dann auf **Speichern**. Dieser Vorgang kann 60 Sekunden oder länger dauern:

   ![Screenshot der Konfigurationsseite](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

## <a name="remove-msi-from-an-azure-vm"></a>Entfernen von MSI von einem virtuellen Azure-Computer

Wenn MSI auf einem virtuellen Computer nicht mehr benötigt wird, gehen Sie wie folgt vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, unter dem Sie den virtuellen Computer bereitstellen möchten.

2. Navigieren Sie zum gewünschten virtuellen Computer.

3. Klicken Sie auf die Seite „Konfiguration“, entfernen Sie MSI von dem virtuellen Computer, indem Sie unter „Verwaltete Dienstidenität“ die Option „Nein“ auswählen, und klicken Sie dann auf **Speichern**. Dieser Vorgang kann 60 Sekunden oder länger dauern:

   ![Screenshot der Konfigurationsseite](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)  

## <a name="related-content"></a>Verwandte Inhalte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](msi-overview.md).
- Dieser Artikel basiert auf dem Schnellstart [Erstellen einer Windows-VM im Azure-Portal](../virtual-machines/windows/quick-create-portal.md) und wurde durch MSI-spezifische Anweisungen ergänzt. 

## <a name="next-steps"></a>Nächste Schritte

- Gewähren Sie der MSI eines virtuellen Azure-Computers über das Azure-Portal den [Zugriff auf eine andere Azure-Ressource](msi-howto-assign-access-portal.md).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.

