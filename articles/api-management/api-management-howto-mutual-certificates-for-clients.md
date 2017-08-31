---
title: "Sichern von APIs über die Clientzertifikatauthentifizierung in API Management – Azure API Management | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie den Zugriff auf APIs mithilfe von Clientzertifikaten sichern können."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: apimpm
ms.translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 7f1d55b90af4e5397d74a8e37b44b5a88530897d
ms.contentlocale: de-de
ms.lasthandoff: 03/31/2017

---

# <a name="how-to-secure-apis-using-client-certificate-authentication-in-api-management"></a>Sichern von APIs über eine Clientzertifikatauthentifizierung in API Management

API Management bietet die Möglichkeit, den Zugriff auf APIs (d.h. vom Client auf API Management) mithilfe von Clientzertifikaten zu sichern. Derzeit können Sie den Fingerabdruck eines Clientzertifikats anhand eines gewünschten Werts prüfen. Sie können den Fingerabdruck auch anhand vorhandener Zertifikate prüfen, die auf API Management hochgeladen wurden.  

Informationen zum Sichern des Zugriffs auf den Back-End-Dienst einer API mithilfe von Clientzertifikaten (d.h. von API Management auf Back-End) finden Sie unter [Sichern von Back-End-Diensten über eine Clientzertifikatauthentifizierung](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates).

## <a name="checking-the-expiration-date"></a>Prüfen des Ablaufdatums

Die unten stehenden Richtlinien können konfiguriert werden, um zu prüfen, ob das Zertifikat abgelaufen ist:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.NotAfter < DateTime.Now)" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-the-issuer-and-subject"></a>Prüfen des Ausstellers und des Antragstellers

Die unten stehenden Richtlinien konfiguriert werden, um den Aussteller und den Antragsteller eines Clientzertifikats zu prüfen:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-the-thumbprint"></a>Prüfen des Fingerabdrucks

Die folgenden Richtlinien können zum Prüfen des Fingerabdrucks eines Clientzertifikats konfiguriert werden:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-a-thumbprint-against-certificates-uploaded-to-api-management"></a>Prüfen eines Fingerabdrucks anhand von auf API Management hochgeladenen Zertifikaten

Das folgende Beispiel zeigt, wie Sie den Fingerabdruck eines Clientzertifikats anhand von auf API Management hochgeladenen Zertifikaten prüfen können: 

```
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

## <a name="next-step"></a>Nächster Schritt

*  [Sichern von Back-End-Diensten über eine Clientzertifikatauthentifizierung](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)
*  [Hochladen von Zertifikaten](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates#a-namestep1-aupload-a-client-certificate)


