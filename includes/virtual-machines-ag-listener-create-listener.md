In diesem Schritt erstellen Sie manuell den Verfügbarkeitsgruppenlistener im Failovercluster-Manager und in SQL Server Management Studio (SSMS).

1. Öffnen Sie den Failovercluster-Manager über den Knoten, der das primäre Replikat hostet.
2. Wählen Sie den Knoten **Netzwerke** aus, und notieren Sie den Netzwerknamen des Clusters. Dieser Name wird in der Variablen $ClusterNetworkName im PowerShell-Skript verwendet.
3. Erweitern Sie den Clusternamen, und klicken Sie dann auf **Rollen**.
4. Klicken Sie im Bereich **Rollen** mit der rechten Maustaste auf den Verfügbarkeitsgruppennamen, und wählen Sie dann **Ressource hinzufügen** > **Clientzugriffspunkt**.
   
    ![Hinzufügen eines Clientzugriffspunkts für die Verfügbarkeitsgruppe](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)
5. Erstellen Sie im Feld **Name** einen Namen für diesen neuen Listener, klicken Sie dann zweimal auf **Weiter**, und klicken Sie anschließend auf **Fertig stellen**. Schalten Sie den Listener oder die Ressource jetzt noch nicht online.
6. Klicken Sie auf die Registerkarte **Ressourcen** , und erweitern Sie dann den Clientzugriffspunkt, den Sie gerade erstellt haben. Sie sehen die Ressource **IP-Adresse** für jedes der Clusternetzwerke im Cluster. Wenn dies eine reine Azure-Lösung ist, wird nur eine IP-Adressressource angezeigt.
7. Wenn Sie eine Hybridlösung konfigurieren, fahren Sie mit diesem Schritt fort. Wenn Sie eine reine Azure-Lösung konfigurieren, fahren Sie mit dem nächsten Schritt fort. 
   
   * Klicken Sie mit der rechten Maustaste auf die IP-Adressressource, die Ihrem lokalen Subnetz entspricht, und wählen Sie dann **Eigenschaften**aus. Notieren Sie den Namen der IP-Adresse und des Netzwerks.
   * Wählen Sie **Statische IP-Adresse** aus, weisen Sie eine nicht verwendete IP-Adresse zu, und klicken Sie dann auf **OK**.
8. Klicken Sie mit der rechten Maustaste auf die IP-Adressressource, die Ihrem Azure-Subnetz entspricht, und wählen Sie dann "Eigenschaften" aus.
   
   > [!NOTE]
   > Kann der Listener später durch eine in Konflikt stehende IP-Adresse, die von DHCP ausgewählt wird, nicht online geschaltet werden, können Sie im Eigenschaftenfenster eine gültige statische IP-Adresse konfigurieren.
   > 
   > 
9. Ändern Sie im gleichen Eigenschaftenfenster **IP-Adresse** den Wert für **IP-Adressname**. Dieser IP-Adressname wird in der Variablen **$IPResourceName** des PowerShell-Skripts verwendet. Wiederholen Sie diesen Schritt für jede IP-Ressource, wenn die Lösung mehrere Azure-VNets umfasst.



<!--HONumber=Nov16_HO3-->


