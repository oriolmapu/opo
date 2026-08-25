# Tema 31. Formats de signatura electrònica: definició i tipus, signatures múltiples, signatures de llarg termini i segells de temps

> **Fonts normatives i tècniques de referència:** [`CORPUS/EiDAS.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/EiDAS.pdf) (Reglament UE 910/2014, eIDAS - Arts. 41 i 42), [`CORPUS/ENI.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENI.pdf) (Norma Tècnica d'Interoperabilitat de Política de Signatura i Segells) i Estàndards ETSI (EN 319 122, EN 319 132 i EN 319 142).

---

## 1. Els Tres Formats Avançats de Signatura Electrònica (Estàndards ETSI)

L'Institut Europeu de Normes de Telecomunicacions (**ETSI**) defineix tres formats estàndard de signatura avançada per garantir la plena interoperabilitat transfronterera a la Unió Europea:

```mermaid
graph TD
    Formats["ELS 3 FORMATS ESTÀNDARD DE SIGNATURA ETSI"]
    
    CADES["1. CAdES (CMS Advanced Electronic Signatures)<br/>- Signa QUALSEVOL FITXER BINARI (imatges, zip, vídeos, dades).<br/>- Genera un fitxer binari (.p7s, .csig)."]
    XADES["2. XAdES (XML Advanced Electronic Signatures)<br/>- Signa DOCUMENTS ESTRUCTURATS EN XML.<br/>- Format estàndard de la FACTURA ELECTRÒNICA (Facturae) i del registre."]
    PADES["3. PAdES (PDF Advanced Electronic Signatures)<br/>- Específic per a DOCUMENTS PDF (ISO 32000).<br/>- Incrusta la signatura dins el PDF; visible amb qualsevol lector (Acrobat)."]

    Formats --> CADES
    Formats --> XADES
    Formats --> PADES
```

---

## 2. Modalitats d'Integració de la Signatura respecte al Document

La relació estructural entre el fitxer de signatura i el document original pot adoptar tres modalitats:

```mermaid
graph LR
    subgraph MODALITATS["MODALITATS D'INTEGRACIÓ DE LA SIGNATURA"]
        Enveloped["1. ENVELOPED (Embolicada / Dins)<br/>La signatura s'incrusta A L'INTERIOR del document original (típic en PDF/PAdES i XML)."]
        Enveloping["2. ENVELOPING (Embolcallant / Contenidora)<br/>L'estructura de la signatura CONTÉ EL DOCUMENT original a dins."]
        Detached["3. DETACHED (Desacoblada / Separada)<br/>La signatura es genera com un FITXER INDEPENDENT separat del document original."]
    end
```

---

## 3. Signatures Múltiples: Co-signatura vs. Contra-signatura

Quan un procediment administratiu requereix la intervenció de més d'un signant, es distingeixen dues estructures de signatura múltiple:

```mermaid
graph TD
    subgraph CO_SIGN["A) CO-SIGNATURA (Paral·lela / Mateix Nivell)"]
        DocOriginal["Document Original"]
        Sign1["Signatura Signant A (Alcalde)"]
        Sign2["Signatura Signant B (Contractista)"]
        DocOriginal --> Sign1
        DocOriginal --> Sign2
        Note1["Tots dos signen directament sobre el mateix document original al mateix nivell."]
    end
    
    subgraph CONTRA_SIGN["B) CONTRA-SIGNATURA (Cascada / Jeràrquica)"]
        DocOrig2["Document Original"]
        SignAlc["1. Signatura de l'Alcalde"]
        SignSec["2. Signatura del Secretari (Contra-signatura)"]
        DocOrig2 --> SignAlc
        SignAlc --> SignSec
        Note2["El Secretari dóna fe signant SOBRE LA SIGNATURA prèvia de l'Alcalde."]
    end
```

| Criteri de Comparació | Co-signatura (*Co-signature*) | Contra-signatura (*Counter-signature*) |
| :--- | :--- | :--- |
| **Estructura** | **Paral·lela** (al mateix nivell). | **Seqüencial / En cascada** (jeràrquica). |
| **Objecte signat** | Cadascú signa directament el **document original**. | El segon signant **signa la signatura del primer**. |
| **Finalitat típica** | Contractes bilaterals, convenis entre administracions. | **Donar fe pública**, visats tècnics o vistiplaus (Secretari/Interventor). |

---

## 4. El Segell de Temps (Time Stamping / TSA - RFC 3161)

El **Segell de Temps** (o cronodatació electrònica) són dades en format electrònic emeses per una **Autoritat de Segellat de Temps (TSA - Time Stamping Authority)** qualificada que vincula de manera indissociable un document o una signatura a una **data i hora oficials precises**:

```mermaid
sequenceDiagram
    autonumber
    participant App as Aplicació Municipal
    participant TSA as Autoritat de Segellat de Temps (TSA / AOC)

    App->>TSA: 1. Enviament del Hash del document (Timestamp Request - RFC 3161)
    TSA->>TSA: 2. Associació amb l'HORA OFICIAL ATÒMICA (ROA / GPS)
    TSA->>App: 3. Lliurament del Segell de Temps signat per la TSA (Timestamp Token - TST)
    Note over App: 4. El document acredita fefaentment la seva existència en aquell segon exacte
```

- **Efectes jurídics (Arts. 41 i 42 eIDAS):** Gaudeix de la **presumpció d'exactitud de la data i hora que indica i de la integritat de les dades** a tota la Unió Europea.

---

## 5. Signatures de Llarg Termini (LTV - Long Term Validation) i Preservació

Els certificats digitals ordinaris tenen una validesa limitada (de 2 a 5 anys). Un cop caduca el certificat o si la CA desapareix, una signatura bàsica no es pot verificar de forma fiable.

Per garantir que un document administratiu municipal pugui ser verificat vàlidament d'aquí a **20, 50 o 100 anys** a l'Arxiu Electrònic Únic, s'utilitzen els **nivells de signatura de llarg termini (LTV)** definits per l'ETSI:

```mermaid
graph TD
    NivellsLTV["NIVELLS DE PRESERVACIÓ DE SIGNATURA ETSI (CAdES / XAdES / PAdES)"]
    
    B["1. Nivell -B (Basic)<br/>Signatura electrònica bàsica amb el certificat del signant."]
    T["2. Nivell -T (Timestamp)<br/>Signatura + SEGELL DE TEMPS (acredita que es va signar abans de caducar el certificat)."]
    LT["3. Nivell -LT (Long Term Validation)<br/>Incrusta al document totes les evidències de validació (respostes OCSP i llistes CRL)."]
    LTA["4. Nivell -LTA (Long Term Archival)<br/>Afegeix periòdicament nous segells de temps d'arxiu amb algorismes actualitzats (arxiu definitiu)."]

    NivellsLTV --> B
    B --> T
    T --> LT
    LT --> LTA
```

---

## 6. Quadre Resum i Preguntes Típiques d'Examen Tipus Test

| Pregunta / Concepte Clau | Resposta Correcta / Trampa Freqüent |
| :--- | :--- |
| **Quin format s'utilitza exclusivament per a la signatura de documents PDF?** | El format **PAdES** (ETSI EN 319 142). |
| **Quin format de signatura utilitza la factura electrònica (Facturae)?** | El format **XAdES** (basat en XML). |
| **Quin format permet signar qualsevol tipus de fitxer binari?** | El format **CAdES** (generant un fitxer binari `.p7s`). |
| **Quina diferència hi ha entre co-signatura i contra-signatura?** | La **co-signatura és paral·lela** (al document); la **contra-signatura és sobre la signatura prèvia** (donar fe). |
| **Quina és la modalitat de signatura en un fitxer separat?** | Modalitat **Detached** (desacoblada / separada). |
| **Què acredita un Segell de Temps (Time Stamping)?** | L'**existència d'un document en un instant precís de data i hora i la seva no alteració** (RFC 3161). |
| **Com es garanteix la validesa d'un document signat durant dècades a l'arxiu?** | Mitjançant el perfil de signatura de **Llarg Termini (LTV / Nivell -LTA)** amb incrustació d'OCSP/CRL i ressegellat. |
