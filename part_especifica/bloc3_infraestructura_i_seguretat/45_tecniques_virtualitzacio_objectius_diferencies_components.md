# Tema 45. Tècniques de virtualització: objectius, diferències, components i implementació

> **Fonts i marcs de referència:** Esquema Nacional de Seguretat ([`CORPUS/ENS_2022.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENS_2022.pdf) - Mesura `[mp.sw.1]` Aïllament i `[mp.if]`), Guia CCN-STIC 823 (*Seguretat en Virtualització*) i principis de virtualització de Popek-Goldberg (Intel VT-x / AMD-V).

---

## 1. Concepte i Objectius Fonamentals de la Virtualització

La **virtualització** és la tecnologia que permet crear una capa d'abstracció entre el maquinari físic (*hardware*) i els sistemes operatius i aplicacions, permetent que múltiples sistemes operatius s'executin de manera simultània, aïllada i independent sobre una mateixa infraestructura física.

```mermaid
flowchart TD
    subgraph OBJECTIUS["OBJECTIUS CLAU DE LA VIRTUALITZACIÓ"]
        O1["1. Consolidació de Servidors: Reduir desenes de servidors físics a pocs amfitrions."]
        O2["2. Aïllament i Seguretat: Una incidència en una VM no contamina la resta."]
        O3["3. Agilitat i Portabilitat: Migració de màquines en calent entre servidors (Live Migration)."]
        O4["4. Optimització de Costos i Eficiència Energètica (Green IT municipal)."]
        O5["5. Ràpida Recuperació davant Desastres mitjançant còpies i snapshots."]
    end
```

---

## 2. Tipologia d'Hipervisors: Tipus 1 vs. Tipus 2

L'element central de la virtualització és l'**Hipervisor** o **Monitor de Màquines Virtuals (VMM - *Virtual Machine Monitor*)**:

```mermaid
flowchart TD
    subgraph T1["HIPERVISOR TIPUS 1 (Bare-Metal / Natiu)"]
        direction TB
        VM_T1["Màquines Virtuals (VMs)"]
        Hyp1["Hipervisor Tipus 1 (ESXi / KVM / Hyper-V / Proxmox)"]
        HW1["Maquinari Físic Directe (CPU, RAM, Discs)"]
        VM_T1 --> Hyp1 --> HW1
    end

    subgraph T2["HIPERVISOR TIPUS 2 (Hosted / Sobre SO)"]
        direction TB
        VM_T2["Màquines Virtuals (VMs)"]
        Hyp2["Hipervisor Tipus 2 (VirtualBox / VMware Workstation)"]
        HostOS["Sistema Operatiu Amfitrió (Windows / Linux)"]
        HW2["Maquinari Físic"]
        VM_T2 --> Hyp2 --> HostOS --> HW2
    end
```

| Criteri de Comparació | Hipervisor Tipus 1 (*Bare-Metal*) | Hipervisor Tipus 2 (*Hosted*) |
| :--- | :--- | :--- |
| **Ubicació en l'Arquitectura** | S'executa **directament sobre el maquinari físic** (sense SO amfitrió). | S'executa com una **aplicació dins d'un SO amfitrió**. |
| **Rendiment i Latència** | **Excel·lent i d'alta eficiència (overhead < 2%)**. | Menor rendiment (*overhead* doble pel SO amfitrió). |
| **Seguretat i Superfície d'Atac** | **Molt alta:** Nucli reduït (*microkernel*) sense serveis innecessaris. | Menor: La seguretat depèn de la integritat del SO amfitrió. |
| **Casos d'Ús Habituals** | **Centres de dades (CPD), servidors de producció municipal**. | **Desenvolupament, proves de laboratori, formació**. |
| **Solucions Tecnològiques Líders** | **VMware ESXi, KVM (Linux), Microsoft Hyper-V, Proxmox VE**. | **Oracle VirtualBox, VMware Workstation / Fusion**. |

---

## 3. Tècniques de Virtualització

```mermaid
flowchart TD
    subgraph TECNIQUES["LES 3 PRINCIPALS TÈCNIQUES DE VIRTUALITZACIÓ"]
        T_Full["1. VIRTUALITZACIÓ COMPLETA (Full Virtualization)<br/>L'hipervisor emula el maquinari al 100%.<br/>El SO convidat NO sap que està virtualitzat i NO requereix modificacions."]
        T_Para["2. PARAVIRTUALITZACIÓ (Para-virtualization)<br/>El SO convidat utilitza controladors optimitzats (VirtIO / Hypercalls).<br/>Comunicació directa amb l'hipervisor amb màxima velocitat."]
        T_HW["3. VIRTUALITZACIÓ ASSISTIDA PER MAQUINARI<br/>Instruccions directes a nivell de CPU (Intel VT-x / AMD-V) i I/O (VT-d).<br/>És l'estàndard universal modern."]
    end
```

---

## 4. Components Principals d'una Plataforma de Virtualització

```mermaid
flowchart TD
    subgraph COMPONENTS["COMPONENTS D'UNA INFRAESTRUCTURA DE VIRTUALITZACIÓ"]
        Compute["1. Compute (Processament i Memòria)<br/>Nuclis virtuals (vCPU) i memòria RAM assignada a les VMs."]
        Storage["2. Emmagatzematge Virtual (DataStores)<br/>Fitxers d'imatge de disc (.vmdk, .vhdx, .qcow2) allotjats a SAN/NAS."]
        Network["3. Xarxa Virtual (vSwitch / Port Groups)<br/>Commutadors virtuals que connecten les vNICs amb les VLANs físiques."]
        Management["4. Gestor Centralitzat de Clúster<br/>Panell d'administració (VMware vCenter, Proxmox Cluster, SCVMM)."]
    end
```

---

## 5. Quadre Resum i Preguntes Típiques d'Examen Tipus Test

| Pregunta / Concepte Clau | Resposta Correcta / Trampa Freqüent |
| :--- | :--- |
| **Quina és la diferència principal entre un hipervisor Tipus 1 i Tipus 2?** | El **Tipus 1 s'executa directament sobre el maquinari bare-metal**, mentre que el **Tipus 2 s'executa sobre un sistema operatiu amfitrió**. |
| **Quin tipus d'hipervisor és KVM o VMware ESXi?** | Són hipervisors de **Tipus 1 (Bare-Metal)**. |
| **Què és la paravirtualització?** | Una tècnica on el sistema convidat utilitza **controladors optimitzats (com VirtIO)** per comunicar-se directament amb l'hipervisor. |
| **Quines tecnologies de CPU permeten la virtualització per maquinari?** | **Intel VT-x** i **AMD-V** (i Intel VT-d / AMD-Vi per a I/O). |
| **Quins són els formats de disc virtual més comuns?** | **VMDK** (VMware), **VHDX** (Microsoft) i **QCOW2** (KVM/Proxmox). |
