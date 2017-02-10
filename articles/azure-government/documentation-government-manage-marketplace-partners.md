---
title: "Azure Marketplace für Azure Government-Partner | Microsoft-Dokumentation"
description: "Dieser Artikel enthält einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government."
services: azure-government
cloud: gov
documentationcenter: 
author: tsingh
manager: asimm
ms.assetid: 7790d3c5-d0fa-4662-b4f0-a174cb7d6c9f
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 11/14/2016
ms.author: zakramer;tsingh;divacc
translationtype: Human Translation
ms.sourcegitcommit: 0839e8e57b2149e50d4512d28b1e57e6592be458
ms.openlocfilehash: 70821864520ff18ba8c64be0ea66376bccee7148


---
# <a name="azure-marketplace-for-azure-government"></a>Azure Marketplace für Azure Government
Wenn Sie ein Azure Government-Partner und an der Veröffentlichung von Angeboten auf dem Azure Marketplace interessiert sind, helfen Ihnen die Informationen in diesem Artikel weiter.

Derzeit werden in Azure Government Marketplace BYOL-VM-Images und Lösungsvorlagen unterstützt. Eine bewährte Methode besteht darin, Lösungsvorlagen zu überprüfen, bevor sie für Azure Government veröffentlicht werden. So wird sichergestellt, dass sie sowohl in öffentlichen Azure-Clouds als auch in Azure Government-Clouds funktionieren. Wenn Sie nur ein VM-Image veröffentlichen, können Sie mit den weiter unten angegebenen Schritten zum Veröffentlichen fortfahren.

## <a name="pre-publishing-validation-for-solution-templates"></a>Überprüfung von Vorlagen vor dem Veröffentlichen

Bevor Sie die Lösungsvorlage für Azure Government veröffentlichen, ist es ratsam, einige bewährte Methoden anzuwenden. So stellen Sie sicher, dass Ihre Vorlage sowohl für die öffentliche Azure-Cloud als auch für Azure Government funktioniert.

1.  Vergewissern Sie sich, dass die Endpunkte in der Lösungsvorlage für die öffentliche Azure-Cloud nicht hartcodiert sind. Sie gelten nämlich nicht für andere Azure-Umgebungen. Ändern Sie stattdessen die Lösungsvorlage, um einen API-Aufruf zum Abrufen des gültigen Endpunkts anzufordern:  

  Beispiel:

  Falscher VHD-URI (hartcodiert) "uri": "[concat('https://', variables('storageAccountName'), '.blob.core.windows.net/', '/osdisk.vhd')]",

  Richtiger VHD-URI (Verweis)

  "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'osdisk.vhd')]",

  Mit der korrigierten Syntax wird sichergestellt, dass die Vorlage in jeder Cloud funktioniert (Gov, Public Azure, AzureStack, China usw.).

2.  Sorgen Sie dafür, dass die in der Lösungsvorlage verwendeten Ressourcen in der unabhängigen Cloud verfügbar sind, für die sie die Veröffentlichung durchführen.
RPs in Azure und API-Version

    Ermitteln Sie die Verfügbarkeit von Azure Government mit dem Ressourcen-Explorer im Portal:

  1.    Anmelden am Azure Government-Portal
  2.    Starten Sie den Ressourcen-Explorer, indem Sie die folgenden Schritte unter „https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-supported-services#supported-regions“ ausführen.

  Am häufigsten verwendete Erweiterungen, Verfügbarkeit in Fairfax  

  | Publisher/Typ | Verfügbare Versionen in Fairfax |
  | --- | --- |
  | Microsoft.OSTCExtensions/CustomScriptForLinux | 1; 1.1; 1.2.2.0; 1.3.0.2; 1.4.1.0; 1.5.2.0 |
  | Microsoft.Compute/CustomScriptExtension | 1.2; 1.3; 1.4; 1.7; 1.8 |
  | Microsoft.Azure.Extensions/DockerExtension | Nicht verfügbar. |
  | Microsoft.Azure.Extensions/CustomScript | 2.0.2 |
  | Microsoft.OSTCExtensions/LinuxDiagnostic | 2.0.9005; 2.1.9005; 2.2.9005; 2.3.9011 |
  | Microsoft.Powershell/DSC | 2.19.0.0 |

