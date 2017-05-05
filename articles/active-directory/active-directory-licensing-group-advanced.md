---

title: "Gruppenbasierte Azure Active Directory-Lizenzierung – zusätzliche Szenarien | Microsoft-Dokumentation"
description: "Weitere Szenarien für die gruppenbasierte Azure Active Directory-Lizenzierung"
services: active-directory
keywords: "Azure AD-Lizenzierung"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/20/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: a7240c52e9351a60e3cf577d8112862c7dc8d913
ms.lasthandoff: 03/14/2017


---

# <a name="scenarios-limitations-and-known-issues-with-using-groups-to-manage-licensing-in-azure-active-directory"></a>Szenarien, Einschränkungen und bekannte Probleme mit der Verwendung von Gruppen zum Verwalten der Lizenzierung in Azure Active Directory

Verwenden Sie die folgenden Informationen und Beispiele, um ein besseres Verständnis der gruppenbasierten Lizenzierung von Azure Active Directory (Azure AD) zu entwickeln.

## <a name="use-group-based-licensing-with-dynamic-groups"></a>Verwenden der gruppenbasierten Lizenzierung mithilfe von dynamischen Gruppen

Sie können die gruppenbasierte Lizenzierung mit jeder beliebigen Sicherheitsgruppe verwenden. Dies bedeutet, dass eine Kombination mit dynamischen Azure AD-Gruppen möglich ist. Dynamische Gruppen führen Regeln für Benutzerobjektattribute aus, um Benutzer automatisch Gruppen hinzufügen und daraus zu entfernen.

Beispielsweise könnten Sie mehrere dynamische Gruppen erstellen, eine pro Produktsatz, den Sie Benutzern zuweisen möchten. Jede Gruppe enthält eine Regel, die ein bestimmtes Attribut für einen Benutzer prüft das angibt, welche Lizenzgruppe er erhalten soll. Sie können das Attribut lokal zuweisen und mit Azure AD synchronisieren, oder Sie können das Attribut direkt in der Cloud verwalten.

Wenn Sie das Attribut angeben, wird der Benutzer einer oder mehreren dieser dynamischen Lizenzierungsgruppen hinzugefügt. Kurz darauf werden dem Benutzer Lizenzen zugewiesen. Wenn das Attribut entfernt wird, verlässt der Benutzer die Gruppen, und die Lizenzen werden entfernt.

### <a name="example"></a>Beispiel

Stellen Sie sich eine lokale Identitätsverwaltungslösung vor, die entscheidet, welche Benutzer Zugriff auf welche Microsoft-Webdienste haben sollen. Die Lösung nutzt **extensionAttribute1** zum Speichern eines Zeichenfolgenwerts, der für die Lizenzen steht, über die Benutzer verfügen sollten. Azure AD Connect synchronisiert diesen Wert mit Azure AD.

In diesem Beispiel besteht das Ziel darin, Office 365 Enterprise E5- und Enterprise Mobility + Security-Lizenzen an Benutzer zu verteilen. Erstellen Sie in Azure AD zwei dynamische Gruppen (eine für jedes Produkt). Der Grund ist, dass einige Benutzer unter Umständen nur eines der Produkte benötigen. Geben Sie anschließend in jeder Gruppe die Regel für dynamische Mitgliedschaft und Lizenzeinstellungen an. Diese sehen wie folgt aus:

#### <a name="office-365-enterprise-e5-base-services"></a>Office 365 Enterprise E5: Basisdienste

![Screenshot: Office 365 Enterprise E5-Basisdienste](media/active-directory-licensing-group-advanced/o365-e5-base-services.png)

#### <a name="enterprise-mobility--security-licensed-users"></a>Enterprise Mobility + Security: Lizenzierte Benutzer

![Screenshot: Lizenzierte Enterprise Mobility + Security-Benutzer](media/active-directory-licensing-group-advanced/o365-e5-licensed-users.png)

