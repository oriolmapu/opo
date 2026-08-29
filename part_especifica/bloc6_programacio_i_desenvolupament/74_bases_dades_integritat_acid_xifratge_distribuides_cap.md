# Tema 74. Sistemes de bases de dades: integritat (ACID), confidencialitat, xifratge (TDE), seguretat ENS i problemàtica de bases de dades distribuïdes (Teorema CAP, 2PC)

> **Fonts i marcs de referència:** Esquema Nacional de Seguretat ([`CORPUS/ENS_2022.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENS_2022.pdf) - Mesures `[mp.if.3]` Xifratge, `[mp.sw.2]` Validació d'entrades contra SQLi i `[op.exp]`), Reglament General de Protecció de Dades ([`CORPUS/LOPD.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/LOPD.pdf)), estàndard **SQL-92 (Nivells d'aïllament)** i **Teorema CAP d'Eric Brewer**.

---

## 1. Integritat Transaccional: Les Propietats ACID

Una **transacció** és una unitat lògica de treball indivisible. Per garantir la integritat absoluta de les dades municipals (com el cobrament d'un impost o el canvi de domicili al padró), els SGBD han de complir les **quatre propietats ACID**:

```mermaid
flowchart TD
    subgraph PROPIETATS_ACID["LES 4 PROPIETATS ACID D'UN SGBD"]
        A["1. A (Atomicity / Atomicitat)<br/>Principi del 'Tot o Res'. La transacció s'executa completament (Commit) o, si falla qualsevol pas, es reverteix íntegrament (Rollback)."]
        C["2. C (Consistency / Consistència)<br/>La transacció només pot portar la base de dades d'un estat vàlid a un altre estat vàlid, respectant totes les regles d'integritat (PK, FK, Check)."]
        I["3. I (Isolation / Aïllament)<br/>L'execució concurrent de múltiples transaccions no produeix interferències; el resultat és idèntic a si s'haguessin executat seqüencialment."]
        D["4. D (Durability / Durabilitat)<br/>Un cop confirmada la transacció (Commit), els canvis són permanents i sobreviuen fins i tot a una caiguda del servidor mitjançant registres WAL (Write-Ahead Logging)."]
    end
```

### 1.1. Nivells d'Aïllament SQL i Fenòmens Anòmals

| Nivell d'Aïllament (SQL-92) | Lectura Bruta (*Dirty Read*) | Lectura No Repetible | Lectura Fantasma (*Phantom Read*) |
| :--- | :---: | :---: | :---: |
| **Read Uncommitted** *(Mínim aïllament)* | ❌ Permesa | ❌ Permesa | ❌ Permesa |
| **Read Committed** *(Estàndard PostgreSQL/Oracle)* | ✅ **Evitada** | ❌ Permesa | ❌ Permesa |
| **Repeatable Read** *(Estàndard MySQL InnoDB)* | ✅ **Evitada** | ✅ **Evitada** | ❌ Permesa |
| **Serializable** *(Màxim aïllament / Mínima concurrència)* | ✅ **Evitada** | ✅ **Evitada** | ✅ **Evitada** |

---

## 2. Confidencialitat i Xifratge de Dades (ENS i RGPD)

Segons les exigències de l'**Esquema Nacional de Seguretat** ([`CORPUS/ENS_2022.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENS_2022.pdf)):

```mermaid
flowchart TD
    subgraph SEGURETAT_BD["NIVELLS DE PROTECCIÓ CRIPTOGRÀFICA A BASES DE DADES"]
        Transit["1. Xifratge en Trànsit (In-Transit)<br/>Totes les connexions entre aplicacions web i el SGBD utilitzen TLS 1.3 (AES-256)."]
        AtRest["2. Xifratge en Repòs (TDE - Transparent Data Encryption)<br/>Xifratge transparent dels fitxers físics de bases de dades, tablespaces i còpies de seguretat a disc."]
        Column["3. Xifratge a Nivell de Camp / Columna<br/>Xifratge específic per a categories especials de dades (dades mèdiques de serveis socials, delictes)."]
        Masking["4. Emmascarament Dinàmic de Dades (Dynamic Data Masking)<br/>Ocultació parcial de dades confidencials en pantalla (ex. NIF: ***4567*)."]
    end
```

- **Prevenció de la Injecció SQL (SQL Injection - OWASP Top 1):**  
  Totes les consultes han d'utilitzar **consultes preparades (*Prepared Statements / Parameterized Queries*)** per separar estrictament el codi SQL de les dades introduïdes per l'usuari, impedint la manipulació de la base de dades.

---

## 3. Problemàtica de les Bases de Dades Distribuïdes: El Teorema CAP

En una arquitectura distribuïda on les dades estan replicades entre diferents servidors o CPDs, el **Teorema CAP (d'Eric Brewer)** demostra que **és impossible garantir simultàniament les 3 propietats davant una fallada de comunicació de xarxa**:

```mermaid
flowchart TD
    subgraph TEOREMA_CAP["EL TRIANGLE DEL TEOREMA CAP"]
        C["C (Consistency - Consistència Forta)<br/>Tots els servidors veuen exactament la mateixa dada a l'instant."]
        A["A (Availability - Disponibilitat)<br/>Cada petició rep una resposta sense errors."]
        P["P (Partition Tolerance - Tolerància a Particions)<br/>El sistema continua funcionant si es trenca la xarxa entre nodes."]

        C --- A --- P --- C
    end
```

> 📌 **Implicació Pràctica:** Com que a les xarxes reals les particions de xarxa ($P$) són inevitables, un sistema distribuït ha de triar entre:
> - **Sistemes CP (Consistència + Tolerància a Particions):** Bloquegen operacions si no poden garantir la consistència (ex. bases de dades relacionals bancàries o tributàries).
> - **Sistemes AP (Disponibilitat + Tolerància a Particions):** Responen sempre, acceptant **Consistència Eventual (*Eventual Consistency*)** a canvi d'estar sempre operatius (ex. sistemes NoSQL com Cassandra o DynamoDB).

---

## 4. Protocols de Coordinació Distribuïda: Two-Phase Commit (2PC)

Per garantir que una transacció distribuïda que afecta múltiples bases de dades municipals sigui atòmica, s'utilitza el protocol **2PC (*Two-Phase Commit*)**:

```mermaid
flowchart TD
    Coord["Coordinador de Transaccions"]
    NodeA["Node 1 (BD Padró)"]
    NodeB["Node 2 (BD Tresoreria)"]

    subgraph FASE1["FASE 1: PREPARACIÓ (Prepare Phase)"]
        Coord -->|"1. PREPARE (Pots fer commit?)"| NodeA
        Coord -->|"1. PREPARE (Pots fer commit?)"| NodeB
        NodeA -->|"2. VOTE_COMMIT (Estic a punt)"| Coord
        NodeB -->|"2. VOTE_COMMIT (Estic a punt)"| Coord
    end

    subgraph FASE2["FASE 2: CONFIRMACIÓ (Commit Phase)"]
        Coord -->|"3. GLOBAL COMMIT (Executa definitivament)"| NodeA
        Coord -->|"3. GLOBAL COMMIT (Executa definitivament)"| NodeB
    end
```

---

## 5. Quadre Resum i Preguntes Típiques d'Examen Tipus Test

| Pregunta / Concepte Clau | Resposta Correcta / Trampa Freqüent |
| :--- | :--- |
| **Què signifiquen les sigles ACID en bases de dades?** | **Atomicitat (Atomicity), Consistència (Consistency), Aïllament (Isolation) i Durabilitat (Durability)**. |
| **Quin és el nivell d'aïllament que evita la Lectura Fantasma?** | El nivell **Serializable** (màxim nivell de concurrència estricta). |
| **Què és el TDE (Transparent Data Encryption)?** | El **xifratge transparent dels fitxers i còpies de la base de dades en repòs (*at-rest*)**. |
| **Com es prevé de forma definitiva l'atac d'Injecció SQL (SQLi)?** | Mitjançant l'ús obligatori de **consultes preparades / parametritzades (*Prepared Statements*)**. |
| **Què postula el Teorema CAP d'Eric Brewer?** | Que un sistema distribuït **només pot garantir 2 de les 3 propietats simultàniament (C, A o P)**. |
| **Com funciona el protocol Two-Phase Commit (2PC)?** | Divideix la transacció en **Fase de Preparació (votació)** i **Fase de Confirmació (*Global Commit*)**. |
