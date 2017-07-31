---
title: Integrieren von Key Vault in SQL Server auf Windows-VMs in Azure (Resource Manager) | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie die Konfiguration der SQL Server-Verschlüsselung zur Verwendung mit dem Azure-Schlüsseltresor automatisieren. In diesem Thema wird beschrieben, wie Sie die Azure-Schlüsseltresor-Integration mit virtuellen SQL Server-Computern verwenden, die mit dem Ressourcen-Manager erstellt wurden."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: cd66dfb1-0e9b-4fb0-a471-9deaf4ab4ab8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/23/2017
ms.author: jroth
ms.translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 7dd5261960d3f93f01e221c5f5d7a397bc77d590
ms.contentlocale: de-de
ms.lasthandoff: 03/25/2017


---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-resource-manager"></a>Konfigurieren der Azure Key Vault-Integration für SQL Server auf virtuellen Azure-Computern (Resource Manager)
> [!div class="op_single_selector"]
> * [Ressourcen-Manager](virtual-machines-windows-ps-sql-keyvault.md)
> * [Klassisch](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a>Übersicht
Es gibt mehrere SQL Server-Verschlüsselungsfeatures, z.B. [Transparent Data Encryption (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [Column Level Encryption (CLE)](https://msdn.microsoft.com/library/ms173744.aspx) und [Sicherungsverschlüsselung](https://msdn.microsoft.com/library/dn449489.aspx). Bei diesen Arten der Verschlüsselung müssen Sie die kryptografischen Schlüssel verwalten und speichern, die Sie für die Verschlüsselung verwenden. Der Azure-Schlüsseltresor-Dienst (Azure Key Vault, AKV) ist dafür ausgelegt, die Sicherheit und Verwaltung dieser Schlüssel an einem sicheren und hoch verfügbaren Speicherort zu verbessern. Mit dem [SQL Server-Connector](http://www.microsoft.com/download/details.aspx?id=45344) kann SQL Server diese Schlüssel aus Azure Key Vault verwenden.

Wenn Sie SQL Server mit lokalen Computern ausführen, können Sie die [Schritte zum Zugreifen auf den Azure-Schlüsseltresor von Ihrem lokalen SQL Server-Computer ausführen](https://msdn.microsoft.com/library/dn198405.aspx). Sie können für SQL Server auf virtuellen Azure-Computern aber Zeit sparen, indem Sie die Funktion für die *Azure-Schlüsseltresor-Integration* verwenden.

Wenn diese Funktion aktiviert ist, wird der SQL Server-Connector automatisch aktiviert und der Anbieter für erweiterbare Schlüsselverwaltung für den Zugriff auf den Azure-Schlüsseltresor konfiguriert. Außerdem werden die Anmeldeinformationen erstellt, um den Zugriff auf Ihren Tresor zu ermöglichen. Wenn Sie sich die Schritte in der oben erwähnten Dokumentation für die Ausführung auf lokalen Computern angesehen haben, haben Sie erfahren, dass die Schritte 2 und 3 mit dieser Funktion automatisiert werden. Der einzige Schritt, den Sie noch manuell ausführen müssen, ist die Erstellung des Schlüsseltresors und der Schlüssel. Ab diesem Punkt ist das gesamte Setup des virtuellen SQL-Computers automatisiert. Nachdem die Funktion dieses Setup abgeschlossen hat, können Sie die T-SQL-Anweisungen ausführen, um mit der Verschlüsselung Ihrer Datenbanken oder Backups zu beginnen, wie Sie dies normalerweise auch tun.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Aktivieren und Konfigurieren der AKV-Integration
Sie können die AKV-Integration während der Bereitstellung aktivieren oder für vorhandene virtuelle Computer konfigurieren.

### <a name="new-vms"></a>Neue virtuelle Computer
Wenn Sie mithilfe von Resource Manager einen neuen virtuellen SQL Server-Computer bereitstellen, bietet das Azure-Portal einen Schritt zum Aktivieren der Azure Key Vault-Integration. Das Azure Key Vault-Feature ist nur für die Enterprise-, Developer- und Evaluation-Edition von SQL Server verfügbar.

![SQL – Azure-Schlüsseltresor-Integration](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Eine ausführliche exemplarische Vorgehensweise zur Bereitstellung finden Sie unter [Bereitstellen eines virtuellen Computers mit SQL Server im Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Vorhandene virtuelle Computer
Wählen Sie für vorhandene virtuelle Computer mit SQL Server Ihren virtuellen Computer mit SQL Server aus. Wählen Sie dann auf dem Blatt **Einstellungen** den Abschnitt **SQL Server-Konfiguration** aus.

![SQL-AKV-Integration für vorhandene virtuelle Computer](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

Klicken Sie auf dem Blatt **SQL Server-Konfiguration** im Abschnitt für die automatisierte Key Vault-Integration auf die Schaltfläche **Bearbeiten**.

![Konfigurieren der SQL-AKV-Integration für vorhandene virtuelle Computer](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Klicken Sie abschließend unten auf dem Blatt **SQL Server-Konfiguration** auf die Schaltfläche **OK**, um die Änderungen zu speichern.

> [!NOTE]
> Sie können die AKV-Integration auch mithilfe einer Vorlage konfigurieren. Weitere Informationen finden Sie unter [Azure quickstart template for Azure Key Vault integration](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update)(Azure-Schnellstartvorlage für die Azure Key Vault-Integration).
> 
> 

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]


