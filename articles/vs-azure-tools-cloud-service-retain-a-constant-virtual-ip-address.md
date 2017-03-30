---
title: "Beibehalten einer konstanten virtuellen IP-Adresse für einen Azure-Clouddienst | Microsoft Docs"
description: "Erfahren Sie, wie Sie sicherstellen können, dass die virtuelle IP-Adresse (VIP) Ihres Azure-Clouddiensts beibehalten wird."
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 4a58e2c6-7a79-4051-8a2c-99182ff8b881
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 12d73e2f490acc92bb4a2e1014bb24f7f672f713
ms.lasthandoff: 03/22/2017


---
# <a name="how-to-retain-a-constant-virtual-ip-address-for-an-azure-cloud-service"></a>Beibehalten einer konstanten virtuellen IP-Adresse für einen Azure-Clouddienst
Wenn Sie einen in Azure gehosteten Clouddienst aktualisieren, müssen Sie möglicherweise sicherstellen, dass die virtuelle IP-Adresse (VIP) des Diensts unverändert bleibt. Viele Domänenverwaltungsdienste nutzen DNS (Domain Name System) für die Registrierung der Domänennamen. DNS funktioniert nur, wenn die VIP unverändert bleibt. Mithilfe des **Veröffentlichungs-Assistenten** in Azure Tools können Sie sicherstellen, dass die VIP Ihres Clouddiensts beibehalten wird, wenn Sie den Dienst aktualisieren. Weitere Informationen zur Verwendung der DNS-Domänenverwaltung für Clouddienste finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für einen Azure-Clouddienst](cloud-services/cloud-services-custom-domain-name.md).

## <a name="publishing-a-cloud-service-without-changing-its-vip"></a>Veröffentlichen eines Clouddiensts, ohne die VIP zu ändern
Die VIP eines Clouddiensts wird zugeordnet, wenn der Clouddienst erstmalig in einer bestimmten Umgebung in Azure bereitgestellt wird (z. B. in der Produktionsumgebung). Die VIP ändert sich nur, wenn Sie die Bereitstellung explizit löschen oder wenn diese durch den Vorgang der Bereitstellungsaktualisierung implizit gelöscht wird. Wenn Sie die VIP beibehalten möchten, dürfen Sie die Bereitstellung nicht löschen. Außerdem müssen Sie sicherstellen, dass Visual Studio die Bereitstellung nicht automatisch löscht. Dieses Verhalten kann gesteuert werden, indem Sie im **Veröffentlichungs-Assistenten** die entsprechenden Bereitstellungseinstellungen festlegen. Dieser Assistent unterstützt verschiedene Bereitstellungsoptionen. Sie können eine neue Bereitstellung oder eine Updatebereitstellung angeben, wobei die Aktualisierung inkrementell oder gleichzeitig erfolgen kann. Bei beiden Arten von Updatebereitstellungen wird die VIP beibehalten. Eine Definition dieser Bereitstellungstypen finden Sie unter [Assistent zur Veröffentlichung einer Azure-Anwendung](vs-azure-tools-publish-azure-application-wizard.md).  Darüber hinaus können Sie festlegen, ob die vorherige Bereitstellung eines Clouddiensts gelöscht wird, wenn ein Fehler auftritt. Die VIP wird möglicherweise unerwartet geändert, wenn diese Option nicht ordnungsgemäß angezeigt wird.

## <a name="updating-a-cloud-service-without-changing-its-vip"></a>Aktualisieren eines Clouddiensts ohne Ändern der zugehörigen VIP
1. Erstellen oder öffnen Sie ein Azure-Clouddienstprojekt in Visual Studio. 

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie im Kontextmenü die Option **Veröffentlichen** aus.

    ![Veröffentlichungsmenü](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/solution-explorer-publish-menu.png)

1. Wählen Sie im Dialogfeld **Azure-Anwendung veröffentlichen** das Azure-Abonnement für die Bereitstellung aus, melden Sie sich gegebenenfalls an, und wählen Sie **Weiter** aus.

    ![Azure-Anwendung veröffentlichen: Anmelden](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-signin.png)

1. Überprüfen Sie auf der Registerkarte **Allgemeine Einstellungen**, ob der Name des Clouddiensts, für den die Bereitstellung durchgeführt wird, sowie die Angaben für **Umgebung**, **Buildkonfiguration** und **Dienstkonfiguration** richtig sind.

    ![Azure-Anwendung veröffentlichen – Allgemeine Einstellungen](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-common-settings.png)

1. Überprüfen Sie auf der Registerkarte **Erweiterte Einstellungen**, ob das richtige Speicherkonto und die richtige Bereitstellungsbezeichnung angegeben sind. Auf dieser Registerkarte muss zudem sichergestellt werden, dass das Kontrollkästchen **Bereitstellung bei Fehler löschen** deaktiviert und das Kontrollkästchen **Bereitstellungsaktualisierung** aktiviert ist. Durch Aktivierung des Kontrollkästchens **Bereitstellungsaktualisierung** wird sichergestellt, dass die Bereitstellung nicht gelöscht und die VIP beibehalten wird, wenn Sie die Anwendung erneut veröffentlichen. Durch Deaktivierung von **Bereitstellung bei Fehler löschen** stellen Sie sicher, dass die VIP beibehalten wird, wenn während der Bereitstellung ein Fehler auftritt.

    ![Azure-Anwendung veröffentlichen – Erweiterte Einstellungen](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-advanced-settings.png)

1. (Optional:) Um festzulegen, wie die Rollen aktualisiert werden sollen, klicken Sie neben **Bereitstellungsaktualisierung** auf **Einstellungen**. Wählen Sie entweder **Inkrementelle Aktualisierung** oder **Gleichzeitige Aktualisierung** aus. Bei Auswahl von **Inkrementelle Aktualisierung** werden die Instanzen der Anwendung nacheinander aktualisiert. Die Verfügbarkeit der Anwendung wird also nicht unterbrochen. Wenn Sie **Gleichzeitige Aktualisierung** auswählen, werden alle Instanzen der Anwendung gleichzeitig aktualisiert. Gleichzeitige Updates sind schneller, der Dienst ist während der Aktualisierung jedoch möglicherweise nicht verfügbar. Wählen Sie abschließend die Option **Weiter** aus.

    ![Azure-Anwendung veröffentlichen – Einstellungen für Bereitstellungsupdate](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-deployment-update-settings.png)

1. Wählen Sie im dann wieder angezeigten Dialogfeld **Azure-Anwendung veröffentlichen** solange die Option **Weiter** aus, bis die Seite **Zusammenfassung** angezeigt wird. Überprüfen Sie die Einstellungen, und wählen Sie dann **Veröffentlichen** aus.
   
    ![Azure-Anwendung veröffentlichen – Zusammenfassung](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-summary.png)

## <a name="next-steps"></a>Nächste Schritte
- [Verwenden des Visual Studio-Assistenten zur Veröffentlichung einer Azure-Anwendung](vs-azure-tools-publish-azure-application-wizard.md)


