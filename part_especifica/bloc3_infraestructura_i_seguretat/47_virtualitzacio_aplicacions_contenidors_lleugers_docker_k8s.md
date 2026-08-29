# Tema 47. Virtualització de programari d'aplicacions. Desplegament de programari d'aplicacions en contenidors lleugers

> **Fonts i marcs de referència:** Esquema Nacional d'Interoperabilitat ([`CORPUS/ENI.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENI.pdf) - Reutilització i transferència tecnològica), Guia CCN-STIC 886 (*Seguretat en Contenidors i Kubernetes*) i especificacions de la Open Container Initiative (OCI).

---

## 1. Virtualització de Programari d'Aplicacions

La **virtualització d'aplicacions** consisteix a encapsular una aplicació juntament amb les seves llibreries (DLLs, dependències) i configuracions de registre en un paquet independent que s'executa en una capa aïllada (*sandbox*) sobre el sistema operatiu de l'usuari:

```mermaid
flowchart LR
    subgraph VIRT_APP["VIRTUALITZACIÓ D'APLICACIONS"]
        AppPack["Paquet d'Aplicació Aïllat (ex. App-V / ThinApp)<br/>(Binaris + DLLs + Claus de Registre Pròpies)"]
        Sandbox["Capa d'Aïllament / Redirecció (Sandbox)"]
        OS["Sistema Operatiu de l'Usuari (Windows / Linux)"]
        AppPack --> Sandbox --> OS
    end
```

- **Principals Avantatges:**
  - **Eliminació del conflicte de llibreries (*DLL Hell*):** Permet executar diferents versions d'un mateix programari incompatibles entre si (per exemple, dues versions de Java runtime) en un mateix ordinador.
  - **Desplegament i actualització neta:** L'aplicació no modifica el registre ni copia fitxers a carpetes del sistema; eliminar l'aplicació és tan senzill com esborrar el paquet.

---

## 2. Contenidors Lleugers (*Containerization*): Màquines Virtuals vs. Contenidors

Mentre que les Màquines Virtuals (VMs) virtualitzen el maquinari complet (incloent un sistema operatiu convidat sencer), els **contenidors** virtualitzen a **nivell de sistema operatiu**, compartint el mateix nucli (*kernel*) del sistema amfitrió:

```mermaid
flowchart TD
    subgraph VM_STACK["A) MÀQUINES VIRTUALS (VMs)"]
        direction TB
        AppVM["Aplicació A"]
        LibVM["Llibreries / Bin"]
        GuestOS["SO Convidat Complet (GBs)"]
        HypVM["Hipervisor (ESXi / KVM)"]
        HWVM["Maquinari Físic"]
        AppVM --> LibVM --> GuestOS --> HypVM --> HWVM
    end

    subgraph CONT_STACK["B) CONTENIDORS LLEUGERS"]
        direction TB
        AppC["Aplicació A / B / C"]
        LibC["Llibreries Específiques (MBs)"]
        Engine["Motor de Contenidors (Docker / Podman)"]
        HostKernel["Kernel Compartit del SO Amfitrió (Linux)"]
        HWCONT["Maquinari Físic"]
        AppC --> LibC --> Engine --> HostKernel --> HWCONT
    end
```

| Criteri de Comparació | Màquines Virtuals (VMs) | Contenidors Lleugers (Docker / Podman) |
| :--- | :--- | :--- |
| **Abstracció** | Nivell de **Maquinari físic** (Hardware). | Nivell de **Sistema Operatiu** (Kernel compartit). |
| **Pes i Mida** | Pesades (**diversos GigaBytes** per VM). | Molt lleugers (**desenes o centenars de MegaBytes**). |
| **Temps d'Arrencada** | Minuts (requereix boot del SO convidat). | **Mil·lisegons o pocs segons** (arrencada immediata). |
| **Mecanismes d'Aïllament** | Aïllament per hipervisor (molt robust). | Aïllament per **Namespaces** i **Cgroups** de Linux. |
| **Densitat per Servidor** | Desenes de VMs per node. | **Centenars de contenidors** per node físic. |

---

