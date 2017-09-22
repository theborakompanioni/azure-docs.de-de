---
title: Verwalten von Azure Stack-Speicherkonten | Microsoft-Dokumentation
description: Informationen zum Suchen, Verwalten, Wiederherstellen und Freigeben von Azure Stack-Speicherkonten
services: azure-stack
documentationcenter: 
author: AniAnirudh
manager: darmour
editor: 
ms.assetid: 627d355b-4812-45cb-bc1e-ce62476dab34
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/6/2017
ms.author: anirudha
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: 6e14bd6312135b45984a82099e68a934ec2a4a70
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="manage-storage-accounts-in-azure-stack"></a>Verwalten von Speicherkonten in Azure Stack
Erfahren Sie, wie Speicherkonten in Azure Stack verwaltet werden, um Speicherkapazität basierend auf Geschäftsanforderungen zu suchen, wiederherzustellen und freizugeben.

## <a name="find"></a>Suchen eines Speicherkontos
Die Liste der Speicherkonten in der Region kann in Azure Stack folgendermaßen angezeigt werden:

1. Navigieren Sie in einem Internetbrowser zu https://adminportal.local.azurestack.external.
2. Melden Sie sich als Cloudbetreiber (mit den Anmeldeinformationen, die Sie während der Bereitstellung angegeben haben) beim Azure Stack-Verwaltungsportal an.
3. Suchen Sie auf dem Standarddashboard nach der Liste **Regionsverwaltung**, und klicken Sie auf die Region, die Sie untersuchen möchten, z.B. **(lokal**).
   
   ![](media/azure-stack-manage-storage-accounts/image1.png)
4. Wählen Sie in der Liste **Ressourcenanbieter** die Option **Speicher** aus.
   
   ![](media/azure-stack-manage-storage-accounts/image2.png)
5. Führen Sie dann auf dem Administratorblatt für Speicherressourcenanbieter einen Bildlauf nach unten zur Registerkarte **Speicherkonten** durch, und klicken Sie darauf.
   
   ![](media/azure-stack-manage-storage-accounts/image3.png)
   
   Daraufhin wird die Seite mit der Liste der Speicherkonten in dieser Region angezeigt.
   
   ![](media/azure-stack-manage-storage-accounts/image4.png)

Standardmäßig werden die ersten 10 Konten angezeigt. Sie können weitere abrufen, indem Sie unten auf der Seite auf den Link **Weitere laden** klicken.

OR

Wenn Sie an einem bestimmten Speicherkonto interessiert sind, können Sie nur **die entsprechenden Konten abrufen**.


**So filtern Sie Konten:**

1. Klicken Sie oben auf dem Blatt auf **Filter**.
2. Auf dem Blatt „Filter“ können Sie einen **Kontonamen**, eine **Abonnement-ID** oder einen **Status** angeben, um eine Feinabstimmung der angezeigten Liste mit Speicherkonten vorzunehmen. Nutzen Sie die Optionen je nach Bedarf.
3. Klicken Sie auf **Aktualisieren**. Die Liste sollte entsprechend aktualisiert werden.
   
    ![](media/azure-stack-manage-storage-accounts/image5.png)
4. Zum Zurücksetzen des Filters klicken Sie auf **Filter**, deaktivieren die Auswahl und aktualisieren die Liste erneut.

Mit dem Suchfeld (oben auf Blatt mit der Speicherkontenliste) können Sie den ausgewählten Text in der Liste der Konten hervorheben. Dies ist äußerst praktisch, wenn der vollständige Name oder die ID nicht einfach verfügbar sind.

Hier können Sie Freitext verwenden, um das gewünschte Konto zu finden.

![](media/azure-stack-manage-storage-accounts/image6.png)

## <a name="look-at-account-details"></a>Überprüfen der Kontodetails
Wenn Sie die Konten, die Sie anzeigen möchten, gefunden haben, können Sie auf ein Konto klicken, um bestimmte Details anzuzeigen. Ein neues Blatt mit Kontodetails wird geöffnet, darunter der Typ des Kontos, die Erstellungszeit, der Standort usw.

![](media/azure-stack-manage-storage-accounts/image7.png)

## <a name="recover-a-deleted-account"></a>Wiederherstellen eines gelöschten Kontos
Es gibt Situationen, in denen Sie ein gelöschtes Konto wiederherstellen müssen.

In Azure Stack gibt es dazu eine sehr einfache Möglichkeit:

