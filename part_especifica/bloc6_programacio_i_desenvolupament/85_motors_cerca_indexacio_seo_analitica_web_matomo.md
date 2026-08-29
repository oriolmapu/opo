# Tema 85. Motors de cerca, indexació (índex invertit), posicionament SEO (Core Web Vitals, Schema.org) i analítica web municipal respectuosa amb el RGPD (Matomo)

> **Fonts i marcs de referència:** Esquema Nacional d'Interoperabilitat ([`CORPUS/ENI.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENI.pdf) - NTI de Catàleg d'estàndards per a metadades i Open Data), Llei 19/2014 de Transparència ([`CORPUS/Transparencia.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/Transparencia.pdf)), Reglament General de Protecció de Dades ([`CORPUS/LOPD.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/LOPD.pdf) - Analítica i transferències internacionals) i directrius del **W3C (Schema.org)** i **Google Search Central**.

---

## 1. Com Funcionen els Motors de Cerca: Rastreig, Indexació i Rànquing

Un motor de cerca (com Google, Bing o el motor de cerca intern del portal municipal) processa la informació en tres fases essencials:

```mermaid
flowchart LR
    subgraph FUNCIONAMENT_MOTOR_CERCA["FASES D'UN MOTOR DE CERCA"]
        Crawl["1. Rastreig (Crawling)<br/>Aranyes web (bots) descobreixen pàgines seguint enllaços, sitemap.xml i robots.txt."] --> Index["2. Indexació (Indexing)<br/>Processament del text, extracció de paraules clau i creació de l'ÍNDEX INVERTIT."]
        Index --> Rank["3. Rànquing (Ranking)<br/>Algorismes que ordenen els resultats per rellevància, autoritat i Core Web Vitals."]
    end
```

- **L'Índex Invertit (*Inverted Index*):** Estructura de dades fonamental que mapeja cada paraula existent amb la llista de documents i pàgines on apareix (utilitzat per motors de cerca avançats com **Elasticsearch** o **Apache Solr** per indexar normatives i actes municipals).
- **Arxius de Control de Rastreig:**
  - `robots.txt`: Directrius que indiquen als bots quines rutes no han de rastrejar (ex. `Disallow: /admin/`).
  - `sitemap.xml`: Fitxer XML que conté la llista completa i jeràrquica de totes les URLs del portal municipal i la seva freqüència d'actualització.

---

## 2. Posicionament en Cercadors (SEO - *Search Engine Optimization*)

El SEO en l'Administració Pública té per objectiu que la ciutadania trobi ràpidament els tràmits i serveis municipals a Internet:

```mermaid
flowchart TD
    subgraph PILARS_SEO["ELS PILARS DEL SEO INSTITUCIONAL"]
        Semantica["1. SEO On-Page i Semàntica HTML5<br/>- Un sol `<h1>` principal per pàgina.<br/>- Etiqueta `<title>` única i descriptiva (50-60 caràcters).<br/>- Meta descripció precisa (`<meta name='description'>`).<br/>- Etiquetes canòniques (`<link rel='canonical'>`) per evitar contingut duplicat."]
        
        Vitals["2. Rendiment Tècnic: Mètriques Core Web Vitals (Google)<br/>- LCP (Largest Contentful Paint): Temps de càrrega del bloc principal (< 2.5 segons).<br/>- INP (Interaction to Next Paint): Temps de resposta a clics de l'usuari (< 200 ms).<br/>- CLS (Cumulative Layout Shift): Estabilitat visual sense salts (< 0.1)."]
        
        Schema["3. Dades Estructurades (Schema.org / JSON-LD)<br/>Metadades que permeten als cercadors entendre que es tracta d'un tràmit públic, un acte municipal o una subvenció."]
    end
```

---

## 3. Analítica Web Municipal i Mesura del Trànsit

L'analítica web permet avaluar com interactua la ciutadania amb la Seu Electrònica (quines són les pàgines més visitades, quants tràmits s'inicien i quants s'abandonen):

### 3.1. Principals Mètriques d'Analítica:
- **Usuaris Únics i Sessions:** Nombre de ciutadans diferents i visites realitzades.
- **Taxa de Rebot (*Bounce Rate*):** Percentatge de visites on l'usuari abandona el web sense interactuar.
- **Embut de Conversió (*Conversion Funnel*):** Percentatge de ciutadans que completen amb èxit un procediment electrònic des de la pàgina informativa fins al rebut de registre.

---

## 4. Privacitat en l'Analítica Web: Google Analytics vs. Matomo (RGPD / APDCAT)

L'ús d'eines d'analítica està fortament condicionat pel **RGPD** ([`CORPUS/LOPD.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/LOPD.pdf)) i les resolucions de les autoritats de protecció de dades (APDCAT i AEPD):

```mermaid
flowchart TD
    subgraph COMPARATIVA_ANALITICA["COMPARATIVA D'EINES D'ANALÍTICA MUNICIPAL"]
        GA4["A) GOOGLE ANALYTICS (GA4)<br/>- Plataforma de tercers propietat de Google.<br/>- Requereix consentiment previ exprés mitjançant banner de cookies.<br/>- Risc de transferències internacionals de dades als EUA."]
        
        Matomo["B) MATOMO ANALYTICS (Recomanació per al Sector Públic)<br/>- Programari lliure de codi obert AUTOALLOTJAT AL CPD MUNICIPAL.<br/>- Anonimització nativa d'adreces IP ciutadanes.<br/>- FUNCIONA SENSE COOKIES DE SEGUIMENT: Exempte de deure de consentiment segons l'APDCAT.<br/>- Control absolut i sobirania de dades 100% dins la UE."]
    end
```

---

## 5. Quadre Resum i Preguntes Típiques d'Examen Tipus Test

| Pregunta / Concepte Clau | Resposta Correcta / Trampa Freqüent |
| :--- | :--- |
| **Quina és l'estructura de dades que permet cerques instantànies de text?** | L'**Índex Invertit (*Inverted Index*)**, utilitzat per motors com Elasticsearch. |
| **Què indica el fitxer `sitemap.xml` d'un portal municipal?** | El **mapa estructurat de totes les URLs del web** per facilitar el rastreig dels cercadors. |
| **Quines són les 3 mètriques clau de Core Web Vitals?** | **LCP** (temps de càrrega principal), **INP** (interactivitat) i **CLS** (estabilitat visual). |
| **Quin valor màxim de LCP es considera bo per a Google?** | Un temps de càrrega **inferior a 2,5 segons ($< 2,5\text{ s}$)**. |
| **Quina eina d'analítica web de codi obert és recomanada per complir el RGPD sense cookies?** | **Matomo Analytics** (autoallotjada al servidor propi). |
| **Quina etiqueta evita problemes de contingut duplicat en SEO?** | L'etiqueta **`<link rel="canonical">`**. |
