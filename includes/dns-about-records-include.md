## Informationen zu Einträgen

Jeder DNS-Eintrag hat einen Namen und einen Typ. Es gibt verschiedene Eintragstypen, je nach den darin enthaltenen Daten. Der häufigste Typ ist ein A-Eintrag, der einer IPv4-Adresse einen Namen zuordnet. Ein weiterer Typ ist ein MX-Eintrag, der einem Mailserver einen Namen zuordnet.

Azure DNS unterstützt alle allgemeine DNS-Eintragstypen: A, AAAA, CNAME, MX, NS, SOA, SRV und TXT. Datensatzgruppen vom Typ SOA werden automatisch mit jeder Zone erstellt. Sie können nicht separat erstellt werden. Beachten Sie, dass SPF-Einträge mit dem TXT-Eintragstyp erstellt werden sollten. Weitere Informationen finden Sie auf [dieser Seite](http://tools.ietf.org/html/rfc7208#section-3.1).

Einträge in Azure DNS werden mit relativen Namen angegeben. Ein „vollqualifizierter“ Domänenname (FQDN) beinhaltet den Zonennamen, ein „relativer“ Name hingegen nicht. Der relative Eintragsname „www“ in der Zone „contoso.com“ gibt beispielsweise den vollqualifizierten Eintragsnamen „www.contoso.com“ an.

## Informationen zu Ressourceneintragssätzen

In einigen Fällen müssen Sie mehrere DNS-Einträge mit einem bestimmten Namen und Typ erstellen. Angenommen, die Website "www.contoso.com" wird auf zwei verschiedenen IP-Adressen gehostet. Dazu sind zwei verschiedene A-Einträge erforderlich, einer für jede IP-Adresse. Dies ist ein Beispiel für eine Datensatzgruppe.

	www.contoso.com.		3600	IN	A	134.170.185.46
	www.contoso.com.		3600	IN	A	134.170.188.221

Azure DNS verwaltet die DNS-Einträge mithilfe von Datensatzgruppen. Eine Datensatzgruppe ist die Auflistung von DNS-Einträgen in einer Zone mit dem gleichen Namen und dem gleichen Typ. Die meisten Datensatzgruppen enthalten einen einzelnen Eintrag, aber Beispiele wie das obige, in dem eine Datensatzgruppe mehr als einen Eintrag enthält, sind nicht ungewöhnlich.

Ressourceneintragssätze vom Typ SOA und CNAME stellen eine Ausnahme dar; die DNS-Standards lassen für diese Typen keine Mehrfacheinträge mit gleichem Namen zu.

Die Gültigkeitsdauer (TTL oder Time-to-Live) gibt an, wie lange jeder Eintrag von den Clients zwischengespeichert wird, bevor er erneut abgefragt wird. Im obigen Beispiel ist der TTL-Wert 3600 Sekunden oder 1 Stunde. Der TTL-Wert wird für die Datensatzgruppe, nicht für jeden Eintrag angegeben, dadurch wird der gleiche Wert für alle Einträge in dieser Datensatzgruppe verwendet.

#### Platzhalter-Ressourceneintragssätze

Azure DNS unterstützt [Platzhalterdatensätze](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Diese werden für alle Abfragen mit einem übereinstimmenden Namen zurückgegeben (es sei denn, es gibt eine genauere Übereinstimmung aus einer Datensatzgruppe ohne Platzhalter). Datensatzgruppen mit Platzhaltern werden für alle Datensatztypen mit Ausnahme von NS und SOA unterstützt.

Um eine Datensatzgruppe mit Platzhaltern zu erstellen, verwenden Sie den Namen der Datensatzgruppe „ * “ oder einen Namen, dessen Bezeichnung mit „ * “ beginnt, z. B. „*.foo“.

#### CNAME-Ressourceneintragssätze

CNAME-Datensatzgruppen können nicht gleichzeitig neben anderen Datensatzgruppen mit dem gleichen Namen vorhanden sein. Sie können einen CNAME z.B. nicht mit dem relativen Namen „www“, und gleichzeitig einen A-Eintrag mit dem relativen Namen „www“ erstellen. Da die Zonenspitze (Name = " @") immer die NS- und SOA-Datensatzgruppen enthält, die beim Erstellen der Zone erstellt wurden, bedeutet dies, dass Sie keine CNAME-Datensatzgruppe an der Zonenspitze erstellen können. Diese Einschränkungen ergeben sich aus den DNS-Standards; sie sind keine Einschränkungen von Azure DNS.