1. Navigieren Sie zur Liste der Speicherkonten. Weitere Informationen finden Sie in diesem Thema unter [Suchen eines Speicherkontos](#find).
2. Suchen Sie das gewünschte Konto in der Liste. Möglicherweise müssen Sie filtern.
3. Überprüfen Sie den *Status* des Kontos. Dieser müsste **Gelöscht** lauten.
4. Klicken Sie auf das Konto; daraufhin wird das Blatt „Kontodetails“ geöffnet.
5. Klicken Sie oben auf diesem Blatt auf die Schaltfläche **Wiederherstellen**.
6. Klicken Sie auf **Ja**, um zu bestätigen.
   
   ![](media/azure-stack-manage-storage-accounts/image8.png)
7. Die Wiederherstellung wird jetzt *ausgeführt. Warten* Sie auf den Hinweis, dass sie erfolgreich war.
   Sie können auch auf das Glockensymbol oben im Portal klicken, um eine Fortschrittsanzeige einzublenden.
   
   ![](media/azure-stack-manage-storage-accounts/image9.png)
   
   Sobald das wiederhergestellte Konto erfolgreich synchronisiert wurde, kann es wieder verwendet werden.

### <a name="some-gotchas"></a>Häufige Fehler
* Der Status des gelöschten Kontos gibt an, dass es **aus der Aufbewahrung entfernt** wurde.
  
  Dies bedeutet, dass der Aufbewahrungszeitraum für das gelöschte Konto überschritten ist und es möglicherweise nicht wiederhergestellt werden kann.
* Das gelöschte Konto wird nicht in der Liste der Benutzerkonten angezeigt.
  
  Dies könnte bedeuten, dass für das gelöschte Konto bereits eine automatische Speicherbereinigung durchgeführt wurde. In diesem Fall kann es nicht wiederhergestellt werden. Siehe [Freigeben von Kapazität](#reclaim) in diesem Thema.

## <a name="set-the-retention-period"></a>Festlegen des Aufbewahrungszeitraums
Die Einstellung für den Aufbewahrungszeitraum ermöglicht einem Cloudbetreiber das Festlegen eines Zeitraums in Tagen (zwischen 0 und 9999 Tage), in dem gelöschte Konten ggf. wiederhergestellt werden können. Der Standardaufbewahrungszeitraum ist auf 15 Tage festgelegt. Mit dem Wert „0“ wird festgelegt, dass alle gelöschten Konten sofort aus der Aufbewahrung entfernt und für die regelmäßige automatische Speicherbereinigung markiert werden.

**Ändern des Aufbewahrungszeitraums:**

1. Navigieren Sie in einem Internetbrowser zu https://adminportal.local.azurestack.external.
2. Melden Sie sich als Cloudbetreiber (mit den Anmeldeinformationen, die Sie während der Bereitstellung angegeben haben) beim Azure Stack-Verwaltungsportal an.
3. Suchen Sie auf dem Standarddashboard nach der Liste **Regionsverwaltung**, und klicken Sie auf die Region, die Sie untersuchen möchten, z.B. **(lokal**).
4. Wählen Sie in der Liste **Ressourcenanbieter** die Option **Speicher** aus.
5. Klicken Sie oben auf **Einstellungen**, um das Einstellungsblatt zu öffnen.
6. Klicken Sie auf **Konfiguration**, und bearbeiten Sie dann den Wert für den Aufbewahrungszeitraum.

   Legen Sie die Anzahl von Tagen fest, und speichern Sie sie.
   
   Dieser Wert ist sofort wirksam und gilt für Ihre gesamte Region.

   ![](media/azure-stack-manage-storage-accounts/image10.png)

## <a name="reclaim"></a>Freigeben von Kapazität
Einer der Nebeneffekte eines Aufbewahrungszeitraums ist, dass ein gelöschtes Konto weiterhin Kapazität verbraucht, bis sein Aufbewahrungszeitraum abläuft. Als Cloudbetreiber benötigen Sie möglicherweise eine Möglichkeit zum Freigeben des Speicherplatzes von gelöschten Konten, auch wenn der Aufbewahrungszeitraum noch nicht abgelaufen ist.

Sie können Kapazität entweder mithilfe des Azure-Portals oder mit PowerShell freigeben.

**Freigeben von Kapazität mithilfe des Portals:**
1. Navigieren Sie zum Blatt „Speicherkonten“. Siehe [Suchen eines Speicherkontos](#find).
2. Klicken Sie oben auf dem Blatt auf **Speicherplatz freigeben**.
3. Lesen Sie die Meldung, und klicken Sie dann auf **OK**.

    ![](media/azure-stack-manage-storage-accounts/image11.png)
4. Warten Sie auf eine Erfolgsmeldung – siehe Glockensymbol im Portal.

    ![](media/azure-stack-manage-storage-accounts/image12.png)
5. Aktualisieren Sie die Seite „Speicherkonten“. Die gelöschten Konten werden nicht mehr in der Liste angezeigt, da sie endgültig gelöscht wurden.

Sie können auch PowerShell verwenden, um den Aufbewahrungszeitraum explizit außer Kraft zu setzen und sofort Kapazität freizugeben.

**Freigeben von Kapazität mithilfe von PowerShell:**   

1. Vergewissern Sie sich, dass Azure PowerShell installiert und konfiguriert ist. Ist dies nicht der Fall, gehen Sie folgendermaßen vor: 
   * Anweisungen zur Installation und zur Zuordnung der aktuellen Azure PowerShell-Version an Ihr Azure-Abonnement finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](http://azure.microsoft.com/documentation/articles/powershell-install-configure/).
   Weitere Informationen zu Azure Resource Manager-Cmdlets finden Sie unter [Verwenden von Azure PowerShell mit Azure Resource Manager](http://go.microsoft.com/fwlink/?LinkId=394767).
2. Führen Sie das folgende Cmdlet aus:

> [!NOTE]
> Wenn Sie dieses Cmdlet ausführen, werden die Konten und deren Inhalte endgültig gelöscht. Es kann nicht wiederhergestellt werden. Verwenden Sie diese Option mit Umsicht.


        Clear-ACSStorageAccount -ResourceGroupName system.local -FarmName <farm ID>


Weitere Informationen finden Sie in der [Azure Stack PowerShell-Dokumentation.](https://msdn.microsoft.com/library/mt637964.aspx)
 

## <a name="migrate-a-container"></a>Migrieren eines Containers
Aufgrund von ungleichmäßiger Speichernutzung durch Mandanten stellt ein Cloudbetreiber möglicherweise fest, dass mindestens ein zugrunde liegender Mandant mehr des gemeinsam genutzten Speicherplatzes belegt als andere. In diesem Fall kann der Cloudbetreiber versuchen, Speicherplatz auf der beanspruchten Freigabe freizugeben, indem einige Blob-Container zu einer anderen Freigabe migriert werden. 

Sie müssen PowerShell verwenden, um Container zu migrieren.
> [!NOTE]
>Die Migration von Blob-Containern unterstützt keine Livemigration und ist zurzeit ein Offlinevorgang. Während der Migration und bis zu ihrem Abschluss können die zugrunde liegenden Blobs in diesem Container nicht verwendet werden und sind „offline“. 

**Migrieren von Containern mithilfe von PowerShell:**

1. Vergewissern Sie sich, dass Azure PowerShell installiert und konfiguriert ist. Ist dies nicht der Fall, gehen Sie folgendermaßen vor:
    * Anweisungen zur Installation und zur Zuordnung der aktuellen Azure PowerShell-Version an Ihr Azure-Abonnement finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](http://azure.microsoft.com/documentation/articles/powershell-install-configure/). Weitere Informationen zu Azure Resource Manager-Cmdlets finden Sie unter [Verwenden von Azure PowerShell mit Azure Resource Manager](http://go.microsoft.com/fwlink/?LinkId=394767).
2. Rufen Sie den Farmnamen ab: 
      
      `$farm = Get-ACSFarm -ResourceGroupName system.local`
3. Rufen Sie die Freigeben ab: 

   `$shares = Get-ACSShare -ResourceGroupName system.local -FarmName $farm.FarmName`

4. Rufen Sie die Container für eine angegebene Freigabe ab. Beachten Sie, dass „count“ und „intent“ optionale Parameter sind:
            
   `$containers = Get-ACSContainer -ResourceGroupName system.local -FarmName $farm.FarmName -ShareName $shares[0].ShareName -Count 4 -Intent Migration`  

   Untersuchen Sie dann „$containers“:

   `$containers`

    ![](media/azure-stack-manage-storage-accounts/image13.png)
5. Rufen Sie die besten Zielfreigaben für die Containermigration ab:

    `$destinationshares= Get-ACSSharesForMigration  -ResourceGroupName system.local -FarmName $farm.farmname -SourceShareName $shares[0].ShareName`

    Untersuchen Sie dann „$destinationshares“:

    `$destinationshares`

    ![](media/azure-stack-manage-storage-accounts/image14.png)
6. Starten Sie die Migration für einen Container. Beachten Sie, dass dies eine asynchrone Implementierung ist, damit eine Freigabeschleife für alle Container verwendet und der Status mithilfe der zurückgegebenen Auftrags-ID nachverfolgt werden kann.

    `$jobId = Start-ACSContainerMigration -ResourceGroupName system.local -FarmName $farm.farmname -ContainerToMigrate $containers[1] -DestinationShareUncPath $destinationshares.UncPath`

    Untersuchen Sie dann „$jobId“:

   ```
   $jobId
   d1d5277f-6b8d-4923-9db3-8bb00fa61b65
   ```
7. Überprüfen Sie den Status des Migrationsauftrags anhand der Auftrags-ID. Wenn die Containermigration abgeschlossen ist, wird der „MigrationStatus“ auf „Abgeschlossen“ festgelegt.

    `Get-ACSContainerMigrationStatus -ResourceGroupName system.local -FarmName $farm.farmname -JobId $jobId`

    ![](media/azure-stack-manage-storage-accounts/image15.png)

8. Sie können die Ausführung eines Migrationsauftrags abbrechen. Auch dabei handelt es sich um einen asynchronen Vorgang, der mithilfe von „$jobid“ nachverfolgt werden kann:

    `Stop-ACSContainerMigration-ResourceGroupName system.local -FarmName $farm.farmname -JobId $jobId-Verbose`

    ![](media/azure-stack-manage-storage-accounts/image16.png)

    Sie können den Status des Migrationsabbruchs erneut überprüfen:

    `Get-ACSContainerMigrationStatus-ResourceGroupName system.local -FarmName $farm.farmname -JobId $jobId`

    ![](media/azure-stack-manage-storage-accounts/image17.png)




  
  
