---
title: "Azure Active Directory: Häufig gestellte Fragen zur Geräteverwaltung | Microsoft-Dokumentation"
description: "Azure Active Directory: Häufig gestellte Fragen zur Geräteverwaltung."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2017
ms.author: markvi
ms.reviewer: jairoc
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: 8b8ab2883234850fe243c4d7473d737c497204b4
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="azure-active-directory-device-management-faq"></a>Azure Active Directory: Häufig gestellte Fragen zur Geräteverwaltung



**F: Wie kann ich ein macOS-Gerät registrieren?**

**A:** So registrieren Sie ein macOS-Gerät:

1.  [Erstellen Sie eine Konformitätsrichtlinie](https://docs.microsoft.com/intune/compliance-policy-create-mac-os).
2.  [Definieren Sie eine Richtlinie zum bedingten Zugriff für macOS-Geräte](active-directory-conditional-access-azure-portal.md). 

**Hinweise:**

- Die in der Richtlinie zum bedingten Zugriff enthaltenen Benutzer benötigen für den Zugriff auf Ressourcen eine [unterstützte Version von Office für macOS](active-directory-conditional-access-technical-reference.md#supported-mobile-apps-and-desktop-clients). 

- Beim ersten Zugriffsversuch werden die Benutzer aufgefordert, das Gerät über das Unternehmensportal zu registrieren.

---

**F: Ich habe das Gerät vor kurzem registriert. Warum kann ich das Gerät nicht in meinen Benutzerinformationen im Azure-Portal sehen?**

**A:** Windows 10-Geräte, die mit der automatischen Geräteregistrierung in die Domäne eingebunden werden, werden nicht in den Informationen unter BENUTZER angezeigt.
Mit PowerShell können Sie alle Geräte anzeigen. 

Nur die folgenden Geräte werden in den Informationen unter BENUTZER aufgeführt:

- Alle persönlichen Geräte, die nicht in das Unternehmen eingebunden sind 
- Alle Geräte ohne Windows 10/Windows Server 2016 
- Alle Geräte ohne Windows 

---

**F: Warum kann ich im Azure-Portal nicht alle Geräte sehen, die in Azure Active Directory registriert sind?** 

**A:** Derzeit besteht keine Möglichkeit, alle registrierten Geräte im Azure-Portal anzuzeigen. Sie können mit Azure PowerShell alle Geräte finden. Weitere Informationen finden Sie im [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0)-Cmdlet.

--- 

**F: Wie ermittle ich den Geräteregistrierungsstatus des Clients?**

**A:** Der Geräteregistrierungsstatus hängt von folgenden Faktoren ab:

- Art des Geräts
- Registrierung des Geräts 
- Details im Zusammenhang damit 
 

---

**F: Warum ist ein Gerät, das ich im Azure-Portal oder mit Windows PowerShell gelöscht habe, immer noch als registriert aufgeführt?**

**A:** Dies ist beabsichtigt. Das Gerät hat keinen Zugriff auf Ressourcen in der Cloud. Wenn Sie das Gerät entfernen und erneut registrieren möchten, muss dies manuell auf dem Gerät erfolgen. 

Für Windows 10 und Windows Server 2016, die in die lokale AD-Domäne eingebunden sind:

1.  Öffnen Sie die Eingabeaufforderung als Administrator.

2.  Geben Sie `dsregcmd.exe /debug /leave` ein

3.  Melden Sie sich ab und erneut an, um den geplanten Task auszulösen, der das Gerät erneut registriert. 

Für andere Windows-Plattformen, die in die lokale AD-Domäne eingebunden sind:

1.  Öffnen Sie die Eingabeaufforderung als Administrator.
2.  Geben Sie `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`ein.
3.  Geben Sie `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`ein.

---

**F: Warum sehe ich doppelte Geräteeinträge im Azure-Portal?**

**A:**

-   Wenn unter Windows 10 und Windows Server 2016 wiederholt versucht wird, dasselbe Gerät zu entfernen und erneut hinzuzufügen, können doppelte Einträge auftreten. 

-   Wenn Sie „Geschäfts-, Schul- oder Unikonto hinzufügen“ verwendet haben, erstellt jeder Windows-Benutzer, der „Geschäfts-, Schul- oder Unikonto hinzufügen“ verwendet, einen neuen Gerätedatensatz mit demselben Gerätenamen.

-   Andere Windows-Plattformen, die mit der automatischen Registrierung in die lokale AD-Domäne eingebunden sind, erstellen einen neuen Gerätedatensatz mit demselben Gerätenamen für jeden Domänenbenutzer, der sich beim Gerät anmeldet. 

-   Ein AADJ-Computer, der gelöscht, neu installiert und mit demselben Namen wieder eingebunden wurde, wird als anderer Datensatz mit demselben Gerätenamen angezeigt.

---

**F: Warum kann ein Benutzer weiterhin Ressourcen von einem Gerät aufrufen, das ich im Azure-Portal deaktiviert habe?**

**A:** Das Widerrufen kann bis zu einer Stunde dauern.

>[!Note] 
>Bei verlorenen Geräten wird empfohlen, das Gerät zu löschen, um sicherzustellen, dass Benutzer nicht auf das Gerät zugreifen können. Weitere Informationen finden Sie unter [Registrieren von Geräten für die Verwaltung in Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 


---

**F: Warum wird meinen Benutzern angezeigt: „You can’t get there from here“ (Von hier haben Sie darauf keinen Zugriff)?**

**A:** Wenn Sie bestimmte Regeln für bedingten Zugriff konfiguriert haben, die einen spezifischen Gerätestatus erfordern, und das Gerät die Kriterien nicht erfüllt, werden Benutzer blockiert, und sie erhalten diese Meldung. Bitte bewerten Sie die Regeln, und stellen Sie sicher, dass das Gerät die Kriterien erfüllen kann, damit diese Meldung nicht mehr angezeigt wird.

---


**F: Ich sehe den Gerätedatensatz im Azure-Portal in den Informationen unter BENUTZER und sehe, dass der Status auf dem Client „registriert“ lautet. Sind diese Einstellungen für den bedingten Zugriff richtig?**

**A:** Der Gerätedatensatz (deviceID) und der Status im Azure-Portal müssen dem Client entsprechen und sämtliche Auswertungskriterien für den bedingten Zugriff erfüllen. Weitere Details finden Sie unter [Erste Schritte bei der Azure Active Directory-Geräteregistrierung](active-directory-device-registration.md).

---

**F: Warum erscheint die Meldung „Benutzername oder Kennwort ist falsch“ für ein Gerät, das ich vor kurzem in Azure AD eingebunden habe?**

**A:** Häufige Ursachen für dieses Szenario:

- Ihre Benutzeranmeldeinformationen sind nicht mehr gültig.

- Ihr Computer kann nicht mit Azure Active Directory kommunizieren. Suchen Sie nach Netzwerkkonnektivitätsproblemen.

- Die Voraussetzungen für Azure AD Join wurden nicht erfüllt. Stellen Sie sicher, dass Sie die Schritte für [Erweitern von Cloudfunktionen auf Windows 10-Geräte über Azure Active Directory Join](active-directory-azureadjoin-overview.md) befolgt haben.  

- Für Verbundanmeldungen muss der Verbundserver einen aktiven WS-Trust-Endpunkt unterstützen. 

---

**F: Warum sehe ich das Dialogfeld „Oops… an error occurred!“ (Hoppla, ein Fehler ist aufgetreten.), wenn ich versuche, meinen PC einzubinden?**

**A:** Dies ist ein Ergebnis der Einrichtung der Azure Active Directory-Registrierung bei Intune. Weitere Informationen finden Sie unter [Einrichten der Windows-Geräteverwaltung](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**F: Warum konnte ich meinen PC nicht einbinden, obwohl ich keine Fehlerinformationen erhalten habe?**

**A:** Wahrscheinlich ist der Benutzer mit dem integrierten Administratorkonto beim Gerät angemeldet. Erstellen Sie ein anderes lokales Konto, bevor Sie Azure Active Directory Join verwenden, um die Einrichtung abzuschließen. 

---

**F: Wo finde ich Anweisungen zum Einrichten der in Azure AD eingebundenen Hybridgeräte?**

**A:** Ausführliche Anweisungen finden Sie unter [Einrichten von in Azure Active Directory eingebundenen Hybridgeräten](device-management-hybrid-azuread-joined-devices-setup.md).

---