## 3. L'Ecosistema de Contenidors: Docker, Podman i Registres

```mermaid
flowchart TD
    subgraph LIFECYCLE["CICLE DE VIDA DELS CONTENIDORS"]
        DockerFile["1. Dockerfile / Containerfile<br/>(Codi de definició de l'entorn)"]
        Image["2. Imatge de Contenidor (Image)<br/>(Plantilla immutable per capes - OverlayFS)"]
        Registry["3. Registre de Contenidors (Registry)<br/>(Magatzem d'imatges: Docker Hub / Harbor privat)"]
        Container["4. Contenidor en Execució (Container)<br/>(Instància viva del servei municipal)"]

        DockerFile -->|"docker build"| Image
        Image -->|"docker push/pull"| Registry
        Image -->|"docker run"| Container
    end
```

- **Mecanismes de Nucli de Linux utilitzats:**
  - **Namespaces:** Proporcionen aïllament lògic (processos `pid`, xarxa `net`, muntatges `mnt`, usuaris `user`).
  - **Control Groups (*cgroups*):** Limiten i mesuren el consum de recursos (CPU, memòria RAM, I/O de disc).

---

## 4. Orquestració de Contenidors a Gran Escala: Kubernetes (K8s)

Quan l'Ajuntament desplega desenes de microserveis (portals de tràmits, passarel·les de pagament, gestors de signatures), cal un **orquestrador automàtic**:

```mermaid
flowchart TD
    subgraph K8S["CLÚSTER DE KUBERNETES (K8s)"]
        Master["Control Plane (Master Node)<br/>- API Server<br/>- Scheduler<br/>- Controller Manager<br/>- etcd (Base de dades d'estat)"]
        
        subgraph WORKERS["Worker Nodes (Servidors d'Execució)"]
            NodeA["Node 1 (Kubelet + Kube-proxy)"]
            NodeB["Node 2 (Kubelet + Kube-proxy)"]
            
            Pod1["Pod A (Seu Electrònica)"]
            Pod2["Pod B (Passarel·la Pagament)"]
            
            NodeA --- Pod1
            NodeB --- Pod2
        end

        Master --> WORKERS
    end
```

- **Conceptes Fonamentals de Kubernetes:**
  - **Pod:** La unitat de desplegament mínima a K8s; conté un o més contenidors que comparteixen xarxa i emmagatzematge.
  - **Autorecuperació (*Self-Healing*):** Si un contenidor cau o un servidor s'avaria, Kubernetes recrea automàticament el Pod en un altre node en segons.
  - **Escalabilitat Automàtica (*Horizontal Pod Autoscaler - HPA*):** Si s'obre el període de matriculació a les escoles bressol i augmenta el trànsit, K8s multiplica el nombre de Pods automàticament.
  - **Actualitzacions sense caiguda (*Rolling Updates*):** Permet actualitzar la versió de l'aplicació web municipal de forma progressiva sense cap minut d'indisponibilitat ciutadana.

---

## 5. Quadre Resum i Preguntes Típiques d'Examen Tipus Test

| Pregunta / Concepte Clau | Resposta Correcta / Trampa Freqüent |
| :--- | :--- |
| **Quina és la diferència fonamental entre una VM i un contenidor?** | Els contenidors **comparteixen el kernel del sistema operatiu amfitrió**; les VMs tenen un SO complet independent. |
| **Quins dos mecanismes de Linux fan possible l'aïllament dels contenidors?** | Els **Namespaces** (aïllament de processos/xarxa) i els **Control Groups (*cgroups*)** (límit de recursos). |
| **Què és un Pod a Kubernetes?** | La **unitat mínima d'execució i desplegament**, que conté un o més contenidors estretament vinculats. |
| **Què permet l'arquitectura d'imatges per capes a Docker?** | Reutilitzar capes comunes només de lectura (*OverlayFS*), **estalviant espai de disc i temps de descàrrega**. |
| **Quin és el propòsit d'un orquestrador com Kubernetes?** | Automatitzar el **desplegament, l'escalat automàtic, la recuperació de fallades (*self-healing*) i el balanç de càrrega**. |
