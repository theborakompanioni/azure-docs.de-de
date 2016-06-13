<properties
	pageTitle="Rollover von Signaturschlüsseln in Azure AD | Microsoft Azure"
	description="Dieser Artikel beschreibt bewährte Verfahren für das Rollover von Signaturschlüsseln für Azure Active Directory."
	services="active-directory"
	documentationCenter=".net"
	authors="priyamohanram"
	manager="mbaldwin"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/21/2016"
	ms.author="priyamo"/>

# Rollover von Signaturschlüsseln in Azure Active Directory

[AZURE.INCLUDE [active-directory-protocols](../../includes/active-directory-protocols.md)]

In diesem Thema wird erläutert, was Sie über die öffentlichen Schlüssel wissen müssen, die in Azure Active Directory (Azure AD) zum Signieren von Sicherheitstoken verwendet werden. Das Rollover dieser Schlüssel erfolgt nach einem sechswöchigen Zeitplan. Im Notfall kann ein Schlüssel jedoch auch vor diesen sechs Wochen geändert werden. Alle Anwendungen, die Azure AD verwenden, sollten den Rolloverprozess für Schlüssel programmgesteuert verarbeiten können. Im Folgenden erfahren Sie, wie die Schlüssel funktionieren und wie Sie Ihre Anwendung für das Rollover von Schlüsseln aktualisieren können.

## Übersicht über Signaturschlüssel in Azure AD

Azure AD verwendet die auf Branchenstandards basierende Verschlüsselung mit öffentlichem Schlüssel zum Einrichten einer Vertrauensstellung zwischen sich selbst und den Anwendungen, die Azure AD verwenden. Konkret funktioniert dies wie folgt: Azure AD verwendet einen Signaturschlüssel, der aus einem Paar mit einem öffentlichen und einem privaten Schlüssel besteht. Wenn sich ein Benutzer bei einer Anwendung anmeldet, die Azure AD für die Authentifizierung verwendet, erstellt Azure AD ein Sicherheitstoken, das Informationen zum Benutzer enthält. Das Token wird von Azure AD mithilfe seines privaten Schlüssels signiert, bevor es zur Anwendung zurückgesendet wird. Um zu überprüfen, ob das Token gültig ist und tatsächlich von Azure AD stammt, muss die Anwendung die Signatur des Tokens überprüfen. Dies erfolgt mithilfe des von Azure AD verfügbar gemachten öffentlichen Schlüssels, der im Dokument mit den Verbundmetadaten des Mandanten enthalten ist. Dieser öffentliche Schlüssel (und der Signaturschlüssel, von dem er abgeleitet ist) entspricht demjenigen, der für alle Mandanten in Azure AD verwendet wird.

Aus Sicherheitsgründen erfolgt das Rollover des öffentlichen Schlüssels von Azure AD alle sechs Wochen. Im Notfall kann ein Schlüssel jedoch auch vor diesen sechs Wochen geändert werden. Jede in Azure AD integrierte Anwendung muss in der Lage sein, ein Schlüsselrollover zu verarbeiten, unabhängig davon, wie häufig dies geschieht. Je nachdem, wann Sie Ihre Anwendung erstellt haben und welche Authentifizierungsbibliothek dafür verwendet wurde, verfügt die Anwendung unter Umständen über die erforderliche Logik, um ein Schlüsselrollover zu verarbeiten. Wenn dies nicht der Fall ist und die Anwendung versucht, einen abgelaufenen öffentlichen Schlüssel zum Überprüfen der Signatur auf einem Token zu verwenden, schlägt die Anmeldeanforderung fehl.

Da ein Schlüssel sich jederzeit ändern kann, steht im Dokument mit den Verbundmetadaten immer mehr als ein gültiger öffentlicher Schlüssel zur Verfügung. Ihre Anwendung sollte einen der im Dokument angegebenen Schlüssel verwenden können, da ein Schlüssel z.B. geändert oder durch einen anderen ersetzt werden kann usw. Daher empfiehlt es sich, dass Ihre Anwendung diese Schlüssel in einer Datenbank oder Konfigurationsdatei zwischenspeichert, um die Effizienz bei der Kommunikation mit Azure AD während des Anmeldevorgangs zu erhöhen und ein Token mithilfe eines anderen Schlüssels schnell zu validieren.

## Aktualisieren Ihrer Anwendung mit Schlüssel-Rollover-Logik

Wie Ihre Anwendung das Schlüsselrollover verarbeitet, hängt von Variablen ab, wie z.B. von dem verwendeten Identitäts-Framework, der Framework-Version oder dem Typ der Anwendung. In den folgenden Abschnitten erfahren Sie, wie Sie die häufigsten Anwendungstypen und Konfigurationen aktualisieren können. Befolgen Sie die Schritte, um sicherzustellen, dass die Logik korrekt funktioniert.

