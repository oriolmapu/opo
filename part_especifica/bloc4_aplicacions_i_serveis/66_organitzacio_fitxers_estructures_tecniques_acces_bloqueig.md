# Tema 66. Organització de fitxers: estructura lògica i física, tècniques d'accés (seqüencial, indexat, hash), factor de bloqueig, expansibilitat i volatilitat

> **Fonts i marcs de referència:** Esquema Nacional d'Interoperabilitat ([`CORPUS/ENI.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENI.pdf) - NTI de Document i Expedient Electrònic), teoria clàssica d'estructures de dades i fitxers (arbres B+, funcions de dispersió Hash) i arquitectura de sistemes de fitxers de sistemes operatius (NTFS, Ext4, ZFS).

---

## 1. Concepte de Fitxer: Estructura Lògica vs. Estructura Física

Un **fitxer** és una col·lecció organitzada de dades o registres relacionats lògicament que s'emmagatzemen com una sola unitat en un suport de memòria secundària no volàtil:

```mermaid
flowchart TD
    subgraph ESTRUCTURES_FITXER["ESTRUCTURA LÒGICA VS. ESTRUCTURA FÍSICA"]
        Logica["A) ESTRUCTURA LÒGICA (Visió del Programari / Usuari)<br/>- Camps: Unitats mínimes amb significat (ex. NIF, Nom, Data).<br/>- Registres Lògics: Agrupació de camps d'un mateix element (ex. expedient ciutadà).<br/>- Fitxer Lògic: Conjunt complet de registres d'una aplicació municipal."]
        
        Fisica["B) ESTRUCTURA FÍSICA (Visió del Maquinari / Disc)<br/>- Blocs Físics / Clústers: La unitat mínima de transferència d'entrada/sortida (I/O) entre disc i memòria RAM (típicament 4.096 bytes = 4 KB).<br/>- Sectors i Pistes: Divisió geomètrica del medi d'emmagatzematge."]

        Logica <-->|"Sistema de Fitxers / Sistema Operatiu"| Fisica
    end
```

---

## 2. El Factor de Bloqueig (*Blocking Factor - Bfr*)

El **Factor de Bloqueig ($Bfr$)** és el nombre de registres lògics que es poden emmagatzemar dins d'un únic bloc físic de disc:

$$\text{Bfr} = \left\lfloor \frac{B}{R} \right\rfloor$$

- On $B$ és la mida del bloc físic (en bytes) i $R$ és la mida d'un registre lògic (en bytes).

```mermaid
flowchart LR
    subgraph BLOQUEIG["TIPUS D'EMPAQUETAMENT EN BLOCS"]
        Unspanned["1. Bloqueig No Segmentat (Unspanned)<br/>Els registres NO es divideixen entre blocs.<br/>Si un registre no cap sencer, l'espai restant es desaprofita (Fragmentació Interna)."]
        Spanned["2. Bloqueig Segmentat (Spanned)<br/>Un registre gran es pot dividir entre el final d'un bloc físic i el principi del següent mitjançant un punter."]
    end
```

---

## 3. Dinàmica i Paràmetres Característics dels Fitxers

| Paràmetre | Definició Tècnica | Exemple en l'Administració Local |
| :--- | :--- | :--- |
| **Volatilitat (*Volatility*)** | Freqüència i taxa d'**altes, baixes i modificacions** de registres al llarg del temps. | - **Alta volatilitat:** Taula de registre d'entrades i sortides diàries de l'OAC o fitxers de logs.<br/>- **Baixa volatilitat:** Catàleg de municipis de Catalunya o arxiu històric d'expedients tancats. |
| **Activitat (*Activity Ratio*)** | Percentatge de registres que es consulten o processen en una execució concreta: $\text{Ràtio} = \frac{\text{Registres processats}}{\text{Total registres}} \times 100$. | - **Alta activitat (>80%):** Procés nocturn massiu d'emissió de rebuts tributaris de l'IBI.<br/>- **Baixa activitat (<1%):** Consulta puntual del certificat d'empadronament d'un ciutadà concret. |
| **Expansibilitat (*Expandability*)** | Taxa de **creixement de la mida del fitxer** prevista a mitjà i llarg termini. | Permet calcular el dimensionament futur de les cabines de disc SAN del CPD (*Capacity Planning*). |

---

