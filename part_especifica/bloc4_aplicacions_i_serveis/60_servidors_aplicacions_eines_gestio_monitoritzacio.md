# Tema 60. Servidors d'aplicacions: arquitectura, gestió, eines d'administració i monitorització (APM, logs)

> **Fonts i marcs de referència:** Esquema Nacional de Seguretat ([`CORPUS/ENS_2022.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENS_2022.pdf) - Mesures `[mp.sw]` Seguretat de programari, `[op.mon]` Monitorització i `[op.exp.8]` Registre d'activitat), Esquema Nacional d'Interoperabilitat ([`CORPUS/ENI.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENI.pdf)) i especificacions d'estàndards **Jakarta EE (Eclipse Foundation)** i **W3C/IETF**.

---

## 1. Concepte de Servidor d'Aplicacions vs. Servidor Web

En l'arquitectura de sistemes municipals de tres capes (*Three-Tier Architecture*), el **servidor d'aplicacions** és la peça central que executa la lògica de negoci i interconnecta els portals ciutadans amb les bases de dades corporatives:

```mermaid
flowchart LR
    subgraph ARQUITECTURA_3_CAPES["ARQUITECTURA CLÀSSICA DE TRES CAPES"]
        Client["Navegador Ciutadà / Seu Electrònica"]
        WebTier["1. CAPA WEB (Servidor Web)<br/>(Nginx / Apache HTTP)<br/>- Contingut estàtic (HTML/CSS/JS)<br/>- Terminació SSL/TLS (HTTPS)<br/>- Reverse Proxy & Balanç"]
        AppTier["2. CAPA D'APLICACIÓ (App Server)<br/>(Tomcat / WildFly / Node.js / IIS)<br/>- Lògica de negoci de tràmits<br/>- Gestió de sessions i transaccions<br/>- Pools de connexió SQL (JDBC)"]
        DBTier["3. CAPA DE DADES (Base de Dades)<br/>(PostgreSQL / Oracle / MySQL)<br/>- Persistència d'expedients<br/>- Registre d'entrades/sortides"]

        Client <--> WebTier <--> AppTier <--> DBTier
    end
```

| Criteri de Comparació | Servidor Web (*Web Server*) | Servidor d'Aplicacions (*App Server*) |
| :--- | :--- | :--- |
| **Funció Principal** | Servir **contingut estàtic** (HTML, imatges, CSS) i gestionar connexions HTTP/HTTPS. | Executar **lògica de negoci dinàmica**, càlculs, processos batch i regles administratives. |
| **Tecnologies / Protocols** | Protocols HTTP, HTTPS, HTTP/2, HTTP/3, WebSockets. | Java EE/Jakarta EE, .NET Core, Python WSGI, PHP-FPM, Node.js. |
| **Accés a Dades** | No accedeix directament a bases de dades; fa de passarel·la (*Reverse Proxy*). | Gestiona **connexions directes a BD mitjançant *Connection Pools*** i transaccions ACID. |
| **Exemples Líders** | **Nginx, Apache HTTP Server, Microsoft IIS**. | **Apache Tomcat, WildFly (JBoss), Node.js (PM2), Gunicorn**. |

---

## 2. Ecosistemes i Tipologia de Servidors d'Aplicacions

```mermaid
flowchart TD
    subgraph ECOSISTEMES_APP["PRINCIPALS MOTORS D'APLICACIONS MUNICIPALS"]
        Java["1. Ecosistema Java (Jakarta EE)<br/>- Apache Tomcat / Eclipse Jetty: Contenidor de Servlets lleuger per a aplicacions web.<br/>- WildFly / JBoss EAP: Servidor empresarial complet (EJB, JTA, JMS, CDI).<br/>- Característica clau: Gestió de memòria JVM (Heap, Garbage Collection) i pools HikariCP."]
        DotNet["2. Ecosistema Microsoft .NET<br/>- Internet Information Services (IIS) amb ASP.NET Core.<br/>- Aïllament per Application Pools (processos w3wp.exe independents)."]
        Modern["3. Ecosistemes Codi Obert Moderns<br/>- Node.js: Arquitectura asíncrona no bloquejant gestionada per PM2.<br/>- Python: Servidors WSGI/ASGI (Gunicorn / Uvicorn).<br/>- PHP: Servidor de processos PHP-FPM associat a Nginx."]
    end
```