In diesem Beispiel ändern Sie einen Benutzer und legen dessen „extensionAttribute1“ auf den Wert `EMS;E5_baseservices;` fest. Der Grund ist, dass dieser Benutzer über beide Lizenzen verfügen soll. Beachten Sie, dass Sie diese Änderung lokal vornehmen können. Im Anschluss an die Synchronisierung der Änderung mit der Cloud wird der Benutzer automatisch beiden Gruppen hinzugefügt, und die Lizenzen werden zugewiesen.

![Screenshot: Festlegen von extensionAttribute1 für den Benutzer](media/active-directory-licensing-group-advanced/user-set-extensionAttribute1.png)

### <a name="modify-a-dynamic-group-membership-rule"></a>Ändern einer Regel für dynamische Gruppenmitgliedschaft

Beim Ändern der Mitgliedschaftsregel einer vorhandenen Gruppe ist Vorsicht geboten. Wenn eine Regel geändert wird, werden alle Benutzer aus der Gruppe entfernt. Die Regel wird ausgewertet, und anschließend werden der Gruppe basierend auf den neuen Bedingungen Benutzer hinzugefügt.

Sind in der Gruppe Lizenzen zugewiesen, werden diese Lizenzen während des Vorgangs für alle Benutzer entfernt. Erst nach Auswertung der neuen Regel und dem erneuten Hinzufügen der Benutzer werden neue Lizenzen angewendet. Dies bedeutet, dass es bei Benutzern zu einer Dienstunterbrechung oder in einigen Fällen zu Datenverlust kommen kann.

Es ist besser, die Mitgliedschaftsregel einer Gruppe, die für die Lizenzzuweisung verwendet wird, nicht zu ändern. Erstellen Sie stattdessen eine neue Gruppe mit der neuen Regel, und legen Sie die gleichen Lizenzeinstellungen wie für die ursprüngliche Gruppe fest. Warten Sie, bis Mitglieder hinzugefügt und Lizenzen auf alle Benutzer angewendet wurden. Erst dann können Sie die ursprüngliche Gruppe löschen. Mit diesem Ansatz ist ein sicherer, gestaffelter Übergang zur neuen Mitgliedschaftsregel ohne Zugriffs- oder Datenverlust für Benutzer gewährleistet.


## <a name="multiple-groups-and-multiple-licenses"></a>Mehrere Gruppen und mehrere Lizenzen

Ein Benutzer kann Mitglied mehrerer Gruppen mit Lizenzen sein. Folgende Punkte sollten berücksichtigt werden:

- Mehrere Lizenzen für das gleiche Produkt können sich überlappen und dazu führen, dass alle aktivierten Dienste auf den Benutzer angewendet werden. Im folgenden Beispiel werden zwei Lizenzierungsgruppen veranschaulicht: *E3 base services* (E3-Basisdienste) enthält die grundlegenden Dienste, die wir zuerst für alle Benutzer bereitstellen möchten. *E3 extended services* (Erweiterte E3-Dienste) enthält zusätzliche Dienste (Sway und Planner), die nur für einige Benutzer bereitgestellt werden sollen. Beachten Sie, dass Yammer für die zukünftige Bereitstellung deaktiviert bleibt. In diesem Beispiel wurde der Benutzer beiden Gruppen hinzugefügt:

  ![Screenshot: Aktivierte Dienste](media/active-directory-licensing-group-advanced/view-enabled-services.png)

  Ergebnis: Für den Benutzer sind sieben der zwölf Dienste im Produkt aktiviert, und es wird nur eine Lizenz dafür genutzt.

- Bei Auswahl der *E3*-Lizenz werden weitere Details angezeigt, einschließlich der Informationen dazu, welche Dienste durch welche Gruppen für den Benutzer aktiviert wurden.

  ![Screenshot: Aktivierte Dienste nach Gruppe](media/active-directory-licensing-group-advanced/view-enabled-service-by-group.png)

## <a name="direct-licenses-coexist-with-group-licenses"></a>Gleichzeitig bestehende direkte Lizenzen und Gruppenlizenzen

Wenn ein Benutzer eine Lizenz aus einer Gruppe erbt, ist es nicht möglich, diese Lizenzzuweisung direkt im Benutzerobjekt zu entfernen oder zu ändern. Stattdessen müssen alle Änderungen in der Gruppe vorgenommen und dann vom System an alle Benutzer weitergegeben werden.

