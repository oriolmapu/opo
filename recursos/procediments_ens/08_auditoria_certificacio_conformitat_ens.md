# Procediment Operatiu de Seguretat (POS): Auditoria i Certificació de Conformitat amb l'ENS

> **Marc Normatiu de Referència:** Reial Decret 311/2022 (ENS) — Arts. 34 (Auditoria de seguretat), 35 (Informes d'auditoria), 41 (Conformitat amb l'ENS), Guia CCN-STIC 803 (Autoavaluació), Guia CCN-STIC 809 (Declaració i Certificació de Conformitat) i Guia CCN-STIC 824 (Metodologia d'Auditoria de l'ENS).  
> **Àmbit d'Aplicació:** Tots els sistemes d'informació, serveis electrònics ciutadans i procediments administratius de l'Ajuntament.

---

## 1. Objectiu i Abast

Aquest procediment estableix el cicle formal per auditar, avaluar i acreditar oficialment el compliment de l'**Esquema Nacional de Seguretat (RD 311/2022)** davant la ciutadania, el Centre Criptològic Nacional (**CCN-CERT**) i els organismes reguladors.

L'ENS estableix una diferenciació metodològica clara segons la categoria de seguretat del sistema:
- **Categoria BÀSICA:** Exigeix una **Declaració de Conformitat** derivada d'una autoavaluació interna o concertada formalment documentada (mínim anual).
- **Categories MITJANA i ALTA:** Exigeixen un **Certificat de Conformitat** expedit exclusivament per una **Entitat d'Auditoria i Certificació acreditada per ENAC**, amb periodicitat **biennal (cada 2 anys)** i una auditoria de seguiment intermèdia anual.

---

## 2. Rols Intervinents segons l'ENS

| Rol ENS | Responsable / Òrgan | Responsabilitats en el Procés d'Auditoria |
| :--- | :--- | :--- |
| **Alcaldia / Ple Municipal** | Òrgan Superior de Govern | Aprova la Política de Seguretat de la Informació (PSI) i signa formalment la Declaració de Conformitat o encarrega la contractació de l'auditoria externa. |
| **Responsable de la Seguretat (RSeg / CISO)** | Cap de Seguretat de la Informació | **Coordinador General de l'Auditoria:** Prepara les evidències, coordina l'autoavaluació prèvia a través de la plataforma INES/e-ENS, defensa el compliment davant els auditors externs i elabora el Pla d'Acció Correctora (PAC). |
| **Responsable de la Informació i del Servei (RI / RS)** | Caps d'Àrea i Serveis | Participen en les entrevistes d'auditoria, defensen la categorització dels seus procediments i faciliten les evidències funcionals. |
| **Responsable del Sistema (RSis)** | Cap d'Informàtica / Administrador TIC | Facilita les evidències tècniques (configuracions, bastionat, regles de firewall, logs del SIEM, còpies de seguretat) i acompanya els auditors a les instal·lacions del CPD. |
| **Entitat de Certificació Externa** | Empresa acreditada per ENAC | Executa l'auditoria formal independent (Fases 1 i 2), dictamina les no conformitats i emet el Certificat de Conformitat amb l'ENS. |

---

## 3. Diagrama de Flux del Procediment

