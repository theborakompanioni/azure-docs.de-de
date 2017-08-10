---
title: 'Azure Active Directory B2C: Benutzerdefinierte Attribute | Microsoft Docs'
description: "Informationen zur Verwendung von benutzerdefinierten Attributen in Azure Active Directory B2C zum Erfassen von Informationen über Ihre Kunden"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 055ffb0a-197b-4716-8dad-1fd8a01e174f
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: 356aaeff3a78fc7b682d621e8e0de9312582b2fe
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Azure Active Directory B2C: Verwenden benutzerdefinierter Attribute zum Erfassen von Informationen über Ihre Kunden
Das Azure Active Directory (Azure AD) B2C-Verzeichnis bietet einen integrierten Satz von Informationen (Attributen): Vorname, Nachname, Ort, Postleitzahl und weitere Attribute. Allerdings hat jede kundenorientierte Anwendung eigene Anforderungen an die Attribute, die von Kunden erfasst werden sollen. Mit Azure AD B2C haben Sie die Möglichkeit, den für die einzelnen Kundenkonten gespeicherten Satz von Attributen zu erweitern. Im [Azure-Portal](https://portal.azure.com/) können Sie benutzerdefinierte Attribute erstellen und wie unten dargestellt in den Registrierungsrichtlinien verwenden. Außerdem können Sie diese Attribute mit der [Azure AD Graph-API](active-directory-b2c-devquickstarts-graph-dotnet.md)lesen und schreiben.

> [!NOTE]
> Benutzerdefinierte Attribute verwenden die [Verzeichnisschemaerweiterungen der Azure AD Graph-API](https://msdn.microsoft.com/library/azure/dn720459.aspx).
> 
> 

## <a name="create-a-custom-attribute"></a>Erstellen eines benutzerdefinierten Attributs
1. [Führen Sie diese Schritte aus, um im Azure-Portal zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klicken Sie auf **Benutzerattribute**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
4. Geben Sie im Feld **Name** den Namen für das benutzerdefinierte Attribut (z.B. „ShoeSize“) und optional eine **Beschreibung** ein. Klicken Sie auf **Erstellen**.
   
   > [!NOTE]
   > Derzeit ist nur der **Datentyp** „String“ verfügbar.
   > 
   > 

Das benutzerdefinierte Attribut steht jetzt in der Liste der **Benutzerattribute**zur Verfügung und kann in den Registrierungsrichtlinien verwendet werden.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Verwenden eines benutzerdefinierten Attributs in der Registrierungsrichtlinie
1. [Führen Sie diese Schritte aus, um im Azure-Portal zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klicken Sie auf **Registrierungsrichtlinien**.
3. Klicken Sie auf die Registrierungsrichtlinie (z. B. „B2C_1_SiUp“), um sie zu öffnen. Klicken Sie oben auf dem Blatt auf **Bearbeiten** .
4. Klicken Sie auf **Registrierungsattribute**, und wählen Sie das benutzerdefinierte Attribut aus (z.B. „ShoeSize“). Klicken Sie auf **OK**.
5. Klicken Sie auf **Anwendungsansprüche** , und wählen Sie das benutzerdefinierte Attribut aus. Klicken Sie auf **OK**.
6. Klicken Sie oben auf dem Blatt auf **Speichern** .

Mit dem Feature "Jetzt ausführen" für die Richtlinie können Sie die Benutzererfahrung überprüfen. „ShoeSize“ sollte jetzt in der Liste der Attribute angezeigt werden, die während der Registrierung von Kunden erfasst werden, sowie in dem Token, das zurück an die Anwendung gesendet wird.

## <a name="notes"></a>Hinweise
* Zusammen mit Registrierungsrichtlinien können benutzerdefinierte Attribute auch in Registrierungs- oder Anmelderichtlinien sowie in Richtlinien für die Profilbearbeitung verwendet werden.
* Die Einschränkung von benutzerdefinierten Attributen ist bekannt. Sie wird nur erstellt, wenn sie zum ersten Mal in einer Richtlinie verwendet wird und nicht, wenn Sie sie zur Liste der **Benutzerattribute**hinzufügen.


