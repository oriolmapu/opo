# Tema 86. Programació conjunta amb agents d'Intel·ligència Artificial: paradigmes (Pair Programming, Agentic Coding), estudi de mercat (GitHub Copilot, Claude Code, Antigravity), seguretat ENS i compliment legal (RGPD / EU AI Act)

> **Fonts i marcs de referència:** Reglament (UE) 2024/1689 d'Intel·ligència Artificial ([`CORPUS/AI_Act_2024.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/AI_Act_2024.pdf)), Esquema Nacional de Seguretat ([`CORPUS/ENS_2022.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENS_2022.pdf) - Mesures `[op.pl.5]` Adquisició i desenvolupament segur), Reglament General de Protecció de Dades ([`CORPUS/LOPD.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/LOPD.pdf) - Confidencialitat) i bones pràctiques d'enginyeria de programari assistida per IA.

---

## 1. La Revolució de la Programació Assistida per IA (*AI Pair Programming*)

La programació assistida per Intel·ligència Artificial ha evolucionat des de l'autocompletat tradicional fins a la creació d'**agents autònoms de desenvolupament de programari (*Agentic Coding*)**:

```mermaid
flowchart TD
    subgraph GENERACIONS_ASSISTENCIA_IA["EVOLUCIÓ DE L'ASSISTÈNCIA AL DESENVOLUPAMENT"]
        G1["1. Autocompletat Tradicional (IntelliSense)<br/>Basat en regles sintàctiques fixes i arbres de tipus de l'IDE."]
        G2["2. Completat per Models de Llenguatge (LLMs / Inline Suggestions)<br/>Predicció probabilística de codi en temps real a mesura que el programador escriu."]
        G3["3. Xat Interactiu al Context de l'IDE (Chat & Code Refactoring)<br/>Converses en llenguatge natural per generar tests unitaris, explicar codi i corregir bugs."]
        G4["4. Agents Autònoms d'Enginyeria de Programari (Agentic Coding)<br/>Sistemes que analitzen el repositori sencer, elaboren plans, executen ordres al terminal, corregeixen lints i creen commits de forma autònoma."]

        G1 --> G2 --> G3 --> G4
    end
```

---

## 2. Estudi de Mercat: Eines Líders de Desenvolupament amb IA

```mermaid
flowchart TD
    subgraph EINES_IA_MERCAT["COMPARATIVA DE PLATAFORMES D'IA PER A PROGRAMACIÓ"]
        Copilot["A) GITHUB COPILOT (GitHub / Microsoft / OpenAI)<br/>- Líder de mercat integrat a VS Code i JetBrains.<br/>- Completat de codi en temps real, revisió de Pull Requests i assistent de terminal (Copilot CLI)."]
        
        ClaudeCode["B) CLAUDE CODE (Anthropic)<br/>- Agent de terminal (CLI) orientat a l'execució d'ordres i refactorització profunda.<br/>- Basat en la família de models Claude (Claude 3.5 / 3.7 Sonnet), destacat per la seva precisió en canvis multi-fitxer i raonament lògic."]
        
        Antigravity["C) ANTIGRAVITY (Google DeepMind / Google)<br/>- Entorn de desenvolupament d'agents avançats (Agentic IDE).<br/>- Integració nativa amb models Gemini de context ultra-llarg (milions de tokens), subagents en paral·lel per a tasques complexes i suport per a eines d'inspecció de navegador (E2E)."]
    end
```

---

## 3. Principals Casos d'Ús a l'Administració Pública

1. **Modernització d'Aplicacions Llegades (*Legacy Modernization*):** Traducció i refactorització d'aplicacions antigues monolítiques municipals cap a arquitectures de microserveis modernes (Spring Boot / Node.js).
2. **Generació Automàtica de Proves Unitàries i d'Integració (TDD):** Creació exhaustiva de tests (JUnit, Jest, PyTest) cobrint casos límit de validació administrativa.
3. **Documentació de Codi i Generació d'Esquemes OpenAPI:** Generació automàtica de documentació de codi i fitxers OpenAPI / Swagger per complir l'ENI ([`CORPUS/ENI.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENI.pdf)).
4. **Detecció Precoç de Vulnerabilitats:** Identificació proactiva de fallades de seguretat OWASP (injeccions SQL, XSS, fuites de memòria) abans de fer el desplegament a producció.

---

## 4. Bones Pràctiques i Enginyeria de Context (*Context Engineering*)

Per obtenir el màxim rendiment dels agents d'IA a la feina diària:
- **Descomposició de Tasques:** Dividir problemes grans en subtasques atòmiques i verificables pas a pas.
- **Fitxers de Regles de Projecte:** Inclusió d'instruccions i directrius d'estil al repositori (com fitxers de regles o memòries tècniques) per guiar el comportament de l'agent.
- **Supervisió Humana Efectiva (*Human-in-the-Loop*):** El desenvolupador públic ha d'exercir sempre de revisor crític, verificant la correcció lògica, el compliment normatiu i l'eficiència del codi proposat per la IA.

---

## 5. Seguretat, Confidencialitat i Marc Legal (ENS, RGPD i EU AI Act)

L'ús d'IA en entorns municipals exigeix garanties de compliment legal i de seguretat de la informació:

```mermaid
flowchart TD
    subgraph REQUISITS_SEGURETAT_IA["REQUISITS DE SEGURETAT I COMPLIMENT LEGAL"]
        Conf["1. Confidencialitat i Propietat del Codi (ENS)<br/>- Prohibició estricta de filtrar claus privades, contrasenyes de bases de dades o dades ciutadanes.<br/>- Exigència de contractes amb clàusules de NO entrenament de models (Zero Data Retention)."]
        
        Sec["2. Prevenció d'Al·lucinacions de Paquets (Package Hallucination)<br/>- Verificació que les llibreries suggerides per la IA existeixen realment en dipòsits oficials per evitar atacs de confusió de dependències."]
        
        AIAct["3. Reglament d'IA de la UE (Reglament 2024/1689)<br/>- Transparència en els sistemes automatitzats del sector públic.<br/>- Supervisió humana obligatòria i avaluació d'impacte en drets fonamentals (AIPD)."]
    end
```

---

## 6. Quadre Resum i Preguntes Típiques d'Examen Tipus Test

| Pregunta / Concepte Clau | Resposta Correcta / Trampa Freqüent |
| :--- | :--- |
| **Què diferencia l'Agentic Coding de l'autocompletat d'IA tradicional?** | L'Agentic Coding **planifica, executa ordres de terminal, gestiona fitxers múltiples i corregeix errors de forma autònoma**. |
| **Quina mesura de seguretat s'ha d'exigir als proveïdors d'eines d'IA segons l'ENS?** | Clàusules empresarials de **no entrenament amb el codi del client (*Zero Data Retention*)**. |
| **Què és l'atac per al·lucinació de paquets (*Package Hallucination*)?** | La creació per part d'atacants de **llibreries malicioses amb noms que les IAs solen inventar erròniament**. |
| **Quin paper és obligatori per al desenvolupador segons el Reglament d'IA de la UE?** | El paper de **supervisió humana efectiva (*Human-in-the-Loop*)**. |
| **Quin entorn d'IA destaca pel desenvolupament mitjançant agents basat en la plataforma Gemini?** | **Antigravity** (Google DeepMind / Google). |
| **Quin agent de terminal (CLI) ha estat desenvolupat per Anthropic?** | **Claude Code**. |