Es ist aber möglich, dem Benutzer zusätzlich zur geerbten Lizenz die gleiche Produktlizenz direkt zuzuweisen. Dies bietet sich beispielsweise an, um zusätzliche Dienste aus dem Produkt nur für einen Benutzer ohne Auswirkungen auf andere Benutzer zu aktivieren.

Direkt zugewiesene Lizenzen können entfernt werden, dies wirkt sich jedoch nicht auf die geerbte Lizenz aus. Nehmen wir beispielsweise an, ein Benutzer erbt eine Office 365 Enterprise E3-Lizenz von einer Gruppe.

1. Zuerst erbt der Benutzer die Lizenz nur von der Gruppe *E3 basic services* (E3-Basisdienste). Dies ermöglicht die Verwendung von vier Dienstplänen, wie in der folgenden Abbildung dargestellt.

  ![Screenshot mit aktivierten Diensten der E3-Gruppe](media/active-directory-licensing-group-advanced/e3-group-enabled-services.png)

2. Indem Sie auf **Zuweisen** klicken, können Sie dem Benutzer direkt eine E3-Lizenz zuweisen. In diesem Fall deaktivieren Sie alle Dienstpläne mit Ausnahme von Yammer Enterprise.

  ![Screenshot: Direkte Zuweisung einer Lizenz zu einem Benutzer](media/active-directory-licensing-group-advanced/assign-license-to-user.png)

3. Das Ergebnis ist, dass der Benutzer weiterhin nur eine Lizenz des E3-Produkts verwendet. Mit der direkten Zuweisung wird der Yammer Enterprise-Dienst aber nur für diesen Benutzer aktiviert. In der folgenden Abbildung ist dargestellt, welche Dienste jeweils durch die Gruppenmitgliedschaft und die direkte Zuweisung aktiviert sind.

  ![Screenshot: Geerbte und direkte Zuweisung im Vergleich](media/active-directory-licensing-group-advanced/direct-vs-inherited-assignment.png)

4. Wenn Sie die direkte Zuweisung verwenden, sind die folgenden Vorgänge zulässig:

  a. Yammer Enterprise kann direkt im Benutzerobjekt deaktiviert werden. Beachten Sie, dass der Umschalter **Ein/Aus** für diesen Dienst aktiviert ist (im Gegensatz zu den Umschaltern für die anderen Dienste). Dies liegt daran, dass dieser Dienst direkt für den Benutzer aktiviert wird und daher geändert werden kann.

  b. Außerdem können zusätzliche Dienste als Teil der direkt zugewiesenen Lizenz aktiviert werden.

  c. Mit der Schaltfläche **Entfernen** kann die direkte Lizenz vom Benutzer entfernt werden. Sie können sehen, dass der Benutzer jetzt nur die geerbte Gruppenlizenz hat und nur die ursprünglichen Dienste aktiviert bleiben.

    ![Screenshot: Entfernen der direkten Zuweisung](media/active-directory-licensing-group-advanced/remove-direct-license.png)

## <a name="usage-location"></a>Verwendungsstandort

