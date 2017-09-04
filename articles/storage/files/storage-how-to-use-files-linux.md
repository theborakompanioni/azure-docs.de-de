---
title: Verwenden von Azure File Storage mit Linux | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie unter Linux eine Azure-Dateifreigabe über SMB einbinden."
services: storage
documentationcenter: na
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 6edc37ce-698f-4d50-8fc1-591ad456175d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/8/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: 646886ad82d47162a62835e343fcaa7dadfaa311
ms.openlocfilehash: d8987082c559a374b8d19fd69e20cf5e81cb25ef
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="use-azure-file-storage-with-linux"></a>Verwenden von Azure File Storage unter Linux
[Azure File Storage](../storage-dotnet-how-to-use-files.md) ist das benutzerfreundliche Clouddateisystem von Microsoft. Azure-Dateifreigaben können in Linux-Distributionen mithilfe des Pakets [cifs-utils](https://wiki.samba.org/index.php/LinuxCIFS_utils) aus dem [Samba-Projekt](https://www.samba.org/) eingebunden werden. Dieser Artikel veranschaulicht zwei Möglichkeiten zum Einbinden einer Azure-Dateifreigabe: bei Bedarf mit dem Befehl `mount` oder beim Start durch Erstellen eines Eintrags in `/etc/fstab`.

> [!NOTE]  
> Wenn Sie eine Azure-Dateifreigabe außerhalb der Azure-Region einbinden möchten, in der sie gehostet wird (beispielsweise lokal oder in einer anderen Azure-Region), muss das Betriebssystem die Verschlüsselungsfunktionen von SMB 3.0 unterstützen. Die Verschlüsselungsfunktionen für SMB 3.0 für Linux wurden im Kernel 4.11 eingeführt. Diese Funktionen ermöglichen das Einbinden einer Azure-Dateifreigabe aus einer lokalen oder anderen Azure-Region. Zum Zeitpunkt der Veröffentlichung wurden diese Funktionen zu Ubuntu 16.04 und höher zurückportiert.


## <a name="prerequisities-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package"></a>Voraussetzungen für das Einbinden einer Azure-Dateifreigabe in Linux und das Paket „cifs-utils“
* **Wählen Sie eine Linux-Distribution, in die das Paket „cifs-utils“ installiert werden kann**: Microsoft empfiehlt die folgenden Linux-Distributionen im Katalog mit den Azure-Images:

    * Ubuntu Server 14.04+
    * RHEL 7+
    * CentOS 7+
    * Debian 8
    * openSUSE 13.2+
    * SUSE Linux Enterprise Server 12

* <a id="install-cifs-utils"></a>**Das Paket „cifs-utils“ ist installiert**: Das Paket „cifs-utils“ kann mithilfe des Paket-Managers für die Linux-Distribution Ihre Wahl installiert werden. 

    Verwenden Sie bei auf **Ubuntu** und **Debian** basierenden Distributionen den Paket-Manager `apt-get`:

    ```
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    Verwenden Sie unter **RHEL** und **CentOS**den Paket-Manager `yum`:

    ```
    sudo yum install samba-client samba-common cifs-utils
    ```

    Verwenden Sie unter **OpenSUSE** den Paket-Manager `zypper`:

    ```
    sudo zypper install samba*
    ```

    Verwenden Sie bei anderen Distributionen den entsprechenden Paket-Manager, oder [kompilieren Sie den Quellcode](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).

* **Legen Sie die Verzeichnis-/Dateiberechtigungen der eingebundenen Freigabe fest**: In den folgenden Beispielen verwenden wir 0777, um allen Benutzern die Berechtigungen „Lesen“, „Schreiben“ und „Ausführen“ zu erteilen. Sie können dies nach Wunsch durch andere [chmod-Berechtigungen](https://en.wikipedia.org/wiki/Chmod) ersetzen. 

* **Name des Speicherkontos:** Zum Einbinden einer Azure-Dateifreigabe benötigen Sie den Namen des Speicherkontos.

* **Speicherkontoschlüssel:** Zum Einbinden einer Azure-Dateifreigabe benötigen Sie den primären (oder sekundären) Speicherschlüssel. SAS-Schlüssel können derzeit nicht zum Einbinden verwendet werden.

* **Prüfen Sie, ob Port 445 geöffnet ist**: SMB kommuniziert über den TCP-Port 445. Vergewissern Sie sich, dass der TCP-Port 445 des Clientcomputers nicht durch die Firewall blockiert wird.

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a>Einbinden der Azure-Dateifreigabe bei Bedarf mit `mount`
1. **[Installieren Sie das Paket „cifs-utils“ für Ihre Linux-Distribution](#install-cifs-utils)**.

2. **Erstellen Sie einen Ordner für den Einbindungspunkt**: Dies ist an einer beliebigen Stelle im Dateisystem möglich.

    ```
    mkdir mymountpoint
    ```

3. **Verwenden Sie den Befehl „mount“ zum Einbinden der Azure-Dateifreigabe.** Ersetzen Sie dabei `<storage-account-name>`, `<share-name>` und `<storage-account-key>` durch die entsprechenden Angaben.

    ```
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> ./mymountpoint -o vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> Wenn Sie die Azure-Dateifreigabe nicht mehr benötigen, können Sie `sudo umount ./mymountpoint` verwenden, um die Einbindung der Freigabe aufzuheben.

## <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a>Erstellen eines dauerhaften Einbindungspunkts für die Azure-Dateifreigabe mit `/etc/fstab`
1. **[Installieren Sie das Paket „cifs-utils“ für Ihre Linux-Distribution](#install-cifs-utils)**.

2. **Erstellen Sie einen Ordner für den Einbindungspunkt**: Dies ist an einer beliebigen Stelle im Dateisystem möglich, aber Sie müssen den absoluten Pfad des Ordners notieren. Im folgenden Beispiel wird ein Ordner unter „root“ erstellt.

    ```
    sudo mkdir /mymountpoint
    ```

3. **Verwenden Sie den folgenden Befehl zum Anfügen der folgenden Zeile an `/etc/fstab`**: Ersetzen Sie dabei `<storage-account-name>`, `<share-name>` und `<storage-account-key>` durch die entsprechenden Angaben.

    ```
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> Mithilfe von `sudo mount -a` können Sie die Azure-Dateifreigabe nach Bearbeitung von `/etc/fstab` einbinden, anstatt neu zu starten.

## <a name="feedback"></a>Feedback
Linux-Benutzer, wir möchten von Ihnen hören!

Der Gruppe "Azure File Storage for Linux users" bietet ein Forum, in dem Sie Ihr Feedback zu File Storage für Linux geben können. Senden Sie eine E-Mail an [Azure File Storage for Linux Users](mailto:azurefileslinuxusers@microsoft.com), um der Benutzergruppe beizutreten.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Azure-Dateispeicher erhalten Sie über diese Links.
* [Referenz zur REST-API des Dateidiensts](http://msdn.microsoft.com/library/azure/dn167006.aspx)
* [Verwenden von AzCopy mit Microsoft Azure Storage](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Verwenden der Azure CLI mit Azure Storage](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)
* [Häufig gestellte Fragen](../storage-files-faq.md)
* [Problembehandlung](storage-troubleshoot-linux-file-connection-problems.md)

