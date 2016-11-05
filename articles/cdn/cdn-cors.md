---
title: Verwenden von Azure CDN mit CORS | Microsoft Docs
description: Erfahren Sie, wie Sie das Azure Content Delivery Network (CDN), mit Cross-Origin Resource Sharing (Ressourcenfreigabe zwischen verschiedenen Ursprüngen; CORS) verwendet wird.
services: cdn
documentationcenter: ''
author: camsoper
manager: erikre
editor: ''

ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2016
ms.author: casoper

---
# Verwendung von Azure CDN mit CORS
## Was ist CORS?
CORS (Cross Origin Resource Sharing; Ressourcenfreigabe zwischen verschiedenen Ursprüngen) ist eine HTTP-Funktion, die einer Webanwendung, die in einer Domäne ausgeführt wird, den Zugriff auf Ressourcen in einer anderen Domäne ermöglicht. Alle modernen Webbrowser eine Sicherheitseinschränkung, die als [Same Origin Policy](http://www.w3.org/Security/wiki/Same_Origin_Policy) bekannt ist, um die Möglichkeiten für Cross-Site Scripting-Angriffe zu verringern. Dies hindert eine Website daran, APIs in einer anderen Domäne aufzurufen. CORS bietet eine sichere Methode, um einer Domäne (der Ursprungsdomäne) den Aufruf von APIs in einer anderen Domäne zu erlauben.

## So funktioniert's
1. Der Browser sendet die OPTIONS-Anforderung mit einem **Ursprung**-HTTP-Header. Der Wert dieses Headers ist die Domäne, die der übergeordnete Seite dient. Wenn eine Seite aus https://www.contoso.com versucht, auf die Benutzerdaten in der fabrikam.com-Domäne zuzugreifen, würde der folgende Anforderungsheader an fabrikam.com gesendet:
   
   `Origin: https://www.contoso.com`
2. Der Server reagiert möglicherweise mit Folgendem:
   
   * Ein **Access-Control-Allow-Origin**-Header in der Antwort gibt an, welche Ursprungswebsites zulässig sind. Beispiel:
     
     `Access-Control-Allow-Origin: https://www.contoso.com`
   * Eine Fehlerseite wird angezeigt, wenn der Server die Anforderungen zwischen verschiedenen Ursprüngen nicht zulässt.
   * Ein **Access-Control-Allow-Origin**-Header mit einem Platzhalter, der alle Domänen ermöglicht:
     
     `Access-Control-Allow-Origin: *`

Für komplexe HTTP-Anforderungen besteht eine „Preflight“-Anforderung, die als erstes ausgeführt wird, um festzustellen, ob sie berichtigt ist, bevor die gesamte Anforderung gesendet wird.

## Platzhalter oder Szenarien mit nur einem Ursprung
CORS für Azure CDN funktioniert automatisch ohne zusätzliche Konfiguration, wenn der **Access-Control-Allow-Origin**-Header auf Platzhalter (*) oder einen einzelnen Ursprung festgelegt ist. Das CDN speichert die erste Antwort zwischen und nachfolgende Anforderungen verwenden den gleichen Header.

Wenn Anforderungen schon an CDN, gesendet wurden, bevor CORS auf den Ursprung konfiguriert wurde, müssen Sie den Inhalt im Inhalt Ihres Endgeräts entfernen, um den Inhalt mit dem **Access-Control-Allow-Origin**-Header erneut zu laden.

## Szenarien mit mehreren Ursprüngen
Wenn Sie eine bestimmte Liste von Ursprüngen für CORS zulassen müssen, wird es etwas komplizierter. Das Problem tritt auf, wenn CDN den **Access-Control-Allow-Origin**-Header für den ersten CORS-Ursprung zwischenspeichert. Wenn ein anderer CORS-Ursprung eine nachfolgende Anforderung erstellt, stellt das CDN den zwischengespeicherten **Access-Control-Allow-Origin**-Header bereit, der jedoch abweicht. Es gibt mehrere Möglichkeiten, dies zu korrigieren:

### Azure CDN Premium von Verizon
Die beste Möglichkeit ist die Verwendung von **Azure CDN Premium von Verizon**, wodurch erweiterte Funktionen geboten werden.

Sie müssen [eine Regel erstellen](cdn-rules-engine.md), um den **Ursprungsheader** in der Anforderung zu überprüfen. Wenn es sich hierbei um einen gültigen Ursprung handelt, legt Ihre Regel den **Access-Control-Allow-Origin**-Header mit dem in der Anforderung bereitgestellten Ursprung fest. Wenn der im **Ursprungsheader** angegebene Ursprung nicht zulässig ist, sollte Ihre Regel den **Access-Control-Allow-Origin**-Header auslassen, der den Brower dazu bringt, die Anforderung abzulehnen.

Es gibt zwei Möglichkeiten, dies mit dem Regelmodul zu tun. In beiden Fällen wird der **Access-Control-Allow-Origin**-Header des Ursprungsservers der Datei ignoriert und das CDN-Regelmodul verwaltet komplett die zulässigen CORS-Ursprünge.

#### Ein regulärer Ausdruck mit allen gültigen Ursprüngen
In diesem Fall erstellen Sie einen regulären Ausdruck, der alle Ursprünge enthält, die Sie zulassen möchten:

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$

> [!TIP]
> **Azure CDN von Verizon** verwendet [Perl Compatible Regular Expressions](http://pcre.org/) als Modul für reguläre Ausdrücke. Sie können ein Tool wie [Regular Expressions 101](https://regex101.com/) verwenden, um Ihre regulären Ausdrücke zu überprüfen. Beachten Sie, dass das Zeichen „/“ in regulären Ausdrücken zulässig ist und nicht mit Escapezeichen versehen werden muss. Dieses Zeichen in Escapezeichen zu setzen ist jedoch die beste Methode und wird von einigen RegEx-Validierern erwartet.
> 
> 

Wenn der reguläre Ausdruck übereinstimmt, ersetzt Ihre Regel den **Access-Control-Allow-Origin**-Header (sofern vorhanden) des Ursprungs mit dem des Ursprungs, der die Anforderung gesendet hat. Sie können zusätzliche CORS-Header hinzufügen, wie z.B. **Access-Control-Allow-Methods**.

![Beispiel für Regeln mit regulären Ausdruck](./media/cdn-cors/cdn-cors-regex.png)

#### Anforderungsheader-Regel für jeden Ursprung
Statt reguläre Ausdrücke können Sie stattdessen eine separate Regel für jeden Ursprung erstellen, den sie zulassen wollen. Verwenden Sie dazu die **Übereinstimmungsbedingung** des [Request Header-Platzhalters](cdn-rules-engine-details.md#match-conditions). Wie bei der Methode des regulären Ausdrucks legt das Regelmodul allein die CORS-Header fest.

![Beispiel für Regeln ohne regulären Ausdruck](./media/cdn-cors/cdn-cors-no-regex.png)

> [!TIP]
> Im obigen Beispiel weist die Verwendung des Platzhalterzeichens „*“ das Regelmodul an, sowohl HTTP als auch HTTPS abzugleichen.
> 
> 

### Azure CDN-Standard
Auf Azure CDN-Standardprofilen ist der einzige Mechanismus, um mehrere Ursprünge ohne Verwendung des Platzhalterursprungs zuzulassen, die Verwendung der [Zwischenspeicherung von Abfragezeichenfolgen](cdn-query-string.md). Sie müssen Abfragezeichenfolgen für den CDN-Endpunkt aktivieren und anschließend eine eindeutige Abfragezeichenfolge für Anforderungen von jeder zulässigen Domäne verwenden. Dadurch speichert das CDN ein separates Objekt für jede eindeutige Abfragezeichenfolge zwischen. Dieser Ansatz ist jedoch nicht ideal, da so mehrere Kopien derselben Datei im CDN zwischengespeichert werden.

<!---HONumber=AcomDC_0803_2016-->