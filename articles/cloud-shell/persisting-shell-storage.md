---
title: Beibehalten von Dateien in Azure Cloud Shell (Vorschauversion) | Microsoft-Dokumentation
description: "Exemplarische Vorgehensweise für das Beibehalten von Dateien in Azure Cloud Shell."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: juluk
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 61a8bfcf3704f361432400771d8fcc8b81927b53
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="persist-files-in-azure-cloud-shell"></a>Beibehalten von Dateien in Azure Cloud Shell
Cloud Shell nutzt Azure-Dateispeicher, um Dateien sitzungsübergreifend beizubehalten.

## <a name="set-up-a-clouddrive-file-share"></a>Einrichten einer `clouddrive`-Dateifreigabe
Beim ersten Start fordert Cloud Shell Sie auf, eine neue oder vorhandene Dateifreigabe zu verknüpfen, um Dateien sitzungsübergreifend beizubehalten.

### <a name="create-new-storage"></a>Erstellen von neuem Speicher

Wenn Sie die grundlegenden Einstellungen verwenden und nur ein Abonnement auswählen, werden von Cloud Shell in der für Sie unterstützten nächstgelegenen Region drei Ressourcen erstellt:
* Ressourcengruppe: `cloud-shell-storage-<region>`
* Speicherkonto: `cs<uniqueGuid>`
* Dateifreigabe: `cs-<user>-<domain>-com-<uniqueGuid>`

![Einstellung „Abonnement“](media/basic-storage.png)

Die Dateifreigabe wird in Ihrem Verzeichnis `$Home` als `clouddrive` bereitgestellt. Die Dateifreigabe wird auch zum Speichern eines 5 GB großen Image verwendet, das für Sie erstellt wird und mit dem Ihr Verzeichnis `$Home` automatisch aktualisiert und beibehalten wird. Dies ist eine einmalige Aktion, und die Dateifreigabe wird in den nachfolgenden Sitzungen automatisch bereitgestellt.

### <a name="use-existing-resources"></a>Verwenden vorhandener Ressourcen

Sie können vorhandene Ressourcen zuordnen, indem Sie die erweiterte Option verwenden. Wählen Sie bei der Anzeige mit der Aufforderung zum Einrichten des Speichers die Option **Erweiterte Einstellungen anzeigen**, um weitere Optionen anzuzeigen. Für vorhandene Dateifreigaben wird ein 5-GB-Benutzerimage bereitgestellt, mit dem Ihr Verzeichnis `$Home` beibehalten wird. Die Dropdownmenüs werden für Ihre Cloud Shell-Region und die Konten für den lokal redundanten und georedundanten Speicher gefiltert.

![Einstellung „Ressourcengruppe“](media/advanced-storage.png)

### <a name="restrict-resource-creation-with-an-azure-resource-policy"></a>Beschränken der Ressourcenerstellung mit einer Azure-Ressourcenrichtlinie
Speicherkonten, die Sie in Cloud Shell erstellen, erhalten das Tag `ms-resource-usage:azure-cloud-shell`. Wenn Sie nicht möchten, dass Benutzer Speicherkonten in Cloud Shell erstellen, können Sie eine [Azure-Ressourcenrichtlinie für Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags) erstellen, die durch das jeweilige Tag ausgelöst werden.

## <a name="how-cloud-shell-works"></a>Funktionsweise von Cloud Shell
Cloud Shell nutzt die beiden folgenden Methoden zum Beibehalten von Dateien:
* Es wird ein Datenträgerimage Ihres Verzeichnisses `$Home` erstellt, um alle Inhalte im Verzeichnis beizubehalten. Das Datenträgerimage wird in der von Ihnen angegebenen Dateifreigabe als `acc_<User>.img` unter `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img` gespeichert, und die Änderungen werden automatisch synchronisiert.

* Ihre angegebene Dateifreigabe wird zur direkten Interaktion mit der Freigabe als `clouddrive` in Ihr Verzeichnis `$Home` eingebunden. `/Home/<User>/clouddrive` wird `fileshare.storage.windows.net/fileshare` zugeordnet.
 
> [!NOTE]
> Alle Dateien in Ihrem Verzeichnis `$Home`, z.B. SSH-Schlüssel, werden dauerhaft in Ihrem Benutzerdatenträgerimage beibehalten, das in der eingebundenen Dateifreigabe gespeichert ist. Nutzen Sie beim dauerhaften Speichern von Informationen in Ihrem Verzeichnis `$Home` und auf der eingebundenen Dateifreigabe die bewährten Methoden.

## <a name="use-the-clouddrive-command"></a>Verwenden des Befehls `clouddrive`
Mit Cloud Shell können Sie den Befehl `clouddrive` ausführen, mit dem Sie die in Cloud Shell bereitgestellte Dateifreigabe manuell aktivieren können.
![Ausführen des Befehls „clouddrive“](media/clouddrive-h.png)

## <a name="mount-a-new-clouddrive"></a>Bereitstellen einer neuen `clouddrive`-Instanz

### <a name="prerequisites-for-manual-mounting"></a>Voraussetzungen für die manuelle Bereitstellung
Sie können die Dateifreigabe aktualisieren, die Cloud Shell zugeordnet ist, indem Sie den Befehl `clouddrive mount` verwenden.

Wenn Sie eine vorhandene Dateifreigabe bereitstellen, müssen die Speicherkonten folgende Voraussetzungen erfüllen:
* Es muss sich um lokal redundanten Speicher oder georedundanten Speicher zur Unterstützung von Dateifreigaben handeln.
* Das Speicherkonto muss sich in Ihrer zugewiesenen Region befinden. Beim Onboarding wird die Region, der Sie zugewiesen sind, in der Ressourcengruppe mit dem Namen `cloud-shell-storage-<region>` aufgelistet.