> [!NOTE]
> Wenn Sie für eine Liste mit zulässigen Werten Standorte angeben, müssen Sie sie regelmäßig aktualisieren, falls neue Regionen hinzugefügt werden.  


## <a name="publishing"></a>Veröffentlichung
> [!NOTE]
> Wenn Sie derzeit kein Azure Certified Marketplace-Partner sind, können Sie vor dem Fortfahren die Schritte unten ausführen, mit denen das [Veröffentlichen und Verwalten eines Angebots](../marketplace-publishing/marketplace-publishing-getting-started.md) veranschaulicht wird.
>
>

### <a name="step-1"></a>Schritt 1
Melden Sie sich für die [Azure-Veröffentlichung](https://publish.windowsazure.com) an.

### <a name="step-2"></a>Schritt 2
Klicken Sie auf das Angebot, das Sie veröffentlichen möchten.

### <a name="step-3"></a>Schritt 3
Klicken Sie auf **SKUS**, und wählen Sie das Feld **Azure Government-Cloud** aus.

> [!NOTE]
> Es werden nur „Bring your own license (BYOL)“-SKUs unterstützt.  Diese Option ist nicht für SKUs mit nutzungsbasierter Bezahlung (PayG) verfügbar.
>
>

![Angebotsseite mit den SKUs und Azure Government Cloud-Optionen](./media/government-manage-marketplace-partner-1.png)

### <a name="step-4"></a>Schritt 4
Klicken Sie auf den Link **+ Add Certification** (+ Zertifizierung hinzufügen), um Ihrem Angebot Zertifizierungen hinzuzufügen.

![Angebotsseite mit Link zum Hinzufügen einer Zertifizierung](./media/government-manage-marketplace-partner-2.png)

### <a name="step-5"></a>Schritt 5
Fordern Sie ein [Testkonto](https://azuregov.microsoft.com/trial/azuregovtrial) für Microsoft Azure Government an, um Ihr Image im Veröffentlichungsportal zu testen.

Ihre Berechtigung als Partner, der für Instanzen auf Bundes-, Staats-, Orts- oder Stammesebene der USA tätig ist, wird überprüft. Sie erhalten die Bestätigung per E-Mail.  Ihr Testkonto ist drei bis fünf Werktage lang verfügbar.

### <a name="step-6"></a>Schritt 6
Klicken Sie auf **Veröffentlichen** und dann auf **Für Stagingumgebung freigeben**.

![Optionen „Veröffentlichen“ und „Für Stagingumgebung freigeben“](./media/government-manage-marketplace-partner-3.png)

Sie werden aufgefordert, ein Abonnement in der Zulassungsliste einzugeben, für das Zugriff auf das bereitgestellte Angebot besteht. Geben Sie die Abonnement-ID Ihres neuen Testkontos für Azure Government ein.

![Eingabeaufforderung zum Angeben der Abonnement-IDs, mit denen auf das Angebot zugegriffen werden kann](./media/government-manage-marketplace-partner-4.png)

### <a name="step-7"></a>Schritt 7
Sobald das Angebot erfolgreich bereitgestellt wurde, können Sie Ihr Image testen, indem Sie sich mit Ihrem Azure Government-Testkonto am [Azure-Portal](https://portal.azure.us) anmelden.

### <a name="step-8"></a>Schritt 8
Nachdem Sie Ihr Image mithilfe des Testabonnements überprüft haben, können Sie das Angebot online verfügbar machen, indem Sie auf **Veröffentlichen** klicken und die Genehmigung für den Produktionsbetrieb anfordern.

![Befehl zum Anforderung der Genehmigung für den Produktionsbetrieb](./media/government-manage-marketplace-partner-5.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Updates erhalten Sie, indem Sie den [Microsoft Azure Government-Blog](https://blogs.msdn.microsoft.com/azuregov/) abonnieren.



<!--HONumber=Dec16_HO3-->


