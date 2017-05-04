---
title: Aktivieren von Remotedebugging mit Continuous Delivery | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie Remotedebugging aktivieren, wenn kontinuierliche Zustellung für die Bereitstellung in Azure verwendet wird."
services: cloud-services
documentationcenter: .net
author: TomArcher
manager: douge
editor: 
ms.assetid: 7d423639-3b2f-4ca5-ac5a-9ac19a217c29
ms.service: cloud-services
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 11/18/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 7a9ef4098fec7b464e654a429c8c854ed4bb73fd
ms.lasthandoff: 04/27/2017


---
# <a name="enable-remote-debugging-when-using-continuous-delivery-to-publish-to-azure"></a>Aktivieren von Remotedebuggen, wenn kontinuierliche Zustellung für die Veröffentlichung in Azure verwendet wird
Sie können mithilfe folgender Schritte das Remotedebuggen in Azure, für Clouddienste oder virtuelle Computer aktivieren, wenn Sie [kontinuierliche Zustellung](cloud-services-dotnet-continuous-delivery.md) für die Veröffentlichung in Azure verwenden.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Aktivieren von Remotedebuggen für Cloud-Dienste
1. Richten Sie im Build-Agent die initiale Umgebung für Azure ein, wie in [Befehlszeilentool für Azure](http://msdn.microsoft.com/library/hh535755.aspx)dargestellt.
2. Da die Remotedebugging-Laufzeit (msvsmon.exe) für das Paket erforderlich ist, müssen Sie die **Remote Tools für Visual Studio** installieren.

    * [Remote Tools für Visual Studio 2017](https://go.microsoft.com/fwlink/?LinkId=746570)
    * [Remote Tools für Visual Studio 2015](https://go.microsoft.com/fwlink/?LinkId=615470)
    * [Remote Tools für Visual Studio 2013 Update 5](https://www.microsoft.com/download/details.aspx?id=48156)
    
    Alternativ können Sie auch die Remotedebugging-Binärdateien aus einem System kopieren, auf dem Visual Studio installiert ist.

3. Erstellen Sie ein Zertifikat, wie in [Übersicht über Zertifikate für Azure Cloud Services](cloud-services-certs-create.md)gezeigt. Bewahren Sie die .pfx-Datei und den RDP-Zertifikatfingerabdruck auf, und laden Sie das Zertifikat in den Zielclouddienst hoch.
4. Verwenden Sie folgende Optionen in der MSBuild-Befehlszeile, um ein Build und ein Paket mit aktiviertem Remotedebugging zu erstellen. (Ersetzen Sie die Elemente in spitzen Klammern durch tatsächliche Pfade zu Ihren System- und Projektdateien).
   
        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"
   
    `VSX64RemoteDebuggerPath` ist der Pfad zu dem Ordner, der „msvsmon.exe“ in den Remote Tools für Visual Studio enthält.
    `RemoteDebuggerConnectorVersion` ist die Azure SDK-Version in Ihrem Clouddienst. Diese Version sollte auch der Version entsprechen, die zusammen mit Visual Studio installiert wurde.
5. Veröffentlichen Sie zum Zielclouddienst, indem Sie das Paket und die .cscfg-Datei verwenden, die im vorherigen Schritt generiert wurden.
6. Importieren Sie das Zertifikat (PFX-Datei) auf den Computer, auf dem Visual Studio mit Azure SDK für .NET installiert ist. Vergewissern Sie sich, dass als Importziel der `CurrentUser\My`-Zertifikatspeicher verwendet wird. Andernfalls tritt beim Anfügen an den Debugger in Visual Studio ein Fehler auf.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Aktivieren von Remotedebuggen für virtuelle Computer
1. Erstellen Sie einen virtuellen Azure-Computer. Informationen finden Sie unter [Erstellen Ihres ersten virtuellen Windows-Computers im Azure-Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) oder [Erstellen und Verwalten virtueller Windows-Computer in Visual Studio](../virtual-machines/windows/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
2. Sehen Sie sich auf der [klassischen Azure-Portalseite](http://go.microsoft.com/fwlink/p/?LinkID=269851)das Dashboard des virtuellen Computers an, um den **RDP-ZERTIFIKATFINGERABDRUCK**des virtuellen Computers anzuzeigen. Dieser Wert wird für den `ServerThumbprint` -Wert in der Erweiterungskonfiguration verwendet.
3. Erstellen Sie ein Clientzertifikat, wie in [Übersicht über Zertifikate für Azure Cloud Services](cloud-services-certs-create.md) gezeigt (behalten Sie die .pfx-Datei und den RDP-Zertifikatfingerabdruck bei).
4. Installieren Sie Azure PowerShell (ab Version 0.7.4) wie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview)beschrieben.
5. Führen Sie folgendes Skript aus, um die RemoteDebug-Erweiterung zu aktivieren. Ersetzen Sie die Pfade und persönlichen Daten mit Ihren eigenen Daten, zum Beispiel Abonnementname, Dienstname und Fingerabdruck.
   
   > [!NOTE]
   > Dieses Skript wird für Visual Studio 2015 konfiguriert. Wenn Sie Visual Studio 2013 oder Visual Studio 2017 verwenden, ändern Sie im Anschluss die Zuweisungen von `$referenceName` und `$extensionName` so, dass stattdessen `RemoteDebugVS2013` oder `RemoteDebugVS2017` verwendet wird.

    ```powershell   
    Add-AzureAccount

    Select-AzureSubscription "My Microsoft Subscription"

    $vm = Get-AzureVM -ServiceName "mytestvm1" -Name "mytestvm1"

    $endpoints = @(
                    ,@{Name="RDConnVS2013"; PublicPort=30400; PrivatePort=30398}
                    ,@{Name="RDFwdrVS2013"; PublicPort=31400; PrivatePort=31398}
                )

    foreach($endpoint in $endpoints)
    {
        Add-AzureEndpoint -VM $vm -Name $endpoint.Name -Protocol tcp -PublicPort $endpoint.PublicPort -LocalPort $endpoint.PrivatePort
    }

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015"
    $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug"
    $extensionName = "RemoteDebugVS2015"
    $version = "1.*"
    $publicConfiguration = "<PublicConfig><Connector.Enabled>true</Connector.Enabled><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Set-AzureVMExtension -ReferenceName $referenceName -Publisher $publisher -ExtensionName $extensionName -Version $version -PublicConfiguration $publicConfiguration

    foreach($extension in $vm.VM.ResourceExtensionReferences)
    {
        if(($extension.ReferenceName -eq $referenceName) `
        -and ($extension.Publisher -eq $publisher) `
        -and ($extension.Name -eq $extensionName) `
        -and ($extension.Version -eq $version))
        {
            $extension.ResourceExtensionParameterValues[0].Key = 'config.txt'
            break
        }
    }

    $vm | Update-AzureVM
    ```

6. Importieren Sie das Zertifikat (PFX-Datei) auf den Computer, auf dem Visual Studio mit dem Azure-SDK für .NET installiert ist.


