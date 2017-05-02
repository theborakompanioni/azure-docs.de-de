Durch Anwenden von Tags können Sie Ihre Azure-Ressourcen logisch nach Kategorien organisieren. Jedes Tag besteht aus einem Schlüssel und einem Wert. So können Sie beispielsweise den Schlüssel „Umgebung“ und den Wert „Produktion“ auf alle Ressourcen in der Produktion anwenden. Ohne dieses Tag ist möglicherweise nicht ohne Weiteres erkennbar, ob eine Ressource für die Entwicklung, für Tests oder für die Produktion vorgesehen ist. „Umgebung“ und „Produktion“ sind allerdings nur Beispiele. Sie selbst definieren die Schlüssel und Werte, die für Ihr Abonnement am sinnvollsten sind.

Nachdem Sie Tags angewendet haben, können Sie alle Ressourcen in Ihrem Abonnement abrufen, die diesen Tagschlüssel und -wert besitzen. Mit Tags können Sie verwandte Ressourcen aus verschiedenen Ressourcengruppen abrufen. Das ist hilfreich, wenn Sie Ressourcen für die Abrechnung oder Verwaltung organisieren müssen.

Für Tags gelten folgende Einschränkungen:

* Jede Ressource oder Ressourcengruppe kann maximal 15 Tagschlüssel-Wert-Paare haben. Diese Einschränkung gilt nur für Tags, die direkt auf die Ressourcengruppe oder die Ressource angewendet werden. Eine Ressourcengruppe kann zahlreiche Ressourcen mit jeweils 15 Tagschlüssel-Wert-Paaren enthalten. 
* Der Tagname darf maximal 512 Zeichen lang sein.
* Der Tagwert darf maximal 256 Zeichen lang sein. 
* Auf die Ressourcengruppe angewendete Tags werden nicht von den in dieser Ressourcengruppe enthaltenen Ressourcen geerbt. 

Wenn Sie einer Ressource mehr als 15 Werte zuordnen müssen, verwenden Sie eine JSON-Zeichenfolge für den Tagwert. Die JSON-Zeichenfolge kann zahlreiche Werte enthalten, die auf einen einzelnen Tagschlüssel angewendet werden. Dieser Artikel enthält ein Beispiel für die Zuweisung einer JSON-Zeichenfolge zum Tagschlüssel.