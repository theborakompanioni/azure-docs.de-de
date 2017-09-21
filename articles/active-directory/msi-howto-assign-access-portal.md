---
title: "Zuweisen von MSI-Zugriff zu einer Azure-Ressource über das Azure-Portal"
description: "Schrittweise Anweisungen für das Zuweisen von MSI zu einer Ressource und das Zuweisen des Zugriffs für eine andere Ressource über das Azure-Portal."
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
ms.openlocfilehash: 9dd02c8d7580cd9233e192f807686b7857ccf696
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-the-azure-portal"></a>Zuweisen des MSI-Zugriffs (Managed Service Identity, verwaltete Dienstidentität) auf eine Ressource über das Azure-Portal

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Nachdem Sie eine Azure-Ressource mit einer MSI konfiguriert haben, können Sie der MSI genau wie bei allen anderen Sicherheitsprinzipalen den Zugriff auf eine andere Ressource erteilen. In diesem Beispiel wird gezeigt, wie Sie der MSI eines virtuellen Azure-Computers über das Azure-Portal den Zugriff auf ein Azure-Speicherkonto gewähren.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/msi-qs-configure-prereqs.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Verwenden Sie RBAC, um der MSI den Zugriff auf eine andere Ressource zuzuweisen.

Nachdem Sie MSI für eine Azure-Ressource aktiviert haben, [zum Beispiel für einen virtuellen Azure-Computer](msi-qs-configure-portal-windows-vm.md), gehen Sie wie folgt vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, unter dem Sie die MSI konfiguriert haben.

2. Navigieren Sie zu der gewünschten Ressource, für die Sie die Zugriffssteuerung ändern möchten. In diesem Beispiel wird einem virtuellen Azure-Computer der Zugriff auf ein Speicherkonto gewährt. Aus diesem Grund navigieren wir zu dem Speicherkonto.

3. Klicken Sie auf die Seite „Zugriffssteuerung (IAM)“ der Ressource, und klicken Sie dann auf „+ Hinzufügen“. Geben Sie dann die Rolle an, weisen Sie den Zugriff für „Virtueller Computer“ zu, und geben Sie das entsprechende Abonnement und die Ressourcengruppe an, in der sich die Ressource befindet. Unter dem Bereich mit den Suchkriterien sollte die Ressource nun angezeigt werden. Wählen Sie die Ressource aus, und klicken Sie auf „Speichern“: 

   ![Screenshot von „Zugriffssteuerung (IAM)“](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  

4. Sie sind zur Hauptseite „Zugriffssteuerung (IAM)“ zurückgekehrt, auf der ein neuer Eintrag für die MSI der Ressource angezeigt wird. In diesem Beispiel wurde dem virtuellen Computer „SimpleWinVM“ aus der Demoressourcengruppe der Zugriff „Mitwirkender“ auf das Speicherkonto gewährt:

   ![Screenshot von „Zugriffssteuerung (IAM)“](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-after.png)

## <a name="troubleshooting"></a>Problembehandlung

Wenn die MSI für die Ressource nicht in der Liste der verfügbaren Identitäten angezeigt wird, stellen Sie sicher, dass die MSI ordnungsgemäß aktiviert wurde. In unserem Fall können wir zurück zum virtuellen Azure-Computer wechseln und wie folgt vorgehen:

- Überprüfen Sie auf der Seite „Konfiguration“, ob für die MSI-Aktivierung „Ja“ ausgewählt ist.
- Vergewissern Sie sich auf der Seite „Erweiterungen“, dass die MSI-Erweiterung erfolgreich bereitgestellt wurde.

Wenn einer dieser Punkte nicht zutrifft, müssen Sie die MSI möglicherweise erneut für die Ressource bereitstellen oder den Fehler bei der Bereitstellung beheben.

## <a name="related-content"></a>Verwandte Inhalte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](msi-overview.md).
- Informationen zum Aktivieren von MSI auf einem virtuellen Azure-Computer finden Sie unter [Konfigurieren einer Azure-VM-MSI (Managed Service Identity, verwaltete Dienstidentität) über das Azure-Portal](msi-qs-configure-portal-windows-vm.md).



