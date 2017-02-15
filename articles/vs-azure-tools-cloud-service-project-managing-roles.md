---
title: Verwalten von Rollen in den Azure Cloud Services-Projekten mit Visual Studio | Microsoft Docs
description: "Informationen zum Hinzufügen neuer Rollen zu Ihrem Azure-Clouddienstprojekt oder Entfernen von Rollen daraus mit Visual Studio."
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 5ec9ae2e-8579-4e5d-999e-8ae05b629bd1
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 8c805f9af3154e46f25a6d24c7df33f390189340


---
# <a name="managing-roles-in-the-azure-cloud-services-projects-with-visual-studio"></a>Verwalten von Rollen in den Azure-Clouddienstprojekten mit Visual Studio
Nach dem Erstellen des Azure-Clouddienstprojekts können Sie ihm neue Rollen hinzufügen oder vorhandene Rollen daraus entfernen. Darüber hinaus können Sie ein vorhandenes Projekt importieren und es in eine Rolle konvertieren. Sie können z. B. eine ASP.NET-Webanwendung importieren und sie als Webrolle festlegen.

## <a name="adding-or-removing-roles"></a>Hinzufügen und Entfernen von Rollen
**So fügen Sie eine Rolle hinzu**

Um eine Rolle hinzuzufügen, öffnen Sie im **Projektmappen-Explorer** im Clouddienstprojekt das Kontextmenü für den Knoten **Rollen** und wählen **Hinzufügen** aus. Sie können eine vorhandene Web- oder Workerrolle in der aktuellen Projektmappe auswählen oder ein neues Web- oder Workerrollenprojekt erstellen. Sie können auch ein entsprechendes Projekt auswählen, z. B. ein ASP.NET-Webanwendungsprojekt, und es einem Rollenprojekt zuordnen.

**So entfernen Sie eine Rollenzuordnung**

Öffnen Sie zum Entfernen einer Rollenzuordnung im Projektmappen-Explorer im Clouddienstprojekt das Kontextmenü für den Knoten **Rollen**, und wählen Sie **Entfernen** aus.

## <a name="removing-and-adding-roles-in-your-cloud-service"></a>Entfernen und Hinzufügen von Rollen im Clouddienst
Wenn Sie eine Rolle aus Ihrem Clouddienstprojekt entfernen, sie aber später wieder zum Projekt hinzufügen möchten, werden nur die Rollendeklaration und grundlegende Attribute wie Endpunkte und Diagnoseinformationen hinzugefügt. Es werden keine zusätzlichen Ressourcen oder Verweise der Datei "ServiceDefinition.csdef" oder "ServiceConfiguration.cscfg" hinzugefügt. Wenn Sie diese Informationen hinzufügen möchten, müssen Sie sie wieder manuell diesen Dateien hinzufügen.

Beispielsweise könnten Sie eine Webdienstrolle entfernen und diese Rolle später wieder der Projektmappe hinzufügen. Wenn Sie dies tun, tritt ein Fehler auf. Fügen Sie zur Vermeidung des Fehlers das im folgenden XML-Code dargestellte `<LocalResources>` -Element wieder der Datei "ServiceDefinition.csdef" hinzu. Verwenden Sie den Namen der Webdienstrolle, die Sie dem Projekt wieder hinzugefügt haben, als Teil des Namensattributs für das **<LocalStorage>** -Element. In diesem Beispiel lautet der Name der Webdienstrolle **WCFServiceWebRole1**:

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Konfigurieren von Rollen in Visual Studio finden Sie unter [Konfigurieren der Rollen für einen Azure-Clouddienst mit Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).




<!--HONumber=Nov16_HO3-->


