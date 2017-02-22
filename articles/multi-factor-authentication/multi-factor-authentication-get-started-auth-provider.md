---
title: Erste Schritte mit Azure Multi-Factor Authentication-Anbietern | Microsoft Docs
description: Lernen Sie, wie Sie einen Azure Multi-Factor Authentication-Anbieter erstellen.
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: a7dd5030-7d40-4654-8fbd-88e53ddc1ef5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/14/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 4572913c2bc732c31f38021f9d3ccc34417de875
ms.openlocfilehash: be4361af8f3f3024ab7f12114760f6f58e77deaf


---
# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Erste Schritte mit Azure Multi-Factor Authentication-Anbietern
Die Überprüfung in zwei Schritten ist standardmäßig für globale Administratoren, die mit Azure Active Directory arbeiten, und für Office 365-Benutzer verfügbar. Falls Sie die [erweiterten Funktionen](multi-factor-authentication-whats-next.md) nutzen möchten, müssen Sie die Vollversion von Azure Multi-Factor Authentication (MFA) erwerben.

> [!NOTE]
> Ein Azure Multi-Factor Authentication-Anbieter wird verwendet, um die Features zu nutzen, die mit der Vollversion von Azure MFA bereitgestellt werden. Er wird für Benutzer verwendet, die **keine Lizenzen über Azure MFA, Azure AD Premium oder EMS besitzen**.  Azure MFA, Azure AD Premium und EMS enthalten standardmäßig die Vollversion von Azure MFA.  Wenn Sie im Besitz von Lizenzen sind, benötigen Sie keinen Azure Multi-Factor Authentication-Anbieter.

Ein Azure Multi-Factor Authentication-Anbieter ist erforderlich, um das SDK herunterzuladen.

> [!IMPORTANT]
> Erstellen Sie zum Herunterladen des SDK einen Azure MFA-Anbieter, wenn Sie über Azure MFA-, AAD Premium- oder EMS-Lizenzen verfügen.  Wenn Sie zu diesem Zweck einen Azure Multi-Factor Authentication-Anbieter erstellen und bereits über Lizenzen verfügen, sollten Sie den Anbieter mit dem Modell **Pro aktiviertem Benutzer** erstellen. Verknüpfen Sie den Anbieter anschließend mit dem Verzeichnis, in dem die Lizenzen für Azure MFA, Azure AD Premium oder EMS enthalten sind.  So wird sichergestellt, dass nur Kosten berechnet werden, wenn die Anzahl eindeutiger Benutzer, die das SDK verwenden, die Anzahl Ihrer Lizenzen übersteigt.

## <a name="create-a-multi-factor-auth-provider"></a>Erstellen eines Anbieters für mehrstufige Authentifizierung
Führen Sie die folgenden Schritte aus, um einen Azure Multi-Factor Authentication-Anbieter zu erstellen.

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) als Administrator an.
2. Wählen Sie im linken Bereich **Active Directory**aus.
3. Wählen Sie oben auf der Seite „Active Directory“ die Option **Anbieter für mehrstufige Authentifizierung**.
   ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. Klicken Sie dann auf **Neu**.
   ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. Wählen Sie unter App Services die Option **Multi-Factor Auth Provider**.
   ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. Wählen Sie **Schnellerfassung**.
   ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
7. Füllen Sie die folgenden Felder aus, und wählen Sie **Erstellen**.
   1. **Name** : Der Name des Multi-Factor Authentication-Anbieters
   2. **Nutzungsmodell**: Gibt an, ob Einzelbenutzer oder die Zahlung pro Überprüfung aktiviert werden soll. Wählen Sie eine der beiden Optionen:
      * Pro Authentifizierung: Ein Kaufmodell, bei dem die Kosten pro Authentifizierung berechnet werden. Wird in der Regel für Szenarien verwendet, in denen Azure Multi-Factor Authentication in einer kundenorientierten Anwendung verwendet wird.
      * Pro aktiviertem Benutzer: Ein Kaufmodell, bei dem die Kosten pro aktiviertem Benutzer berechnet werden. Wird in der Regel für den Mitarbeiterzugriff auf Anwendungen wie Office 365 verwendet. Wählen Sie diese Option, wenn Sie über einige Benutzer verfügen, die bereits für Azure MFA lizenziert sind.
   3. **Verzeichnis** : Der Azure Active Directory-Mandant, dem der Multi-Factor Authentication-Anbieter zugeordnet ist. Bedenken Sie dabei Folgendes:
      * Zum Erstellen eines Anbieters für mehrstufige Authentifizierung benötigen Sie kein Azure AD-Verzeichnis. Lassen Sie das Feld leer, wenn Sie nur den Azure Multi-Factor Authentication-Server oder das Azure Multi-Factor Authentication-SDK verwenden möchten.
      * Der Multi-Factor Authentication-Anbieter muss einem Azure AD-Verzeichnis zugeordnet werden, damit die erweiterten Features genutzt werden können.
      * Azure AD Connect, AAD Sync oder DirSync ist nur erforderlich, wenn Sie Ihre lokale Active Directory-Umgebung mit einem Azure AD-Verzeichnis synchronisieren.  Wenn Sie nur ein Azure AD-Verzeichnis verwenden, das nicht synchronisiert ist, ist dies nicht erforderlich.
        ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)
8. Nach dem Klicken auf „Erstellen“ wird der Multi-Factor Authentication-Anbieter erstellt und die folgende Meldung angezeigt: **Der Anbieter für Multi-Factor Authentication wurde erstellt**. Klicken Sie auf **OK**.
   ![MFA-Anbieter erstellen](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)




<!--HONumber=Feb17_HO3-->


