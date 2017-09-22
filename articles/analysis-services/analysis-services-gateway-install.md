---
title: Installieren eines lokalen Datengateways | Microsoft-Dokumentation
description: Informationen zum Installieren und Konfigurieren eines lokalen Datengateways.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/20/2017
ms.author: owend
ms.translationtype: HT
ms.sourcegitcommit: a29f1e7b39b7f35073aa5aa6c6bd964ffaa6ffd0
ms.openlocfilehash: d614352a62dc7aca012e9b144473604ae9829af9
ms.contentlocale: de-de
ms.lasthandoff: 09/21/2017

---
# <a name="install-and-configure-an-on-premises-data-gateway"></a>Installieren und Konfigurieren eines lokalen Datengateways
Ein lokales Datengateway ist erforderlich, wenn sich mindestens ein Azure Analysis Services-Server in der gleichen Region mit lokalen Datenquellen verbindet. Weitere Informationen zum Gateway finden Sie unter [Lokales Datengateway](analysis-services-gateway.md).

## <a name="prerequisites"></a>Voraussetzungen
**Mindestanforderungen.**

* .NET 4.5 Framework
* 64-Bit-Version von Windows 7/Windows Server 2008 R2 (oder höher)

**Empfohlen.**

* 8-Kern-CPU
* 8 GB Arbeitsspeicher
* 64-Bit-Version von Windows 2012 R2 (oder höher)

**Wichtige Hinweise:**

* Wenn sich Ihr Gateway während des Setups bei Azure registriert, wird die Standardregion für Ihr Abonnement ausgewählt. Sie können eine andere Region auswählen. Wenn Sie Server in mehreren Regionen haben, müssen Sie für jede Region ein Gateway installieren. 
* Das Gateway darf nicht auf einem Domänencontroller installiert werden.
* Auf einem einzelnen Computer kann nur ein Gateway installiert werden.
* Installieren Sie das Gateway auf einem Computer, der eingeschaltet bleibt und nicht in den Ruhezustand versetzt wird.
* Installieren Sie das Gateway nicht auf einem Computer, der über eine Drahtlosverbindung mit dem Netzwerk verfügt. Die Leistung kann beeinträchtigt werden.


## <a name="download"></a>Herunterladen
 [Gateway herunterladen](https://aka.ms/azureasgateway)

## <a name="install"></a>Installieren

1. Führen Sie das Setup aus.

2. Wählen Sie einen Speicherort, akzeptieren Sie die Lizenzbedingungen, und klicken Sie dann auf **Installieren**.

   ![Installationsspeicherort und Lizenzbedingungen](media/analysis-services-gateway-install/aas-gateway-installer-accept.png)

3. Geben Sie ein Konto für die Anmeldung bei Azure ein. Das Konto muss sich in Ihrem Azure Active Directory-Mandanten befinden. Dieses Konto wird vom Gatewayadministrator verwendet. 

   ![Eingeben eines Kontos für die Anmeldung bei Azure](media/analysis-services-gateway-install/aas-gateway-installer-account.png)

   > [!NOTE]
   > Wenn Sie sich mit einem Domänenkonto anmelden, wird es Ihrem Unternehmenskonto in Azure AD zugeordnet. Ihr Unternehmenskonto wird vom Gatewayadministrator verwendet.

## <a name="register"></a>Registrieren
Um eine Gatewayressource in Azure zu erstellen, müssen Sie die lokale Instanz registrieren, die Sie mit dem Gateway-Clouddienst installiert haben. 

1.  Wählen Sie **Ein neues Gateway auf diesem Computer registrieren** aus.

    ![Registrieren](media/analysis-services-gateway-install/aas-gateway-register-new.png)

2. Geben Sie einen Namen und Wiederherstellungsschlüssel für Ihr Gateway ein. Standardmäßig verwendet das Gateway die Standardregion Ihres Abonnements. Wenn Sie eine andere Region auswählen müssen, wählen Sie **Region ändern** aus.

   ![Registrieren](media/analysis-services-gateway-install/aas-gateway-register-name.png)


## <a name="create-resource"></a>Erstellen einer Azure-Gatewayressource
Nachdem Sie Ihr Gateway registriert und installiert haben, müssen Sie in Ihrem Azure-Abonnement eine Gatewayressource erstellen. Melden Sie sich bei Azure mit dem Konto an, das Sie beim Registrieren des Gateways verwendet haben.

1. Klicken Sie im Azure-Portal auf **Neuen Dienst erstellen** > **Unternehmensintegration** > **Lokales Datengateway** > **E**.

   ![Erstellen einer Gatewayressource](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. Geben Sie in **Verbindungsgateway erstellen** diese Einstellungen ein:

    * **Name**: Geben Sie einen Namen für die Gatewayressource ein. 

    * **Abonnement**: Wählen Sie das Azure-Abonnement aus, das Sie Ihrer Gatewayressource zuordnen möchten. 
    Dieses Abonnement muss mit dem Abonnement für Ihre Server identisch sein.
   
      Das standardmäßige Abonnement basiert auf dem Azure-Konto, das Sie zum Anmelden verwendet haben.

    * **Ressourcengruppe**: Erstellen Sie eine Ressourcengruppe, oder wählen Sie eine vorhandene Ressourcengruppe aus.

    * **Speicherort**: Wählen Sie die Region aus, in der Sie Ihr Gateway registriert haben.

    * **Installationsname**: Wenn Ihre Gatewayinstallation noch nicht ausgewählt ist, wählen Sie das registrierte Gateway aus. 

    Klicken Sie auf **Erstellen**, wenn Sie fertig sind.

## <a name="connect-servers"></a>Verbinden von Servern mit der Gatewayressource

1. Klicken Sie in der Übersicht über Ihre Azure Analysis Services-Server auf **Lokales Datengateway**.

   ![Verbinden des Servers mit dem Gateway](media/analysis-services-gateway-install/aas-gateway-connect-server.png)

2. Wählen Sie in **Lokales Datengateway für die Verbindung auswählen** Ihre Gatewayressource aus, und klicken Sie dann auf **Ausgewähltes Gateway verbinden**.

   ![Verbinden des Servers mit der Gatewayressource](media/analysis-services-gateway-install/aas-gateway-connect-resource.png)

    > [!NOTE]
    > Wenn Ihr Gateway nicht in der Liste nicht angezeigt wird, befindet sich Ihr Server wahrscheinlich nicht in der Region, die Sie beim Registrieren des Gateways angegeben haben. 

Das ist alles. Wenn Sie Ports öffnen oder eine Problembehandlung durchführen müssen, lesen Sie den Artikel zum [lokalen Datengateway](analysis-services-gateway.md).

## <a name="next-steps"></a>Nächste Schritte
* [Verwalten von Analysis Services](analysis-services-manage.md)   
* [Abrufen von Daten aus Azure Analysis Services](analysis-services-connect.md)

