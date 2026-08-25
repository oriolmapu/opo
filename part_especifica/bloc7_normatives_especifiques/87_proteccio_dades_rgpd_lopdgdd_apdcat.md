# Tema 87. Protecció de dades de caràcter personal. Privacitat des del disseny i per defecte. Registre d'activitats de tractament. Anàlisi de riscs i avaluació d'impacte en protecció de dades. L'Agència Catalana de Protecció de Dades

> **Fonts normatives de referència:** [`CORPUS/LOPD.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/LOPD.pdf) (Llei Orgànica 3/2018, de 5 de desembre, de protecció de dades personals i garantia dels drets digitals - LOPDGDD), Reglament (UE) 2016/679 General de Protecció de Dades (RGPD) i Llei 32/2010 de l'Autoritat Catalana de Protecció de Dades (APDCAT).

---

## 1. Marc Normatiu i Principis Relatius al Tractament (Art. 5 RGPD)

La protecció de dades personals és un dret fonamental (**Art. 18.4 CE**) que a la Unió Europea es regeix pel **Reglament (UE) 2016/679 (RGPD)** i a l'Estat espanyol per la **Llei Orgànica 3/2018 (LOPDGDD)**.

```mermaid
flowchart TD
    Principis["PRINCIPIS DEL TRACTAMENT DE DADES (Art. 5 RGPD)"]
    
    P1["1. Licitut, Lleialtat i Transparència<br/>Tractament basat en base jurídica legítima i amb informació clara."]
    P2["2. Limitació de la Finalitat<br/>Recollides amb finalitats determinades, explícites i legítimes."]
    P3["3. Minimització de Dades<br/>Adequades, pertinents i limitades al necessari ('Data Minimization')."]
    P4["4. Exactitud<br/>Dades exactes i degudament actualitzades."]
    P5["5. Limitació del Termini de Conservació<br/>Mantigudes només durant el temps necessari per a la finalitat."]
    P6["6. Integritat i Confidencialitat<br/>Garantia de seguretat contra tractaments no autoritzats o pèrdua."]
    P7["7. Responsabilitat Proactiva (Accountability)<br/>El Responsable ha de poder DEMOSTRAR el compliment efectiu."]

    Principis --> P1
    Principis --> P2
    Principis --> P3
    Principis --> P4
    Principis --> P5
    Principis --> P6
    Principis --> P7
```

---

## 2. Privacitat des del Disseny i per Defecte (Art. 25 RGPD)

```mermaid
flowchart LR
    subgraph PRIVACITAT["DISSENY I DEFECTE (Art. 25 RGPD)"]
        Disseny["1. PRIVACITAT DES DEL DISSENY (Privacy by Design)<br/>Integració de garanties tècniques i organitzatives (xifrat, pseudonimització)<br/>des de la fase inicial de concepció del sistema o procediment municipal."]
        Defecte["2. PRIVACITAT PER DEFECTE (Privacy by Default)<br/>Per defecte, el sistema només tracta les dades estrictament necessàries<br/>(mínima quantitat, mínim accés i mínim temps de conservació)."]
    end
```

---

## 3. El Registre d'Activitats de Tractament (RAT - Art. 30 RGPD / Art. 31 LOPDGDD)

Tots els ajuntaments i organismes públics estan obligats a portar i mantenir actualitzat un **Registre d'Activitats de Tractament (RAT)** i a fer-lo **públic per mitjans electrònics** (al Portal de Transparència i a la Seu Electrònica):

| Contingut Obligatori del RAT (Art. 30.1 RGPD) | Descripció Operativa en l'Àmbit Municipal |
| :--- | :--- |
| **Identificació del Responsable** | Nom de l'Ajuntament i dades de contacte del **Delegat de Protecció de Dades (DPD)**. |
| **Finalitats del tractament** | Gestió del padró, tramitació de llicències d'obres, gestió tributària, serveis socials, policia local, gestió de nòmines. |
| **Base jurídica de legitimació** | Missió en interès públic / exercici de poders públics (**Art. 6.1.e RGPD**), obligació legal (**Art. 6.1.c**) o consentiment. |
| **Categories d'interessats i de dades** | Veïns del municipi, contribuents, empleats públics, dades identificatives, econòmiques o de salut. |
| **Destinataris i cessions previstes** | Altres administracions públiques (Agència Tributària, Seguretat Social, Generalitat) o jutjats. |
| **Transferències internacionals** | Si es transfereixen dades fora de l'Espai Econòmic Europeu (i garanties aplicades). |
| **Terminis previstos de supressió** | Criteris de conservació segons taules d'avaluació documental i arxiu. |
| **Mesures de seguretat tècniques** | Descripció general de les mesures aplicades d'acord amb l'Esquema Nacional de Seguretat (ENS). |

---

## 4. Anàlisi de Riscos i Avaluació d'Impacte (AIPD / DPIA - Art. 35 RGPD)

El RGPD substitueix l'antic model burocràtic per un **enfocament basat en el risc**:

```mermaid
flowchart TD
    subgraph GESTIO_RISC["GESTIÓ DEL RISC EN PROTECCIÓ DE DADES"]
        AR["1. ANÀLISI DE RISCOS PREVI (Obligatori per a tot tractament)<br/>Avalua la probabilitat i gravetat dels riscos per als drets i llibertats dels ciutadans."]
        AIPD["2. AVALUACIÓ D'IMPACTE - AIPD (Art. 35 RGPD)<br/>Preceptiva quan el tractament pugui comportar un ALT RISC per als drets."]
    end
```

### 4.1. Quan és preceptiva una AIPD a l'Ajuntament?
L'Avaluació d'Impacte relativa a la Protecció de Dades és **obligatòria en tot cas** quan:
1. Es realitzi una **avaluació sistemàtica i exhaustiva d'aspectes personals basada en tractament automatitzat / perfilat**.
2. Es tractin a gran escala **categories especials de dades** (dades de salut a serveis socials, ideologia, dades biomètriques per a control d'accessos, dades penals/policials).
3. Es dugui a terme una **observació o vigilància sistemàtica a gran escala d'una zona d'accés públic** (sistemes de videovigilància municipal, lectors de matrícules en Zones de Baixes Emissions).

- **Consulta Prèvia (Art. 36 RGPD):** Si l'AIPD indica que el tractament comporta un alt risc que l'Ajuntament no pot mitigar amb mesures tècniques, ha de consultar preceptivament l'**Autoritat Catalana de Protecció de Dades (APDCAT)** abans d'iniciar el tractament.

---

## 5. El Delegat de Protecció de Dades (DPD / DPO) i Violacions de Seguretat

### 5.1. El Delegat de Protecció de Dades (Arts. 37 a 39 RGPD / Arts. 34 a 37 LOPDGDD)
- **Obligatorietat absoluta:** Totes les administracions públiques (incloent tots els ajuntaments) han de designar obligatòriament un **DPD**.
- **Perfil i posició:** Nomenat sobre la base de les seves qualitats professionals i coneixements jurídics i tècnics. Actua amb **plena independència funcional** (no pot rebre instruccions sobre l'exercici de les seves funcions ni ser sancionat per acomplir-les) i reporta directament al màxim nivell de govern municipal (Alcaldia).
- **Funcions:** Informar i assessorar el consistori, supervisar el compliment del RGPD, actuar com a punt de contacte per als ciutadans i cooperar amb l'autoritat de control (APDCAT).

---

### 5.2. Gestió de Violacions de Seguretat de Dades (*Data Breaches* - Arts. 33 i 34 RGPD)
Quan es produeixi un incident de seguretat que afecti la confidencialitat o integritat de dades personals:
1. **Notificació a l'Autoritat de Control (APDCAT):** S'ha de notificar sense dilació indeguda i, com a màxim, en el termini de **72 hores** des que se'n tingui constància (llevat que sigui improbable que comporti un risc).
2. **Comunicació als Afectats:** Si la violació comporta un **alt risc per als drets i llibertats** de les persones, s'ha de comunicar directament als ciutadans afectats en un llenguatge clar i senzill.

---

## 6. L'Autoritat Catalana de Protecció de Dades (APDCAT - Llei 32/2010)

L'**APDCAT** és l'organisme independent de dret públic amb personalitat jurídica pròpia que vetlla pel compliment de la normativa de protecció de dades a les institucions públiques de Catalunya:

```mermaid
flowchart TD
    subgraph APDCAT_BOX["AUTORITAT CATALANA DE PROTECCIÓ DE DADES (APDCAT)"]
        Amb["Àmbit d'Actuació:<br/>Generalitat, Ajuntaments de Catalunya, Consorcis, Universitats públiques catalanes."]
        F1["1. Tutela dels Drets dels ciutadans (reclamacions de drets ARCO-POL)"]
        F2["2. Potestat Inspectora i Sancionadora sobre el sector públic català"]
        F3["3. Emissió de Dictàmens preceptius i resolució de Consultes Prèvies"]
        F4["4. Registre dels Delegats de Protecció de Dades (DPD) de Catalunya"]
    end

    Amb --> F1
    Amb --> F2
    Amb --> F3
    Amb --> F4
```

- **Règim sancionador aplicable a les Administracions Públiques (Art. 77 LOPDGDD):** Quan la infracció la comet un ajuntament o autoritat pública, la sanció de l'APDCAT **no té caràcter pecuniari (no s'imposen multes econòmiques a l'erari públic)**, sinó que es dicta un **requeriment d'adopció de mesures correctores**, es formula una **amonestació pública** i es pot instar la incoació d'expedient disciplinari contra els responsables.

---

## 7. Quadre Resum i Preguntes Típiques d'Examen Tipus Test

| Pregunta / Concepte Clau | Resposta Correcta / Trampa Freqüent |
| :--- | :--- |
| **Què exigeix el principi de Responsabilitat Proactiva (*Accountability*)?** | El Responsable ha d'aplicar mesures tècniques i **estar en condicions de DEMOSTRAR el compliment del RGPD**. |
| **És obligatori el Delegat de Protecció de Dades (DPD) a un Ajuntament?** | **Sí, és obligatori per a totes les administracions públiques** (Art. 37.1.a RGPD). |
| **Quin termini màxim hi ha per notificar una violació de seguretat a l'APDCAT?** | Com a màxim **72 hores** des que se'n té constància (Art. 33.1 RGPD). |
| **Quan és obligatòria una Avaluació d'Impacte (AIPD)?** | Quan el tractament comporti un **alt risc** (dades especials a gran escala, perfilat, videovigilància pública massiva). |
| **On s'ha de publicar el Registre d'Activitats de Tractament (RAT)?** | S'ha de fer **públic per mitjans electrònics** (Seu Electrònica / Portal de Transparència) (Art. 31.2 LOPDGDD). |
| **Quina autoritat supervisa els ajuntaments catalans en protecció de dades?** | L'**Autoritat Catalana de Protecció de Dades (APDCAT)** (Llei 32/2010). |
| **Es poden imposar multes econòmiques a un ajuntament segons la LOPDGDD?** | **No com a regla general**; se'ls imposa una **amonestació i requeriment de mesures correctores** (Art. 77 LOPDGDD). |
