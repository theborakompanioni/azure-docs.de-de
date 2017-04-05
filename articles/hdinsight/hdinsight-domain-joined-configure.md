---
title: "Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über das Einrichten und Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 0cbb49cc-0de1-4a1a-b658-99897caf827c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: saurinsh
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 424ee513afce6ab689c8804594754b1b49234754
ms.lasthandoff: 03/25/2017


---
# <a name="configure-domain-joined-hdinsight-clusters-preview"></a>Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern (Vorschau)
Erfahren Sie, wie man Azure HDInsight-Cluster mit Azure Active Directory (Azure AD) und [Apache Ranger](http://hortonworks.com/apache/ranger/) einrichtet, und profitieren Sie von strengen Richtlinien für Authentifizierung und Rollenbasierte Zugriffssteuerung (role-based access control, RBAC).  In die Domäne eingebundenes HDInsight kann nur unter Linux-basierten Clustern konfiguriert werden. Weitere Informationen finden Sie unter [Introduce Domain-joined HDInsight clusters (Einführung in HDInsight-Cluster mit Domänenverknüpfung)](hdinsight-domain-joined-introduction.md).

Dieser Artikel ist das erste Tutorial einer Reihe:

* Erstellen eines HDInsight-Clusters mit Verbindung zu Azure AD (über die Eigenschaft „Azure Directory Domain Services“) mit Apache Ranger aktiviert.
* Erstellen und Anwenden von Hive-Richtlinien durch Apache Ranger, und Erteilen der Erlaubnis an Nutzer (z.B. Datenanalysten), sich mithilfe von ODBC-basierten Tools, z.B. Excel, Tableau usw. mit Hive zu verbinden. Microsoft arbeitet daran, schon bald andere Workloads, wie z.B. HBase, Spark und Storm zu in HDInsight mit Domänenverknüpfung hinzuzufügen.

Ein Beispiel für die fertige Topologie sehen Sie hier:

![In die Domäne eingebundene HDInsight-Topologie](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-topology.png)

Azure AD unterstützt zur Zeit nur klassische virtuelle Netzwerke (VNets), und Linux-basierte HDInsight-Cluster nur Azure Resource Manager-basierte VNets. Daher erfordert die Integration von HDInsight und Azure AD zwei VNets und Peering zwischen ihnen. Informationen über den Vergleich der beiden Bereitstellungsmodelle finden Sie unter [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen](../azure-resource-manager/resource-manager-deployment-model.md). Die beiden VNets müssen sich in der gleichen Region wie die Azure AD DS befinden.

Namen für Azure-Dienste müssen global eindeutig sein. In diesem Tutorial werden die folgenden Namen verwendet. Contoso ist ein fiktiver Name. Sie müssen *contoso* durch einen anderen Namen ersetzen, wenn Sie das Tutorial durcharbeiten. 

**Namen:**

| Eigenschaft | Wert |
| --- | --- |
| Azure AD VNet |contosoaadvnet |
| Azure AD VNet-Ressourcengruppe |contosoaadrg |
| Virtueller Azure AD-Computer (VM) |contosoaadadmin. Dieser virtuelle Computer wird verwendet, um die Organisationseinheit und die Reverse-DNS-Zone zu konfigurieren. |
| Azure AD-Verzeichnis |contosoaaddirectory |
| Name Ihrer Azure AD-Domäne |contoso (contoso.onmicrosoft.com) |
| HDInsight VNet |contosohdivnet |
| HDInsight VNet-Ressourcengruppe |contosohdirg |
| HDInsight-Cluster |contosohdicluster |

In diesem Tutorial finden Sie die Schritte zum Konfigurieren eines in die Domäne eingebundenen HDInsight-Clusters. Jeder Abschnitt enthält Links zu anderen Artikeln mit weiteren Hintergrundinformationen.

## <a name="prerequisite"></a>Voraussetzung:
* Machen Sie sich mit [Azure AD-Domänendiensten](https://azure.microsoft.com/services/active-directory-ds/) und deren [Preis](https://azure.microsoft.com/pricing/details/active-directory-ds/)-Struktur vertraut.
* Stellen Sie sicher, dass sich Ihre Abonnement auf der Zulassungsliste befindet. Dazu können Sie eine E-Mail mit Ihrer Abonnement-ID an hdipreview@microsoft.com senden.
* Ein SSL-Zertifikat, das von einer Signaturstelle für Ihre Domäne signiert ist. Es wird ein Zertifikat für sicheres LDAP benötigt. Selbstsignierte Zertifikate können nicht verwendet werden.

## <a name="procedures"></a>Prozeduren
1. Erstellen Sie ein klassisches Azure-VNet für Azure AD.  
2. Erstellen und konfigurieren Sie Azure AD und Azure AD DS.
3. Fügen Sie einen virtuellen Computer zum klassischen VNet hinzu, um eine Organisationseinheit zu erstellen. 
4. Erstellen Sie eine Organisationseinheit für Azure AD DS.
5. Erstellen Sie ein HDInsight-VNet im Ressourcenverwaltungsmodus von Azure.
6. Richten Sie Reverse-DNS-Zonen für Azure AD DS ein.
7. Führen Sie das Peering der beiden VNets durch.
8. Erstellen Sie ein HDInsight-Cluster.

> [!NOTE]
> In diesem Lernprogramm wird davon ausgegangen, dass Sie noch nicht über Azure AD verfügen. Wenn Sie bereits darüber verfügen, können Sie diesen Teil in Schritt 2 überspringen.
> 
> 

Es gibt ein PowerShell-Skript, mit dem die Schritte 3 bis 7 automatisiert werden können.  Weitere Informationen finden Sie unter [Configure Domain-joined HDInsight clusters use Azure PowerShell](hdinsight-domain-joined-configure-use-powershell.md) (Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit Azure PowerShell).

## <a name="create-an-azure-classic-vnet"></a>Erstellen Sie ein klassisches Azure-VNet
In diesem Abschnitt erstellen Sie ein klassisches VNet mithilfe des Azure-Portals. Im nächsten Abschnitt aktivieren Sie Azure AD DS für Azure AD in diesem klassischen VNet. Weitere Informationen über die folgende Vorgehensweise und über das Verwenden anderer Methoden zum Erstellen eines VNet finden Sie unter [Erstellen eines virtuellen Netzwerks (klassisch) über das Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-portal.md).

**Um ein klassisches VNet zu erstellen**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an. 
2. Klicken Sie auf **Neu** > **Netzwerk** > **Virtuelles Netzwerk**.
3. Wählen Sie unter **Bereitstellungsmodell auswählen** die Option **Klassisch** aus, und klicken Sie dann auf **Erstellen**.
4. Geben Sie folgende Werte ein bzw. wählen diese aus:
   
   * **Name**: contosoaadvnet
   * **Adressraum**: 10.1.0.0/16
   * **Subnetzname**: Subnet1
   * **Subnetzadressbereich**: 10.1.0.0/24
   * **Abonnement** : (Wählen Sie ein Abonnement aus, das für die Erstellung dieses VNet verwendet wird.)
   * **ResourceGroup**: contosoaadrg
   * **Standort**: (Wählen Sie eine Region für Ihren HDInsight-Cluster.)
     
     > [!IMPORTANT]
     > Sie müssen einen Speicherort auswählen, der Azure AD DS unterstützt. Weitere Informationen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/en-us/regions/services/). 
     > 
     > Das klassische VNet und die Ressourcengruppe müssen sich in der gleichen Region befinden wie Azure AD DS.
     > 
     > 
5. Klicken Sie auf **Erstellen**, um das VNet zu erstellen.

## <a name="create-and-configure-azure-ad-ds-for-your-azure-ad"></a>Erstellen und Konfigurieren von Azure AD DS für Azure AD
In diesem Abschnitt werden Sie:

1. Azure AD erstellen.
2. Azure AD-Benutzer erstellen. Diese Benutzer sind Domänenbenutzer. Sie verwenden den ersten Benutzer, um den HDInsight-Cluster mit Azure AD zu erstellen.  Die anderen beiden Benutzer sind in diesem Tutorial optional. Sie werden in [Configure Hive policies for Domain-joined HDInsight clusters (Konfigurieren von Hive-Richtlinien für in die Domäne eingebundene HDInsight-Cluster)](hdinsight-domain-joined-run-hive.md) verwendet, wenn Sie Apache Ranger-Richtlinien erstellen.
3. Eine AAD DC Administratorengruppe erstellen und den Azure AD-Benutzer zur Gruppe hinzufügen. Sie verwenden diesen Benutzer zum Erstellen der Organisationseinheit.
4. Azure AD Domain Services (Azure AD DS) für Azure AD Aktivieren.
5. LDAPS für Azure AD konfigurieren. Das Lightweight Directory Access Protocol (LDAP) wird zum Lesen und Schreiben in Azure AD verwendet.

Wenn Sie lieber ein vorhandenes Azure AD verwenden möchten, können Sie die Schritte 1 und 2 überspringen.

**Erstellen von Azure AD**

1. Öffnen Sie das [klassische Azure-Portal](https://manage.windowsazure.com), klicken Sie auf **Neu** > **App Services** > **Active Directory** > **Directory** > **Benutzerdefiniert erstellen**. 
2. Geben Sie folgende Werte ein bzw. wählen diese aus:
   
   * **Name**: contosoaaddirectory
   * **Domänenname**: contoso.  Dieser Name muss global eindeutig sein.
   * **Land oder Region**: Wählen Sie Ihr Land oder Ihre Region aus.
3. Klicken Sie auf **Fertig stellen**.

**Einen Azure AD-Benutzer erstellen**

1. Öffnen Sie das [klassische Azure-Portal](https://manage.windowsazure.com), und klicken Sie auf **Active Directory** -> **contosoaaddirectory**. 
2. Klicken Sie im Menü am oberen Rand auf **Benutzer** .
3. Klicken Sie auf **Benutzer hinzufügen**.
4. Geben Sie den **Benutzername** ein, und klicken Sie dann auf **Weiter**. 
5. Konfigurieren Sie das Benutzerprofil. Wählen Sie **Globaler Administrator** in **Rolle**, und klicken Sie dann auf **Weiter**.  Die Rolle „Globaler Administrator“ ist notwendig, um Organisationseinheiten zu erstellen.
6. Klicken Sie auf **Erstellen**, um ein temporäres Kennwort zu erhalten.
7. Erstellen Sie eine Kopie des Kennworts, und klicken Sie dann auf **Fertig stellen**. Im weiteren Verlauf dieses Tutorials werden Sie diesen Benutzer (den globalen Administrator) verwenden, um sich auf dem virtuellen Admin-Computer anzumelden, damit Sie eine Organisationseinheit erstellen und Reverse-DNS konfigurieren können.

Verfahren Sie auf das gleiche Verfahren, um zwei weitere Benutzer mit der Rolle **Benutzer** hinzuzufügen: hiveuser1 und hiveuser2. Die folgenden Benutzer werden in [Configure Hive policies for Domain-joined HDInsight clusters (Konfigurieren von Hive-Richtlinien für in die Domäne eingebundene HDInsight-Cluster)](hdinsight-domain-joined-run-hive.md) verwendet.

**Erstellen einer AAD DC-Administratorengruppe und Hinzufügen eines Azure AD-Benutzers**

1. Öffnen Sie das [klassische Azure-Portal](https://manage.windowsazure.com), und klicken Sie auf **Active Directory** > **contosoaaddirectory**. 
2. Klicken Sie im Menü am oberen Rand auf **Gruppen**.
3. Klicken Sie auf **Gruppe hinzufügen** oder **Gruppen hinzufügen**.
4. Geben Sie folgende Werte ein bzw. wählen diese aus:
   
   * **Name**: AAD DC Administrators.  Ändern Sie nicht den Gruppennamen.
   * **Gruppentyp**: Sicherheit.
5. Klicken Sie auf **Fertig stellen**.
6. Klicken Sie auf **AAD DC Administrators** um die Gruppe zu öffnen.
7. Klicken Sie auf **Mitglieder hinzufügen**.
8. Wählen Sie den im vorherigen Schritt erstellten Benutzer aus, und klicken Sie auf **Fertig stellen**.
9. Führen Sie die gleichen Schritte erneut aus, um eine weitere Gruppe mit dem Namen **HiveUsers** zu erstellen, und fügen Sie die beiden Hive-Benutzer zur Gruppe hinzu.

Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD Domain Services – Erstellen der Administratorengruppe für Azure AD-Domänencontroller](../active-directory-domain-services/active-directory-ds-getting-started.md).

**Aktivieren von Azure AD DS für Azure AD**

1. Öffnen Sie das [klassische Azure-Portal](https://manage.windowsazure.com), und klicken Sie auf **Active Directory** > **contosoaaddirectory**. 
2. Klicken Sie im oberen Menü auf **Konfigurieren**.
3. Scrollen Sie zu **Domain Services** herunter und legen Sie die folgenden Werte fest:
   
   * **Domänendienste für dieses Verzeichnis aktivieren**: Ja.
   * **DNS-Domänennamen der Domänendienste**: Zeigt den standardmäßigen DNS-Namen des Azure-Verzeichnis an. Zum Beispiel contoso.onmicrosoft.com.
   * **Domänendienste mit diesem virtuellen Netzwerk verbinden**: Wählen Sie das klassische virtuelle Netzwerk aus, das Sie erstellt haben, d.h. **contosoaadvnet**.
4. Klicken Sie unten auf der Seite auf **Speichern**. Neben **Domänendienste für dieses Verzeichnis aktivieren** erscheint **Pending...**.  
5. Warten Sie, bis **Ausstehend...** ausgeblendet wurde und **IP-Adresse** aufgefüllt wird. Zwei IP-Adressen werden aufgefüllt. Dies sind die IP-Adressen der Domänencontroller, die von den Domänendiensten bereitgestellt werden. Jede IP-Adresse wird angezeigt, sobald der entsprechende Domänencontroller bereitgestellt wurde und bereit ist. Notieren Sie sich die beiden IP-Adressen. Diese werden später benötigt werden.

Weitere Informationen finden Sie unter [Azure AD Domänendienste – Aktivieren von Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-enableaadds.md).

**Synchronisieren Ihres Kennworts**

Wenn Sie Ihre eigene Domäne verwenden, müssen Sie das Kennwort synchronisieren. Weitere Informationen finden Sie unter [Aktivieren der Kennwortsynchronisierung für Azure AD-Domänendienste](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md).

**Konfigurieren von LDAPS für Azure AD**

1. Rufen Sie für Ihre Domäne ein SSL-Zertifikat ab, das von einer Signierstelle signiert wurde. Selbstsignierte Zertifikate können nicht verwendet werden. Wenn Sie kein SSL-Zertifikat abrufen können, wenden Sie sich für eine Ausnahme an hdipreview@microsoft.com.
2. Öffnen Sie das [klassische Azure-Portal](https://manage.windowsazure.com), und klicken Sie auf **Active Directory** > **contosoaaddirectory**. 
3. Klicken Sie im oberen Menü auf **Konfigurieren**.
4. Scrollen Sie zu **domain services** herunter.
5. Klicken Sie auf **Zertifikat konfigurieren**.
6. Folgen Sie den Anweisungen, um die Zertifikatsdatei und das Kennwort anzugeben. Neben **Domänendienste für dieses Verzeichnis aktivieren** erscheint **Pending...**.  
7. Warten Sie, bis **Ausstehend...** ausgeblendet und **sicheres LDAP-Zertifikat** aufgefüllt ist.  Das kann 10 Minuten oder mehr dauern.

> [!NOTE]
> Wenn gerade Hintergrundaufgaben unter Azure AD DS ausgeführt werden, erhalten Sie möglicherweise eine Fehlermeldung beim Hochladen des Zertifikats – <i>There is an operation being performed for this tenant. Please try again later (Für diesen Mandanten wird gerade eine Operation ausgeführt.Versuchen Sie es später erneut)</i>.  Für den Fall, dass dieser Fehler auftritt, versuchen Sie es bitte später noch einmal. Bei der zweiten Domänencontroller-IP kann es bis zu 3 Stunden dauern, bis diese bereitgestellt wird.
> 
> 

Weitere Informationen finden Sie unter [Konfigurieren von sicherem LDAP (LDAPS) für eine durch Azure AD Domain Services verwaltete Domäne](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="configure-an-organizational-unit-and-reverse-dns"></a>Konfigurieren von Organisationseinheiten und Reverse-DNS
In diesem Abschnitt fügen Sie einen virtuellen Computer zum Azure AD VNet hinzu, und installieren Verwaltungstools auf diesem virtuellen Computer, damit Sie eine Organisationseinheit und Reverse-DNS konfigurieren können. Reverse-DNS-Suche ist für die Kerberos-Authentifizierung erforderlich.

**Verbinden des virtuellen Computers mit einem virtuellen Netzwerk.**

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) auf **Neu** > **Compute** > **Virtueller Computer** > **Aus Katalog**.
2. Wählen Sie ein Image aus, und klicken Sie dann auf **Weiter**.  Wenn Sie nicht wissen, welches Sie benutzen müssen, wählen Sie das Standardimage aus, **Windows Server 2012 R2 Datacenter**.
3. Geben Sie folgende Werte ein bzw. wählen diese aus:
   
   * Name des virtuellen Computers: **contosoaadadmin**
   * Ebene: **Basic**
   * Neuer Benutzername: (Geben Sie einen Benutzernamen ein)
   * Kennwort: (Geben Sie ein Kennwort ein)
     
     Bitte beachten Sie, dass der Benutzername und das Kennwort der lokale Administrator sind.
4. Klicken Sie auf **Weiter**
5. Wählen Sie in **Region/Virtuelles Netzwerk** das neue virtuelle Netzwerk, das Sie im letzten Schritt erstellt haben (contosoaadvnet), und klicken Sie dann auf **Weiter**.
6. Klicken Sie auf **Fertig stellen**.

**Herstellen einer RDP-Verbindung zum virtuellen Computer**

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) auf **Virtuelle Computer** > **contosoaadadmin**.
2. Klicken Sie im Hauptmenü auf **Dashboard** .
3. Klicken Sie unten auf der Seite auf **Verbinden**.
4. Führen Sie die Anweisung aus, und verwenden Sie den Benutzernamen und das Kennwort des lokalen Administrators und stellen Sie eine Verbindung her.

**Einbinden des Virtuellen Computers in die Azure AD-Domäne**

1. Klicken Sie in der RDP-Sitzung auf **Start**, und klicken Sie dann auf **Server-Manager**.
2. Klicken Sie im linken Menü auf **Lokaler Server**.
3. Klicken Sie in der Arbeitsgruppe auf **Workgroup**.
4. Klicken Sie auf **Ändern**.
5. Klicken Sie auf **Domäne**, geben Sie **contoso.onmicrosoft.com** ein, und klicken Sie dann auf **OK**.
6. Geben Sie Ihre Anmeldeinformationen des Domänenbenutzers ein, und klicken Sie auf **OK**.
7. Klicken Sie auf **OK**.
8. Klicken Sie auf **OK** und stimmen Sie einem Neustart des Computers zu.
9. Klicken Sie auf **Schließen**.
10. Klicken Sie auf **Jetzt neu starten**.

Weitere Informationen finden Sie unter [Einbinden eines virtuellen Windows Server-Computers in eine verwaltete Domäne](../active-directory-domain-services/active-directory-ds-admin-guide-join-windows-vm.md).

**Installieren von Active Directory-Verwaltungstools und DNS-Tools**

1. Stellen Sie mithilfe des Azure AD-Benutzerkontos eine RDP-Verbindung zu **contosoaadadmin** her.
2. Klicken Sie auf **Start**, und klicken Sie dann auf **Server-Manager**.
3. Klicken Sie im linken Menü auf **Dashboard** .
4. Klicken Sie auf **Verwalten**, und klicken Sie dann auf **Rollen und Features hinzufügen**.
5. Klicken Sie auf **Weiter**.
6. Wählen Sie **Rollenbasierte oder featurebasierte Installation** und klicken Sie auf **Weiter**.
7. Wählen Sie den aktuellen virtuellen Computer im Serverpool aus, und klicken Sie auf **Weiter**.
8. Klicken Sie auf **Weiter**, um Rollen zu überspringen.
9. Erweitern Sie **Remoteserver-Verwaltungstools**, erweitern Sie **Rollenverwaltungstools**, wählen Sie **AD DS und AD LDS-Tools** und **DNS-Servertools** aus, und klicken Sie dann auf **Weiter**. 
10. Klicken Sie auf **Weiter**
11. Klicken Sie auf **Installieren**.

Weitere Informationen finden Sie unter [Installieren von Active Directory-Verwaltungstools auf dem virtuellen Computer](../active-directory-domain-services/active-directory-ds-admin-guide-administer-domain.md#task-2---install-active-directory-administration-tools-on-the-virtual-machine).

**Konfigurieren von Reverse-DNS**

1. Stellen Sie mithilfe des Azure AD-Benutzerkontos eine RDP-Verbindung zu contosoaadadmin her.
2. Klicken Sie auf **Start**, klicken Sie auf **Verwaltungstools**, und klicken Sie dann auf **DNS**. 
3. Klicken Sie auf **Nein**, um das Hinzufügen von ContosoAADAdmin zu überspringen.
4. Wählen Sie **folgenden Computer**, geben Sie die IP-Adresse des ersten DNS-Servers ein, den Sie zuvor konfiguriert haben, und klicken Sie dann auf **OK**.  Sie sehen, dass Domänencontroller/DNS zum linken Bereich hinzugefügt wurde.
5. Erweitern Sie den Domänencontroller/DNS-Server, klicken Sie mit der rechten Maustaste auf **Reverse-Lookupzonen**, und klicken Sie dann auf **Neue Zone**. Der Assistent zum Erstellen neuer Zonen wird geöffnet.
6. Klicken Sie auf **Weiter**.
7. Wählen Sie **Primäre Zone** aus, und klicken Sie dann auf **Weiter**.
8. Wählen Sie **Auf allen DNS-Servern, die auf Domänencontrollern in dieser Domäne ausgeführt werden** aus, und klicken Sie auf **Weiter**.
9. Wählen Sie **IPv4 Reverse-Lookupzone** aus, und klicken Sie dann auf **Weiter**.
10. Geben Sie in **Netzwerk-ID** das Präfix für den Netzwerkbereich des HDInsight-VNet und klicken Sie auf **Weiter**. Im folgenden Abschnitt erstellen Sie das HDInsight-VNet.
11. Klicken Sie auf **Weiter**.
12. Klicken Sie auf **Weiter**.
13. Klicken Sie auf **Fertig stellen**.

Die Organisationseinheit, die Sie als nächstes erstellen, wird beim Erstellen des HDInsight-Clusters verwendet. Die Hadoop-Systembenutzer und Computerkonten werden in dieser OE platziert.

**Erstellen einer Organisationseinheit (Organizational Unit, OE) unter einer durch Azure AD-Domänendienste verwaltete Domäne**

1. Stellen Sie mithilfe des Domänenkontos in der Gruppe **AAD DC Administrators** eine RDP-Verbindung zu **contosoaadadmin** her.
2. Klicken Sie auf **Start**, klicken Sie auf **Verwaltungstools**, und klicken Sie dann auf **Active Directory-Verwaltungscenter**.
3. Klicken Sie im linken Bereich auf den Domänennamen. Beispiel: contoso.
4. Klicken Sie unter dem Domänennamen im Bereich **Aufgabe** unter dem Domänennamen auf **Neu**, und klicken Sie dann auf **Organisationseinheit**.
5. Geben Sie einen Namen ein, z.B. **HDInsightOU**, und klicken Sie dann auf **OK**. 

Weitere Informationen finden Sie unter [Erstellen einer Organisationseinheit (OE) in einer durch Azure AD Domain Services verwalteten Domäne](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md).

## <a name="create-a-resource-manager-vnet-for-hdinsight-cluster"></a>Erstellen Sie ein Resource Manager-VNet für HDInsight-Cluster
In diesem Abschnitt erstellen Sie ein Azure Ressource Manager-VNet, das für HDInsight-Cluster verwendet wird. Weitere Informationen zum Erstellen eines Azure-VNET mit anderen Methoden finden Sie unter [Erstellen eines virtuellen Netzwerks](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

Nachdem Sie das VNet erstellt haben, konfigurieren Sie das Resource Manager-VNET, um die gleichen DNS-Server zu verwenden, wie beim Azure AD-VNET. Wenn Sie die Schritte in diesem Tutorial befolgt haben, um das klassische VNet und Azure AD zu erstellen, sind die DNS-Server 10.1.0.4 und 10.1.0.5.

**Erstellen eines Resource Manager-VNET**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Neu**, **Netzwerk**, und dann auf **Virtuelles Netzwerk**. 
3. Wählen Sie unter **Bereitstellungsmodell auswählen** die Option **Resource Manager**, und klicken Sie auf **Erstellen**.
4. Geben Sie folgende Werte ein bzw. wählen diese aus:
   
   * **Name**: contosohdivnet
   * **Adressraum**: 10.2.0.0/16. Stellen Sie sicher, dass dieser Adressbereich nicht mit dem IP-Adressenbereich des klassischen VNet überlappt.
   * **Subnetzname**: Subnet1
   * **Subnetzadressbereich**: 10.2.0.0/24
   * **Abonnement**: (Wählen Sie Ihr Azure-Abonnement aus.)
   * **Ressourcengruppe**: contosohdirg
   * **Standort**: (Wählen Sie den gleichen Standort aus wie für das Azure AD-VNet, d.h. contosoaadvnet.)
5. Klicken Sie auf **Erstellen**.

**Konfigurieren von DNS für das Resource Manager-VNET**

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Weitere Dienste** -> **Virtuelle Netzwerke**. Achten Sie darauf, dass Sie nicht auf **Virtuelle Netzwerke (klassisch)** klicken.
2. Klicken Sie auf **contosohdivnet**.
3. Klicken Sie auf der linken Seite des neuen Blatts auf **DNS-Server**.
4. Klicken Sie auf **Benutzerdefiniert**, und geben Sie die folgenden Werte ein:
   
   * 10.1.0.4
   * 10.1.0.5
     
     Diese IP-Adressen der DNS-Server müssen mit den DNS-Servern im Azure AD-VNet (klassisches VNet) übereinstimmen.
5. Klicken Sie auf **Speichern**.

## <a name="peer-the-azure-ad-vnet-and-the-hdinsight-vnet"></a>Führen Sie das Peering der Netzwerke Azure AD-VNet und HDInsight VNet durch.
**Durchführen des Peering der beiden VNets**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie im linken Menü auf **Weitere Dienste**.
3. Klicken Sie auf **Virtuelle Netzwerke**. Klicken Sie nicht auf **Virtuelle Netzwerke (klassisch)**.
4. Klicken Sie auf **contosohdivnet**.  Dies ist das HDInsight-VNet.
5. Klicken Sie im linken Menü des Blatts auf **Peering**.
6. Klicken Sie im Menü am oberen Rand auf **Hinzufügen**. Damit wird das Blatt **Peering hinzufügen** geöffnet.
7. Legen Sie im Blatt **Peering hinzufügen** die folgenden Werte fest, oder wählen Sie diese aus:
   
   * **Name**: ContosoAADHDIVNetPeering
   * **Bereitstellungsmodell für das virtuelle Netzwerk**: Klassisch
   * **Abonnement**: Wählen Sie den Namen Ihres Abonnements aus, das für das klassische (Azure AD) VNet verwendet wird.
   * **Virtuelles Netzwerk**: contosoaadvnet.
   * **Zugriff auf virtuelles Netzwerk zulassen**: (aktivieren)
   * **Weitergeleiteten Datenverkehr zulassen**: (aktivieren) Lassen Sie die beiden anderen Kontrollkästchen deaktiviert.
8. Klicken Sie auf **OK**.

## <a name="create-hdinsight-cluster"></a>Erstellen des HDInsight-Clusters
In diesem Abschnitt erstellen Sie einen Linux-basierten Hadoop-Cluster in HDInsight, entweder mithilfe des Azure-Portals oder einer [Azure Resource Manager-Vorlage](../azure-resource-manager/resource-group-template-deploy.md). Andere Methoden zur Erstellung von Clustern und Informationen zu den Einstellungen finden Sie unter [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Weitere Informationen zur Verwendung einer Resource Manager-Vorlage zum Erstellen von Hadoop-Clustern in HDInsight finden Sie unter [Erstellen von Hadoop-Clustern in HDInsight mithilfe von Resource Manager-Vorlagen](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

**Erstellen eines in die Domäne eingebundenen HDInsight-Clusters mithilfe des Azure-Portals**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Neu**, **Intelligence + Analyse**, und klicken Sie dann auf **HDInsight**.
3. Geben Sie auf dem Blatt **Neuer HDInsight-Cluster** die folgenden Werte ein, oder wählen Sie diese aus:
   
   * **Clustername**: Geben Sie einen neuen Namen für den in die Domäne eingebundenen HDInsight-Cluster ein.
   * **Abonnement** : Wählen Sie ein Azure-Abonnement aus, das für die Erstellung dieses Clusters verwendet wird.
   * **Clusterkonfiguration**:
     
     * **Clustertyp**: Hadoop. HDInsight mit Domänenverknüpfung wird derzeit nur auf Hadoop-Clustern unterstützt.
     * **Betriebssystem**: Linux.  HDInsight mit Domänenverknüpfung wird derzeit nur auf Linux-basierten HDInsight-Clustern unterstützt.
     * **Version**: Hadoop 2.7.3 (HDI 3.5). HDInsight mit Domänenverknüpfung wird derzeit nur auf HDInsight-Clustern, Version 3.5 unterstützt.
     * **Clustertyp**: PREMIUM
       
       Klicken Sie zum Speichern der Änderungen auf **Auswählen**.
   * **Anmeldeinformationen**: Konfigurieren Sie die Anmeldeinformationen für den Benutzer des Clusters und den Benutzer von SSH.
   * **Datenquelle**: Erstellen Sie ein neues Speicherkonto oder verwenden Sie ein vorhandenes Speicherkonto, das als Standardspeicherkonto für den HDInsight-Cluster verwendet werden kann. Der Standort muss dem Standort der beiden VNets entsprechen.  Dieser Standort muss auch dem Standort des HDInsight-Clusters entsprechen.
   * **Preise**: Wählen Sie die Anzahl der Workerknoten Ihres Clusters aus.
   * **Erweiterte Konfiguration**: 
     
     * **Domain-joining & Vnet/Subnet (Domänenbeitritt & VNET-Subnetz)**: 
       
       * **Domäneneinstellungen**: 
         
         * **Domänenname**: contoso.onmicrosoft.com
         * **Domänenbenutzername**: Geben Sie einen Domänenbenutzernamen ein. Diese Domäne muss über die folgenden Berechtigungen verfügen: Hinzufügen von Computern zur Domäne, und Platzieren dieser Computer in der konfigurierten Organisationseinheit; Erstellen von Dienstprinzipalen innerhalb der konfigurierten Organisationseinheit; Erstellen von Reverse-DNS-Einträgen. Dieser Benutzer der Domäne wird der Administrator dieses in die Domäne eingebundenen HDInsight-Clusters.
         * **Domänenkennwort**: Geben Sie das Benutzerkennwort für die Domäne ein.
         * **Organisationseinheit**: Geben Sie den Distinguished Name der zuvor konfigurierten OE ein. Beispiel: OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com
         * **LDAPS URL**: ldaps://contoso.onmicrosoft.com:636
         * **Access user group (Zugriff auf die Benutzergruppe)**: Geben Sie die Sicherheitsgruppe an, deren Benutzer Sie mit dem Cluster synchronisieren möchten. Beispiel: HiveUsers.
           
           Klicken Sie zum Speichern der Änderungen auf **Auswählen**.
           
           ![Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-portal-domain-setting.png)
       * **Virtuelles Netzwerk**: contosohdivnet
       * **Subnetz**: Subnet1
         
         Klicken Sie zum Speichern der Änderungen auf **Auswählen**.        
         Klicken Sie zum Speichern der Änderungen auf **Auswählen**.
   * **Ressourcengruppe**: Wählen Sie die Ressourcengruppe aus, die für das HDInsight VNet verwendet wird (contosohdirg).
4. Klicken Sie auf **Erstellen**.  

Eine weitere Möglichkeit in die Domäne eingebundene HDInsight-Cluster zu erstellen ist es, eine Azure Resource Management-Vorlage zu verwenden. Im Folgenden sehen Sie, wie das funktioniert:

**Erstellen eines in die Domäne eingebundenen HDInsight-Clusters mithilfe einer Resource Management-Vorlage**

1. Klicken Sie auf das folgende Bild, um eine Resource Manager-Vorlage im Azure-Portal zu öffnen. Die Resource Manager-Vorlage befindet sich in einem öffentlichen Blobcontainer. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="./media/hdinsight-domain-joined-configure/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Geben Sie auf dem Blatt **Parameter** folgende Werte ein:
   
   * **Abonnement**: (Wählen Sie Ihr Azure-Abonnement aus).
   * **Ressourcengruppe**: Klicken Sie auf **Vorhandene Verwenden**, und geben Sie die gleiche Ressourcengruppe an wie die, die Sie verwendet haben.  Beispiel: contosohdirg. 
   * **Standort**: Geben Sie den Standort einer Ressourcengruppe an.
   * **Clustername**: Geben Sie einen Namen für den Hadoop-Cluster ein, den Sie erstellen möchten. Beispiel: contosohdicluster.
   * **Clustertyp**: Wählen Sie einen Clustertyp aus.  Der Standardwert lautet **hadoop**.
   * **Standort**: Wählen Sie einen Standort für den Cluster aus.  Das standardmäßige Speicherkonto verwendet den gleichen Speicherort.
   * **Cluster Worker Node count (Anzahl von Workerknoten im Cluster)**: Wählen Sie die Anzahl der Workerknoten aus.
   * **Cluster-Benutzername und -Kennwort**: Der Standardname für die Anmeldung lautet **admin**.
   * **SSH-Benutzername und -Kennwort**: Der Standardname für die Anmeldung lautet **sshuser**.  Sie können auch einen anderen Namen festlegen. 
   * **Virtual Network-ID**: /subscriptions/&lt;SubscriptionID>/resourceGroups/&lt;ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/&lt;VNetName>
   * **Virtuelles Netzwerk/Subnetz**: /subscriptions/&lt;SubscriptionID>/resourceGroups/&lt;ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/&lt;VNetName>/subnets/Subnet1
   * **Domänenname**: contoso.onmicrosoft.com
   * **Organization Unit DN (Organisationseinheit DN)**: OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com
   * **Cluster Users Group D Ns (Nutzergruppe Cluster D Ns)**: "\"CN=HiveUsers,OU=AADDC Users,DC=<DomainName>,DC=onmicrosoft,DC=com\""
   * **LDAPUrls**: ["ldaps://contoso.onmicrosoft.com:636"]
   * **DomainAdminUserName**: (Geben Sie den Benutzernamen des Administrators der Domäne ein)
   * **DomainAdminPassword**: (Geben Sie das Kennwort des Administrators der Domäne ein)
   * **Ich stimme den oben genannten Geschäftsbedingungen zu**: (aktivieren)
   * **An Dashboard anheften**: (aktivieren)
3. Klicken Sie auf **Kaufen**. Es wird eine neue Kachel mit dem Titel **Deploying Template deployment (Vorlagenbereitstellung bereitstellen)** angezeigt. Das Erstellen eines Clusters dauert ca. 20 Minuten. Wenn der Cluster erstellt wurde, öffnen Sie ihn, indem Sie im Portal auf das Clusterblatt klicken.

Löschen Sie den Cluster, wenn Sie das Tutorial beendet haben. Mit HDInsight werden Ihre Daten im Azure-Speicher gespeichert, sodass Sie einen Cluster problemlos löschen können, wenn er nicht verwendet wird. Für einen HDInsight-Cluster fallen auch dann Gebühren an, wenn er nicht verwendet wird. Da die Gebühren für den Cluster erheblich höher sind als die Kosten für den Speicher, ist es sinnvoll, nicht verwendete Cluster zu löschen. Anweisungen zum Löschen eines Clusters finden Sie unter [Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal](hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Konfigurieren eines in die Domäne eingebundenen HDInsight-Clusters finden Sie unter [Configure Domain-joined HDInsight clusters use Azure PowerShell](hdinsight-domain-joined-configure-use-powershell.md) (Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit Azure PowerShell).
* Informationen zum Konfigurieren von Hive-Richtlinien und zum Ausführen von Hive-Abfragen finden Sie unter [Configure Hive policies in Domain-joined HDInsight (Preview)](hdinsight-domain-joined-run-hive.md) (Konfigurieren von Hive-Richtlinien für in die Domäne eingebundene HDInsight-Cluster).
* Informationen zum Verwenden von SSH für das Herstellen von Verbindungen mit in die Domäne eingebundenen HDInsight-Clustern finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).