```mermaid
flowchart TD
    Inici(["🏛️ 1. Inici del Cicle de Compliment ENS"]) --> CatSistemes["🎯 2. Categorització Oficial dels Sistemes Municipals (Art. 15)<br/>• Avaluació de les 5 dimensions (DAICT) per part dels RI i RS<br/>• Determinació del nivell del sistema: Bàsica, Mitjana o Alta"]
    
    CatSistemes --> Bifurcacio{"Nivell de Seguretat Máxim"}
    
    %% CIRCUIT BÀSIC
    Bifurcacio -- "Categoria BÀSICA" --> AutoAval["📋 3A. Autoavaluació Anual Documentada (Art. 34.3)<br/>• Emplenament del qüestionari oficial a la plataforma INES / e-ENS<br/>• Recull d'evidències documentals i tècniques"]
    AutoAval --> InformeIntern["📄 Redacció de l'Informe d'Autoavaluació pel RSeg"]
    InformeIntern --> DeclaracioConformitat["✍️ 4A. Emissió de la DECLARACIÓ DE CONFORMITAT [Art. 41.1]<br/>• Signatura pel Responsable de Seguretat i Decret d'Alcaldia<br/>• Publicació del distintiu oficial a la Seu Electrònica municipal"]
    
    %% CIRCUIT MITJÀ / ALT
    Bifurcacio -- "Categoria MITJANA o ALTA" --> ContractacioENAC["📑 3B. Contractació d'Auditoria Externa Acreditada [Art. 34.1]<br/>• Licitació amb Entitats de Certificació acreditades per ENAC<br/>• Fixació de l'abast d'auditoria (Seu electrònica, Padró, Gestió Tributària)"]
    
    ContractacioENAC --> Fase1Doc["🔍 4B. Fase 1: Auditoria Documental de Compliment<br/>• Revisió de la PSI, inventari d'actius, anàlisi de riscos (PILAR/MAGERIT)<br/>• Examen dels Procediments Operatius de Seguretat (POS) i protocols"]
    
    Fase1Doc --> Fase2InSitu["🏢 5B. Fase 2: Auditoria In Situ i Verificació Tècnica<br/>• Inspecció física del Centre de Processament de Dades (CPD)<br/>• Mostreig tècnic: Bastionat de servidors, regles de firewall, còpies immutables<br/>• Entrevistes als rols de seguretat (RI, RS, RSeg, RSis, DPD)"]
    
    Fase2InSitu --> InformeAuditoria["📊 6B. Informe d'Auditoria i Detecció de Troballes<br/>• Identificació de No Conformitats Majors (NCM), Menors (NCm) i Oportunitats de Millora"]
    
    InformeAuditoria --> HiHaNCM{"Existeixen No Conformitats<br/>Majors (NCM)?"}
    
    HiHaNCM -- "SÍ" --> PAC_Major["🛠️ Pla d'Acció Correctora Urgent (Màx. 3 mesos)<br/>• Resolució de les deficiències greus i reauditoria extraordinària"]
    PAC_Major --> Fase2InSitu
    
    HiHaNCM -- "NO" --> EmissioCertificat["📜 7B. EMISSIÓ DEL CERTIFICAT DE CONFORMITAT AMB L'ENS [Art. 41.2]<br/>• Lliurament per l'Entitat Certificadora acreditada per ENAC<br/>• Vigència legal de 2 ANYS (amb seguiment anual)"]
    
    EmissioCertificat --> SegellSeu["🛡️ 8B. Publicació del Segell Oficial de Seguretat ENS<br/>• Inserció del Segell de Conformitat amb codi QR a la Seu Electrònica<br/>• Comunicació a la plataforma INES del CCN-CERT"]
    
    DeclaracioConformitat & SegellSeu --> SeguimentContinu(["🔄 MILLORA CONTÍNUA I AUDITORIA BIENNAL / SEGUIMENT ANUAL"])
```

---

## 4. Fases Detallades d'Execució