## 4. Tipus d'Organització de Fitxers i Mètodes d'Accés

L'organització d'un fitxer determina com s'ubiquen físicament els seus registres i quins algorismes s'utilitzen per recuperar-los:

```mermaid
flowchart TD
    subgraph METODES_ORGANITZACIO["MÈTODES PRINCIPALS D'ORGANITZACIÓ DE FITXERS"]
        Seq["1. ORGANITZACIÓ SEQÜENCIAL<br/>- Els registres es guarden un darrere l'altre en ordre físic.<br/>- Accés: Cal llegir N-1 registres per arribar al registre N (Cost O(N)).<br/>- Ús òptim: Processos batch d'alta activitat (càlcul de nòmines dels funcionaris)."]
        
        Index["2. ORGANITZACIÓ SEQÜENCIAL INDEXADA (ISAM / Arbres B+)<br/>- Fitxer de dades ordenat + Fitxer d'Índex jeràrquic (Clau -> Punter).<br/>- Accés: Cerca binària ràpida a l'índex en cost O(log N) i salt directe a disc.<br/>- Ús òptim: Sistemes de bases de dades i gestors d'expedients municipals."]
        
        Direct["3. ORGANITZACIÓ DIRECTA O HASHING (Accés Aleatori)<br/>- La posició física es calcula amb una funció matemàtica: Adreça = h(Clau).<br/>- Accés: Instantani en temps constant O(1).<br/>- Gestió de Col·lisions: Encadenament o àrees de desbordament (Overflow)."]
    end
```

---

## 5. Estructures d'Arbres B i B+ (*B-Trees / B+ Trees*)

En els sistemes de fitxers corporatius (com **NTFS, ext4, XFS o ZFS**) i en els motors de bases de dades relacionals (PostgreSQL, Oracle), l'estructura per excel·lència és l'**Arbre B+**:

```mermaid
flowchart TD
    subgraph BTREE_PLUS["ARBRE B+ EN SISTEMES DE FITXERS"]
        Root["Node Arrel (Claus de divisió)"]
        Int1["Node Intermedi A"]
        Int2["Node Intermedi B"]
        
        Leaf1["Fulla 1: [Dades / Punters a Disc]"]
        Leaf2["Fulla 2: [Dades / Punters a Disc]"]
        Leaf3["Fulla 3: [Dades / Punters a Disc]"]

        Root --> Int1
        Root --> Int2
        Int1 --> Leaf1
        Int1 --> Leaf2
        Int2 --> Leaf3
        Leaf1 <-->|"Llista doblement encadenada per a lectures seqüencials ràpides"| Leaf2
        Leaf2 <--> Leaf3
    end
```

- **Propietats de l'Arbre B+:**
  1. Arbre perfectament balancejat (totes les fulles estan al mateix nivell de profunditat).
  2. Els nodes interns només contenen claus per guiar la cerca; **totes les dades reals i punters a disc estan a les fulles**.
  3. Les fulles estan interconnectades de forma seqüencial, permetent tant cerques ràpides directes com cerques per rangs de valors (ex. *tots els expedients de l'any 2026*).

---

## 6. Quadre Resum i Preguntes Típiques d'Examen Tipus Test

| Pregunta / Concepte Clau | Resposta Correcta / Trampa Freqüent |
| :--- | :--- |
| **Com es calcula el Factor de Bloqueig (Bfr)?** | Dividint la mida del bloc físic entre la mida del registre lògic: $\text{Bfr} = \lfloor B / R \rfloor$. |
| **Què és la volatilitat d'un fitxer?** | La **freqüència d'altes, baixes i modificacions** que experimenta un fitxer. |
| **Quin mètode d'organització és més eficient per a processos d'alta activitat?** | L'**organització seqüencial** (ideal per a processos batch com nòmines o tributs). |
| **Quina complexitat temporal té l'accés directe per funció Hash?** | **Temps constant $O(1)$** en el cas ideal lliure de col·lisions. |
| **Per què s'utilitzen arbres B+ en lloc d'arbres binaris a discs?** | Perquè tenen un **factor de ramificació molt alt que minimitza el nombre d'operacions d'E/S a disc**. |
| **On s'emmagatzemen les dades reals en un Arbre B+?** | **Exclusivament en els nodes fulla**, que a més estan encadenats entre si. |
