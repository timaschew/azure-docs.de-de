---
title: Azure Disk Encryption – Voraussetzungen| Microsoft-Dokumentation
description: Dieser Artikel enthält die Voraussetzungen für die Verwendung von Microsoft Azure Disk Encryption für IaaS-VMs.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 09/14/2018
ms.openlocfilehash: ad8bf0217dcd07a7272a220f2d91ed6bc40523bc
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2018
ms.locfileid: "46498588"
---
# <a name="azure-disk-encryption-prerequisites"></a>Azure Disk Encryption – Voraussetzungen 
 Dieser Artikel „Azure Disk Encryption – Voraussetzungen“ thematisiert, was Sie beachten müssen, bevor Sie Azure Disk Encryption verwenden können. Azure Disk Encryption ist in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/) integriert, um die Verwaltung von Verschlüsselungsschlüsseln zu erleichtern. Zum Konfigurieren von Azure Disk Encryption können Sie [Azure PowerShell](/powershell/azure/overview), die [Azure CLI](/cli/azure/) oder das [Azure-Portal](https://portal.azure.com) verwenden.

Bevor Sie Azure Disk Encryption auf Azure-IaaS-VMs für die unterstützten Szenarien aktivieren, die im Artikel [Azure Disk Encryption – Übersicht](azure-security-disk-encryption-overview.md) erörtert werden, beachten Sie die folgenden Voraussetzungen. 

> [!NOTE]
> Einige Empfehlungen führen möglicherweise zu einer erhöhten Daten-, Netzwerk- oder Computeressourcenauslastung, was zusätzliche Lizenz- oder Abonnementkosten nach sich ziehen kann. Sie müssen über ein gültiges aktives Azure-Abonnement verfügen, um in den unterstützten Regionen Ressourcen in Azure zu erstellen.


## <a name="bkmk_OSs"></a> Unterstützte Betriebssysteme
Azure Disk Encryption wird auf folgenden Betriebssystemen unterstützt:

- Windows Server-Versionen: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 und Windows Server 2016.
    - Für Windows Server 2008 R2 müssen Sie .NET Framework 4.5 installieren, bevor Sie die Verschlüsselung in Azure aktivieren. Sie können die Installation über Windows Update durchführen mit dem optionalen Update Microsoft.NET Framework 4.5.2 für Windows Server 2008 R2 x64-basierte Systeme ([KB2901983](https://support.microsoft.com/kb/2901983)).    
- Windows-Clientversionen: Windows 8-Client und Windows 10-Client.
- Azure Disk Encryption wird nur für bestimmte auf dem Azure-Katalog basierenden Linux-Serverdistributionen und -Versionen unterstützt. Die Liste der derzeit unterstützten Versionen finden Sie unter [Häufig gestellte Fragen zu Azure Disk Encryption](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport).
- Azure Disk Encryption setzt voraus, dass Ihr Schlüsseltresor und die VMs zur selben Azure-Region und zum selben Azure-Abonnement gehören. Bei einer Konfiguration der Ressourcen in unterschiedlichen Regionen kann Azure Disk Encryption nicht aktiviert werden.

## <a name="bkmk_LinuxPrereq"></a> Zusätzliche Voraussetzungen für Linux-IaaS-VMs 

- Azure Disk Encryption für Linux setzt 7 GB RAM auf dem virtuellen Computer voraus, um die Verschlüsselung von Betriebssystemdatenträgern für [unterstützte Images](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) zu aktivieren. Sobald die Verschlüsselung des Betriebssystemdatenträgers abgeschlossen ist, kann die VM so konfiguriert werden, dass sie mit weniger Speicherplatz läuft.
- Vor dem Aktivieren der Verschlüsselung müssen die zu verschlüsselnden Datenträger ordnungsgemäß in „/etc/fstab“ aufgelistet werden. Verwenden Sie für diesen Eintrag einen persistenten Blockgerätenamen, da Gerätenamen im Format „/dev/sdX“ beim Neustart, insbesondere nach Anwendung der Verschlüsselung, nicht zuverlässig mit demselben Datenträger verknüpft werden können. Weitere Informationen zu diesem Verhalten finden Sie unter [Behandeln von Problemen mit geänderten Gerätenamen von Linux-VMs](../virtual-machines/linux/troubleshoot-device-names-problems.md).
- Stellen Sie sicher, dass die Einstellungen für „/etc/fstab“ ordnungsgemäß für die Einbindung konfiguriert sind. Um diese Einstellungen zu konfigurieren, führen Sie den Befehl „mount -a“ aus, oder starten Sie die VM neu, und lösen Sie das Einbinden so erneut aus. Wenn der Vorgang abgeschlossen wurde, sehen Sie sich die Ausgabe des Befehls „lsblk“ an, um zu überprüfen, ob das Laufwerk noch eingebunden ist. 
    - Wenn die Datei „/etc/fstab“ das Laufwerk vor der Aktivierung der Verschlüsselung nicht ordnungsgemäß einbindet, kann auch Azure Disk Encryption es nicht richtig einbinden.
    - Azure Disk Encryption verschiebt die Einbindungsinformationen aus „/etc/fstab“ in die eigene Konfigurationsdatei als Teil des Verschlüsselungsprozesses. Der Eintrag fehlt in „/etc/fstab“, nachdem die Verschlüsselung des Datenlaufwerks abgeschlossen wurde.
    -  Nach dem Neustart dauert es einige Zeit, bis Azure Disk Encryption die neu verschlüsselten Datenträger eingebunden hat. Sie sind nach einem Neustart nicht sofort verfügbar. Der Prozess benötigt Zeit, um die verschlüsselten Laufwerke zu starten, zu entsperren und dann einzubinden, bevor sie für andere Prozesse verfügbar sind. Dies kann je nach Systemeigenschaften mehr als eine Minute nach dem Neustart dauern.

Ein Beispiel für Befehle, die verwendet werden können, um Datenträger einzubinden und die notwendigen „/etc/fstab“-Einträge zu erstellen, finden Sie in den [Zeilen 244–248 dieser Skriptdatei](https://github.com/ejarvi/ade-cli-getting-started/blob/master/validate.sh#L244-L248). 


## <a name="bkmk_GPO"></a> Netzwerk und Gruppenrichtlinie

**IaaS-VMs müssen die folgenden Anforderungen an die Netzwerkendpunktkonfiguration erfüllen, um Azure Disk Encryption zu aktivieren:**
  - Um ein Token für die Verbindungsherstellung mit Ihrem Schlüsseltresor zu erhalten, muss der virtuelle IaaS-Computer eine Verbindung mit dem Azure Active Directory-Endpunkt \[login.microsoftonline.com\] herstellen können.
  - Um die Verschlüsselungsschlüssel in Ihren Schlüsseltresor schreiben zu können, muss der virtuelle IaaS-Computer eine Verbindung mit dem Schlüsseltresorendpunkt herstellen können.
  - Der virtuelle IaaS-Computer muss eine Verbindung mit dem Azure-Speicherendpunkt herstellen können, an dem das Azure-Erweiterungsrepository gehostet wird, sowie mit einem Azure Storage-Konto, das die VHD-Dateien hostet.
  -  Wenn Ihre Sicherheitsrichtlinie den Zugriff von virtuellen Azure-Computern auf das Internet beschränkt, können Sie den obigen URI auflösen und eine spezielle Regel konfigurieren, um ausgehende Verbindungen mit den IP-Adressen zuzulassen. Weitere Informationen finden Sie unter [Zugreifen auf Azure Key Vault hinter einer Firewall](../key-vault/key-vault-access-behind-firewall.md).    


**Gruppenrichtlinie:**
 - Die Azure Disk Encryption-Lösung verwendet für virtuelle Windows-IaaS-Computer die externe BitLocker-Schlüsselschutzvorrichtung. Für VMs, die der Domäne beigetreten sind, sollten Sie keine Gruppenrichtlinien nutzen, mit denen TPM-Schutzvorrichtungen durchgesetzt werden. Informationen zur Gruppenrichtlinie „BitLocker ohne kompatibles TPM zulassen“ finden Sie unter [BitLocker Group Policy Reference](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#a-href-idbkmk-unlockpol1arequire-additional-authentication-at-startup) (Referenz zur BitLocker-Gruppenrichtlinie).

-  Die BitLocker-Richtlinie für VMs mit Domänenbeitritt und benutzerdefinierten Gruppenrichtlinien muss die folgende Einstellung enthalten: [Speichern von BitLocker-Wiederherstellungsinformationen durch Benutzer konfigurieren -> 256-Bit-Wiederherstellungsschlüssel zulassen](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings). Azure Disk Encryption schlägt fehl, wenn benutzerdefinierte Einstellungen für die Gruppenrichtlinie nicht mit BitLocker kompatibel sind. Auf Computern ohne korrekte Richtlinieneinstellung müssen Sie die neue Richtlinie anwenden und die Aktualisierung der neuen Richtlinie erzwingen (gpupdate.exe /force). Danach ist möglicherweise ein Neustart erforderlich.  


## <a name="bkmk_PSH"></a> Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) bietet eine Reihe von Cmdlets, die das [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)-Modell für die Verwaltung von Azure-Ressourcen verwenden. Azure PowerShell kann mit [Azure Cloud Shell](../cloud-shell/overview.md) im Browser verwendet oder auf dem lokalen Computer installiert (siehe Anleitung unten) und in einer beliebigen PowerShell-Sitzung verwendet werden. Falls Sie die lokale Installation bereits durchgeführt haben, verwenden Sie die neueste Version des Azure PowerShell SDKs, um Azure Disk Encryption zu konfigurieren. Laden Sie die neueste Version von [Azure PowerShell](https://github.com/Azure/azure-powershell/releases) herunter.

### <a name="install-azure-powershell-for-use-on-your-local-machine-optional"></a>Installieren Sie Azure PowerShell für die Verwendung auf Ihrem lokalen Computer (optional): 
1. Folgen Sie den Anweisungen unter dem Link für Ihr Betriebssystem, und fahren Sie dann mit den weiteren Schritten fort.      
    - [Installieren und Konfigurieren von Azure PowerShell unter Windows](/powershell/azure/install-azurerm-ps) 
        - Installieren Sie PowerShellGet und Azure PowerShell, und laden Sie das AzureRM-Modul herunter. 
    - [Installieren und Konfigurieren von Azure PowerShell unter macOS und Linux](/powershell/azure/install-azurermps-maclinux)
        -  Installieren Sie PowerShell Core und Azure PowerShell für .NET Core, und laden Sie das Az-Modul herunter.

2. Überprüfen Sie, welche Versionen des AzureRM-Moduls installiert sind. [Aktualisieren Sie das Azure PowerShell-Modul](/powershell/azure/install-azurerm-ps#update-the-azure-powershell-module) bei Bedarf.
    -  Das AzureRM-Modul muss mindestens Version 6.0.0 aufweisen.
    - Es wird empfohlen, die neueste AzureRM-Modulversion zu verwenden.

     ```powershell
     Get-Module AzureRM -ListAvailable | Select-Object -Property Name,Version,Path
     ```

3. Melden Sie sich mithilfe des Cmdlets [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) bei Azure an.
     
     ```azurepowershell-interactive
     Connect-AzureRmAccount
     # For specific instances of Azure, use the -Environment parameter.
     Connect-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)
    
     <# If you have multiple subscriptions and want to specify a specific one, 
     get your subscription list with Get-AzureRmSubscription and 
     specify it with Set-AzureRmContext.  #>
     Get-AzureRmSubscription
     Set-AzureRmContext -SubscriptionId "xxxx-xxxx-xxxx-xxxx"
     ```

4.  Weitere Informationen finden Sie bei Bedarf unter [Erste Schritte mit Azure PowerShell](/powershell/azure/get-started-azureps).

## <a name="bkmk_CLI"></a> Installieren der Azure CLI für die Verwendung auf Ihrem lokalen Computer (optional)

Die [Azure CLI 2.0](/cli/azure) ist ein Befehlszeilentool zum Verwalten von Azure-Ressourcen. Sie wurde entwickelt, um Daten flexible abzufragen, Vorgänge mit langer Ausführungsdauer als nicht blockierende Prozesse zu unterstützen und das Erstellen von Skripts zu vereinfachen. Azure PowerShell kann mit [Azure Cloud Shell](../cloud-shell/overview.md) im Browser verwendet oder auf dem lokalen Computer installiert und in einer beliebigen PowerShell-Sitzung verwendet werden.

1. [Installieren Sie die Azure CLI](/cli/azure/install-azure-cli) für die Verwendung auf Ihrem lokalen Computer (optional):

2. Überprüfen Sie, welche Version installiert ist.
     
     ```azurecli-interactive
     az --version
     ``` 

3. Melden Sie sich mit [az login](/cli/azure/authenticate-azure-cli) in Azure an.
     
     ```azurecli
     az login
     
     # If you would like to select a tenant, use: 
     az login --tenant "<tenant>"

     # If you have multiple subscriptions, get your subscription list with az account list and specify with az account set.
     az account list
     az account set --subscription "<subscription name or ID>"
     ```

4. Weitere Informationen finden Sie bei Bedarf unter [Erste Schritte mit Azure CLI 2.0](/cli/azure/get-started-with-azure-cli). 


## <a name="prerequisite-workflow-for-key-vault"></a>Workflowvoraussetzungen für Key Vault
Wenn Sie bereits mit den Key Vault- und Azure AD-Voraussetzungen für Azure Disk Encryption vertraut sind, können Sie das [PowerShell-Skript zur Überprüfung der Azure Disk Encryption-Voraussetzungen](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 ) verwenden. Weitere Informationen zur Verwendung des Skripts zur Überprüfung der Voraussetzungen finden Sie in der [Schnellstartanleitung zum Verschlüsseln eines virtuellen Computers](quick-encrypt-vm-powershell.md) sowie im [Anhang zu Azure Disk Encryption](azure-security-disk-encryption-appendix.md#bkmk_prereq-script). 

1. Erstellen Sie bei Bedarf eine Ressourcengruppe.
2. Erstellen eines Schlüsseltresors 
3. Legen Sie die erweiterten Zugriffsrichtlinien für den Schlüsseltresor fest.

>[!WARNING]
>Stellen Sie vor dem Löschen eines Schlüsseltresors sicher, dass Sie keine vorhandenen virtuellen Computer damit verschlüsselt haben. Um einen Tresor vor dem versehentlichen Löschen zu schützen, [aktivieren Sie das vorläufige Löschen](../key-vault/key-vault-soft-delete-powershell.md#enabling-soft-delete) und eine [Ressourcensperre](../azure-resource-manager/resource-group-lock-resources.md) für den Tresor. 
 
## <a name="bkmk_KeyVault"></a> Erstellen einer Key Vault-Instanz 
Azure Disk Encryption ist in [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) integriert, damit Sie die Verschlüsselungsschlüssel und Geheimnisse für die Datenträgerverschlüsselung in Ihrem Key Vault-Abonnement steuern und verwalten können. Sie können für Azure Disk Encryption einen neuen Schlüsseltresor erstellen oder einen bereits vorhandenen verwenden. Weitere Informationen zu Schlüsseltresoren finden Sie unter [Erste Schritte mit Azure Key Vault](../key-vault/key-vault-get-started.md) und [Schützen eines Schlüsseltresors](../key-vault/key-vault-secure-your-key-vault.md). Sie können eine Resource Manager-Vorlage, Azure PowerShell oder die Azure CLI verwenden, um einen Schlüsseltresor zu erstellen. 


>[!WARNING]
>Um sicherzustellen, dass die Verschlüsselungsgeheimnisse die Regionsgrenzen nicht verlassen, müssen den Schlüsseltresor und die VMs für Azure Disk Encryption sich in derselben Region angeordnet sein. Erstellen und verwenden Sie einen Schlüsseltresor, der sich in derselben Region wie die zu verschlüsselnde VM befindet. 


### <a name="bkmk_KVPSH"></a> Erstellen eines Schlüsseltresors mit PowerShell

Sie können mit Azure PowerShell mit dem Cmdlet [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/New-AzureRmKeyVault) einen Schlüsseltresor erstellen. Weitere Cmdlets für Key Vault finden Sie unter [AzureRM.KeyVault](/powershell/module/azurerm.keyvault/). 

1. Stellen Sie ggf. eine [Verbindung mit Ihrem Azure-Abonnement](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH) her. 
2. Erstellen Sie ggf. mit [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup) eine neue Ressourcengruppe.  Verwenden Sie [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation), um Standorte von Rechenzentren aufzulisten. 
     
     ```azurepowershell-interactive
     # Get-AzureRmLocation 
     New-AzureRmResourceGroup –Name 'MySecureRG' –Location 'East US'
     ```

3. Erstellen Sie mit [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/New-AzureRmKeyVault) einen neuen Schlüsseltresor.
    
      ```azurepowershell-interactive
     New-AzureRmKeyVault -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -Location 'East US'
     ```

4. Notieren Sie **Tresorname**, **Ressourcengruppenname**, **Ressourcen-ID**, **Vault-URI** und **Objekt-ID**, die zur späteren Verwendung beim Verschlüsseln der Datenträger zurückgegeben werden. 


### <a name="bkmk_KVCLI"></a> Erstellen eines Schlüsseltresors mit der Azure CLI
Sie können Ihren Schlüsseltresor mit der Azure CLI mit den [az keyvault](/cli/azure/keyvault#commands)-Befehlen verwalten. Verwenden Sie [az keyvault create](/cli/azure/keyvault#az-keyvault-create), um einen Schlüsseltresor zu erstellen.

1. Stellen Sie ggf. eine [Verbindung mit Ihrem Azure-Abonnement](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI) her.
2. Erstellen Sie ggf. mit [az group create](/cli/azure/group#az-group-create) eine neue Ressourcengruppe. Verwenden Sie [az account list-locations](/cli/azure/account#az-account-list), um Orte aufzulisten. 
     
     ```azurecli-interactive
     # To list locations: az account list-locations --output table
     az group create -n "MySecureRG" -l "East US"
     ```

3. Erstellen Sie mit [az keyvault create](/cli/azure/keyvault#az-keyvault-create) einen neuen Schlüsseltresor.
    
     ```azurecli-interactive
     az keyvault create --name "MySecureVault" --resource-group "MySecureRG" --location "East US"
     ```

4. Notieren Sie **Tresorname** (Name), **Ressourcengruppenname**, **Ressourcen-ID** (ID), **Vault-URI** und **Objekt-ID**, die zur späteren Verwendung zurückgegeben werden. 

### <a name="bkmk_KVRM"></a> Erstellen eines Schlüsseltresors mit einer Resource Manager-Vorlage

Sie können mit der [Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create) einen Schlüsseltresor erstellen.

1. Klicken Sie in der Azure-Schnellstartvorlage auf **Bereitstellung in Azure**.
2. Wählen Sie Abonnement, Ressourcengruppe, Ressourcengruppenstandort, Schlüsseltresorname, Objekt-ID, rechtliche Bedingungen und Vereinbarung aus, und klicken Sie auf **Kaufen**. 


## <a name="bkmk_KVper"></a> Festlegen von erweiterten Zugriffsrichtlinien für Schlüsseltresore
Die Azure-Plattform benötigt Zugriff auf die Verschlüsselungsschlüssel oder geheimen Schlüssel in Ihrem Schlüsseltresor, um sie für den virtuellen Computer zur Verfügung zu stellen, damit die Volumes gestartet und entschlüsselt werden können. Aktivieren Sie die Datenträgerverschlüsselung im Schlüsseltresor, damit Bereitstellungen nicht fehlschlagen.  

### <a name="bkmk_KVperPSH"></a> Festlegen der erweiterten Zugriffsrichtlinien für Schlüsseltresore mit Azure PowerShell
 Verwenden Sie das PowerShell-Cmdlet für Schlüsseltresore [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy), um die Datenträgerverschlüsselung für den Schlüsseltresor zu aktivieren.

  - **Aktivieren von Key Vault für die Datenträgerverschlüsselung:** „EnabledForDiskEncryption“ ist für die Verwendung von Azure Disk Encryption erforderlich.
      
     ```azurepowershell-interactive 
     Set-AzureRmKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -EnabledForDiskEncryption
     ```

  - **Aktivieren von Key Vault für die Bereitstellung (falls erforderlich):** Der Ressourcenanbieter „Microsoft.Compute“ wird aktiviert, um Geheimnisse aus diesem Schlüsseltresor abzurufen, wenn dieser Schlüsseltresor bei der Ressourcenerstellung (z. B. beim Erstellen einer VM) referenziert wird.

     ```azurepowershell-interactive
      Set-AzureRmKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -EnabledForDeployment
     ```

  - **Aktivieren von Key Vault für die Vorlagenbereitstellung (falls erforderlich):** Azure Resource Manager kann aus diesem Schlüsseltresor Geheimnisse abrufen, wenn dieser Schlüsseltresor in einer Vorlagenbereitstellung referenziert wird.

     ```azurepowershell-interactive             
     Set-AzureRmKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -EnabledForTemplateDeployment`
     ```

### <a name="bkmk_KVperCLI"></a> Festlegen der erweiterten Zugriffsrichtlinien für Schlüsseltresore mit der Azure CLI
Verwenden Sie [az keyvault update](/cli/azure/keyvault#az-keyvault-update), um die Datenträgerverschlüsselung für den Schlüsseltresor zu aktivieren. 

 - **Aktivieren von Key Vault für die Datenträgerverschlüsselung:** „Enabled-for-disk-encryption“ ist erforderlich. 

     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MySecureRG" --enabled-for-disk-encryption "true"
     ```  

 - **Aktivieren von Key Vault für die Bereitstellung (falls erforderlich):** Der Ressourcenanbieter „Microsoft.Compute“ wird aktiviert, um Geheimnisse aus diesem Schlüsseltresor abzurufen, wenn dieser Schlüsseltresor bei der Ressourcenerstellung (z. B. beim Erstellen einer VM) referenziert wird.

     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MySecureRG" --enabled-for-deployment "true"
     ``` 

 - **Aktivieren von Key Vault für die Vorlagenbereitstellung (falls erforderlich):** Der Resource Manager kann Geheimnisse aus dem Tresor abrufen.
     ```azurecli-interactive  
     az keyvault update --name "MySecureVault" --resource-group "MySecureRG" --enabled-for-template-deployment "true"
     ```


### <a name="bkmk_KVperrm"></a> Festlegen der erweiterten Zugriffsrichtlinien für Schlüsseltresore im Azure-Portal

1. Wählen Sie Ihren Schlüsseltresor aus, navigieren Sie zu **Zugriffsrichtlinien**, und **Klicken Sie, um erweiterte Zugriffsrichtlinien anzuzeigen**.
2. Wählen Sie das Feld **Zugriff auf Azure Disk Encryption für Volumeverschlüsselung aktivieren** aus.
3. Wählen Sie ggf. **Zugriff auf Azure Virtual Machines für Bereitstellung aktivieren** und/oder **Zugriff auf Azure Resource Manager für Vorlagenbereitstellung aktivieren** aus. 
4. Klicken Sie auf **Speichern**.

![Erweiterte Zugriffsrichtlinien für Schlüsseltresore in Azure](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)


## <a name="bkmk_KEK"></a> Einrichten eines Schlüssels zur Schlüsselverschlüsselung (optional)
Wenn Sie Verschlüsselungsschlüssel mit einem Schlüssel für die Schlüsselverschlüsselung zusätzlich schützen möchten, fügen Sie Ihrem Schlüsseltresor einen Schlüsselverschlüsselungsschlüssel hinzu. Verwenden Sie das Cmdlet [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey), um im Schlüsseltresor einen Schlüsselverschlüsselungsschlüssel zu erstellen. Sie können den KEK auch aus Ihrem lokalen Hardwaresicherheitsmodul (HSM) für die Schlüsselverwaltung importieren. Weitere Informationen finden Sie in der [Key Vault](../key-vault/key-vault-hsm-protected-keys.md)-Dokumentation. Wenn ein Schlüsselverschlüsselungsschlüssel angegeben wird, verwendet Azure Disk Encryption diesen, um Verschlüsselungsgeheimnisse vor dem Schreiben in Key Vault zu umschließen. 

* Ihre URLs für das Geheimnis des Schlüsseltresors und den Schlüsselverschlüsselungsschlüssel (Key Encryption Key, KEK) müssen mit einer Versionsangabe versehen sein. Azure erzwingt diese Einschränkung der Versionsverwaltung. Gültige URLs für Geheimnisse und KEKs finden Sie in den folgenden Beispielen:

  * Beispiel für eine gültige Geheimnis-URL: *https://contosovault.vault.azure.net/secrets/EncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Beispiel für eine gültige KEK-URL: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Die Angabe von Portnummern als Teil von URLs für Schlüsseltresorgeheimnisse und KEK-URLs wird von Azure Disk Encryption nicht unterstützt. Beispiele für nicht unterstützte und unterstützte Schlüsseltresor-URLs finden Sie hier:

  * Unzulässige Schlüsseltresor-URL: *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Zulässige Schlüsseltresor-URL: *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

### <a name="bkmk_KEKPSH"></a> Festlegen eines Schlüssels für die Schlüsselverschlüsselung mit Azure PowerShell 
Bevor Sie das PowerShell-Skript verwenden, sollten Sie mit den Voraussetzungen für Azure Disk Encryption vertraut sein, um die Schritte im Skript zu verstehen. Das Beispielskript muss ggf. an Ihre Umgebung angepasst werden. Dieses Skript erstellt alle Voraussetzungen für Azure Disk Encryption und verschlüsselt eine vorhandene IaaS-VM, indem es den Datenträgerschlüssel mit einem Schlüssel für die Schlüsselverschlüsselung umschließt. 

 ```powershell
 # Step 1: Create a new resource group and key vault in the same location.
     # Fill in 'MyLocation', 'MySecureRG', and 'MySecureVault' with your values.
     # Use Get-AzureRmLocation to get available locations and use the DisplayName.
     # To use an existing resource group, comment out the line for New-AzureRmResourceGroup
     
     $Loc = 'MyLocation';
     $rgname = 'MySecureRG';
     $KeyVaultName = 'MySecureVault'; 
     New-AzureRmResourceGroup –Name $rgname –Location $Loc;
     New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname -Location $Loc;
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $KeyVaultResourceId = (Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname).ResourceId;
     $diskEncryptionKeyVaultUrl = (Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname).VaultUri;
     
 #Step 2: Enable the vault for disk encryption.
     Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $rgname -EnabledForDiskEncryption;
      
 #Step 3: Create a new key in the key vault with the Add-AzureKeyVaultKey cmdlet.
     # Fill in 'MyKeyEncryptionKey' with your value.
     
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName -Destination 'Software';
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     
 #Step 4: Encrypt the disks of an existing IaaS VM
     # Fill in 'MySecureVM' with your value. 
     
     $VMName = 'MySecureVM';
     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
```


 
## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Aktivieren von Azure Disk Encryption für Windows](azure-security-disk-encryption-windows.md)

> [!div class="nextstepaction"]
> [Aktivieren von Azure Disk Encryption für Linux](azure-security-disk-encryption-linux.md)

