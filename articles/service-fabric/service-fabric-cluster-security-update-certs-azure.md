<properties
   pageTitle="Aktualisieren von Zertifikaten für einen Azure-Cluster | Microsoft Azure"
   description="Beschreibt das Hochladen eines neuen Zertifikats in Azure Key Vault und das Aktualisieren oder Entfernen eines Zertifikats für einen vorhandenen Service Fabric-Cluster."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/27/2016"
   ms.author="chackdan"/>

# Hinzufügen oder Entfernen von Zertifikaten für einen Service Fabric-Cluster in Azure

Service Fabric ermöglicht Ihnen, zwei Zertifikate anzugeben – ein primäres und ein sekundäres – wenn Sie die Zertifikatssicherheit während der Erstellung des Clusters konfigurieren. Standardmäßig ist das Zertifikat, das Sie zum Zeitpunkt der Erstellung angeben, das primäre Zertifikat. Nach der Erstellung des Clusters können Sie ein neues Zertifikat als sekundäres hinzufügen oder ein vorhandenes Zertifikat entfernen. Weitere Informationen zur Verwendung von X.509-Zertifikaten durch Service Fabric finden Sie unter [Schützen von Service Fabric-Clustern](service-fabric-cluster-security.md).

>[AZURE.NOTE] Für einen sicheren Cluster muss stets mindestens ein gültiges (nicht widerrufenes oder abgelaufenes) Zertifikat (primär oder sekundär) bereitgestellt sein. Andernfalls können Sie nicht auf den Cluster zugreifen.

## Hinzufügen eines sekundären Zertifikats
Um ein anderes Zertifikat als sekundäres hinzuzufügen, müssen Sie das Zertifikat in einen Azure-Schlüsseltresor hochladen und dann für die virtuellen Computer im Cluster bereitstellen. Weitere Informationen finden Sie unter [Deploy certificates to VMs from a customer-managed Key Vault](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx) (Bereitstellen von Zertifikaten für VMs über einen vom Kunden verwalteten Schlüsseltresor).

1. [Hochladen eines X.509-Zertifikats in den Schlüsseltresor](service-fabric-cluster-azure-secure-with-certs.md#step-2-upload-the-x509-certificate-to-the-key-vault)
2. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und wechseln Sie zu der Clusterressource, der Sie dieses Zertifikat hinzufügen möchten.
3. Klicken Sie unter **Einstellungen** auf die Zertifikateinstellung, und geben Sie den sekundären Zertifikatfingerabdruck ein.
4. Klicken Sie auf **Speichern**. Eine Bereitstellung wird gestartet, nach deren erfolgreichem Abschluss Sie sowohl das primäre als auch das sekundäre Zertifikat zum Ausführen von Verwaltungsvorgängen im Cluster verwenden können.

![Screenshot mit Zertifikatfingerabdrücken im Portal][SecurityConfigurations_02]

## Entfernen eines Zertifikats
Es folgt der Prozess zum Entfernen eines alten Zertifikats, damit der Cluster es nicht mehr verwendet:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und navigieren Sie zu den Sicherheitseinstellungen Ihres Clusters.
2. Entfernen Sie eines der Zertifikate.
3. Klicken Sie auf **Speichern**. Daraufhin beginnt eine neue Bereitstellung. Nachdem die Bereitstellung abgeschlossen ist, kann das entfernte Zertifikat nicht mehr zum Verbinden mit dem Cluster verwendet werden.

## Nächste Schritte
Lesen Sie die folgenden Artikel, um weitere Informationen zur Clusterverwaltung zu enthalten:

- [Service Fabric-Cluster-Upgradeprozess und Erwartungen](service-fabric-cluster-upgrade.md)
- [Rollenbasierte Zugriffssteuerung für Service Fabric-Clients](service-fabric-cluster-security-roles.md)


<!--Image references-->
[SecurityConfigurations_02]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_02.png

<!---HONumber=AcomDC_0608_2016-->