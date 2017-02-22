---
title: Erstellen eines Clouddienstcontainers mit PowerShell | Microsoft-Dokumentation
description: Dieser Artikel beschreibt das Erstellen eines Clouddienstcontainers mit PowerShell. Der Container hostet Web- und Workerrollen.
services: cloud-services
documentationcenter: .net
author: cawaMS
manager: timlt
editor: 
ms.assetid: c8f32469-610e-4f37-a3aa-4fac5c714e13
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
translationtype: Human Translation
ms.sourcegitcommit: c1551b250ace3aa6775932c441fcfe28431f8f57
ms.openlocfilehash: 89e517a17417475f975dbaa0cee8517145a7c6c3


---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Verwenden des Azure PowerShell-Befehls zum Erstellen eines leeren Clouddienstcontainers
Dieser Artikel erläutert das schnelle Erstellen eines Cloud Services-Containers mithilfe von Azure PowerShell-Cmdlets. Führen Sie die folgenden Schritte aus:

1. Installieren Sie das Microsoft Azure PowerShell-Cmdlet über die [Azure PowerShell-Downloadseite](http://aka.ms/webpi-azps) .
2. Öffnen Sie die PowerShell-Eingabeaufforderung.
3. Melden Sie sich mithilfe von [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) an.

   > [!NOTE]
   > Weitere Anweisungen zum Installieren des Azure PowerShell-Cmdlets und zum Herstellen einer Verbindung zu Ihrem Azure-Abonnement finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).
   >
   >
4. Verwenden Sie das **New-AzureService** -Cmdlet zum Erstellen eines leeren Clouddienstcontainers.

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. Folgen Sie diesem Beispiel, um das Cmdlet aufzurufen:

   ```
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Führen Sie Folgendes aus, um weitere Informationen zum Erstellen eines Azure-Clouddiensts zu erhalten:

```
Get-help New-AzureService
```

### <a name="next-steps"></a>Nächste Schritte
* Informationen zur Verwaltung der Clouddienstbereitstellung erhalten Sie unter den Befehlen [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx) und [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx). Weitere Informationen finden Sie auch unter [Konfigurieren von Clouddiensten](cloud-services-how-to-configure.md) .
* Um Ihr Clouddienstprojekt in Azure zu veröffentlichen, nutzen Sie das Codebeispiel **PublishCloudService.ps1** auf der Seite [Kontinuierliche Zustellung für Cloud Services in Azure](cloud-services-dotnet-continuous-delivery.md).



<!--HONumber=Dec16_HO2-->