### <a name="supported-storage-regions"></a>Unterstützte Speicherregionen
Die Azure-Dateien müssen sich in derselben Region wie der Cloud Shell-Computer befinden, auf dem Sie diese bereitstellen. Cloud Shell-Cluster sind derzeit in folgenden Regionen vorhanden:
|Bereich|Region|
|---|---|
|Amerika|USA, Osten; USA, Süden-Mitte; USA, Westen|
|Europa|„Europa, Norden“, „Europa, Westen“|
|Asien-Pazifik|Indien, Mitte; Asien, Südosten|

### <a name="the-clouddrive-mount-command"></a>Befehl `clouddrive mount`

> [!NOTE]
> Wenn Sie eine neue Dateifreigabe bereitstellen, wird für Ihr Verzeichnis `$Home` ein neues Benutzerimage erstellt, da Ihr vorheriges `$Home`-Image auf der vorherigen Dateifreigabe vorgehalten wird.

Führen Sie den Befehl `clouddrive mount` mit den folgenden Parametern aus:

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

Führen Sie `clouddrive mount -h` wie hier gezeigt aus, um weitere Details anzuzeigen:

![Ausführen des Befehls „clouddrive mount“](media/mount-h.png)

## <a name="unmount-clouddrive"></a>Aufheben der Bereitstellung von `clouddrive`
Sie können die Bereitstellung einer Dateifreigabe in Cloud Shell jederzeit aufheben. Sobald die Bereitstellung der Dateifreigabe aufgehoben ist, werden Sie aufgefordert, vor der nächsten Sitzung eine neue Dateifreigabe bereitzustellen.

So entfernen Sie eine Dateifreigabe aus Cloud Shell:
1. Führen Sie `clouddrive unmount`aus.
2. Lesen und bestätigen Sie die Eingabeaufforderungen.

Ihre Dateifreigabe ist weiterhin vorhanden, sofern Sie sie nicht manuell löschen. Cloud Shell sucht in nachfolgenden Sitzungen nicht mehr nach dieser Dateifreigabe.

Führen Sie `clouddrive unmount -h` wie hier gezeigt aus, um weitere Details anzuzeigen:

![Ausführen des Befehls „clouddrive unmount“](media/unmount-h.png)

> [!WARNING]
> Durch das Ausführen dieses Befehls werden keine Ressourcen gelöscht. Wenn Sie die Ressourcengruppe, das Speicherkonto oder die Dateifreigabe, die bzw. das Cloud Shell zugeordnet ist, manuell löschen, werden das Image des `$Home`-Verzeichnisses und alle anderen Dateien in der Dateifreigabe gelöscht. Diese Aktion kann nicht rückgängig gemacht werden.

## <a name="list-clouddrive-file-shares"></a>Auflisten von `clouddrive`-Dateifreigaben
Führen Sie den folgenden `df`-Befehl aus, um zu ermitteln, welche Dateifreigabe als `clouddrive` bereitgestellt wird. 

Der Dateipfad zu „clouddrive“ zeigt den Namen Ihres Speicherkontos und die Dateifreigabe in der URL. Beispiel: `//storageaccountname.file.core.windows.net/filesharename`

```
justin@Azure:~$ df
Filesystem                                               1K-blocks     Used Available Use% Mounted on
overlay                                                   30428648 15585636  14826628  52% /
tmpfs                                                       986704        0    986704   0% /dev
tmpfs                                                       986704        0    986704   0% /sys/fs/cgroup
/dev/sda1                                                 30428648 15585636  14826628  52% /etc/hosts
shm                                                          65536        0     65536   0% /dev/shm
//mystoragename.file.core.windows.net/fileshareName        6291456  5242944   1048512  84% /usr/justin/clouddrive
/dev/loop0                                                 5160576   601652   4296780  13% /home/justin
```

## <a name="transfer-local-files-to-cloud-shell"></a>Übertragen Sie lokale Dateien auf Cloud Shell
Das Verzeichnis `clouddrive` wird mit dem Blatt „Storage“ im Azure-Portal synchronisiert. Verwenden Sie dieses Blatt, um lokale Dateien in Ihre oder aus Ihrer Dateifreigabe zu übertragen. Die Aktualisierung von Dateien aus Cloud Shell wird auf der GUI für die Dateispeicherung widergespiegelt, wenn Sie das Blatt aktualisieren.

### <a name="download-files"></a>Herunterladen von Dateien

![Auflisten von lokalen Dateien](media/download.png)
1. Navigieren Sie im Azure-Portal zur bereitgestellten Dateifreigabe.
2. Wählen Sie die Zieldatei aus.
3. Wählen Sie die Schaltfläche **Herunterladen**.

### <a name="upload-files"></a>Hochladen von Dateien

![Hochzuladende lokale Dateien](media/upload.png)
1. Navigieren Sie zu Ihrer bereitgestellten Dateifreigabe.
2. Wählen Sie die Schaltfläche **Hochladen**.
3. Wählen Sie die Dateien aus, die Sie hochladen möchten.
4. Bestätigen Sie den Upload.

Nun sollten Sie die Dateien angezeigt werden, auf die Sie in Ihrem Verzeichnis `clouddrive` in Cloud Shell zugreifen können.

## <a name="next-steps"></a>Nächste Schritte
[Cloud Shell-Schnellstart](quickstart.md) <br>
[Erfahren Sie mehr über Azure File Storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage). <br>
[Weitere Informationen zu Speichertags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>

