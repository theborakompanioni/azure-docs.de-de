#### <a name="create-an-a-record-set-with-single-record"></a>Erstellen einer A-Datensatzgruppe mit einem einzelnen Datensatz

Verwenden Sie `azure network dns record-set create`zum Erstellen einer Datensatzgruppe. Geben Sie die Ressourcengruppe, den Zonennamen, den relativen Namen der Datensatzgruppe, den Datensatztyp und die Gültigkeitsdauer (TTL) an.

```azurecli
azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300
```

Fügen Sie mithilfe von `azure network dns record-set add-record`nach dem Erstellen der A-Datensatzgruppe die IPv4 Adresse zur Datensatzgruppe hinzu:

```azurecli
azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1
```

#### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Erstellen einer AAAA-Datensatzgruppe mit einem einzelnen Datensatz

```azurecli
azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"
```

#### <a name="create-a-cname-record-set-with-a-single-record"></a>Erstellen einer CNAME-Datensatzgruppe mit einem einzelnen Datensatz

CNAME-Datensätze lassen nur einen einzelnen Zeichenfolgenwert zu.

```azurecli
azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300

azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"
```

#### <a name="create-an-mx-record-set-with-a-single-record"></a>Erstellen einer MX-Datensatzgruppe mit einem einzelnen Datensatz

In diesem Beispiel verwenden wir den Namen der Datensatzgruppe "@" zum Erstellen des MX-Eintrags auf oberster Ebene der Zone (in diesem Fall „contoso.com“). Dies ist bei MX-Einträgen üblich.

```azurecli
azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5
```

#### <a name="create-an-ns-record-set-with-a-single-record"></a>Erstellen einer NS-Datensatzgruppe mit einem einzelnen Datensatz

```azurecli
azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300

azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com"
```

#### <a name="create-a-ptr-record-set-with-a-single-record"></a>Erstellen eines PTR-Eintragssatzes mit einem einzelnen Eintrag

In diesem Fall ist „my-arpa-zone.com“ die ARPA-Zone, die Ihren IP-Adressbereich darstellt.  Jeder PTR-Eintragssatz in dieser Zone entspricht einer IP-Adresse innerhalb dieses IP-Adressbereichs.

```azurecli
azure network dns record-set add-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"
```

#### <a name="create-an-srv-record-set-with-a-single-record"></a>Erstellen einer SRV-Datensatzgruppe mit einem einzelnen Datensatz

Wenn Sie einen SRV-Datensatz im Zonenstamm erstellen, können Sie im Datensatznamen „_service“ und „_protocol“ festlegen. Es ist nicht erforderlich, "@" im Datensatznamen anzugeben.

```azurecli
azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300

azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com"
```

#### <a name="create-a-txt-record-set-with-single-record"></a>Erstellen einer TXT-Datensatzgruppe mit einem einzelnen Datensatz

```azurecli
azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record"
```


<!--HONumber=Nov16_HO3-->


