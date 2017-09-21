Es ist wichtig, dass Sie in Bezug auf die ausgeführten Anwendungen auf den Schutz Ihres virtuellen Computers (VM) achten. Das Schützen Ihrer VMs kann einen oder mehrere Azure-Dienste und -Features enthalten, die für den sicheren Zugriff auf Ihre VMs und die sichere Speicherung Ihrer Daten sorgen. Dieser Artikel enthält Informationen dazu, wie Sie den Schutz Ihrer VM und Anwendungen aufrechterhalten.

## <a name="antimalware"></a>Antimalware

Die Bedrohungslage für Cloudumgebungen ist dynamisch. Dies erhöht den Druck, effektive Schutzmaßnahmen durchzuführen, um die Compliance- und Sicherheitsanforderungen zu erfüllen. [Microsoft Antimalware for Azure](../articles/security/azure-security-antimalware.md) ist eine kostenlose Echtzeit-Schutzfunktion zum Bestimmen und Entfernen von Viren, Spyware und anderer Schadsoftware. Warnungen können so konfiguriert werden, dass Sie benachrichtigt werden, wenn versucht wird, bekannte Schadsoftware oder unerwünschte Software zu installieren oder auf Ihrer VM auszuführen.

## <a name="azure-security-center"></a>Azure Security Center

Mit [Azure Security Center](../articles/security-center/security-center-intro.md) können Sie Bedrohungen Ihrer VMs verhindern, erkennen und bekämpfen. Security Center bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Azure-Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Spektrum an Sicherheitslösungen verwendet werden.

## <a name="encryption"></a>Verschlüsselung

Zum Verbessern der Sicherheit und Konformität von [Windows-VMs](../articles/virtual-machines/windows/encrypt-disks.md) und [Linux-VMs](../articles/virtual-machines/linux/encrypt-disks.md) können virtuelle Datenträger in Azure verschlüsselt werden. Virtuelle Datenträger auf virtuellen Windows-Computern werden im Ruhezustand mithilfe von BitLocker verschlüsselt. Virtuelle Datenträger auf virtuellen Linux-Computern werden im Ruhezustand mithilfe von dm-crypt verschlüsselt. 

Für die Verschlüsselung virtueller Datenträger in Azure fallen keine Gebühren an. Kryptografische Schlüssel werden in Azure Key Vault mit Softwareschutz gespeichert. Alternativ können Sie Schlüssel aber auch in Hardwaresicherheitsmodulen (HSMs) mit FIPS 140-2 Level 2-Zertifizierung importieren oder generieren. Die kryptografischen Schlüssel dienen zum Verschlüsseln und Entschlüsseln virtueller Datenträger, die an Ihren virtuellen Computer angefügt sind. Diese kryptografischen Schlüssel werden allein von Ihnen kontrolliert, und Sie können deren Verwendung überwachen. Über einen Azure Active Directory-Dienstprinzipal werden die kryptografischen Schlüssel beim Ein- und Ausschalten virtueller Computer sicher ausgegeben.

## <a name="key-vault-and-ssh-keys"></a>Key Vault und SSH-Schlüssel

Geheimnisse und Zertifikate können als Ressourcen modelliert und von [Key Vault](../articles/key-vault/key-vault-whatis.md) bereitgestellt werden. Sie können Azure PowerShell zum Erstellen von Schlüsseltresoren für [Windows-VMs](../articles/virtual-machines/windows/key-vault-setup.md) und die Azure CLI für [Linux-VMs](../articles/virtual-machines/linux/key-vault-setup.md) verwenden. Außerdem können Sie Schlüssel für die Verschlüsselung erstellen.

Mit Key Vault-Zugriffsrichtlinien können separate Berechtigungen für Schlüssel, Geheimnisse und Zertifikate gewährt werden. Dadurch haben Sie beispielsweise die Möglichkeit, einem Benutzer nur Zugriff auf Schlüssel, aber keine Berechtigungen für Geheimnisse zu gewähren. Zugriffsberechtigungen für Schlüssel, Geheimnisse oder Zertifikate gelten jedoch auf Tresorebene. Mit anderen Worten: [Key Vault-Zugriffsrichtlinien](../articles/key-vault/key-vault-secure-your-key-vault.md) unterstützen keine Berechtigungen auf Objektebene.

Wenn Sie eine Verbindung mit VMs herstellen, sollten Sie die Verschlüsselung mit öffentlichem Schlüssel (Public-Key Cryptography) herstellen, um für ein sichereres Verfahren zum Anmelden zu sorgen. Bei diesem Verfahren erfolgt die Authentifizierung nicht per Benutzername und Kennwort. Stattdessen werden mithilfe des SSH-Befehls (Secure Shell) ein öffentlicher und ein privater Schlüssel ausgetauscht. Kennwörter sind anfällig für Brute-Force-Angriffe. Das gilt insbesondere bei virtuellen Computern mit Internetzugriff (also beispielsweise für Webserver). Mit einem SSH-Schlüsselpaar (Secure Shell) können Sie eine [Linux-VM](../articles/virtual-machines/linux/mac-create-ssh-keys.md) erstellen, bei deren Authentifizierung SSH-Schlüssel verwendet werden, sodass zum Anmelden keine Kennwörter mehr erforderlich sind. Sie können auch SSH-Schlüssel verwenden, um für eine [Windows-VM](../articles/virtual-machines/linux/ssh-from-windows.md) eine Verbindung mit einer Linux-VM herzustellen.

## <a name="policies"></a>Richtlinien

Mit [Azure Resource Manager-Richtlinien](../articles/azure-resource-manager/resource-manager-policy.md) kann das gewünschte Verhalten für die [Windows-VMs](../articles/virtual-machines/windows/policy.md) und [Linux-VMs](../articles/virtual-machines/linux/policy.md) Ihrer Organisation definiert werden. Mithilfe von Richtlinien kann eine Organisation verschiedene Konventionen und Regeln im gesamten Unternehmen durchsetzen. Die Durchsetzung des gewünschten Verhaltens hilft dabei, Risiken zu mindern, und trägt gleichzeitig zum Erfolg des Unternehmens bei.

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

Mithilfe der [rollenbasierten Zugriffssteuerung (RBAC)](../articles/active-directory/role-based-access-control-what-is.md) können Sie Aufgaben in Ihrem Team verteilen und Benutzern nur den Zugriff auf Ihre VM gewähren, den sie zur Ausführung ihrer Aufgaben benötigen. Anstatt allen Benutzern den uneingeschränkten Zugriff auf die VM zu gewähren, können Sie auch nur bestimmte Aktionen zulassen. Sie können die Zugriffssteuerung für die VM im [Azure-Portal](../articles/active-directory/role-based-access-control-configure.md), per [Azure CLI](https://docs.microsoft.com/cli/azure/role) oder mit [Azure PowerShell](../articles/active-directory/role-based-access-control-manage-access-powershell.md) konfigurieren.


## <a name="next-steps"></a>Nächste Schritte
- Führen Sie die Schritte aus, um die VM-Sicherheit mit dem Azure Security Center für [Linux](../articles/virtual-machines/linux/tutorial-azure-security.md) oder [Windows](../articles/virtual-machines/windows/tutorial-azure-security.md) zu überwachen.