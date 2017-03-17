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
ms.date: 03/03/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 09f0ed3f7624bb242c40868710fb3eae49cda906
ms.openlocfilehash: a739e2320f8ea42912d169353a956e2b2f551619
ms.lasthandoff: 03/01/2017


---

# <a name="scenarios-limitations-and-known-issues-with-using-groups-to-manage-licensing-in-azure-active-directory"></a>Szenarien, Einschränkungen und bekannte Probleme mit der Verwendung von Gruppen zum Verwalten der Lizenzierung in Azure Active Directory 

## <a name="group-based-licensing-using-dynamic-groups"></a>Gruppenbasierte Lizenzierung mithilfe von dynamischen Gruppen

Gruppenbasierte Lizenzierung kann mit jeder beliebigen Sicherheitsgruppe verwendet werden, d.h. sie kann mit dynamischen Azure AD-Gruppen kombiniert werden. Dynamische Gruppen führen Regeln für Benutzerobjektattribute aus, um Benutzer automatisch Gruppen hinzufügen und daraus zu entfernen.

Beispielsweise könnten Sie mehrere dynamische Gruppen erstellen, eine pro Produktsatz, den Sie Benutzern zuweisen möchten. Jede Gruppe enthält eine Regel, die ein bestimmtes Attribut für einen Benutzer prüft das angibt, welche Lizenzgruppe er erhalten soll. Das Attribut kann lokal zugewiesen und mit Azure AD synchronisiert sein oder direkt in der Cloud verwaltet werden.

Wenn das Attribut angegeben ist, wird der Benutzer mindestens einer dieser dynamischen Lizenzierungsgruppen hinzugefügt. Kurz darauf werden dem Benutzer Lizenzen zugewiesen. Wenn das Attribut entfernt wird, verlässt der Benutzer die Gruppen, und Lizenzen werden entfernt.

### <a name="example"></a>Beispiel

In diesem Beispiel habe ich eine lokale Identitätsverwaltungslösung, die entscheidet, welche Benutzer Zugriff auf welche Microsoft Online Services haben sollen. Sie verwendet „extensionAttribute1“ zum Speichern eines Zeichenfolgenwerts, der die Lizenzen darstellt, über die der Benutzer verfügen sollte, und den Azure AD Connect mit Azure AD synchronisiert. Ich möchte Office 365 E5- und EMS-Lizenzen an meine Benutzer verteilen. In Azure AD habe ich zwei dynamische Gruppen erstellt – eine für jedes Produkt, da möglicherweise einige Benutzer nur eines der Produkte benötigen, das andere jedoch nicht. Ich habe in jeder Gruppe die Regel für dynamische Mitgliedschaft und Lizenzeinstellungen angegeben. Diese sehen wie folgt aus:

#### <a name="o365-e5--base-services"></a>O365 E5 – Basisdienste

![O365 E5-Basisdienste](media/active-directory-licensing-group-advanced/o365-e5-base-services.png)

#### <a name="ems-e5--licensed-users"></a>EMS E5 – lizenzierte Benutzer

![Lizenzierte O365 E5-Benutzer](media/active-directory-licensing-group-advanced/o365-e5-licensed-users.png)

Ich habe lokal einen Benutzer geändert und für sein extensionAttribute1-Element den Wert `EMS;E5_baseservices;` festgelegt, da der Benutzer beide Lizenzen erhalten soll. Im Anschluss an die Synchronisierung der Änderung mit der Cloud wurde der Benutzer automatisch beiden Gruppen hinzugefügt und Lizenzen wurden zugewiesen.

![Festlegen des extensionAttribute1-Element des Benutzers](media/active-directory-licensing-group-advanced/user-set-extensionAttribute1.png)

### <a name="modifying-a-dynamic-group-membership-rule"></a>Ändern einer Regel für dynamische Gruppenmitgliedschaft

Beim Ändern der Mitgliedschaftsregel einer vorhandenen Gruppe ist Vorsicht geboten. Wenn eine Regel geändert wird, werden alle Benutzer aus der Gruppe entfernt, die Regel wird ausgewertet, und Benutzer werden der Gruppe basierend auf den neuen Bedingungen hinzugefügt.

Sind in der Gruppe Lizenzen zugewiesen, wird diese Lizenz während des Vorgangs von allen Benutzern entfernt. Erst nach Auswertung der neuen Regel und dem erneuten Hinzufügen der Benutzer werden neue Lizenzen angewendet. Dies bedeutet, dass es bei Benutzern zu einer Dienstunterbrechung oder in einigen Fällen zu Datenverlust kommt.

