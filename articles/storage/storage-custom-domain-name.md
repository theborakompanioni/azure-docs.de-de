---
title: "Konfigurieren eines benutzerdefinierten Domänennamens für Ihren Azure Blob Storage-Endpunkt | Microsoft Docs"
description: Verwenden Sie das Azure-Portal, um Ihren eigenen kanonischen Namen (CNAME) dem Blob Storage-Endpunkt in einem Azure Storage-Konto zuzuordnen.
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: aaafd8c5-eacb-49dc-8c8b-3f7011ad5e92
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: e99294069f92f51d212b38b1c5ee12232c6dc77d
ms.lasthandoff: 04/27/2017


---
# <a name="configure-a-custom-domain-name-for-your-blob-storage-endpoint"></a>Konfigurieren eines benutzerdefinierten Domänennamens für Ihren Blob Storage-Endpunkt

Sie können eine benutzerdefinierte Domäne für den Zugriff auf Blob-Daten in Ihrem Azure-Speicherkonto konfigurieren. Der Standardendpunkt für den Blobspeicher ist `<storage-account-name>.blob.core.windows.net`. Wenn Sie dem Blobendpunkt für Ihr Speicherkonto eine benutzerdefinierte Domäne und Unterdomäne wie **www.contoso.com** zuordnen, können Benutzer über diese Domäne auf Blobdaten in Ihrem Speicherkonto zugreifen.

> [!IMPORTANT]
> Azure Storage unterstützt noch nicht HTTPS für benutzerdefinierte Domänen. Bisher gibt es diesbezüglich noch keinen Zeitplan, den wir mitteilen könnten. Wir sind uns aber bewusst, dass unsere Kunden diese Funktionalität wünschen.
>

Die folgende Tabelle enthält einige Beispiel-URLs für Blobdaten, die sich in einem Speicherkonto namens **mystorageaccount** befinden. Die für das Speicherkonto registrierte benutzerdefinierte Domäne ist **www.contoso.com**:

| Ressourcentyp | Standard-URL | Benutzerdefinierte Domänen-URL |
| --- | --- | --- |
| Speicherkonto | http://mystorageaccount.blob.core.windows.net | http://www.contoso.com |
| Blob |http://mystorageaccount.blob.core.windows.net/mycontainer/myblob | http://www.contoso.com/mycontainer/myblob |
| Stammcontainer | http://mystorageaccount.blob.core.windows.net/myblob oder http://mystorageaccount.blob.core.windows.net/$root/myblob| http://www.contoso.com/myblob oder http://www.contoso.com/$root/myblob |

## <a name="direct-vs-intermediary-domain-mapping"></a>Direkte und zwischengeschaltete Domänenzuordnung

Es gibt zwei Möglichkeiten, Ihre benutzerdefinierte Domäne an den Blobendpunkt Ihres Speicherkontos zu binden: direkte CNAME-Zuordnung und Verwenden der Unterdomäne *asverify* als zwischengeschaltete Domäne.

### <a name="direct-cname-mapping"></a>Direkte CNAME-Zuordnung

Die erste und einfachste Methode besteht darin, einen CNAME-Eintrag (kanonischer Name) zu erstellen, in dem Ihre benutzerdefinierte Domäne und Unterdomäne direkt dem Blobendpunkt zugeordnet wird. Ein CNAME-Eintrag ist ein DNS-Feature (Domain Name System), das eine Quelldomäne zu einer Zieldomäne zuordnet. In diesem Fall ist die Quelldomäne Ihre eigene benutzerdefinierte Domäne und Unterdomäne, z. B. *www.contoso.com*. Die Zieldomäne ist Ihr Blob-Dienstendpunkt, beispielsweise *mystorageaccount.blob.core.windows.net*.

