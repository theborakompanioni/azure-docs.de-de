### Konfigurieren einer eingehenden Netzwerksicherheitsgruppen-Regel für den virtuellen Computer

Wenn Sie eine Verbindung mit SQL Server über das Internet herstellen möchten, müssen Sie für die Netzwerksicherheitsgruppe eine eingehende Regel für den Port konfigurieren, auf dem Ihre SQL Server-Instanz lauscht. Standardmäßig ist dies TCP-Port 1433.

1. Wählen Sie im Portal **Virtuelle Computer**, und wählen Sie dann die SQL Server-VM aus.

3. Wählen Sie dann die **Netzwerkschnittstellen** aus.

	![Netzwerkschnittstelle](./media/virtual-machines-sql-server-connection-steps/rm-network-interface.png)

4. Wählen Sie die Netzwerkschnittstelle für Ihren virtuellen Computer aus.

4. Klicken Sie auf den Link **Netzwerksicherheitsgruppe**.

	![Netzwerkschnittstelle](./media/virtual-machines-sql-server-connection-steps/rm-network-security-group.png)

6. Erweitern Sie in den Eigenschaften der Netzwerksicherheitsgruppe die Option **Eingehende Sicherheitsregeln**.

5. Klicken Sie auf die Schaltfläche **Hinzufügen**.

6. Geben Sie unter **Name** den Namen „SQLServerPublicTraffic“ an.

7. Ändern Sie **Protokoll** in **TCP**.

8. Geben Sie einen **Zielportbereich** von 1433 an (oder den Port, auf dem die SQL Server-Instanz lauscht).

9. Stellen Sie sicher, dass **Aktion** auf **Zulassen** festgelegt ist. Das Dialogfeld mit den Sicherheitsregeln sollte in etwa dem folgenden Screenshot entsprechen.

	![Netzwerksicherheitsregel](./media/virtual-machines-sql-server-connection-steps/rm-network-security-rule.png)

9. Klicken Sie auf **OK**, um die Regel für den virtuellen Computer zu speichern.

>[AZURE.NOTE] Es ist möglich, dem Subnetz eine zweite Netzwerksicherheitsgruppe zuzuordnen (dies erfolgt getrennt von der Netzwerksicherheitsgruppe auf dem virtuellen Computer). Dies wird nicht standardmäßig eingerichtet. Wenn Sie eine Netzwerksicherheitsgruppe im Subnetz erstellt haben, müssen Sie Port 1433 in beiden Netzwerksicherheitsgruppen öffnen – sowohl im Subnetz als auch auf dem virtuellen Computer.

<!---HONumber=AcomDC_0921_2016-->