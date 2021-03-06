---
title: Unterstützungsmatrix für die Notfallwiederherstellung von VMware-VMs oder physischen Servern in einem sekundären VMware-Standort mit Azure Site Recovery | Microsoft-Dokumentation
description: Dieser Artikel fasst die Unterstützung der Notfallwiederherstellung für VMware-VMs und physische Server in einem sekundären Standort mit Azure Site Recovery zusammen.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: raynew
ms.openlocfilehash: 7e86757f42a90de971155137b44d1a8ad9cc9ac1
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52847914"
---
# <a name="support-matrix-for-disaster-recovery-of-vmware-vms-and-physical-servers-to-a-secondary-site"></a>Unterstützungsmatrix für die Notfallwiederherstellung von VMware-VMs und physischen Servern

Dieser Artikel beschreibt, was unterstützt wird, wenn Sie den [Azure Site Recovery](site-recovery-overview.md)-Dienst für die Notfallwiederherstellung von VMware-VMs oder physischen Windows-/Linux-Servern in einem sekundären VMware-Standort verwenden.

- Wenn Sie VMware-VMs oder physische Server nach Azure replizieren möchten, lesen Sie [diese Unterstützungsmatrix](vmware-physical-azure-support-matrix.md).
- Wenn Sie Hyper-V-VMs an einen sekundären Standort replizieren möchten, lesen Sie [diese Unterstützungsmatrix](hyper-v-azure-support-matrix.md).

> [!NOTE]
> Die Replikation von lokalen VMware-VMs und physischen Servern erfolgt durch InMage Scout. InMage Scout ist im Abonnement des Azure Site Recovery-Diensts enthalten.


## <a name="host-servers"></a>Hostserver

**Betriebssystem** | **Details**
--- | ---
vCenter-Server | vCenter 5.5, 6.0 und 6.5<br/><br/> Wenn Sie Version 6.0 oder 6.5 ausführen, beachten Sie, dass nur 5.5-Features unterstützt werden.


## <a name="replicated-vm-support"></a>Unterstützung replizierter VMs

In der folgenden Tabelle werden die unterstützten Betriebssysteme für mit Site Recovery replizierte Computer aufgeführt. Unter dem unterstützten Betriebssystem können beliebige Workloads ausgeführt werden.

**Betriebssystem** | **Details**
--- | ---
Windows Server | Windows Server 2016 (64 Bit), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 mit mindestens SP1
Linux | Red Hat Enterprise Linux 6.7, 6.8, 6.9, 7.1, 7.2 <br/><br/> CentOS 6.5, 6.6, 6.7, 6.8, 6.9, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, 6.8, unter dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird <br/><br/> SUSE Linux Enterprise Server 11 SP3, 11 SP4 


## <a name="linux-machine-storage"></a>Speicher eines Linux-Computers

Nur Linux-Computer mit dem folgenden Speicher können repliziert werden:

- Dateisystem (EXT3, ETX4, ReiserFS, XFS)
- Multipfadsoftware (Geräte-Mapper)
- Volume-Manager (LVM2)
- Physische Server mit HP CCISS-Controllerspeicher werden nicht unterstützt.
- Das ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.

## <a name="network-configuration---hostguest-vm"></a>Netzwerkkonfiguration – Host/Gast-VM

**Konfiguration** | **Unterstützt**  
--- | --- 
Host – NIC-Teamvorgang | JA 
Host – VLAN | JA 
Host – IPv4 | JA 
Host – IPv6 | Nein  
Gast-VM – NIC-Teamvorgang | Nein 
Gast-VM – IPv4 | JA
Gast-VM – IPv6 | Nein 
Gast-VM – Windows/Linux – Statische IP-Adresse | JA
Gast-VM – Multi-NIC | JA


## <a name="storage"></a>Storage

### <a name="host-storage"></a>Hostspeicher

**Speicher (Host)** | **Unterstützt** 
--- | --- 
NFS | JA 
SMB 3.0 | N/V 
SAN (ISCSI) | JA 
Multipfad (MPIO) | JA 

### <a name="guest-or-physical-server-storage"></a>Gast- oder physischer Serverspeicher

**Konfiguration** | **Unterstützt** 
--- | --- 
VMDK | JA 
VHD/VHDX | N/V 
Gen 2-VM | N/V 
Freigegebener Clusterdatenträger | JA 
Verschlüsselter Datenträger | Nein  
UEFI| JA 
NFS | Nein  
SMB 3.0 | Nein  
RDM | JA 
Datenträger > 1 TB | JA 
Volume mit Stripesetdatenträgern > 1 TB<br/><br/> LVM | JA 
Speicherplätze | Nein  
Datenträger laufendem Systembetrieb hinzufügen/entfernen | JA 
Ausschließen von Datenträgern | JA 
Multipfad (MPIO) | N/V 

## <a name="vaults"></a>Tresore

**Aktion** | **Unterstützt** 
--- | --- 
Verschieben von Tresoren zwischen Ressourcengruppen (innerhalb oder zwischen Abonnements) | Nein  
Verschieben von Speicher, Netzwerk, Azure-VMs zwischen Ressourcengruppen (innerhalb oder zwischen Abonnements) | Nein  

## <a name="mobility-service-and-updates"></a>Mobilitätsdienst und -updates

Der Mobilitätsdienst koordiniert die Replikation zwischen lokalen VMware-Servern oder physischen Servern und dem sekundärem Standort. Stellen Sie beim Einrichten der Replikation sicher, dass die neueste Version des Mobilitätsdiensts und von anderen Komponenten installiert ist.

**Aktualisieren** | **Details** 
--- | --- 
Scout-Updates | [Informationen und Download](vmware-physical-secondary-disaster-recovery.md#updates) für die neuesten Scout-Updates | Scout-Updates sind kumulativ.
Komponentenupdates | Scout-Updates umfassen Updates für alle Komponenten, einschließlich RX-Server, Konfigurationsserver, Prozess- und Masterzielserver, vContinuum-Server und Quellserver, die Sie schützen möchten.<br/><br/> [Weitere Informationen](vmware-physical-secondary-disaster-recovery.md#download-and-install-component-updates).


## <a name="next-steps"></a>Nächste Schritte

Herunterladen des [InMage Scout-Benutzerhandbuchs](https://aka.ms/asr-scout-user-guide).

- [Replizieren von Hyper-V-VMs in VMM-Clouds in einer sekundären Cloud](tutorial-vmm-to-vmm.md)
- [Replizieren von VMware-VMs und physischen Servern an einem sekundären Standort](tutorial-vmware-to-vmware.md)