### Mit Visual Studio 2013 erstellte Webanwendungen

Wenn Ihre Anwendung mithilfe einer Webanwendungsvorlage in Visual Studio 2013 erstellt wurde und Sie im Menü **Authentifizierung ändern** die Option **Organisationskonten** ausgewählt haben, verfügt sie bereits über die für ein Schlüsselrollover erforderliche Logik. Diese Logik speichert den eindeutigen Bezeichner Ihrer Organisation und die Signaturschlüsselinformationen in zwei Datenbanktabellen, die dem Projekt zugeordnet sind. Sie finden die Verbindungszeichenfolge für die Datenbank in der „Web.config“-Datei des Projekts.

Wenn Sie die Authentifizierung Ihrer Lösung manuell hinzugefügt haben, verfügt die Anwendung nicht über die erforderliche Logik für ein Schlüsselrollover. Sie müssen sie in dem Fall selbst schreiben oder die unter „Manuelles Abrufen des neuesten Schlüssels und Aktualisieren Ihrer Anwendung“ beschriebenen Schritte ausführen.

Die folgenden Schritte helfen Ihnen dabei, sicherzustellen, dass die Logik in Ihrer Anwendung korrekt funktioniert.

1. Öffnen Sie die Lösung in Visual Studio 2013, und klicken Sie dann auf die Registerkarte **Server-Explorer** im rechten Fensterbereich.
2. Erweitern Sie **Datenverbindungen**, **DefaultConnection** und anschließend **Tabellen**. Suchen Sie die Tabelle **IssuingAuthorityKeys**, klicken Sie mit der rechte Maustaste darauf, und klicken Sie anschließend auf **Tabellendaten anzeigen**.
3. In der Tabelle **IssuingAuthorityKeys** befindet sich mindestens eine Zeile, die dem Fingerabdruckwert des Schlüssels entspricht. Löschen Sie alle Zeilen in der Tabelle.
4. Klicken Sie mit der rechten Maustaste auf die Tabelle **Mandanten**, und klicken Sie anschließend auf **Tabellendaten anzeigen**.
5. Die Tabelle **Mandanten** enthält mindestens eine Zeile, die einer eindeutigen Verzeichnismandanten-ID entspricht. Löschen Sie alle Zeilen in der Tabelle. Wenn Sie die Zeilen in den Tabellen **Mandanten** und **IssuingAuthorityKeys** nicht löschen, erhalten Sie einen Laufzeitfehler.
6. Erstellen Sie die Anwendung, und führen Sie sie aus. Wenn Sie sich bei Ihrem Konto angemeldet haben, können Sie die Anwendung anhalten.
7. Kehren Sie zum **Server-Explorer** zurück, und sehen Sie sich die Werte in der Tabelle **IssuingAuthorityKeys** und **Mandanten** an. Sie werden feststellen, dass sie automatisch mit den entsprechenden Informationen aus dem Verbundmetadaten-Dokument aufgefüllt worden sind.

### Mit Visual Studio 2012 erstellte Webanwendungen