### Fase 1: Categorització dels Sistemes i Abast d'Auditoria
1. El **Comitè de Seguretat de la Informació** consolida l'abast dels sistemes d'informació municipals.
2. Si qualsevol servei gestiona informació amb impacte Mitjà o Alt en alguna de les 5 dimensions (com ara tràmits telemàtics amb certificat, gestió econòmica o padró d'habitants), tot el sistema que hi dona suport queda categoritzat en nivell **Mitjà o Alt**.

### Fase 2: Circuit per a Categoria Bàsica (Declaració de Conformitat)
1. **Autoavaluació:** El Responsable de Seguretat analitza les mesures de l'Annex II aplicables a nivell Bàsic mitjançant les eines **e-ENS** o la plataforma **INES** del CCN-CERT.
2. **Informe d'adequació:** Es redacta l'informe anual d'autoavaluació identificant l'estat de cada mesura.
3. **Declaració de Conformitat:** L'Alcalde/ssa o el Regidor delegat signa la *Declaració de Conformitat amb l'ENS*, que es publica a la Seu Electrònica i té una validesa d'un any.

### Fase 3: Circuit per a Categories Mitjana i Alta (Certificat de Conformitat ENAC)

#### Etapa A: Auditoria Documental (Fase 1)
L'equip auditor d'ENAC examina el corpus documental municipal:
- Política de Seguretat de la Informació (PSI) aprovada formalment.
- Nomenaments formals dels rols ENS (RI, RS, RSeg, RSis, DPD).
- Anàlisi de Riscos actualitzada (metodologia MAGERIT / eina PILAR).
- Procediments Operatius de Seguretat (POS) de còpies, canvis, incidents i accessos.
- Evidències de formació en ciberseguretat als empleats públics.

#### Etapa B: Auditoria Tècnica i Presencial (Fase 2)
1. **Inspecció d'instal·lacions:** Visita al CPD municipal per comprovar el control d'accés biomètric/targeta, sistema de climatització redundada, SAIs i extinció automàtica d'incendis mitjançant gas innocent (`[mp.if]`).
2. **Comprovacions tècniques aleatòries:**
   - Connexió a la consola de l'Active Directory / Entra ID per verificar que cap usuari comú és administrador local i que el MFA està activat (`[op.acc]`).
   - Verificació del xifratge BitLocker en una mostra de portàtils (`[mp.eq.3]`).
   - Comprovació de la immutabilitat de les còpies de seguretat i simulacre de restauració (`[op.cont]`).
   - Revisió de les regles de firewall i segmentació de xarxa per VLANs.

#### Etapa C: Dictamen, Pla d'Acció Correctora (PAC) i Emissió
1. L'equip auditor emet l'**Informe d'Auditoria**:
   - **No Conformitat Major (NCM):** Absència d'una mesura obligatòria o fallada que posa en risc greu el sistema (bloqueja l'emissió del certificat fins a la seva subsanació).
   - **No Conformitat Menor (NCm):** Desviació puntual que no compromet directament la seguretat global. L'Ajuntament presenta un Pla d'Acció Correctora (PAC) amb terminis de solució.
2. Un cop aprovat el PAC, la comissió de certificació de l'entitat acreditada emet el **Certificat de Conformitat amb l'ENS**.

### Fase 4: Vigència i Publicitat del Distintiu de Seguretat (Art. 41)
- **Vigència:** El certificat té una validesa màxima de **2 ANYS**.
- **Auditoria de seguiment:** Al cap d'un any d'haver obtingut el certificat s'ha de superar una auditoria intermèdia de seguiment.
- **Distintiu oficial:** L'Ajuntament adquireix el dret d'incorporar a la capçalera de la seva Seu Electrònica el **Segell Oficial de Conformitat amb l'ENS**, acompanyat de la categoria acreditada (Mitjana o Alta) i el número de certificat emès per ENAC.

---

## 5. Taula Comparativa: Declaració vs. Certificat de Conformitat

| Característica | Categoria BÀSICA | Categories MITJANA i ALTA |
| :--- | :--- | :--- |
| **Instrument Formal** | **Declaració de Conformitat** | **Certificat de Conformitat** |
| **Òrgan Avaluador** | Autoavaluació pel propi Ajuntament | **Entitat de Certificació acreditada per ENAC** |
| **Periodicitat Ordinària** | Autoavaluació anual | **Auditoria cada 2 ANYS** (+ seguiment anual) |
| **Eina Oficial de Suport** | Plataforma INES / e-ENS (CCN-CERT) | Metodologia Guia CCN-STIC 824 |
| **Exhibició de Segell** | Distintiu de Declaració a la Seu | **Segell Oficial de Certificació ENS** amb codi ENAC |
| **Auditoria Extraordinària** | En canvis estructurals rellevants | Preceptiva si hi ha canvis substancials o incidents greus |
