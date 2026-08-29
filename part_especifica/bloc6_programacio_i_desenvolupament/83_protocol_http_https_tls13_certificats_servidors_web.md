# Tema 83. Protocol HTTP/HTTPS: evolució (HTTP/1.1, HTTP/2, HTTP/3 QUIC), seguretat TLS 1.3, gestió de certificats (X.509, ACME, T-CAT) i servidors web (Nginx, Apache, Tomcat)

> **Fonts i marcs de referència:** Esquema Nacional de Seguretat ([`CORPUS/ENS_2022.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENS_2022.pdf) - Mesura `[mp.com.4]` Trànsit xifrat i Guia CCN-STIC 807), Esquema Nacional d'Interoperabilitat ([`CORPUS/ENI.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENI.pdf)) i especificacions IETF (**RFC 7540 HTTP/2, RFC 9114 HTTP/3, RFC 8446 TLS 1.3, RFC 8555 ACME**).

---

## 1. Evolució del Protocol HTTP: De HTTP/1.1 a HTTP/3

El protocol de transferència d'hipertext és la base de la comunicació a la World Wide Web:

```mermaid
flowchart TD
    subgraph EVOLUCIO_HTTP["EVOLUCIÓ DEL PROTOCOL HTTP (IETF)"]
        H1["1. HTTP/1.1 (RFC 7230 - 1999)<br/>- Protocol basat en text pla sobre TCP.<br/>- Connexions persistents (Keep-Alive) i mètode Pipeline.<br/>- Pateix bloqueig de cap de línia a nivell d'aplicació (Head-of-Line Blocking)."]
        
        H2["2. HTTP/2 (RFC 7540 - 2015)<br/>- Protocol binari sobre TCP.<br/>- MULTIPLEXACIÓ: Múltiples peticions i respostes simultànies sobre una sola connexió TCP.<br/>- Compressió de capçaleres mitjançant HPACK i Server Push."]
        
        H3["3. HTTP/3 (RFC 9114 - 2022)<br/>- Funciona sobre el protocol de transport QUIC (basat en UDP / Port 443).<br/>- ELIMINA EL BLOQUEIG HEAD-OF-LINE A NIVELL DE TRANSPORT.<br/>- Handshake segur 0-RTT i migració de connexió sense tall en canviar de Wi-Fi a 5G."]

        H1 --> H2 --> H3
    end
```

---

## 2. El Protocol HTTPS i Xifratge TLS 1.3 (RFC 8446)

El protocol **HTTPS (*HTTP Secure*)** utilitza **TLS (*Transport Layer Security*)** sobre el port **TCP 443** per garantir la confidencialitat, la integritat i l'autenticitat dels tràmits ciutadans:

```mermaid
flowchart LR
    subgraph TLS_HANDSHAKE["ESTABLIMENT DE CONNEXIÓ SEGURA TLS 1.3 (1-RTT)"]
        Client["Navegador Ciutadà"] -->|"1. ClientHello (Claus Efímeres ECDHE + Xifrats)"| Server["Servidor Seu Electrònica"]
        Server -->|"2. ServerHello + Certificat Digital X.509 + Finished"| Client
        Client <-->|"3. Canvi immediat a canal xifrat d'alta velocitat (AES-256-GCM)"| Server
    end
```

- **Secret Directe Perfecte (*Perfect Forward Secrecy - PFS*):** Exigit per l'ENS; garanteix que, encara que una clau privada del servidor fos robada en el futur, ningú podrà desxifrar el trànsit capturat en el passat.

### 2.1. Capçaleres de Seguretat Web Obligatòries a l'ENS:
- **HSTS (*HTTP Strict Transport Security*):** Força el navegador a connectar-se sempre per HTTPS, evitant atacs de degradació (*SSL Strip*):  
  `Strict-Transport-Security: max-age=31536000; includeSubDomains; preload`
- **CSP (*Content Security Policy*):** Immunitza la pàgina contra atacs de Cross-Site Scripting (XSS).
- **X-Frame-Options: DENY:** Impedeix atacs de segrest de clics (*Clickjacking*).

---

## 3. Gestió de Certificats Digitals de Servidor (X.509 v3)

```mermaid
flowchart TD
    subgraph CICLE_CERTIFICAT["CICLE DE VIDA D'UN CERTIFICAT WEB"]
        CSR["1. Generació de Claus i CSR (Certificate Signing Request)<br/>(Conté la clau pública i el nom del domini: seu.ajuntament.cat)"]
        CA["2. Autoritat de Certificació (Consorci AOC / T-CAT de Seu)<br/>(Valida la titularitat pública de l'ens i signa el certificat)"]
        Deploy["3. Instal·lació i Renovació Automàtica (Protocol ACME / Certbot)<br/>(Desplegament al servidor web amb comprovació de caducitat)"]

        CSR --> CA --> Deploy
    end
```

- **Certificats del Consorci AOC:** A Catalunya, les seus electròniques municipals utilitzen certificats de **Seu Electrònica i Segell Electrònic (T-CAT)** emesos per l'AOC d'acord amb el Reglament eIDAS i l'ENI.

---

## 4. Servidors Web HTTP vs. Servidors d'Aplicació Web

En un entorn corporatiu municipal s'utilitza una combinació desacoblada de servidors:

```mermaid
flowchart LR
    Internet["Ciutadania (Internet HTTPS 443)"] --> Nginx["SERVIDOR WEB / REVERSE PROXY (Nginx)<br/>- Terminació SSL/TLS<br/>- Servir contingut estàtic (Imatges, CSS)<br/>- Balanç de càrrega i regles WAF"]
    
    subgraph DMZ_INTERNA["XARXA INTERNA PROTEGIDA"]
        Nginx -->|"Proxy Pass (Port intern 8080)"| Tomcat["SERVIDOR D'APLICACIÓ (Apache Tomcat)<br/>Execució de codi Java i lògica d'expedients"]
    end
```

---

## 5. Quadre Resum i Preguntes Típiques d'Examen Tipus Test

| Pregunta / Concepte Clau | Resposta Correcta / Trampa Freqüent |
| :--- | :--- |
| **Quin protocol de transport utilitza HTTP/3 en lloc de TCP?** | El protocol **QUIC (basat en UDP / Port 443)**. |
| **Quina millora clau introdueix HTTP/2 davant HTTP/1.1?** | La **Multiplexació de fluxos binaris** sobre una única connexió TCP. |
| **Quina capçalera de seguretat obliga el navegador a utilitzar exclusivament HTTPS?** | La capçalera **HSTS (*HTTP Strict Transport Security*)**. |
| **Quin protocol permet la renovació automàtica de certificats web?** | El protocol **ACME (RFC 8555)** utilitzat per eines com Certbot. |
| **Quina és la funció de Nginx situat al davant d'Apache Tomcat?** | Actuar com a **Reverse Proxy, terminador SSL/TLS i servidor de contingut estàtic**. |
| **Què és el Secret Directe Perfecte (PFS) a TLS 1.3?** | Una propietat que **evita desxifrar comunicacions passades si es compromet la clau privada en el futur**. |
