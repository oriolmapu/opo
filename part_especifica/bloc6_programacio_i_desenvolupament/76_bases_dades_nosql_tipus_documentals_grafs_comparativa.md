# Tema 76. Bases de dades NoSQL: models de dades (Documentals, Clau-Valor, Columnars, Grafs), model BASE vs. ACID, escalabilitat horitzontal i persistència poliglota

> **Fonts i marcs de referència:** Esquema Nacional d'Interoperabilitat ([`CORPUS/ENI.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENI.pdf) - NTI de Catàleg d'estàndards per a formats JSON/XML), Esquema Nacional de Seguretat ([`CORPUS/ENS_2022.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENS_2022.pdf) - Protecció de bases de dades) i teoria de sistemes distribuïts NoSQL (**Model BASE**).

---

## 1. Concepte i Origen del Moviment NoSQL (*Not Only SQL*)

Les bases de dades **NoSQL** van sorgir per superar les limitacions dels sistemes relacionals tradicionals davant el volum massiu de dades (*Big Data*), les dades no estructurades procedents de sensors d'Internet de les Coses (IoT) i la necessitat d'**escalabilitat horitzontal massiva (*Scale-Out*)**:

```mermaid
flowchart TD
    subgraph ACID_VS_BASE["COMPARATIVA DE MODELS DE CONSISTÈNCIA"]
        ACID["A) MODEL ACID (Bases de Dades Relacionals / SQL)<br/>- Consistència immediata i forta.<br/>- Esquema rígid definit prèviament (Schema-on-Write).<br/>- Escalabilitat Vertical (servidors més grans amb més CPU/RAM)."]
        
        BASE["B) MODEL BASE (Bases de Dades NoSQL)<br/>- Basically Available (Disponibilitat bàsica garantida).<br/>- Soft-State (L'estat del sistema pot variar amb el temps sense entrades).<br/>- Eventual Consistency (Consistència eventual: les dades s'igualaran al cap d'uns segons).<br/>- Esquema dinàmic i flexible (Schema-on-Read) amb Escalabilitat Horitzontal (afegir més servidors)."]
    end
```

---

## 2. Tipologia de Bases de Dades NoSQL

Les tecnologies NoSQL es classifiquen en quatre grans famílies segons la seva estructura interna d'emmagatzematge:

```mermaid
flowchart TD
    subgraph FAMILIES_NOSQL["LES QUATRE FAMÍLIES DE BASES DE DADES NOSQL"]
        Doc["1. DOCUMENTALS (Document Stores)<br/>Emmagatzemen documents semiestructurats (JSON / BSON).<br/>Exemples: MongoDB, CouchDB.<br/>Ús municipal: Gestor d'expedients electrònics i catàlegs de tràmits."]
        
        KV["2. CLAU-VALOR (Key-Value Stores)<br/>Accés ultraràpid per clau única a un valor en memòria RAM.<br/>Exemples: Redis, Memcached.<br/>Ús municipal: Memòria cau (cache) de la Seu Electrònica i sessions d'usuaris."]
        
        Col["3. COLUMNARS (Wide-Column Stores)<br/>Organitzen les dades en famílies de columnes d'alta compressió.<br/>Exemples: Apache Cassandra, Apache HBase.<br/>Ús municipal: Telelectura massiva de sensors IoT de Smart City i logs."]
        
        Graf["4. DE GRAFS (Graph Databases)<br/>Emmagatzemen Nodes (entitats) i Vèrtexs/Arestes (relacions).<br/>Exemples: Neo4j, Amazon Neptune.<br/>Ús municipal: Xarxes de mobilitat, prevenció del frau i xarxes d'aigua/clavegueram."]
    end
```

---

## 3. Taula Comparativa: SQL vs. Tipologies NoSQL

| Família / Model | Model de Dades | Consistència | Escalabilitat Típica | Productes de Mercat Líders | Aplicació Típica Municipal |
| :--- | :--- | :---: | :---: | :--- | :--- |
| **Relacional (SQL)** | Taules amb files i columnes (Esquema rígid) | **ACID (Forta)** | **Vertical** (*Scale-Up*) | **PostgreSQL, MariaDB, Oracle, SQL Server** | **Comptabilitat, Padró d'Habitants, Gestió Tributària.** |
| **Documental** | Documents jeràrquics **JSON / BSON** | Flexible / Configurable | **Horitzontal** (*Sharding*) | **MongoDB, CouchDB** | **Expedients electrònics i formularis web dinàmics.** |
| **Clau-Valor** | Parells Clau $\rightarrow$ Valor (*Hash Map*) | Eventual / En memòria | **Horitzontal** | **Redis, Memcached** | **Sessions d'usuaris i memòria cau d'alt rendiment.** |
| **Columnar** | Famílies de columnes (*Column-Family*) | **BASE (Eventual)** | **Horitzontal Massiva** | **Apache Cassandra, ScyllaDB** | **Sensors IoT municipals i sèries temporals.** |
| **De Grafs** | Nodes, Arestes (*Edges*) i Propietats | ACID / Configurable | Centrada en Consultes | **Neo4j, ArangoDB** | **Anàlisi de xarxes de sanejament i detecció de frau.** |

---

## 4. El Concepte de Persistència Poliglota (*Polyglot Persistence*)

A l'Ajuntament modern no s'utilitza una única base de dades per a tot, sinó l'estratègia de **Persistència Poliglota**: utilitzar el tipus de base de dades més adequat per a cada problema específic:

```mermaid
flowchart TD
    subgraph ARQUITECTURA_POLIGLOTA["PERSISTÈNCIA POLIGLOTA A L'AJUNTAMENT"]
        App["Plataforma de Serveis Municipals"]
        
        App -->|"Dades Transaccionals Tributàries"| PG[("PostgreSQL (Relacional ACID)")]
        App -->|"Metadades d'Expedients i Tràmits"| Mongo[("MongoDB (Documental JSON)")]
        App -->|"Accés a Sessions i Cau Web"| Redis[("Redis (Clau-Valor en RAM)")]
        App -->|"Sèries Temporals Sensors IoT"| Cass[("Cassandra (Columnar)")]
    end
```

---

## 5. Quadre Resum i Preguntes Típiques d'Examen Tipus Test

| Pregunta / Concepte Clau | Resposta Correcta / Trampa Freqüent |
| :--- | :--- |
| **Què signifiquen les sigles del model BASE a NoSQL?** | **Basically Available, Soft-state, Eventual consistency** (oposat al model ACID). |
| **Quin tipus de base de dades NoSQL és MongoDB?** | Una base de dades **Documental (basada en documents JSON/BSON)**. |
| **Quin tipus de base de dades és Redis?** | Una base de dades **Clau-Valor (*Key-Value*) en memòria RAM**. |
| **Quin model NoSQL és el més eficient per analitzar relacions complexes?** | Les bases de dades **de Grafs (com Neo4j)**, basades en nodes i arestes. |
| **Quina família NoSQL és òptima per a dades massives de sensors IoT?** | Les bases de dades **Columnars (com Apache Cassandra)**. |
| **Què és la Persistència Poliglota?** | L'ús combinat de **diferents motors de bases de dades (SQL i NoSQL)** segons la necessitat de cada servei. |