Es wird empfohlen, die Mitgliedschaftsregel für eine Gruppe, die zum Zuweisen von Lizenzen verwendet wird, nicht zu ändern. Erstellen Sie stattdessen eine neue Gruppe mit der neuen Regel, und legen Sie die gleichen Lizenzeinstellungen wie für die ursprüngliche Gruppe fest. Warten Sie, bis Mitglieder hinzugefügt und Lizenzen auf alle Benutzer angewendet wurden. Löschen Sie erst danach die ursprüngliche Gruppe. Mit diesem Ansatz ist ein sicherer, gestaffelter Übergang zur neuen Mitgliedschaftsregel ohne Zugriffs- oder Datenverlust für Benutzer gewährleistet.


## <a name="multiple-groups-and-multiple-licenses"></a>Mehrere Gruppen und mehrere Lizenzen

Ein Benutzer kann Mitglied mehrerer Gruppen mit Lizenzen sein. Folgende Punkte sollten berücksichtigt werden:

1. Mehrere Lizenzen für das gleiche Produkt können sich überschneiden und dazu führen, dass alle aktivierten Dienste auf den Benutzer angewendet werden. Angenommen, wir haben zwei Lizenzierungsgruppen erstellt: *E3 – Basisdienste* enthält die grundlegenden Dienste, die wir zuerst für alle Benutzer bereitstellen möchten; *E3 – erweiterte Dienste* enthält zusätzliche Dienste (Sway und Planner), die wir mit einigen Benutzern, testen möchten, während Yammer für eine zukünftige Bereitstellung deaktiviert bleibt. In diesem Beispiel wurde der Benutzer beiden Gruppen hinzugefügt:

  ![Aktivierte Dienste anzeigen](media/active-directory-licensing-group-advanced/view-enabled-services.png)

  Ergebnis: Für den Benutzer sind 7 der 12 Dienste im Produkt aktiviert, und es wird nur eine Lizenz dafür genutzt.

2. Bei Auswahl der *E3*-Lizenz werden weitere Details angezeigt, einschließlich der Informationen darüber, welche Dienste durch welche Gruppen für den Benutzer aktiviert wurden.

  ![Aktivierte Dienste nach Gruppe anzeigen](media/active-directory-licensing-group-advanced/view-enabled-service-by-group.png)

## <a name="direct-licenses-coexisting-with-group-licenses"></a>Gleichzeitig bestehende direkte Lizenzen und Gruppenlizenzen

Wenn ein Benutzer eine Lizenz aus einer Gruppe erbt, ist es nicht möglich, diese Lizenzzuweisung direkt im Benutzerobjekt zu entfernen oder zu ändern. Stattdessen müssen alle Änderungen in der Gruppe vorgenommen und dann vom System an alle Benutzer weitergegeben werden.

Es ist jedoch möglich, dem Benutzer zusätzlich zur geerbten Lizenz die gleiche SKU-Lizenz direkt zuzuweisen. Dies bietet sich beispielsweise an, um zusätzliche Dienste aus der SKU nur für einen Benutzer ohne Auswirkungen auf andere Benutzer zu aktivieren.

Direkt zugewiesene Lizenzen können entfernt werden, dies wirkt sich jedoch nicht auf die geerbte Lizenz aus. Sehen wir uns den folgenden Benutzer an, der eine *Office 365 Enterprise E3*-Lizenz aus einer Gruppe erbt, mit der einige Dienste aktiviert werden.

1. Zuerst erbt der Benutzer die Lizenz nur aus der Gruppe *E3 – grundlegende Dienste*. Dadurch werden folgende vier Servicepläne aktiviert:

  ![E3-Gruppe – aktivierte Dienste](media/active-directory-licensing-group-advanced/e3-group-enabled-services.png)

2. Mit der Schaltfläche **Zuweisen** können wir dem Benutzer direkt eine E3-Lizenz zuweisen – in diesem Fall werden wir alle Servicepläne außer Yammer Enterprise deaktivieren.

  ![Zuweisen einer Lizenz an einen Benutzer](media/active-directory-licensing-group-advanced/assign-license-to-user.png)

3. Ergebnis: Der Benutzer belegt weiterhin nur eine Lizenz des E3-Produkts, die direkte Zuweisung aktiviert jedoch den Dienst *Yammer Enterprise* nur für diesen Benutzer. In der Liste sehen Sie, welche Dienste jeweils durch die Gruppenmitgliedschaft und die direkte Zuweisung aktiviert sind:

  ![Geerbte und direkte Zuweisung im Vergleich](media/active-directory-licensing-group-advanced/direct-vs-inherited-assignment.png)

