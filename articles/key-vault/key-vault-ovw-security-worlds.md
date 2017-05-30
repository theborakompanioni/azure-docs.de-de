---
ms.assetid: 
title: Azure Key Vault-Sicherheitsumgebungen (Security Worlds) | Microsoft Docs
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e92b1b234e4ceea5e0dd5d09ab3203c4a86f633
ms.openlocfilehash: 6477e8d91b71361ef91763418b22596deb216f02
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a>Sicherheitsumgebungen und geografische Grenzen von Azure Key Vault

Azure Key Vault ist ein mehrinstanzenfähiger Dienst, der einen Pool von Hardwaresicherheitsmodulen (Hardware Security Modules, HSMs) an jedem Azure-Standort verwendet. 

Alle HSMs an Azure-Standorten in der gleichen geografischen Region verwenden dieselbe kryptografische Grenze (Thales Security World). Beispielsweise verwenden „USA, Osten“ und „USA, Westen“ dieselbe Sicherheitsumgebung, weil sie dem geografischen Standort USA angehören. Ebenso verwenden alle Azure-Standorte in Japan und alle Azure-Standorte in Australien, Indien und so weiter jeweils die gleiche Sicherheitsumgebung. 

Eine Sicherung, die von einem Schlüssel aus einem Schlüsseltresor an einem Azure-Standort erstellt wurde, kann in einem Schlüsseltresor an einem anderen Azure-Standort wiederhergestellt werden, sofern die zwei folgenden Bedingungen erfüllt sind:

- Beide Azure-Standorte gehören derselben geografischen Region an.
- Beide Schlüsseltresore gehören zum selben Azure-Abonnement.

Beispiel: Eine Sicherung, die von einem bestimmten Abonnement von einem Schlüssel in einem Schlüsseltresor in Westindien erstellt wurde, kann nur in einem anderen Schlüsseltresor im selben Abonnement und in derselben geografischen Region (Westindien, Zentralindien oder Südindien) wiederhergestellt werden. 



