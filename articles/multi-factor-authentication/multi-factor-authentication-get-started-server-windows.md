---
title: Windows-Authentifizierung und Azure MFA-Server | Microsoft-Dokumentation
description: "Dies ist die Azure Multi-Factor Authentication-Seite, die bei der Bereitstellung der Windows-Authentifizierung und des Azure Multi-Factor Authentication-Servers Unterstützung bietet."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 19a4043f-c4ce-43c0-80e7-2548ee92cb74
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/06/2017
ms.author: joflore
ms.reviewer: yossib
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: e66b606433f8924a509f2d04dae67ff00ded6dca
ms.openlocfilehash: e3ad33dd12b4fb831ba43738bc5af4e5561a0682
ms.contentlocale: de-de
ms.lasthandoff: 02/07/2017

---
# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>Windows-Authentifizierung und Azure Multi-Factor Authentication-Server
Die Windows-Authentifizierung für Anwendungen können Sie mithilfe des Windows-Authentifizierungsabschnitts des Azure Multi-Factor Authentication-Servers aktivieren und konfigurieren. Bedenken Sie vor dem Einrichten der Windows-Authentifizierung Folgendes:

* Nach der Einrichtung muss Azure Multi-Factor Authentication für Terminaldienste neu gestartet werden.
* Wenn "Multi-Factor Authentication-Benutzerabgleich erforderlich" aktiviert ist und Sie nicht in der Benutzerliste aufgeführt sind, können Sie sich nach dem Neustart des Computers nicht anmelden.
* Vertrauenswürdige IPs hängen davon ab, ob die Anwendung der Client-IP die Authentifizierung bereitstellen kann. Derzeit werden nur Terminaldienste unterstützt.  

> [!NOTE]
> Dieses Feature wird nicht unterstützt, um Terminaldienste unter Windows Server 2012 R2 zu sichern.

## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a>Um eine Anwendung mit der Windows-Authentifizierung zu sichern, verwenden Sie das folgende Verfahren.
1. Klicken Sie im Azure Multi-Factor Authentication-Server auf das Symbol "Windows-Authentifizierung".
   ![Windows-Authentifizierung](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)
2. Aktivieren Sie das Kontrollkästchen **Windows-Authentifizierung aktivieren**. Standardmäßig ist dieses Kontrollkästchen deaktiviert.
3. Die Registerkarte "Anwendungen" gibt dem Administrator die Möglichkeit, eine oder mehrere Anwendungen für die Windows-Authentifizierung zu konfigurieren.
4. Wählen Sie einen Server oder eine Anwendung, und geben Sie an, ob der Server/die Anwendung aktiviert ist. Klicken Sie auf **OK**.
5. Klicken Sie auf **Hinzufügen...**.
6. Auf der Registerkarte "Vertrauenswürdige IP-Adressen" können Sie Azure Multi-Factor Authentication für Windows-Sitzungen überspringen, die aus bestimmten IPs stammen. Wenn Mitarbeiter die Anwendung im Büro und zu Hause verwenden, können Sie z. B. entscheiden, dass deren Telefone für Azure Multi-Factor Authentication im Büro nicht klingeln sollen. Dazu geben Sie das Bürosubnetz als Eintrag "Vertrauenswürdige IPs" an.
7. Klicken Sie auf **Hinzufügen...**.
8. Wählen Sie **Einzelne IP-Adresse** aus, wenn Sie eine einzelne IP-Adresse überspringen möchten.
9. Wählen Sie **IP-Bereich** aus, wenn Sie einen ganzen IP-Bereich überspringen möchten. Beispiel 10.63.193.1-10.63.193.100.
10. Wählen Sie **Subnetz** aus, wenn Sie einen IP-Bereich mithilfe der Subnetznotation angeben möchten. Geben Sie die Start-IP des Subnetzes an, und wählen Sie die entsprechende Netzmaske aus der Dropdown-Liste.
11. Klicken Sie auf **OK**.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Drittanbieter-VPN-Appliances für Azure MFA-Server](multi-factor-authentication-advanced-vpn-configurations.md)

- [Verbessern der vorhandenen Authentifizierungsinfrastruktur mit der NPS-Erweiterung für Azure MFA](multi-factor-authentication-nps-extension.md)