Einige Microsoft-Dienste sind nicht an allen Standorten verfügbar. Bevor einem Benutzer eine Lizenz zugewiesen werden kann, muss der Administrator die Eigenschaft **Usage location** (Verwendungsstandort) für den Benutzer angeben. Dies ist im [Azure-Portal](https://portal.azure.com) unter **Benutzer** &gt; **Profil** &gt; **Einstellungen** möglich.

Bei der Gruppenlizenzzuweisung erben alle Benutzer ohne Verwendungsstandort den Standort des Verzeichnisses. Wenn sich Ihre Benutzer an unterschiedlichen Standorten befinden, achten Sie darauf, diese in Ihren Benutzerobjekten richtig anzugeben, bevor Sie Gruppen mit Lizenzen Benutzer hinzufügen.

## <a name="use-powershell-to-see-who-has-inherited-and-direct-licenses"></a>Verwenden von PowerShell, um anzuzeigen, wer über geerbte direkte Lizenzen verfügt

Während des Vorschauzeitraums einer Azure AD-Version kann PowerShell nicht verwendet werden, um Gruppenlizenzzuweisungen vollständig zu steuern. Es kann aber verwendet werden, um grundlegende Informationen zu Benutzerstatus abzurufen und zu ermitteln, ob Lizenzen aus einer Gruppe geerbt oder direkt zugewiesen wurden. Das folgende Codebeispiel zeigt, wie ein Administrator einen einfachen Bericht zu Lizenzzuweisungen erstellen kann.

1. Führen Sie das `connect-msolservice`-Cmdlet aus, um die Authentifizierung durchzuführen und eine Verbindung mit Ihrem Mandanten herzustellen.

2. `Get-MsolAccountSku` kann verwendet werden, um alle bereitgestellten Produktlizenzen im Mandanten zu ermitteln.

  ![Screenshot: Get-Msolaccountsku-Cmdlet](media/active-directory-licensing-group-advanced/get-msolaccountsku-cmdlet.png)

3. In diesem Beispiel soll ermittelt werden, welchen Benutzern die Enterprise Mobility + Security-Lizenz direkt, über eine Gruppe oder mit beiden Methoden zugewiesen wurde. Sie können das folgende PowerShell-Skript verwenden.
  
  ```
  #Returns TRUE if the user has the license assigned directly
  function UserHasLicenseAssignedDirectly
  {
      Param([Microsoft.Online.Administration.User]$user, [string]$skuId)
      foreach($license in $user.Licenses)
      {
          #we look for the specific license SKU in all licenses assigned to the user
          if ($license.AccountSkuId -ieq $skuId)
          {
              #GroupsAssigningLicense contains a collection of IDs of objects assigning the license
              #This could be a group object or a user object (contrary to what the name suggests)
              #If the collection is empty, this means the license is assigned directly. This is the case for users who have never been licensed via groups in the past
              if ($license.GroupsAssigningLicense.Count -eq 0)
              {
                  return $true
              }
              #If the collection contains the ID of the user object, this means the license is assigned directly
              #Note: the license may also be assigned through one or more groups in addition to being assigned directly
              foreach ($assignmentSource in $license.GroupsAssigningLicense)
              {
                  if ($assignmentSource -ieq $user.ObjectId)
                  {
                      return $true
                  }
              }
              return $false
          }
      }
      return $false
  }
  #Returns TRUE if the user is inheriting the license from a group
  function UserHasLicenseAssignedFromGroup
  {
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId)
     foreach($license in $user.Licenses)
     {
        #we look for the specific license SKU in all licenses assigned to the user
        if ($license.AccountSkuId -ieq $skuId)
        {
          #GroupsAssigningLicense contains a collection of IDs of objects assigning the license
          #This could be a group object or a user object (contrary to what the name suggests)
            foreach ($assignmentSource in $license.GroupsAssigningLicense)
          {
                  #If the collection contains at least one ID not matching the user ID this means that the license is inherited from a group.
                  #Note: the license may also be assigned directly in addition to being inherited
                  if ($assignmentSource -ine $user.ObjectId)

            {
                      return $true
            }
          }
              return $false
        }
      }
      return $false
  }
  ```

4. Mit dem restlichen Skript werden alle Benutzer abgerufen und diese Funktionen für alle Benutzer ausgeführt. Anschließend wird die Ausgabe als Tabelle formatiert.
    
  ```
  #the license SKU we are interested in
  $skuId = "reseller-account:EMS"
  #find all users that have the SKU license assigned
  Get-MsolUser -All | where {$_.isLicensed -eq $true -and $_.Licenses.AccountSKUID -eq $skuId} | select `
      ObjectId, `
      @{Name="SkuId";Expression={$skuId}}, `
      @{Name="AssignedDirectly";Expression={(UserHasLicenseAssignedDirectly $_ $skuId)}}, `
      @{Name="AssignedFromGroup";Expression={(UserHasLicenseAssignedFromGroup $_ $skuId)}}
  ```

5. Die Ausgabe des abgeschlossenen Skripts wird wie folgt angezeigt:

  ![Screenshot: Ausgabe des PowerShell-Skripts](media/active-directory-licensing-group-advanced/powershell-script-output.png)

## <a name="limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme

Wenn Sie die gruppenbasierte Lizenzierung verwenden, ist es ratsam, sich mit der folgenden Liste der Einschränkungen und bekannten Probleme vertraut zu machen.

- Die gruppenbasierte Lizenzierung unterstützt derzeit keine „geschachtelten Gruppen“ (Gruppen, die andere Gruppen enthalten). Wenn Sie eine Lizenz auf eine geschachtelte Gruppe anwenden, werden die Lizenzen nur auf unmittelbare Mitglieder der obersten Gruppenebene angewendet.

- Eine oder mehrere Lizenzen können ggf. nicht geändert werden, da sie von einer Gruppenmitgliedschaft geerbt werden. Verwenden Sie das Azure-Verwaltungsportal, um gruppenbasierte Lizenzen anzuzeigen oder zu ändern.

- Das [Office 365-Verwaltungsportal](https://portal.office.com ) unterstützt derzeit keine gruppenbasierte Lizenzierung. Wenn ein Benutzer eine Lizenz aus einer Gruppe erbt, wird diese Lizenz im Office-Verwaltungsportal als normale Benutzerlizenz angezeigt. Falls Sie versuchen, diese Lizenz zu ändern (z.B. zum Deaktivieren eines Diensts in der Lizenz) oder zu entfernen, wird im Portal eine Fehlermeldung zurückgegeben. Geerbte Gruppenlizenzen können nicht direkt für einen Benutzer geändert werden.

- Wenn ein Benutzer aus einer Gruppe entfernt wird und die Lizenz verliert, werden die Status der Dienstpläne aus dieser Lizenz (z.B. Exchange Online oder SharePoint Online) in „Angehalten“ geändert. Diese Dienstpläne sind nicht auf einen endgültigen deaktivierten Status festgelegt. Dies ist eine Vorsichtsmaßnahme, um ein versehentliches Entfernen von Benutzerdaten zu vermeiden, wenn ein Administrator einen Fehler bei der Verwaltung der Gruppenmitgliedschaft macht.

- Wenn Lizenzen für eine sehr große Gruppe zugewiesen oder geändert werden (z.B. 100.000 Benutzer), kann dies die Leistung beeinträchtigen. Vor allem das Volumen der Änderungen, das von der Azure AD-Automation generiert wird, kann sich negativ auf die Leistung Ihrer Verzeichnissynchronisierung zwischen Azure AD und lokalen Systemen auswirken. Dies kann zu Verzögerungen bei der Verzeichnissynchronisierung in Ihrer Umgebung führen.

- Die Automatisierung der Lizenzverwaltung reagiert nicht automatisch auf alle Typen von Änderungen in der Umgebung. Beispielsweise kann es sein, dass Sie keine verfügbaren Lizenzen mehr haben und für einige Benutzer daher ein Fehlerzustand gilt. Sie können einige direkt zugewiesene Lizenzen von anderen Benutzern entfernen, um mehr verfügbare Arbeitsplätze freizugeben. Das System reagiert aber nicht automatisch auf diese Änderung und entfernt nicht den Fehlerstatus von den Benutzern.

  Als Problemumgehung für diese Art von Einschränkungen können Sie in Azure AD zum Blatt **Gruppe** wechseln und auf **Erneut verarbeiten** klicken. Hierdurch werden alle Benutzer in dieser Gruppe verarbeitet und die Fehlerzustände nach Möglichkeit behoben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen Szenarien für die gruppenbasierte Lizenzverwaltung finden Sie unter:

* [Was ist die gruppenbasierte Lizenzierung in Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Zuweisen von Lizenzen zu einer Gruppe in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Bestimmen und Beheben von Lizenzproblemen für eine Gruppe in Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Migrieren einzelner lizenzierter Benutzer zur gruppenbasierten Lizenzierung in Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)

