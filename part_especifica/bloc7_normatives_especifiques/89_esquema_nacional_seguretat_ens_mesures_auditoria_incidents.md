# Tema 89. Esquema nacional de seguretat: mesures per garantir la seguretat de la informació i els serveis electrònics, auditoria de seguretat, resposta davant incidents de seguretat i certificació de la seguretat

> **Font normativa de referència:** [`CORPUS/ENS_2022.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENS_2022.pdf)  
> **Text:** Reial Decret 311/2022, de 3 de maig, pel qual es regula l'Esquema Nacional de Seguretat (ENS). Text consolidat.

---

## 1. Objecte, Finalitat i Àmbit de l'ENS (RD 311/2022)

L'**Esquema Nacional de Seguretat (ENS - RD 311/2022)** estableix els principis bàsics, requisits mínims i mesures de protecció que han d'aplicar totes les administracions públiques (incloent tots els ajuntaments) i els seus proveïdors tecnològics per garantir la **seguretat de la informació tractada i dels serveis prestats per mitjans electrònics**.

```mermaid
flowchart TD
    ENS_OBJ["OBJECTIU I FONAMENTS DE L'ENS (RD 311/2022)"]
    
    subgraph AMBIT["Àmbit d'Aplicació (Art. 2)"]
        A1["Totes les Administracions Públiques (AGE, CCAA, Ajuntaments)"]
        A2["Sector Públic Institucional (Organismes Autònoms, EPEL)"]
        A3["Proveïdors del sector privat que presten serveis a administracions"]
    end

    subgraph FINALITAT["Finalitats Clau"]
        F1["Generar confiança ciutadana en l'administració electrònica"]
        F2["Protecció integral davant ciberamenaces i ransomware"]
        F3["Garantir la continuïtat dels serveis públics"]
    end

    ENS_OBJ --> AMBIT
    ENS_OBJ --> FINALITAT
```

---

## 2. Els Set Principis Bàsics de l'ENS (Arts. 5 a 11)

```mermaid
flowchart TD
    PrincipisENS["ELS 7 PRINCIPIS BÀSICS DE L'ENS"]
    
    P1["1. Seguretat com a procés integral (humans, tècnics i organitzatius)"]
    P2["2. Gestió de la seguretat basada en els RISCOS (MAGERIT / PILAR)"]
    P3["3. Prevenció, detecció, resposta i recuperació"]
    P4["4. Línies de defensa en profunditat (múltiples capes de seguretat)"]
    P5["5. Vigilància contínua i monitorització (SOC / CCN-CERT)"]
    P6["6. Reavaluació periòdica dels riscos i millora contínua"]
    P7["7. Diferenciació de responsabilitats (Rols de seguretat)"]

    PrincipisENS --> P1
    PrincipisENS --> P2
    PrincipisENS --> P3
    PrincipisENS --> P4
    PrincipisENS --> P5
    PrincipisENS --> P6
    PrincipisENS --> P7
```

---

## 3. Dimensions de Seguretat i Categorització de Sistemes (Arts. 13 a 15)

### 3.1. Les Cinc Dimensions de la Seguretat (DAICT)
1. **Disponibilitat (D):** Garantia que els usuaris autoritzats tenen accés a la informació i serveis quan ho requereixen.
2. **Autenticitat (A):** Garantia que la informació o comunicació prové de la font o identitat autèntica que declara ser.
3. **Integritat (I):** Garantia que la informació no ha estat alterada, modificada o esborrada de forma no autoritzada.
4. **Confidencialitat (C):** Garantia que la informació només és accessible a persones, processos o sistemes autoritzats.
5. **Traçabilitat (T):** Garantia que les accions sobre el sistema es poden rastrejar de forma unívoca fins a l'usuari que les ha executat (*registre de logs*).

---

### 3.2. Categories de Seguretat del Sistema d'Informació (Art. 15)
El sistema d'informació de l'Ajuntament es classifica en una de les **tres categories de seguretat**, determinades pel nivell d'impacte més alt (**Baix, Mitjà o Alt**) que tindria un incident en qualsevol de les 5 dimensions:

```mermaid
flowchart LR
    subgraph CATEGORIES["CATEGORITZACIÓ DEL SISTEMA (Art. 15 RD 311/2022)"]
        CB["1. Categoria BÀSICA<br/>L'impacte màxim d'un incident és BAIX en totes les dimensions.<br/>(Exigència: Declaració de Conformitat)."]
        CM["2. Categoria MITJANA<br/>L'impacte màxim és MITJÀ en almenys una dimensió.<br/>(Exigència: CERTIFICAT DE CONFORMITAT i auditoria biennal)."]
        CA["3. Categoria ALTA<br/>L'impacte màxim és ALT en almenys una dimensió.<br/>(Exigència: CERTIFICAT DE CONFORMITAT i màximes mesures)."]
    end
```

---

## 4. Marc Operacional i Mesures de Seguretat (Annex II RD 311/2022)

Les mesures de seguretat s'agrupen en tres grans marcs:

```mermaid
flowchart TD
    subgraph MARCS_MESURES["MESURES DE SEGURETAT DE L'ENS (Annex II)"]
        subgraph ORG["1. Marc Organitzatiu [org]"]
            O1["Política de Seguretat de la Informació (PSI)"]
            O2["Rols: Responsable Informació, Responsable Servei, Responsable Seguretat"]
            O3["Procediments d'autorització d'accessos"]
        end
        
        subgraph OP["2. Marc Operacional [op]"]
            OP1["Control d'accés i autenticació (MFA obligatori)"]
            OP2["Còpies de seguretat (Estratègia 3-2-1 immutable)"]
            OP3["Pla de Continuïtat i Recuperació davant Desastres (DRP)"]
            OP4["Gestió de canvis i gestió de vulnerabilitats"]
        end
        
        subgraph MP["3. Mesures de Protecció [mp]"]
            MP1["Protecció d'instal·lacions (CPD segur, control d'accés físic)"]
            MP2["Protecció d'equips (xifratge de discs BitLocker, bloqueig USB)"]
            MP3["Protecció de comunicacions (xifrat TLS 1.3, VPN corporativa)"]
            MP4["Protecció de la informació (antivirus EDR, SIEM corporatiu)"]
        end
    end
```

---

## 5. Auditoria de Seguretat de l'ENS (Art. 34 RD 311/2022)

- **Obligatorietat:** Els sistemes d'informació de categoria **MITJANA o ALTA** han de sotmetre's preceptivament a una **auditoria formal de seguretat ordinària com a mínim cada 2 ANYS**.
- **Auditoria extraordinària:** S'ha d'efectuar obligatòriament abans dels 2 anys si es produeixen modificacions substancials en els sistemes o infraestructures tecnològiques que puguin repercutir en les mesures de seguretat.
- **Sistemes de Categoria Bàsica:** No exigeixen auditoria formal externa biennal, però han de sotmetre's a una **autoavaluació documentada anual** que verifiqui el compliment de les mesures.

---

## 6. Resposta davant Incidents de Seguretat i Notificació (Arts. 36 a 38)

Tots els ajuntaments han de disposar d'un procediment de gestió i resposta davant ciberatacs i incidents de seguretat:

```mermaid
sequenceDiagram
    autonumber
    participant Aj as Ajuntament (Responsable Seguretat)
    participant SOC as Centre Operacions Ciberseguretat (SOC Local)
    participant CCN as CCN-CERT (Plataforma LUCÍA)
    participant APD as APDCAT (si afecta dades personals)

    Aj->>SOC: 1. Detecció d'anomalia o ciberatac (ransomware, intrusió)
    SOC->>SOC: 2. Contenció immediata i aïllament d'equips afectats
    SOC->>CCN: 3. Notificació obligatòria de l'incident al CCN-CERT (Plataforma LUCÍA)
    alt L'incident afecta dades de caràcter personal (Data Breach)
        Aj->>APD: 4. Notificació preceptiva a l'APDCAT en el termini MÀXIM DE 72 HORES
    end
    SOC->>Aj: 5. Recuperació de sistemes mitjançant còpies de seguretat immutables
    SOC->>CCN: 6. Informe final d'anàlisi forense i tancament d'incident
```

- **Plataforma d'intercanvi:** Les notificacions d'incidents al **CCN-CERT** es realitzen a través de les eines oficials **LUCÍA** i **INES**.

---

## 7. Certificació de Conformitat amb l'ENS (Art. 41)

| Categoria del Sistema | Instrument de Conformitat Exigit | Entitat que l'Emet / Termini |
| :--- | :--- | :--- |
| **Categoria BÀSICA** | **Declaració de Conformitat amb l'ENS** | Formulada pel mateix Ajuntament mitjançant autoavaluació interna (amb suport de la Guia CCN-STIC 803 / e-ENS). |
| **Categories MITJANA i ALTA** | **Certificat de Conformitat amb l'ENS** | Emès exclusivament per una **Entitat d'Auditoria i Certificació acreditada per ENAC** (Entitat Nacional d'Acreditació). Té una **vigència màxima de 2 ANYS** (amb auditoria de seguiment intermèdia a l'any). |

- **Distintiu de Seguretat:** Els ajuntaments certificats adquireixen el dret a exhibir el **Segell oficial de Conformitat amb l'ENS** a la seva Seu Electrònica i portals web.

---

## 8. Quadre Resum i Preguntes Típiques d'Examen Tipus Test

| Pregunta / Concepte Clau | Resposta Correcta / Trampa Freqüent |
| :--- | :--- |
| **Quina norma regula l'Esquema Nacional de Seguretat actual?** | El **Reial Decret 311/2022 (RD 311/2022, ENS)**. |
| **Quines són les 5 dimensions de la seguretat a l'ENS?** | **Disponibilitat, Autenticitat, Integritat, Confidencialitat i Traçabilitat (DAICT)**. |
| **Quines categories de seguretat preveu l'ENS?** | **BÀSICA, MITJANA i ALTA** (determinades pel màxim impacte). |
| **Amb quina freqüència s'ha de fer l'auditoria ordinària a sistemes Mitjans/Alts?** | Com a mínim **cada 2 ANYS** (Art. 34 RD 311/2022). |
| **A quin organisme s'han de notificar obligatòriament els ciberincidents?** | Al **CCN-CERT** (a través de la plataforma *LUCÍA*) i a l'Agència de Ciberseguretat de Catalunya. |
| **Qui pot emetre el Certificat de Conformitat amb l'ENS per a nivell Mitjà/Alt?** | Una **Entitat de Certificació acreditada per ENAC** (Art. 41 RD 311/2022). |
| **Quina vigència té el Certificat de Conformitat amb l'ENS?** | Una vigència de **2 ANYS**, amb auditories de seguiment anuals. |
