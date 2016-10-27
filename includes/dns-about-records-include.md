## <a name="about-records"></a>Informationen zu Einträgen

Jeder DNS-Eintrag hat einen Namen und einen Typ. Datensätze werden anhand der darin enthaltenen Daten nach verschiedenen Typen unterteilt. Der häufigste Typ ist ein A-Eintrag, der einer IPv4-Adresse einen Namen zuordnet. Ein weiterer Typ ist ein MX-Eintrag, der einem Mailserver einen Namen zuordnet.

Azure DNS unterstützt alle allgemeinen DNS-Eintragstypen, z.B. A, AAAA, CNAME, MX, NS, PTR, SOA, SRV und TXT. Beachten Sie Folgendes:
- SOA-Ressourceneintragssätze werden automatisch mit jeder Zone erstellt; sie können nicht separat erstellt werden.
- SPF-Einträge sollten mit dem TXT-Eintragstyp erstellt werden. Weitere Informationen finden Sie auf [dieser Seite](http://tools.ietf.org/html/rfc7208#section-3.1).

Einträge in Azure DNS werden mit relativen Namen angegeben. Ein „vollqualifizierter“ Domänenname (FQDN) beinhaltet den Zonennamen, ein „relativer“ Name hingegen nicht. Der relative Eintragsname „www“ in der Zone „contoso.com“ gibt beispielsweise den vollqualifizierten Eintragsnamen „www.contoso.com“ an.

## <a name="about-record-sets"></a>Informationen zu Ressourceneintragssätzen

In einigen Fällen müssen Sie mehrere DNS-Einträge mit einem bestimmten Namen und Typ erstellen. Angenommen, die Website „www.contoso.com“ wird auf zwei verschiedenen IP-Adressen gehostet. Für diese Website sind zwei verschiedene A-Einträge erforderlich (einer für jede IP-Adresse). Dies ist ein Beispiel für einen Ressourceneintragssatz:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS verwaltet die DNS-Einträge mithilfe von Ressourceneintragssätzen. Bei einem Ressourceneintragssatz handelt es sich um die Auflistung von DNS-Einträgen in einer Zone, die den gleichen Namen und den gleichen Typ aufweisen. Die meisten Ressourceneintragssätze enthalten nur einen einzelnen Eintrag. Aber Beispiele wie dieses, in dem ein Ressourceneintragssatz mehr als einen Eintrag enthält, sind nicht ungewöhnlich.

SOA- und CNAME-Ressourceneintragssätze stellen hierbei Ausnahmen dar. Die DNS-Standards lassen mehrere Datensätze mit demselben Namen für diese Typen nicht zu.

Die Gültigkeitsdauer (TTL oder Time-to-Live) gibt an, wie lange jeder Eintrag von den Clients zwischengespeichert wird, bevor er erneut abgefragt wird. In diesem Beispiel beträgt der TTL-Wert 3.600 Sekunden bzw. 1 Stunde. Der TTL-Wert wird für den Ressourceneintragssatz und nicht für jeden Eintrag angegeben. Dadurch wird der gleiche Wert für alle Einträge in diesem Ressourceneintragssatz verwendet.

#### <a name="wildcard-record-sets"></a>Platzhalter-Ressourceneintragssätze

Azure DNS unterstützt [Platzhalterdatensätze](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Diese werden für alle Abfragen mit einem übereinstimmenden Namen zurückgegeben (es sei denn, es gibt eine genauere Übereinstimmung aus einem Ressourceneintragssatz ohne Platzhalter). Ressourceneintragssätze mit Platzhaltern werden für alle Eintragstypen mit Ausnahme von NS und SOA unterstützt.  

Verwenden Sie zum Erstellen eines Ressourceneintragssatzes mit Platzhaltern den Ressourceneintragssatz-Namen „\*“. Oder verwenden Sie einen Namen mit „\*“, z.B. „\*.foo“.

#### <a name="cname-record-sets"></a>CNAME-Ressourceneintragssätze

CNAME-Ressourceneintragssätze können nicht gleichzeitig neben anderen Ressourceneintragssätzen mit dem gleichen Namen vorhanden sein. Sie können einen CNAME-Ressourceneintragssatz z.B. nicht mit dem relativen Namen „www“ und gleichzeitig einen A-Eintrag mit dem relativen Namen „www“ erstellen. Da die Zonenspitze (Name = ‘@’)) immer die NS- und SOA-Ressourceneintragssätze enthält, die beim Erstellen der Zone erstellt wurden, können Sie keinen CNAME-Ressourceneintragssatz an der Zonenspitze erstellen. Diese Einschränkungen ergeben sich aus den DNS-Standards und sind keine Einschränkungen von Azure DNS.


<!--HONumber=Oct16_HO2-->


