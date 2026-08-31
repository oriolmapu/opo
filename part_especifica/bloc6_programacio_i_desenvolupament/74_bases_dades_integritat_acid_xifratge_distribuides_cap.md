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

### 1.1. Nivells d'Aïllament SQL i Fenòmens Anòmals (SQL-92)

L'aïllament (*Isolation*) a les bases de dades regula **què pot veure una transacció mentre altres usuaris o processos estan modificant dades simultàniament**. 

Si dues transaccions s'executen alhora sense control, poden produir-se **tres anomalies o errors greus de concurrència**:

```mermaid
flowchart TD
    subgraph ANOMALIES_CONCURRENCIA[" ELS 3 FENÒMENS ANÒMALS DE CONCURRÈNCIA"]
        DR["1. LECTURA BRUTA (Dirty Read)<br/>Una transacció llegeix dades modificades per una altra transacció que ENCARA NO HA FET COMMIT.<br/>Si la primera transacció fa ROLLBACK, la segona haurà treballat amb dades fictícies."]
        
        NRR["2. LECTURA NO REPETIBLE (Non-Repeatable Read)<br/>Dins d'una mateixa transacció, en llegir la MATEIXA FILA dues vegades s'obtenen valors diferents perquè una altra transacció ha modificat (UPDATE) i confirmat (COMMIT) la fila entremig."]
        
        PR["3. LECTURA FANTASMA (Phantom Read)<br/>Dins d'una mateixa transacció, en fer dues consultes PER RANG (ex. SELECT de totes les llicències pendents), la segona consulta retorna NOVES FILES 'fantasma' perquè una altra transacció ha inserit (INSERT) o esborrat (DELETE) registres entremig."]
    end
```

#### 📖 Exemples Pràctics a l'Administració Local:
1. **Exemple de Lectura Bruta (*Dirty Read*):**
   - El sistema de recaptació (Transacció A) inicia la cancel·lació d'un deute de 500 € d'un ciutadà, deixant-lo a 0 €, però encara no ha confirmat l'operació (*sense COMMIT*).
   - L'OAC (Transacció B) consulta el saldo del ciutadà i li emet un certificat d'estar al corrent de pagament perquè llegeix 0 €.
   - De sobte, la Transacció A falla per un error de xarxa i fa *ROLLBACK* (el deute torna a ser de 500 €). S'ha emès un certificat oficial erroni per culpa d'una lectura bruta!
2. **Exemple de Lectura No Repetible (*Non-Repeatable Read*):**
   - Un administratiu (Transacció A) obre un expedient i llegeix el domicili d'un ciutadà: *"Carrer Major, 1"*.
   - El ciutadà (Transacció B), des de la Seu Electrònica, canvia el seu domicili a *"Avinguda Diagonal, 5"* i fa *COMMIT*.
   - Si la Transacció A torna a consultar la mateixa fila abans de tancar l'expedient, troba un domicili diferent al que havia llegit uns segons abans dins del mateix procés.
3. **Exemple de Lectura Fantasma (*Phantom Read*):**
   - El Cap d'Urbanisme (Transacció A) compta quantes llicències d'obres pendents hi ha: `SELECT COUNT(*) FROM llicencies WHERE estat = 'Pendent'` $\rightarrow$ **10 llicències**.
   - Un ciutadà (Transacció B) registra una nova sol·licitud a la Seu Electrònica (`INSERT`) i fa *COMMIT*.
   - Quan la Transacció A torna a fer la mateixa consulta per generar l'informe final, obté **11 llicències** (ha aparegut una fila fantasma en el conjunt).

---

#### ⚖️ Els 4 Nivells d'Aïllament i el seu Compromís (Consistència vs. Rendiment)

L'estàndard **SQL-92** defineix 4 nivells d'aïllament. Com més alt és el nivell, **més garanties d'integritat hi ha, però menor és la concurrència i la velocitat** del sistema (a causa dels bloquejos de registres):

| Nivell d'Aïllament (SQL-92) | Lectura Bruta (*Dirty Read*) | Lectura No Repetible | Lectura Fantasma (*Phantom Read*) | Ús i Configuració per Defecte |
| :--- | :---: | :---: | :---: | :--- |
| **1. Read Uncommitted** *(Mínim aïllament)* | ❌ Permesa | ❌ Permesa | ❌ Permesa | Risc màxim. Només per a consultes estadístiques massives on la precisió exacta no importa. |
| **2. Read Committed** | ✅ **Evitada** | ❌ Permesa | ❌ Permesa | **Nivell per defecte a PostgreSQL, Oracle i SQL Server**. Només es llegeixen dades confirmades amb `COMMIT`. |
| **3. Repeatable Read** | ✅ **Evitada** | ✅ **Evitada** | ❌ Permesa | **Nivell per defecte a MySQL (motor InnoDB)** mitjançant control de concurrència multiversió (MVCC). |
| **4. Serializable** *(Màxim aïllament)* | ✅ **Evitada** | ✅ **Evitada** | ✅ **Evitada** | Simula una execució 100% seqüencial. Evita totes les anomalies, però pot provocar bloquejos (*deadlocks*) i avortaments de transaccions. |

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
