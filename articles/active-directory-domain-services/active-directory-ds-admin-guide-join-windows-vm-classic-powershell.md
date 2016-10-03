<properties
	pageTitle="Azure Active Directory Domain Services: Administratorhandbuch | Microsoft Azure"
	description="Binden Sie einen virtuellen Windows-Computer mit Azure PowerShell und dem klassischen Bereitstellungsmodell in eine verwaltete Domäne ein."
	services="active-directory-ds"
	documentationCenter=""
	authors="mahesh-unnikrishnan"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory-ds"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/20/2016"
	ms.author="maheshu"/>


# Einbinden eines virtuellen Windows Server-Computers in eine verwaltete Domäne mit PowerShell

> [AZURE.SELECTOR]
- [Klassisches Azure-Portal – Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell – Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

> [AZURE.IMPORTANT] Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Ressourcen-Manager und klassische Bereitstellungen](../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Die Azure AD-Domänendienste unterstützen das Resource Manager-Modell derzeit nicht.

Diese Schritte zeigen, wie Sie eine Reihe von Azure PowerShell-Befehlen anpassen, mit denen ein Windows-basierter virtueller Azure-Computer mit einem Bausteinansatz erstellt und vorab konfiguriert wird. Mithilfe dieser Schritte können Sie einen Windows-basierten virtuellen Azure-Computer erstellen und in eine durch die Azure AD-Domänendienste verwaltete Domäne einbinden.

Diese Schritte folgen einem lückenfüllenden Ansatz zur Erstellung von Azure PowerShell-Befehlssätzen. Dieser Ansatz kann hilfreich sein, wenn Sie noch nicht mit PowerShell gearbeitet haben oder wissen möchten, welche Werte Sie für die erfolgreiche Konfiguration angeben müssen. Erweiterte PowerShell-Benutzer können die Befehle verwenden und sie durch eigene Werte für die Variablen ersetzen (Zeilen, die mit "$" beginnen).

Wenn Sie dies noch nicht getan haben, verwenden Sie die Anweisungen unter [Gewusst wie: Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md), um Azure PowerShell auf Ihrem lokalen Computer zu installieren. Öffnen Sie anschließend eine Windows PowerShell-Eingabeaufforderung.

## Schritt 1: Hinzufügen Ihres Kontos

1. Geben Sie an der PowerShell-Eingabeaufforderung **Add-AzureAccount** ein, und drücken Sie die **EINGABETASTE**.
2. Geben Sie die mit Ihrem Azure-Abonnement verknüpfte E-Mail-Adresse ein, und klicken Sie auf **Weiter**.
3. Geben Sie das Kennwort für Ihr Konto ein.
4. Klicken Sie auf **Anmelden**.

## Schritt 2: Festlegen Ihres Abonnements und Speicherkontos

Legen Sie Ihr Azure-Abonnement und -Speicherkonto fest, indem Sie diese Befehle in der Windows PowerShell-Eingabeaufforderung ausführen. Ersetzen Sie alles in den Anführungszeichen, einschließlich der Zeichen < und >, durch die korrekten Namen.

	$subscr="<subscription name>"
	$staccount="<storage account name>"
	Select-AzureSubscription -SubscriptionName $subscr –Current
	Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Sie erhalten den korrekten Abonnementnamen aus der Eigenschaft "SubscriptionName" der Ausgabe des Befehls **Get-AzureSubscription**. Sie erhalten den korrekten Speicherkontonamen aus der Eigenschaft "Label" der Ausgabe des Befehls **Get-AzureStorageAccount**, nachdem Sie den Befehl **Select-AzureSubscription** ausgeführt haben.


## Schritt 3: Exemplarische Vorgehensweise – Bereitstellen des virtuellen Computers und Einbinden des Computers in die virtuelle Domäne
Hier finden Sie den entsprechenden Azure PowerShell-Befehlssatz zum Erstellen dieses virtuellen Computers, mit Leerzeilen zwischen jedem Block für Lesbarkeit.

Geben Sie Informationen zum bereitzustellenden virtuellen Windows-Computer an.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

Die InstanceSize-Werte für virtuelle Computer der D-, DS- oder G-Serie finden Sie unter [Größen virtueller Computer und Clouddienste für Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Stellen Sie Informationen über die verwaltete Domäne bereit.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Geben Sie den Namen des Clouddiensts an.

    $svcname="Contoso100-test"

Geben Sie den Namen des virtuellen Netzwerks an, dem der virtuelle Computer beitreten soll. Stellen Sie sicher dass die durch die Azure AD-Domänendienste verwaltete Domäne in diesem virtuellen Netzwerk verfügbar ist.

    $vnetname="MyPreviewVnet"

Wählen Sie das VM-Image aus, das zur Bereitstellung des virtuellen Computers verwendet werden soll.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Konfigurieren Sie den virtuellen Computer, indem Sie den Namen, die Instanzgröße und das zu verwendende Image festlegen.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Rufen Sie Anmeldeinformationen für den lokalen Administrator des virtuellen Computers ab. Wählen Sie ein sicheres Kennwort für den lokalen Administrator. Sie können die Sicherheit des Kennworts unter [Password Checker: Using Strong Passwords](https://www.microsoft.com/security/pc-security/password-checker.aspx) (Kennwortprüfung – Verwenden sicherer Kennwörter) überprüfen.

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

Rufen Sie Anmeldeinformationen für ein Benutzerkonto ab, das zur Administratorengruppe für Azure AD-Domänencontroller gehört, um den virtuellen Computer in die verwaltete Domäne einzubinden. Geben Sie nicht den Domänenname an – in unserem Beispiel wurde „bob“ als Benutzername verwendet.

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

Konfigurieren Sie den virtuellen Computer – geben Sie die Anforderungen für den Domänenbeitritt und die erforderlichen Anmeldeinformationen an.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Legen Sie ein Subnetz für den virtuellen Computer fest.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Optional: Verweisen Sie auf die IP-Adresse der Domäne. Wenn Sie die IP-Adressen der durch die Azure AD-Domänendienste verwalteten Domäne als DNS-Server für das virtuelle Netzwerk festlegen, ist dieser Schritt nicht notwendig.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Stellen Sie jetzt den in die Domäne eingebundenen virtuellen Windows-Computer bereit.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## Skript für die Bereitstellung eines virtuellen Windows-Computers und für den automatischen Beitritt zu einer durch die Azure AD-Domänendienste verwalteten Domäne
Dieser PowerShell-Befehlssatz erstellt einen virtuellen Computer für einen Branchenserver, für den Folgendes gilt:

- das Windows Server 2012 R2 Datacenter-Image verwendet
- Ist ein besonders kleiner virtueller Computer.
- Heißt „contoso-test“.
- Tritt automatisch der verwalteten Domäne „contoso100“ bei.
- Wird dem gleichen virtuellen Netzwerk hinzugefügt wie die verwaltete Domäne.

Hier ist das vollständige Beispielskript zur Erstellung des virtuellen Windows-Computers und zum automatischen Beitritt des Computers zur durch die Azure AD-Domänendienste verwalteten Domäne.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## Verwandte Inhalte
- [Azure AD-Domänendienste – Leitfaden zu den ersten Schritten](./active-directory-ds-getting-started.md)

- [Verwalten einer durch Azure AD-Domänendienste verwalteten Domäne](./active-directory-ds-admin-guide-administer-domain.md)

<!---HONumber=AcomDC_0921_2016-->