---

## 3. Gestió, Clústers i Alta Disponibilitat

Per garantir la continuïtat de serveis crítics municipals (com el Padró d'Habitants o el Gestor d'Expedients), els servidors d'aplicacions es configuren en **alta disponibilitat**:

```mermaid
flowchart TD
    subgraph CLUSTER_APP["CLÚSTER EN ALTA DISPONIBILITAT"]
        Proxy["Balançador de Càrrega (HAProxy / Nginx Upstream)"]
        Node1["Node App 1 (Tomcat Instància A)"]
        Node2["Node App 2 (Tomcat Instància B)"]
        Cache[("Emmagatzematge de Sessions en Memòria<br/>(Clúster Redis / Memcached)")]

        Proxy --> Node1
        Proxy --> Node2
        Node1 <--> Cache
        Node2 <--> Cache
    end
```

- **Mecanismes Clau de Gestió:**
  1. **Pools de Connexions a Base de Dades (*Connection Pooling*):** Mantenen connexions obertes i reutilitzables cap a la base de dades, evitant el sobrecost d'obrir i tancar connexions a cada tràmit ciutadà.
  2. **Persistència de Sessions (*Session Sharing*):** L'estat de sessió dels usuaris es guarda en un magatzem ràpid en memòria (**Redis**). Si un node cau, el ciutadà continua el seu tràmit al segon node sense desconnectar-se.
  3. **Automatització i Infraestructura com a Codi (IaC):** Desplegament i actualització de servidors d'aplicacions mitjançant receptes d'**Ansible** o plantilles de contenidors.

---

## 4. Eines de Monitorització, Rendiment (APM) i Gestió de Logs

Segons les mesures de monitorització `[op.mon]` i registre d'activitat `[op.exp.8]` de l'**Esquema Nacional de Seguretat (ENS)**:

```mermaid
flowchart TD
    subgraph EINES_MONITORITZACIO["EINES DE CONTROL I OBSERVABILITAT"]
        APM["1. Monitorització del Rendiment d'Aplicacions (APM)<br/>(Elastic APM / Dynatrace / Prometheus)<br/>Mesura temps de resposta de peticions, colls d'ampolla i consultes SQL lentes."]
        Metrics["2. Mètriques de Sistema i JVM<br/>(Grafana + Prometheus / Zabbix)<br/>Ús de CPU, saturació de memòria Heap i temps de pausa del Garbage Collector."]
        Logs["3. Gestió Centralitzada de Registres (Logs)<br/>(Pila ELK: Elasticsearch + Logstash + Kibana / Grafana Loki)<br/>Indexació i traçabilitat d'errors, auditories d'accés i alertes de seguretat."]
    end
```

---

## 5. Quadre Resum i Preguntes Típiques d'Examen Tipus Test

| Pregunta / Concepte Clau | Resposta Correcta / Trampa Freqüent |
| :--- | :--- |
| **Quina és la diferència funcional entre Nginx i Apache Tomcat?** | **Nginx és un servidor web/reverse proxy** per a contingut estàtic; **Tomcat és un contenidor de servlets/aplicacions** Java per a lògica dinàmica. |
| **Què és un Connection Pool (com HikariCP)?** | Un mecanisme que **manté connexions obertes a la base de dades per reutilitzar-les**, optimitzant el rendiment. |
| **Quina eina permet compartir sessions entre servidors d'un clúster?** | Un magatzem de dades en memòria distribuït com **Redis** o **Memcached**. |
| **Què és una eina d'APM (Application Performance Monitoring)?** | Un programari que **monitoritza les transaccions internes de l'aplicació, consultes a BD i colls d'ampolla**. |
| **Quins components formen la pila clàssica de gestió de logs ELK?** | **Elasticsearch** (motor de cerca/indexació), **Logstash** (recol·lector/processador) i **Kibana** (panell de visualització). |
| **Com aïlla Microsoft IIS diferents aplicacions en un mateix servidor?** | Mitjançant **Application Pools** associats a processos `w3wp.exe` independents. |