4. Die folgenden Vorgänge sind mit der direkten Zuweisung zulässig:

  a. *Yammer Enterprise* kann direkt im Benutzerobjekt deaktiviert werden. Beachten Sie, dass die Ein/Aus-Schaltfläche im Gegensatz zu den anderen Dienstschaltflächen aktiviert ist – der Grund dafür ist, dass dieser Dienst direkt für den Benutzer aktiviert ist und daher geändert werden kann.

  b. Außerdem können zusätzliche Dienste als Teil der direkt zugewiesenen Lizenz aktiviert werden.

  c. Mit der Schaltfläche **Entfernen** kann die direkte Lizenz vom Benutzer entfernt werden. Sie können sehen, dass der Benutzer jetzt nur die geerbte Gruppenlizenz hat und nur die ursprünglichen Dienste aktiviert bleiben.

    ![Entfernen der direkten Zuweisung](media/active-directory-licensing-group-advanced/remove-direct-license.png)

## <a name="usage-location"></a>Verwendungsstandort

Einige Microsoft-Dienste sind nicht in an allen Standorten verfügbar. Bevor einem Benutzer eine Lizenz zugewiesen werden kann, muss der Administrator daher für den Benutzer die Eigenschaft „Verwendungsstandort“ angeben. Dies ist im [Azure-Portal](https://portal.azure.com) unter „Benutzer“ &gt;„Profil“ &gt;„Einstellungen“ möglich.

Bei Verwendung der Gruppenlizenzzuweisung erben alle Benutzer ohne Verwendungsstandort den Standort des Verzeichnisses. Wenn sich Ihre Benutzer an unterschiedlichen Standorten befinden, achten Sie darauf, diese in Ihren Benutzerobjekten richtig anzugeben, bevor Sie Gruppen mit Lizenzen Benutzer hinzufügen.

## <a name="use-powershell-to-see-who-has-inherited-and-direct-licenses"></a>Verwenden von PowerShell, um anzuzeigen, wer über geerbte direkte Lizenzen verfügt

Während der öffentlichen Vorschau können Gruppenlizenzzuweisungen nicht vollständig mithilfe von PowerShell gesteuert werden. Es kann jedoch verwendet werden, um grundlegende Informationen zu Benutzerstatus abzurufen und zu ermitteln, ob Lizenzen aus einer Gruppe geerbt oder direkt zugewiesen sind. Das folgende Codebeispiel zeigt, wie ein Administrator einen einfachen Bericht zu Lizenzzuweisungen erstellen kann.

1. Führen Sie das Cmdlet `connect-msolservice` aus, um die Authentifizierung durchzuführen und eine Verbindung mit Ihrem Mandanten herzustellen.

2. `Get-MsolAccountSku` kann verwendet werden, um alle bereitgestellten SKU-Lizenzen im Mandanten zu ermitteln.

  ![Verwenden des Cmdlets „Get-Msolaccountsku“](media/active-directory-licensing-group-advanced/get-msolaccountsku-cmdlet.png)

3. In diesem Beispiel möchten wir ermitteln, welchen Benutzern die *EMS*-Lizenz direkt, über eine Gruppe oder mit beiden Methoden zugewiesen ist. Wir verwenden ein PowerShell-Skript, das zwei Funktionen enthält, die diese Frage für ein Benutzerobjekt und eine SKU beantworten können.
  ```
  # Returns TRUE if the user has the license assigned directly

  function UserHasLicenseAssignedDirectly
  {
      Param([Microsoft.Online.Administration.User]$user, [string]$skuId)

      foreach($license in $user.Licenses)
      {
          # we look for the specific license SKU in all licenses assigned to the user
          if ($license.AccountSkuId -ieq $skuId)
          {
              # GroupsAssigningLicense contains a collection of IDs of objects assigning the license
              # This could be a group object or a user object (contrary to what the name suggests)
              # If the collection is empty, this means the license is assigned directly - this is the case for users who have never been licensed via groups in the past

              if ($license.GroupsAssigningLicense.Count -eq 0)
              {
                  return $true
              }
              # If the collection contains the ID of the user object, this means the license is assigned directly
              # Note: the license may also be assigned through one or more groups in addition to being assigned directly
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
  # Returns TRUE if the user is inheriting the license from a group
  function UserHasLicenseAssignedFromGroup
  {
      Param([Microsoft.Online.Administration.User]$user, [string]$skuId)
      foreach($license in $user.Licenses)
      {
          # we look for the specific license SKU in all licenses assigned to the user
          if ($license.AccountSkuId -ieq $skuId)
          {
              # GroupsAssigningLicense contains a collection of IDs of objects assigning the license
              # This could be a group object or a user object (contrary to what the name suggests)
              foreach ($assignmentSource in $license.GroupsAssigningLicense)
              {
                  # If the collection contains at least one ID not matching the user ID this means that the license is inherited from a group.
                  # Note: the license may also be assigned directly in addition to being inherited
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
4. Der Rest des Skripts ruft alle Benutzer ab, führt diese Funktionen für die einzelnen Benutzer durch und formatiert anschließend die Ausgabe in einer Tabelle.

  ```
  # the license SKU we are interested in
  $skuId = "reseller-account:EMS"
  # find all users that have the SKU license assigned
  Get-MsolUser -All | where {$_.isLicensed -eq $true -and $_.Licenses.AccountSKUID -eq $skuId} | select `
      ObjectId, `
      @{Name="SkuId";Expression={$skuId}}, `
      @{Name="AssignedDirectly";Expression={(UserHasLicenseAssignedDirectly $_ $skuId)}}, `
      @{Name="AssignedFromGroup";Expression={(UserHasLicenseAssignedFromGroup $_ $skuId)}}
  ```

5. Die Ausgabe des abgeschlossenen Skripts wird wie folgt angezeigt:

  ![PowerShell-Skriptausgabe](media/active-directory-licensing-group-advanced/powershell-script-output.png)

## <a name="limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme

1. Die gruppenbasierte Lizenzierung unterstützt derzeit keine „geschachtelten Gruppen“ (Gruppen, die andere Gruppen enthalten). Wenn Sie eine Lizenz auf eine geschachtelte Gruppe anwenden, werden die Lizenzen nur auf unmittelbare Mitglieder der obersten Gruppenebene angewendet.

2. Gruppenbasierte Lizenzierung ist derzeit nur über [das Azure-Portal](https://portal.azure.com) verfügbar. Zu diesem Zeitpunkt ist es nicht möglich, Lizenzen in Gruppen mithilfe von PowerShell festzulegen oder zu ändern.

3. Das [Office 365-Verwaltungsportal](https://portal.office.com ) unterstützt derzeit keine gruppenbasierte Lizenzierung. Wenn ein Benutzer eine Lizenz aus einer Gruppe erbt, wird diese Lizenz im Office-Verwaltungsportal als normale Benutzerlizenz angezeigt. Wenn Sie versuchen, diese Lizenz zu ändern (z.B. um einen Dienst in der Lizenz zu deaktivieren oder die Lizenz zu entfernen) gibt das Portal eine Fehlermeldung zurück (da geerbte Gruppenlizenzen nicht direkt für einen Benutzer geändert werden können).

  `One or more of the licenses could not be modified because they are inherited from a group membership. To view or modify group based licenses visit the Azure admin portal.`

4. Wenn ein Benutzer aus einer Gruppe entfernt wird und die Lizenz verliert, werden die Status der Servicepläne aus dieser Lizenz (z.B. Exchange Online oder SharePoint Online) in „ausgesetzt“ anstatt in einen endgültigen deaktivierten Zustand geändert. Dies ist eine Vorsichtsmaßnahme, um ein versehentliches Entfernen von Benutzerdaten zu vermeiden, wenn ein Administrator einen Fehler bei der Verwaltung der Gruppenmitgliedschaft macht.

  Wir werden einen Workflow implementieren, in dem diese Servicepläne für diese Benutzer schließlich vollständig deaktiviert werden. Bis diese Option verfügbar ist, werden u.U. einige Dienste weiterhin für Benutzer ausgeführt, die aus einer Gruppe entfernt wurden und keine Lizenz mehr haben.

5. Wenn Lizenzen einer sehr großen Benutzergruppe (z.B. 100.000 Benutzer) zugewiesen oder dafür geändert werden, wirkt sich die große Anzahl der von Azure AD Automation generierten Änderungen möglicherweise negativ auf die Leistung der Verzeichnissynchronisierung zwischen Azure AD und lokalen Systemen aus. Dies kann zu Verzögerungen bei der Verzeichnissynchronisierung in Ihrer Umgebung führen.

6. Die Automatisierung der Lizenzverwaltung reagiert nicht automatisch auf alle Typen von Änderungen in der Umgebung. Angenommen, Sie haben nicht mehr genügend Lizenzen und einige Benutzer weisen den Fehlerstatus „Nicht genügend Lizenzen“ aus. In diesem Fall können Sie einige direkt zugewiesene Lizenzen von anderen Benutzern entfernen, um mehr verfügbare Arbeitsplätze freizugeben. Das System reagiert jedoch nicht automatisch auf diese Änderung und entfernt nicht den Fehlerstatus von den Benutzern.

  Als Problemumgehung für diese Art von Einschränkungen können Sie in Azure AD zum Blatt „Gruppe“ wechseln und auf die Schaltfläche zum **erneuten Verarbeiten** klicken. Dadurch werden alle Benutzer in dieser Gruppe verarbeitet und die Fehlerzustände nach Möglichkeit behoben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen Szenarien für die gruppenbasierte Lizenzverwaltung finden Sie in folgenden Artikeln:

* [Was ist die gruppenbasierte Lizenzierung in Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Zuweisen von Lizenzen zu einer Gruppe in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Bestimmen und Beheben von Problemen für eine Gruppe in Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Migrieren einzelner lizenzierter Benutzer zur gruppenbasierten Lizenzierung in Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)