Wenn Ihre Anwendung in Visual Studio 2012 erstellt wurde, haben Sie wahrscheinlich das Identitäts- und Zugriffstool zum Konfigurieren Ihrer Anwendung verwendet. Es ist auch möglich, dass Sie die [Validierung der Ausstellernamenregistrierung (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) verwendet haben. Die VINR ist für die Verwaltung von Informationen zu vertrauenswürdigen Identitätsanbietern (Azure AD) und den Schlüsseln, die zum Überprüfen der von ihnen ausgestellten Token verwendet werden, zuständig. Die VINR erleichtert es zudem, die in einer „Web.config“-Datei gespeicherten Schlüsselinformationen automatisch zu aktualisieren, indem das aktuelle Ihrem Verzeichnis zugeordnete Verbundmetadaten-Dokument heruntergeladen wird. Mit dem aktuellen Dokument wird geprüft, ob die Konfiguration veraltet ist, und bei Bedarf wird die Anwendung aktualisiert, sodass der neue Schlüssel verwendet wird.

Wenn Sie die Anwendung mithilfe eines der Codebeispiele oder der von Microsoft bereitgestellten Dokumentation zur exemplarischen Vorgehensweise erstellt haben, ist die Logik für das Schlüsselrollover in Ihrem Projekt bereits enthalten. Beachten Sie, dass der folgende Code in Ihrem Projekt bereits vorhanden ist. Wenn Ihre Anwendung diese Logik noch nicht enthält, führen Sie die folgenden Schritte aus, um sie hinzuzufügen und um sicherzustellen, dass sie korrekt funktioniert.

1. Fügen Sie im **Projektmappen-Explorer** einen Verweis auf die **System.IdentityModel**-Assembly für das entsprechende Projekt hinzu.
2. Öffnen Sie die Datei **Global.asax.cs**, und fügen Sie die folgenden using-Anweisungen hinzu:
```
using System.Configuration;
using System.IdentityModel.Tokens;
```
3. Fügen Sie der Datei **Global.asax.cs** die folgende Methode hinzu:
```
protected void RefreshValidationSettings()
{
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
}
```
4. Rufen Sie die Methode **RefreshValidationSettings()** in der Methode **Application\_Start()** in **Global.asax.cs** wie dargestellt auf:
```
protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
}
```

Nachdem Sie diese Schritte ausgeführt haben, wird die „Web.config“-Datei Ihrer Anwendung mit den neuesten Informationen aus den Verbundmetadaten-Dokument, einschließlich der neuesten Schlüssel, aktualisiert. Diese Aktualisierung erfolgt jedes Mal, wenn Ihr Anwendungspool in IIS wiederverwendet wird. Standardmäßig ist IIS so eingestellt, dass Anwendungen alle 29 Stunden wiederverwendet werden.

Gehen Sie folgendermaßen vor, um sicherzustellen, dass die Logik für das Schlüsselrollover funktioniert.

1. Wenn Sie überprüft haben, dass Ihre Anwendung den oben dargestellten Code verwendet, öffnen Sie die Datei **Web.config**, navigieren Sie zum Block **<issuerNameRegistry>**, und suchen Sie speziell die folgenden Zeilen:
```
<issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
```
2. Ändern Sie in der Einstellung **<add thumbprint=””>** den Fingerabdruckwert, indem Sie ein Zeichen durch ein anderes ersetzen. Speichern Sie die Datei **Web.config**.

3. Erstellen Sie die Anwendung, und führen Sie sie anschließend aus. Wenn Sie den Anmeldevorgang abschließen, aktualisiert die Anwendung den Schlüssel, indem die erforderlichen Informationen vom Verbundmetadaten-Dokument Ihres Verzeichnisses heruntergeladen werden. Wenn beim Anmelden Probleme auftreten, stellen Sie sicher, dass die Änderungen in Ihrer Anwendung richtig sind. Lesen Sie hierzu das Thema [Adding Sign-On to Your Web Application Using Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) (Hinzufügen der Anmeldung zu einer Web-Anwendung mithilfe von Azure AD), oder laden Sie das folgende Codebeispiel herunter: [Multi-Tenant Cloud Application for Microsoft Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b) (Mehrinstanzenfähige Cloudanwendung für Azure Active Directory).


### Mit Visual Studio 2008 oder 2010 erstellte Webanwendungen und Windows Identity Foundation (WIF) V1. 0 für .NET 3.5

Wenn Sie eine Anwendung auf WIF v1. 0 erstellt haben, gibt es keine automatische Aktualisierung der Konfiguration Ihrer Anwendung, um einen neuen Schlüssel zu verwenden. Die einfachste Möglichkeit zum Aktualisieren des Schlüssels erfolgt mithilfe des Tools „FedUtil“, das im WIF SDK enthalten ist. Dieses kann das neueste Metadatendokument abrufen und Ihre Konfiguration aktualisieren. Diesbezügliche Anweisungen sind unten aufgeführt. Alternative Möglichkeiten sind:

- Befolgen Sie die Anweisungen im Abschnitt „Manuelles Abrufen des neuesten Schlüssels und Aktualisieren Ihrer Anwendung“, und erstellen Sie eine Logik, um die Schritte programmgesteuert auszuführen.
- Aktualisieren Sie Ihre Anwendung auf .NET 4.5, das die aktuelle Version von WIF im System-Namespace enthält. Verwenden Sie anschließend die [Validierung der Ausstellernamenregistrierung (VINR)](https://msdn.microsoft.com/library/dn205067.aspx), um automatische Aktualisierungen der Anwendungskonfiguration durchzuführen.


1. Stellen Sie sicher, dass das WIF v1. 0-SDK auf Ihrem Entwicklungscomputer für Visual Studio 2008 oder 2010 installiert ist. Wenn es noch nicht installiert ist, können Sie es [hier herunterladen](https://www.microsoft.com/de-DE/download/details.aspx?id=4451).
2. Öffnen Sie die Projektmappe in Visual Studio, klicken Sie anschließend mit der rechten Maustaste auf das betreffende Projekt, und wählen Sie **Update federation metadata** (Verbundmetadaten aktualisieren). Wenn diese Option nicht verfügbar ist, wurde FedUtil und/oder das WIF v1. 0 SDK nicht installiert.
3. Wenn Sie dazu aufgefordert werden, wählen Sie **Aktualisieren**, damit Ihre Verbundmetadaten aktualisiert werden. Wenn Sie Zugriff auf die Serverumgebung haben, in der die Anwendung gehostet wird, können Sie optional die [automatische Metadaten-Aktualisierungsplanung](https://msdn.microsoft.com/library/ee517272.aspx) von FedUtil verwenden.
4. Klicken Sie auf **Fertig stellen**, um die Aktualisierung abzuschließen.

### Web-APIs, die JSON-Webtoken (JWT) verwenden

Wenn Sie über eine Anwendung verfügen, die eine Web-API mithilfe eines JWT-Tokens aufruft, das von Azure AD zum Autorisieren der Anforderung ausgestellt wurde, wird das JWT-Token genauso validiert wie eine Anmeldeaufforderung: mithilfe des öffentlichen Schlüssels von Azure AD zum Überprüfen der Signatur. Web-API-Anwendungen müssen darauf vorbereitet sein, das Schlüsselrollover zu verarbeiten, da sie letztlich das gleiche X509-Zertifikat zum Signieren des Tokens verwenden.

Wenn Sie eine Web-API-Anwendung mithilfe der Web-API-Vorlage in Visual Studio 2013 erstellt haben und im Menü **Authentifizierung ändern** die Option **Organisationskonten** ausgewählt haben, verfügt diese bereits über die erforderliche Logik für ein Schlüsselrollover. Wenn Sie die Authentifizierung manuell konfiguriert haben, gehen Sie folgendermaßen vor, um zu erfahren, wie Sie Ihre Web-API konfigurieren, damit die Schlüsselinformationen automatisch aktualisiert werden.

Der folgende Codeausschnitt veranschaulicht, wie die neuesten Schlüssel vom Verbundmetadaten-Dokument abgerufen werden. Verwenden Sie anschließend den [JWT-Tokenhandler](https://msdn.microsoft.com/library/dn205065.aspx) zum Überprüfen des Tokens. Bei diesem Codeausschnitt wird davon ausgegangen, dass Sie Ihre eigenen Verfahren zum Zwischenspeichern verwenden, um den Schlüssel zum Überprüfen zukünftiger Token von Azure AD in einer Datenbank, Konfigurationsdatei usw. beizubehalten.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Classic Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### Manuelles Abrufen des neuesten Schlüssels und Aktualisieren Ihrer Anwendung

Wenn Ihr Anwendungstyp oder Ihre Plattform derzeit kein Verfahren für die automatische Aktualisierung des Schlüssels unterstützt, können Sie die folgenden Schritte ausführen:

1. Wechseln Sie in einem Webbrowser zu https://manage.windowsazure.com, melden Sie sich bei Ihrem Konto, und klicken Sie dann auf das Active Directory-Symbol im linken Menü.
2. Klicken Sie auf das Verzeichnis, in dem Ihre Anwendung registriert ist, und klicken Sie anschließend auf den Link **Endpunkte anzeigen** auf der Befehlsleiste.
3. Kopieren Sie aus der Liste der einmaligen Anmeldung und der Verzeichnisendpunkte den **Verbundmetadaten-Dokument**-Link.
4. Öffnen Sie eine neue Registerkarte in Ihrem Browser, und wechseln Sie zur URL, die Sie gerade kopiert haben. Daraufhin wird Ihnen der Inhalt des Verbund-Metadaten-XML-Dokuments angezeigt. Weitere Informationen zu diesem Dokument finden Sie im Thema zu Verbundmetadaten.
5. Wenn Sie eine Anwendung für die Verwendung eines neuen Schlüssels aktualisieren möchten, suchen Sie jeweils den Block **<RoleDescriptor>**, und kopieren Sie den Wert des **<X509Certificate>**-Elements jedes Blocks. Beispiel:
```
<RoleDescriptor xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType">
      <KeyDescriptor use="signing">
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
                <X509Data>
                    <X509Certificate>MIIDPjC…BcXWLAIarZ</X509Certificate>
```
6. Wenn Sie den Wert des **<X509Certificate>**-Elements kopiert haben, öffnen Sie einen Texteditor, und fügen Sie den Wert ein. Stellen Sie sicher, dass Sie alle nachfolgenden Leerzeichen entfernen, und speichern Sie die Datei mit der Erweiterung **.cer**.

Dadurch haben Sie das X509-Zertifikat erstellt, das als öffentlicher Schlüssel für Azure AD verwendet wird. Mit den Details des Zertifikats wie etwa Fingerabdruck und Ablaufdatum können Sie manuell oder programmgesteuert prüfen, ob das Zertifikat und der Fingerabdruck Ihrer Anwendung gültig sind.

<!---HONumber=AcomDC_0601_2016-->