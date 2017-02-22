---
title: Upgraden von PhoneFactor auf den Azure MFA-Server | Microsoft-Dokumentation
description: "Informieren Sie sich über die ersten Schritte mit dem Azure MFA-Server, wenn Sie ein Upgrade des älteren PhoneFactor-Agents ausführen möchten."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 42838ff7-bdf2-4d06-bacc-b3839a00cd76
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/06/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 4547a805c1827a703bf0ef118387882e45c3f241
ms.openlocfilehash: 5bbd7088f5d4986a5b1c9f5695a5defb678307a9


---
# <a name="upgrade-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>Upgraden des PhoneFactor-Agents auf den Azure Multi-Factor Authentication-Server
Wenn Sie den PhoneFactor-Agent v5.x oder eine ältere Version auf den Azure Multi-Factor Authentication-Server upgraden möchten, müssen Sie zunächst den PhoneFactor-Agent und die dazugehörigen Komponenten deinstallieren. Anschließend können Sie den Multi-Factor Authentication-Server und die dazugehörigen Komponenten installieren.

## <a name="uninstall-the-phonefactor-agent"></a>Deinstallieren des PhoneFactor-Agents

1. Sichern Sie zunächst die PhoneFactor-Datendatei. Der Standardinstallationspfad lautet "C:\Programme\PhoneFactor\Data\Phonefactor.pfdata".

2. Wenn das Benutzerportal installiert ist:
  1. Navigieren Sie zum Installationsordner, und sichern Sie die Datei "Web.config". Der Standardinstallationspfad lautet "C:\inetpub\wwwroot\PhoneFactor".

  2. Wenn Sie dem Portal benutzerdefinierte Designs hinzugefügt haben, sichern Sie den benutzerdefinierten Ordner unter dem Verzeichnis "C:\inetpub\wwwroot\PhoneFactor\App_Themes".

  3. Deinstallieren Sie das Benutzerportal entweder über den PhoneFactor-Agent (nur verfügbar, wenn auf demselben Server wie der PhoneFactor-Agent installiert) oder über "Programme und Funktionen" von Windows.

3. Wenn der Webdienst der mobilen App installiert ist:

  1. Navigieren Sie zum Installationsordner, und sichern Sie die Datei "Web.config". Der Standardinstallationspfad lautet „C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService“.

  2. Deinstallieren Sie den Webdienst der mobilen App über „Programme und Funktionen“ von Windows.

4. Wenn das Webdienst-SDK installiert ist, deinstallieren Sie es über den PhoneFactor-Agent oder über "Programme und Funktionen" von Windows.

5. Deinstallieren Sie den PhoneFactor-Agent über "Programme und Funktionen" von Windows.

## <a name="install-the-multi-factor-authentication-server"></a>Installieren des Multi-Factor Authentication-Servers

Der Installationspfad wird aus der Registrierung der vorherigen PhoneFactor-Agent-Installation übernommen. Die Installation erfolgt also unter dem gleichen Pfad (beispielsweise „C:\Programme\PhoneFactor“). Für Neuinstallationen wird ein anderer Standardinstallationspfad verwendet (z. B. "C:\Programme\Multi-Factor Authentication Server"). Während der Installation wird die noch vorhandene PhoneFactor-Agent-Datendatei aktualisiert. Deshalb sind nach dem Installieren des neuen Multi-Factor Authentication-Servers die Benutzer und Einstellungen noch vorhanden.

1. Aktivieren Sie den Multi-Factor Authentication-Server, wenn Sie dazu aufgefordert werden, und stellen Sie sicher, dass er der richtigen Replikationsgruppe zugewiesen ist.

2. Wenn das Webdienst-SDK zuvor installiert war, installieren Sie das neue Webdienst-SDK über die Benutzeroberfläche des Multi-Factor Authentication-Servers.

  Der Standardname des virtuellen Verzeichnisses lautet jetzt „MultiFactorAuthWebServiceSdk“ und nicht mehr „PhoneFactorWebServiceSdk“. Wenn Sie den vorherigen Namen verwenden möchten, müssen Sie den Namen des virtuellen Verzeichnisses während der Installation ändern. Wenn Sie hingegen die Verwendung des neuen Standardnamens während der Installation zulassen, müssen Sie die URL in allen Anwendungen ändern, die auf das Webdienst-SDK verweisen, z. B. im Benutzerportal und im Webdienst der mobilen App, damit sie auf das richtige Verzeichnis verweist.

