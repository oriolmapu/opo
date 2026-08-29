# Tema 77. Metodologies àgils (Scrum, Kanban, XP) vs. tradicionals i tècniques de definició de requisits (IEEE 830, Casos d'Ús UML, Històries d'Usuari INVEST)

> **Fonts i marcs de referència:** Esquema Nacional d'Interoperabilitat ([`CORPUS/ENI.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENI.pdf)), Esquema Nacional de Seguretat ([`CORPUS/ENS_2022.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENS_2022.pdf) - Desenvolupament de sistemes), Guia oficial de **Scrum (Ken Schwaber & Jeff Sutherland)**, estàndard **ISO/IEC/IEEE 29148 (Enginyeria de Requisits)** i especificació **UML 2.5 (OMG)**.

---

## 1. Metodologies Tradicionals (En Cascada) vs. Metodologies Àgils (*Agile*)

El desenvolupament de programari municipal ha evolucionat del model tradicional lineal i rígid en cascada (*Waterfall*) cap als marcs de treball àgils basats en el **Manifest Àgil (2001)**:

```mermaid
flowchart TD
    subgraph COMPARATIVA_METODOLOGIES["CASCADA TRADICIONAL VS. MARC ÀGIL"]
        Waterfall["A) MODEL EN CASCADA (Waterfall / MÈTRICA v3)<br/>Fases rígides seqüencials: Requisits -> Anàlisi -> Disseny -> Construcció -> Proves -> Desplegament.<br/>- El client no veu res fins al final del projecte (alt risc de desviació)."]
        
        Agile["B) METODOLOGIES ÀGILS (Scrum / Kanban)<br/>Iteracions curtes (Sprints de 2-4 setmanes) que lliuren increments de programari funcional usable.<br/>- Adaptació contínua al canvi i col·laboració directa amb els departaments usuaris."]
    end
```

---

## 2. Principals Metodologies Àgils: Scrum, Kanban i XP

### 2.1. El Marc de Treball Scrum
És el marc àgil més utilitzat a l'Administració Local per a la creació de noves aplicacions municipals:

```mermaid
flowchart LR
    subgraph SCRUM_FLOW["EL CICLE D'UN SPRINT A SCRUM"]
        PB["Product Backlog<br/>(Llista prioritzada pel Product Owner)"] --> SP["Sprint Planning<br/>(Planificació de l'Sprint)"]
        SP --> SB["Sprint Backlog<br/>(Tasques de l'Sprint)"]
        SB --> Sprint["Sprint (2 a 4 setmanes)<br/>Daily Scrum (15 min/dia)"]
        Sprint --> Incr["Increment de Programari Funcional<br/>(Sprint Review + Retrospective)"]
    end
```

- **Els 3 Rols de Scrum:**
  1. **Product Owner (PO):** Representa els interessos dels departaments de l'Ajuntament i de la ciutadania; és l'únic responsable de prioritzar el *Product Backlog*.
  2. **Scrum Master:** Líder al servei de l'equip que facilita els esdeveniments, elimina impediments externs i assegura l'aplicació correcta de Scrum.
  3. **Developers (Equip de Desenvolupament):** Equip multidisciplinari i autoorganitzat que construeix l'increment funcional.
- **Definició de Fet (*Definition of Done - DoD*):** Criteris objectius i de qualitat que ha de complir una tasca per considerar-se acabada (codi provat, documentat i sense vulnerabilitats ENS).

---

### 2.2. Mètode Kanban
- Basat en la filosofia *Lean*: se centra a **visualitzar el flux de treball en un tauler** (*To Do $\rightarrow$ In Progress $\rightarrow$ Done*) i **limitar el treball en curs (WIP - *Work In Progress*)** a cada columna per evitar colls d'ampolla.
- És ideal per a equips d'explotació i manteniment TIC (*Helpdesk* / suport d'incidències).

### 2.3. Extreme Programming (XP - Kent Beck)
- Centrat en l'excel·lència tècnica mitjançant pràctiques com el **Desenvolupament Guiat per Proves (TDD - *Test-Driven Development*)**, la **Programació en Parella (*Pair Programming*)** i la integració contínua.

---

## 3. Tècniques per a la Definició de Requisits del Sistema

```mermaid
flowchart TD
    subgraph TECNIQUES_REQUISITS["TÈCNIQUES D'ENGINYERIA DE REQUISITS"]
        SRS["1. Especificació Formal de Requisits (IEEE 830 / ISO 29148)<br/>- Requisits Funcionals: Serveis i accions del sistema (ex. 'Permetre el pagament amb targeta').<br/>- Requisits No Funcionals: Rendiment, seguretat ENS, accessibilitat WCAG 2.1 AA i disponibilitat."]
        
        UC["2. Casos d'Ús UML (Use Cases - Ivar Jacobson)<br/>Diagrames visuals de comportament que mostren la interacció entre Actors (ciutadà, administratiu) i el sistema mitjançant relacions <<include>> (obligatòria) i <<extend>> (opcional)."]
        
        US["3. Històries d'Usuari (User Stories en entorns àgils)<br/>Descripció curta des del punt de vista de l'usuari final seguint la plantilla estàndard."]
    end
```

### 3.1. Estructura de les Històries d'Usuari i el Model INVEST
- **Format Canònic:**
  $$\text{Com a } [\text{Rol d'usuari}], \text{ vull } [\text{Acció / Funcionalitat}] \text{ per tal de } [\text{Benefici / Objectiu}].$$
- **El Model INVEST (Criteris de Qualitat d'una Història d'Usuari):**
  - **I**ndependent: No depèn d'altres històries.
  - **N**egociable: Detalls oberts a discussió entre l'equip i el Product Owner.
  - **V**aluable: Aporta valor clar i mesurable al ciutadà o a l'Ajuntament.
  - **E**stimable: L'equip en pot estimar l'esforç en punts d'història (*Story Points*).
  - **S**mall (Petita): Cap dins d'un sol Sprint.
  - **T**estable: Disposa de **Criteris d'Acceptació clars** per validar que funciona (*Given-When-Then*).

---

## 4. Quadre Resum i Preguntes Típiques d'Examen Tipus Test

| Pregunta / Concepte Clau | Resposta Correcta / Trampa Freqüent |
| :--- | :--- |
| **Qui és l'únic responsable de prioritzar el Product Backlog a Scrum?** | El **Product Owner (PO)**. |
| **Quina és la durada habitual d'un Sprint a Scrum?** | Entre **1 i 4 setmanes** (amb lliurament d'un increment de programari funcional). |
| **Quina és la regla fonamental del mètode Kanban?** | **Limitar el treball en curs (WIP - Work In Progress)** per evitar colls d'ampolla. |
| **Quina diferència hi ha entre una relació `<<include>>` i `<<extend>>` a Casos d'Ús UML?** | **`<<include>>` és una inclusió obligatòria** del cas d'ús; **`<<extend>>` és una extensió condicional o opcional**. |
| **Què signifiquen les sigles del model INVEST per a Històries d'Usuari?** | **Independent, Negociable, Valuable, Estimable, Small (Petita) i Testable (Verificable)**. |
| **Què és el TDD (Test-Driven Development) a Extreme Programming?** | Escriure **primer les proves automàtiques abans d'escriure el codi funcional**. |