Die direkte Methode ist unter [Registrieren einer benutzerdefinierten Domäne](#register-a-custom-domain) beschrieben.

### <a name="intermediary-mapping-with-asverify"></a>Zwischengeschaltete Zuordnung mit *asverify*

Auch bei der zweiten Methode werden CNAME-Einträge verwendet, aber zunächst wird eine spezielle Unterdomäne genutzt, die von Azure erkannt wird, um Downtime zu vermeiden: **asverify**.

Die Zuordnung Ihrer benutzerdefinierten Domäne zu einem Blobendpunkt kann zu einer kurzzeitigen Downtime für die Domäne führen, während Sie die Domäne im [Azure-Portal](https://portal.azure.com) registrieren. Wenn Ihre benutzerdefinierte Domäne derzeit eine Anwendung mit einer Vereinbarung zum Servicelevel (SLA) unterstützt, die keine Downtime zulässt, können Sie die Azure-Unterdomäne *asverify* als zwischengeschalteten Registrierungsschritt verwenden. Dieser zwischengeschaltete Schritt stellt sicher, dass Benutzer auf Ihre Domäne zugreifen können, während die DNS-Zuordnung vorgenommen wird.

Die Zwischenschaltungsmethode ist unter [Registrieren einer benutzerdefinierten Domäne mithilfe der Unterdomäne *asverify*](#register-a-custom-domain-using-the-asverify-subdomain) beschrieben.

## <a name="register-a-custom-domain"></a>Registrieren einer benutzerdefinierten Domäne
Verwenden Sie dieses Verfahren, um Ihre benutzerdefinierte Domäne zu registrieren, wenn es unerheblich ist, ob die Domäne Benutzern kurzzeitig nicht zur Verfügung steht, oder wenn Ihre benutzerdefinierte Domäne derzeit keine Anwendung hostet.

Wenn Ihre benutzerdefinierte Domäne derzeit eine Anwendung unterstützt, die keine Downtime zulässt, gehen Sie nach dem Verfahren vor, das unter [Registrieren einer benutzerdefinierten Domäne mithilfe der Unterdomäne *asverify*](#register-a-custom-domain-using-the-asverify-subdomain) skizziert ist.

Um einen benutzerdefinierten Domänennamen zu konfigurieren, müssen Sie einen neuen CNAME-Eintrag in DNS erstellen. Der CNAME-Eintrag gibt einen Alias für einen Domänennamen an. In diesem Fall ordnet er die Adresse Ihrer benutzerdefinierten Domäne dem Blob Storage-Endpunkt für Ihr Speicherkonto zu.

In der Regel können Sie die DNS-Einstellungen Ihrer Domäne auf der Website Ihrer Domänenregistrierungsstelle verwalten. Die Methoden zum Festlegen eines CNAME-Datensatzes der verschiedenen Registrierungen sind zwar ähnlich, jedoch unterscheiden sie sich auch in einigen Punkten. Das Konzept ist allerdings gleich. Einige Basispakete für die Domänenregistrierung bieten keine DNS-Konfiguration, daher müssen Sie möglicherweise Ihr Domänenregistrierungspaket aktualisieren, bevor Sie den CNAME-Eintrag erstellen können.

1. Navigieren Sie zum Speicherkonto im [Azure-Portal](https://portal.azure.com).
1. Wählen Sie unter **BLOB-DIENST** auf dem Menüblatt die Option **Benutzerdefinierte Domäne** aus, um das Blatt *Benutzerdefinierte Domäne* zu öffnen.
1. Melden Sie sich bei der Website Ihrer Domänenregistrierungsstelle an, und öffnen Sie die Seite für die DNS-Verwaltung. Diese finden Sie beispielsweise in einem Abschnitt mit der Bezeichnung **Domänenname**, **DNS** oder **Namenserververwaltung**.
1. Suchen Sie den Abschnitt zur Verwaltung von CNAMEs. Möglicherweise müssen Sie eine Seite mit erweiterten Einstellungen aufrufen und nach den Wörtern **CNAME**, **Alias** oder **Unterdomänen** suchen.
1. Erstellen Sie einen neuen CNAME-Eintrag, und geben Sie einen Unterdomänenalias wie etwa **www** oder **photos** an. Geben Sie dann einen Hostnamen (Ihr Blob-Dienstendpunkt) im Format **mystorageaccount.blob.core.windows.net** an (hierbei ist *mystorageaccount* der Name Ihres Speicherkontos). Der zu verwendende Hostname wird im [Azure-Portal](https://portal.azure.com) im ersten Element auf dem Blatt *Benutzerdefinierte Domäne* angezeigt.
1. Geben Sie in das Textfeld auf dem Blatt *Benutzerdefinierte Domäne* im [Azure-Portal](https://portal.azure.com) den Namen Ihrer benutzerdefinierten Domäne einschließlich Unterdomäne ein. Wenn Ihre Domäne beispielsweise **contoso.com** und Ihr Unterdomänenalias **www** lautet, geben Sie **www.contoso.com** ein. Lautet Ihre Unterdomäne **photos**, geben Sie **photos.contoso.com** ein. Die Unterdomäne ist *erforderlich*.
1. Wählen Sie auf dem Blatt **Benutzerdefinierte Domäne** die Option *Speichern* aus, um Ihre benutzerdefinierte Domäne zu registrieren. Wenn die Registrierung erfolgreich ist, sehen Sie eine Meldung, die besagt, dass Ihr Speicherkonto erfolgreich aktualisiert wurde.

Sobald Ihr neuer CNAME-Eintrag über DNS weitergegeben wurde, können Ihre Benutzer Blobdaten anzeigen, indem sie Ihre benutzerdefinierten Domäne verwenden, sofern die Benutzer die entsprechenden Berechtigungen haben.

## <a name="register-a-custom-domain-using-the-asverify-subdomain"></a>Registrieren einer benutzerdefinierten Domäne mithilfe der Unterdomäne *asverify*
Gehen Sie nach diesem Verfahren vor, um Ihre benutzerdefinierte Domäne zu registrieren, wenn diese derzeit eine Anwendung mit einer SLA unterstützt, die keine Downtime zulässt. Durch Erstellen eines CNAME-Eintrags, in dem von `asverify.<subdomain>.<customdomain>` auf `asverify.<storageaccount>.blob.core.windows.net` verwiesen wird, können Sie Ihre Domäne vorab bei Azure registrieren. Anschließend können Sie einen zweiten CNAME-Eintrag erstellen, in dem von `<subdomain>.<customdomain>` auf `<storageaccount>.blob.core.windows.net` verwiesen wird. Dies ist der Punkt, ab dem Datenverkehr zu Ihrer benutzerdefinierten Domäne an Ihren Blobendpunkt geleitet wird.

Die Unterdomäne **asverify** ist eine spezielle Unterdomäne, die von Azure erkannt wird. Indem Sie Ihrer eigenen Unterdomäne `asverify` voranstellen, erlauben Sie Azure, Ihre benutzerdefinierte Domäne zu erkennen, ohne den DNS-Eintrag für die Domäne zu ändern. Wenn Sie den DNS-Eintrag für die Domäne ändern, wird diese dem Blobendpunkt ohne Downtime zugeordnet.

1. Navigieren Sie zum Speicherkonto im [Azure-Portal](https://portal.azure.com).
1. Wählen Sie unter **BLOB-DIENST** auf dem Menüblatt die Option **Benutzerdefinierte Domäne** aus, um das Blatt *Benutzerdefinierte Domäne* zu öffnen.
1. Melden Sie sich bei der Website Ihres DNS-Anbieters an, und öffnen Sie die Seite für die DNS-Verwaltung. Diese finden Sie beispielsweise in einem Abschnitt mit der Bezeichnung **Domänenname**, **DNS** oder **Namenserververwaltung**.
1. Suchen Sie den Abschnitt zur Verwaltung von CNAMEs. Möglicherweise müssen Sie eine Seite mit erweiterten Einstellungen aufrufen und nach den Wörtern **CNAME**, **Alias** oder **Unterdomänen** suchen.
1. Erstellen Sie einen neuen CNAME-Eintrag, und geben Sie einen Unterdomänenalias an, der die Unterdomäne *asverify* enthält. Beispielsweise **asverify.www** oder **asverify.photos**. Geben Sie dann einen Hostnamen (Ihr Blob-Dienstendpunkt) im Format **asverify.mystorageaccount.blob.core.windows.net** an (hierbei ist **mystorageaccount** der Name Ihres Speicherkontos). Der zu verwendende Hostname wird im [Azure-Portal](https://portal.azure.com) im zweiten Element auf dem Blatt *Benutzerdefinierte Domäne* angezeigt.
1. Geben Sie in das Textfeld auf dem Blatt *Benutzerdefinierte Domäne* im [Azure-Portal](https://portal.azure.com) den Namen Ihrer benutzerdefinierten Domäne einschließlich Unterdomäne ein. Fügen Sie *asverify* nicht ein. Wenn Ihre Domäne beispielsweise **contoso.com** und Ihr Unterdomänenalias **www** lautet, geben Sie **www.contoso.com** ein. Lautet Ihre Unterdomäne **photos**, geben Sie **photos.contoso.com** ein. Die Unterdomäne ist erforderlich.
1. Aktivieren Sie das Kontrollkästchen **Indirekte CNAME-Überprüfung verwenden**.
1. Wählen Sie auf dem Blatt **Benutzerdefinierte Domäne** die Option *Speichern* aus, um Ihre benutzerdefinierte Domäne zu registrieren. Wenn die Registrierung erfolgreich ist, sehen Sie eine Meldung, die besagt, dass Ihr Speicherkonto erfolgreich aktualisiert wurde. An diesem Punkt wurde Ihre benutzerdefinierte Domäne von Azure überprüft, allerdings wird der Datenverkehr zu Ihrer Domäne noch nicht zu Ihrem Speicherkonto geleitet.
1. Kehren Sie zur Website Ihrer DNS-Registrierungsstelle zurück, und erstellen Sie einen weiteren CNAME-Eintrag, in dem Ihre Unterdomäne dem Endpunkt Ihres Blob-Diensts zugeordnet wird. Geben Sie etwa die Unterdomäne **www** oder **photos** (ohne *asverify*) und den Hostnamen **mystorageaccount.blob.core.windows.net** an (wobei **mystorageaccount** der Name Ihres Speicherkontos ist). Mit diesem Schritt ist die Registrierung Ihrer benutzerdefinierten Domäne abgeschlossen.
1. Abschließend können Sie den von Ihnen erstellten CNAME-Eintrag löschen, der die Unterdomäne **asverify** enthält, denn dieser Eintrag war nur als zwischengeschalteter Schritt erforderlich.

Sobald Ihr neuer CNAME-Eintrag über DNS weitergegeben wurde, können Ihre Benutzer Blobdaten anzeigen, indem sie Ihre benutzerdefinierten Domäne verwenden, sofern die Benutzer die entsprechenden Berechtigungen haben.

## <a name="test-your-custom-domain"></a>Testen Ihrer benutzerdefinierten Domäne

Um zu bestätigen, dass Ihre benutzerdefinierte Domäne tatsächlich Ihrem Blob-Dienstendpunkt zugeordnet ist, erstellen Sie einen Blob in einem öffentlichen Container in Ihrem Speicherkonto. Greifen Sie anschließend in einem Webbrowser mit einem URI im folgenden Format auf den Blob zu:

`http://<subdomain.customdomain>/<mycontainer>/<myblob>`

Sie könnten beispielsweise den folgenden URI verwenden, um auf ein Webformular im Container **myforms** in der benutzerdefinierten Unterdomäne **photos.contoso.com** zuzugreifen:

`http://photos.contoso.com/myforms/applicationform.htm`

## <a name="deregister-a-custom-domain"></a>Aufheben der Registrierung einer benutzerdefinierten Domäne

Um die Registrierung einer benutzerdefinierten Domäne für Ihren Blob Storage-Endpunkt aufzuheben, verwenden Sie eines der folgenden Verfahren.

### <a name="azure-cli-20"></a>Azure CLI 2.0

Verwenden Sie den CLI-Befehl [az storage account update](https://docs.microsoft.com/cli/azure/storage/account#update), und geben Sie eine leere Zeichenfolge (`""`) als Wert für das `--custom-domain`-Argument an, um die Registrierung der benutzerdefinierten Domäne zu entfernen.

* Befehlsformat:

  ```azurecli
  az storage account update \
      --name <storage-account-name> \
      --resource-group <resource-group-name> \
      --custom-domain ""
  ```

* Befehlsbeispiel:

  ```azurecli
  az storage account update \
      --name mystorageaccount \
      --resource-group myresourcegroup \
      --custom-domain ""
  ```

### <a name="powershell"></a>PowerShell

Verwenden Sie das PowerShell-Cmdlet [Set-AzureRmStorageAccount](/powershell/module/azurerm.storage/set-azurermstorageaccount), und geben Sie eine leere Zeichenfolge (`""`) als Wert für das `-CustomDomainName`-Argument an, um die Registrierung der benutzerdefinierten Domäne zu entfernen.

* Befehlsformat:
  
  ```powershell
  Set-AzureRmStorageAccount `
      -ResourceGroupName "<resource-group-name>" `
      -AccountName "<storage-account-name>" `
      -CustomDomainName ""
  ```

* Befehlsbeispiel:

  ```powershell
  Set-AzureRmStorageAccount `
      -ResourceGroupName "myresourcegroup" `
      -AccountName "mystorageaccount" `
      -CustomDomainName ""
  ```

### <a name="azure-portal"></a>Azure-Portal

Derzeit ist es nicht möglich, über das Azure-Portal eine benutzerdefinierte Domänenregistrierung zu entfernten. Dies ist ein bekanntes Problem. Derzeit gibt es kein Datum für die Lösung dieses Problems. Dieser Artikel wird aber aktualisiert, sobald das Problem gelöst wurde. Verwenden Sie in der Zwischenzeit bitte Azure CLI 2.0 oder Azure PowerShell, um die benutzerdefinierte Domäneneinstellung zu entfernen.

## <a name="next-steps"></a>Nächste Schritte
* [Zuordnen von CDN-Inhalt (Content Delivery Network) zu einer benutzerdefinierten Domäne](../cdn/cdn-map-content-to-custom-domain.md)