3. Wenn das Benutzerportal zuvor auf dem PhoneFactor-Agent-Server installiert war, installieren Sie das neue Multi-Factor Authentication-Benutzerportal über die Benutzeroberfläche des Multi-Factor Authentication-Servers.

  Der Standardname des virtuellen Verzeichnisses lautet jetzt „MultiFactorAuth“ und nicht mehr „PhoneFactor“. Wenn Sie den vorherigen Namen verwenden möchten, müssen Sie den Namen des virtuellen Verzeichnisses während der Installation ändern. Wenn Sie hingegen die Verwendung des neuen Standardnamens bei der Installation zulassen, klicken Sie im Multi-Factor Authentication-Server auf das Benutzerportal-Symbol, und aktualisieren Sie auf der Registerkarte "Einstellungen" die Benutzerportal-URL.

4. Wenn das Benutzerportal und/oder der Webdienst der mobilen App zuvor auf einem anderen Server als dem PhoneFactor-Agent installiert wurde:

  1. Navigieren Sie zum Installationspfad (z.B. „C:\Programme\PhoneFactor“), und kopieren Sie die geeigneten Installationsprogramme auf den anderen Server. Für das Benutzerportal und den Webdienst der mobilen App gibt es 32-Bit- und 64-Bit-Installationsprogramme. Ihre Namen lauten „MultiFactorAuthenticationUserPortalSetupXX.msi“ bzw. „MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi“.

  2. Um das Benutzerportal auf dem Webserver zu installieren, öffnen Sie eine Eingabeaufforderung als Administrator, und führen Sie „MultiFactorAuthenticationUserPortalSetupXX.msi“ aus.

    Der Standardname des virtuellen Verzeichnisses lautet jetzt „MultiFactorAuth“ und nicht mehr „PhoneFactor“. Wenn Sie den vorherigen Namen verwenden möchten, müssen Sie den Namen des virtuellen Verzeichnisses während der Installation ändern. Wenn Sie hingegen die Verwendung des neuen Standardnamens bei der Installation zulassen, klicken Sie im Multi-Factor Authentication-Server auf das Benutzerportal-Symbol, und aktualisieren Sie auf der Registerkarte "Einstellungen" die Benutzerportal-URL. Vorhandene Benutzer müssen über die neue URL informiert werden.

  3. Wechseln Sie zum Installationsspeicherort des Benutzerportals (z.B. „C:\inetpub\wwwroot\MultiFactorAuth“), und bearbeiten Sie die Datei „web.config“. Kopieren Sie die Werte in den Abschnitten "appSettings" und "applicationSettings" aus der ursprünglichen Datei "web.config", die vor dem Upgrade auf die neue Datei "web.config" gesichert wurde. Wenn beim Installieren des Webdienst-SDK der neue Standardname des virtuellen Verzeichnisses beibehalten wurde, ändern Sie die URL im Abschnitt "applicationSettings", damit sie auf den richtigen Speicherort verweist. Wenn in der vorherigen Datei „web.config“ weitere Standardeinstellungen geändert wurden, wenden Sie diese Änderungen auf die neue Datei „web.config“ an.

  4. Um den Webdienst der mobilen App auf dem Webserver zu installieren, öffnen Sie eine Eingabeaufforderung als Administrator, und führen Sie „MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi“ aus.

    Der Standardname des virtuellen Verzeichnisses lautet jetzt „MultiFactorAuthMobileAppWebService“ und nicht mehr „PhoneFactorPhoneAppWebService“. Wenn Sie den vorherigen Namen verwenden möchten, müssen Sie den Namen des virtuellen Verzeichnisses während der Installation ändern. Sie sollten einen kürzeren Namen wählen, damit er von den Endbenutzern einfacher auf ihren Mobilgeräten eingegeben werden kann. Wenn Sie hingegen die Verwendung des neuen Standardnamens bei der Installation zulassen, klicken Sie auf dem Multi-Factor Authentication-Server auf das Symbol „Mobile App“, und aktualisieren Sie die URL des Webdiensts der mobilen App.

  5. Wechseln Sie zum Installationsspeicherort des Webdiensts der mobilen App (z.B. „C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService“), und bearbeiten Sie die Datei „web.config“. Kopieren Sie die Werte in den Abschnitten "appSettings" und "applicationSettings" aus der ursprünglichen Datei "web.config", die vor dem Upgrade auf die neue Datei "web.config" gesichert wurde. Wenn beim Installieren des Webdienst-SDK der neue Standardname des virtuellen Verzeichnisses beibehalten wurde, ändern Sie die URL im Abschnitt "applicationSettings", damit sie auf den richtigen Speicherort verweist. Wenn in der vorherigen Datei „web.config“ weitere Standardeinstellungen geändert wurden, wenden Sie diese Änderungen auf die neue Datei „web.config“ an.

## <a name="next-steps"></a>Nächste Schritte

- [Installieren Sie das Benutzerportal](multi-factor-authentication-get-started-portal.md) für den Azure Multi-Factor Authentication-Server.

- [Konfigurieren Sie die Windows-Authentifizierung](multi-factor-authentication-get-started-server-windows.md) für Ihre Anwendungen. 



<!--HONumber=Feb17_HO1-->


