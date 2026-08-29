# Tema 55. Comunicacions de telefonia fixa i mòbil: xarxes, tecnologies, Telefonia IP (VoIP, SIP, RTP), evolució cel·lular (4G LTE, 5G), consum energètic i dispositius clients

> **Fonts i marcs de referència:** Esquema Nacional de Seguretat ([`CORPUS/ENS_2022.pdf`](file:///home/oriol/Projectes/OPOS/CORPUS/ENS_2022.pdf) - Mesura `[mp.com.2]` Protecció de veu sobre IP), estàndards IETF de telefonia IP (**RFC 3261 SIP**, **RFC 3550 RTP**, **RFC 4566 SDP**) i especificacions mòbils del consorci **3GPP (Release 15/16/17 per a 5G NR i 5G RedCap)**.

---

## 1. Evolució de la Telefonia Fixa: De la RTB a la Telefonia IP (ToIP)

La telefonia municipal ha evolucionat des de les antigues xarxes commutades de circuits (RTC analògica i RDSI digital) cap a la **Telefonia IP (Veu sobre IP - VoIP)**, on la veu es digitalitza, es comprimeix en paquets IP i es transmet a través de la mateixa xarxa de dades Ethernet corporativa:

```mermaid
flowchart LR
    subgraph ARQUITECTURA_VOIP["ARQUITECTURA DE TELEFONIA IP MUNICIPAL"]
        IP_Phone["Telèfons IP d'Oficina (RJ-45 / PoE)"]
        Softphone["Softphones (Ordinadors i Mòbils)"]
        PBX["Centraleta Telefònica IP (IP-PBX / Asterisk / 3CX)"]
        SIPT["Troncal SIP (SIP Trunk d'Operador sobre Fibra)"]
        PSTN["Xarxa Telefònica Pública Externa"]

        IP_Phone <--> PBX
        Softphone <--> PBX
        PBX <--> SIPT <--> PSTN
    end
```

---

## 2. Protocols Fonamentals de la Telefonia IP (VoIP)

La comunicació de veu sobre IP separa estrictament la **senyalització** (establir i penjar trucades) del **transport d'àudio**:

```mermaid
flowchart TD
    subgraph PROTOCOLS_VEU["SEPARACIÓ DE PROTOCOLS EN TELEFONIA IP"]
        SIP["1. PROTOCOL SIP (Session Initiation Protocol - RFC 3261)<br/>- Capa de Senyalització (Port UDP/TCP 5060 o TLS 5061).<br/>- Missatges de text plans tipus HTTP: INVITE, 180 RINGING, 200 OK, ACK, BYE.<br/>- Negocia els paràmetres de la trucada mitjançant SDP (Session Description Protocol)."]
        RTP["2. PROTOCOL RTP (Real-Time Transport Protocol - RFC 3550)<br/>- Capa de Transport de la Veu (sobre UDP amb ports dinàmics parells).<br/>- Transporta la veu digitalitzada en temps real amb segells de temps i números de seqüència."]
        RTCP["3. PROTOCOL RTCP (RTP Control Protocol)<br/>- Monitoritza la qualitat de la transmissió (mesura el Jitter, latència i pèrdua de paquets)."]
    end
```

### 2.1. Còdecs de Veu Principals
- **G.711 (PCM):** Còdec estàndard sense compressió de 64 kbps (G.711 A-law a Europa). Ofereix màxima qualitat sense retard de càlcul.
- **G.729:** Còdec comprimit d'alta eficiència que redueix el consum a **8 kbps** (ideal per a seus municipals amb amplada de banda limitada).
- **Opus:** Còdec obert d'última generació d'alta definició (HD Voice) que adapta dinàmicament la seva velocitat (de 6 a 510 kbps) a l'estat de la xarxa.

---

## 3. Evolució de les Xarxes de Telefonia Mòbil Cel·lular

```mermaid
flowchart TD
    subgraph GENERACIONS_MOBILS["EVOLUCIÓ DE LES GENERACIONS MÒBILS"]
        G2["2G (GSM - 1991): Digitalització de la veu, SMS (160 caràcters) i dades GPRS/EDGE."]
        G3["3G (UMTS / HSPA+ - 2001): Arribada d'Internet mòbil de banda ampla (fins a 42 Mbps)."]
        G4["4G (LTE / LTE-Advanced - 2010): Xarxa 100% basada en paquets IP (All-IP) i trucades de veu sobre LTE (VoLTE)."]
        G5["5G (5G NR - 3GPP Rel. 15/16): Latència ultra-baixa, connectivitat massiva IoT i alta velocitat."]

        G2 --> G3 --> G4 --> G5
    end
```

---

## 4. La Tecnologia 5G i el seu Impacte en l'Administració Local

La tecnologia **5G (5G New Radio - 3GPP)** és una plataforma de connectivitat per a la ciutat intel·ligent (*Smart City*) basada en **tres pilars d'ús**:

```mermaid
flowchart TD
    subgraph PILARS_5G["ELS TRES PILARS DEL 5G (ITU-R)"]
        EMBB["1. eMBB (Enhanced Mobile Broadband)<br/>- Velocitats màximes de 10 a 20 Gbps.<br/>- Transmissió de vídeo 4K/8K en temps real des de drons de la Policia Local i brigada."]
        MMTC["2. mMTC (Massive Machine Type Communications)<br/>- Connectivitat massiva de fins a 1 MILIÓ DE DISPOSITIUS PER KM2.<br/>- Xarxes de sensors municipals (residus, aigua, enllumenat, aparcament)."]
        URLLC["3. URLLC (Ultra-Reliable Low-Latency Communications)<br/>- LATÈNCIA ULTRA-BAIXA D'1 MIL·LISEGON (1 ms) i fiabilitat del 99,999%.<br/>- Control crític de semàfors en emergències, vehicles connectats i teleassistència mèdica."]
    end
```

### 4.1. Conceptes Clau del 5G:
- **Modes de Desplegament:**
  - **5G NSA (*Non-Standalone*):** Utilitza antenes 5G connectades a l'antic nucli de xarxa 4G (*Evolved Packet Core*).
  - **5G SA (*Standalone*):** Desplegament pur amb nucli de xarxa nadiu 5G (*5G Core*), permetent el 100% de les prestacions de latència d'1 ms i *Network Slicing*.
- **Segmentació de Xarxa (*Network Slicing*):** Permet a l'operador dividir la xarxa física 5G en múltiples xarxes virtuals independents amb garanties de servei (QoS) estrictes (ex. un *Slice* prioritari exclusiu per a la Policia Local i Bombers que mai es col·lapsarà durant esdeveniments massius).

---

## 5. Dinàmica del Consum Energètic, Freqüències i Dispositius Clients al 5G

La relació entre freqüències, rendiment i consum al 5G presenta una dinàmica tècnica específica:

### 5.1. La Paradoxa del Consum: Eficiència per Bit vs. Potència Instantània
- **Eficiència per Bit ($\text{Joules/bit}$):** El 5G és fins a un **90% més eficient que el 4G per cada Gigabyte transmès**. Com que transmet a velocitats molt més altes, transfereix els paquets en mil·lisegons i permet que el mòdem torni ràpidament a l'estat de baix consum (*sleep*).
- **Potència Instantània de Pic ($\text{Watts}$):** Durant la transmissió activa a màxima velocitat, el consum instantani és més alt a causa de l'amplada dels canals (100 a 400 MHz vs. 20 MHz en 4G) i el processament digital de senyal (*Massive MIMO*).

```mermaid
flowchart TD
    subgraph FREQUENCIES_5G["BANDES 5G I CONSUM ENERGÈTIC"]
        FR1["1. Freqüències Baixes i Mitjanes (FR1: 700 MHz - 3,5 GHz)<br/>- 700 MHz (Banda n28): Màxima cobertura i baix consum (similar a 4G).<br/>- 3,5 GHz (Banda n78): Banda principal per a capacitat i velocitat."]
        FR2["2. Freqüències d'Ones Mil·limètriques (FR2 / mmWave: 26 - 28 GHz)<br/>- Altíssima velocitat i mínima latència (< 1 ms).<br/>- Major atenuació (Llei de Friis); major consum als amplificadors de radiofreqüència per vèncer la pèrdua de senyal."]
    end
```

---

### 5.2. Mecanismes d'Estalvi Energètic en 5G NR (New Radio)
1. **BWP (*Bandwidth Part*):** El dispositiu no necessita escoltar constantment tot el canal de 100 MHz. Si només rep text o està en repòs, el mòdem commuta dinàmicament a una subbanda estreta de 10-20 MHz reduint dràsticament el consum, obrint-se a 100 MHz només en descàrregues massives.
2. **C-DRX (*Connected-mode Discontinuous Reception*):** El mòdem entra en micro-son (*micro-sleep*) durant mil·lisegons i només es desperta periòdicament per comprovar si hi ha dades assignades.
3. **WUS (*Wake-Up Signal*):** Senyal de baixíssima potència que avisa el dispositiu abans d'encendre els circuits principals de ràdio.

---

### 5.3. Tipologia de Dispositius Clients 5G a l'Administració Local

```mermaid
flowchart TD
    subgraph CLIENTS_5G["DISPOSITIUS CLIENTS 5G MUNICIPALS"]
        D1["1. Smartphones i Tauletes Corporatives (eMBB)<br/>Mòdems d'alta capacitat amb gestió dinàmica BWP per a agents de policia i inspectors."]
        D2["2. Encaminadors 5G FWA / CPEs Municipals<br/>Routers d'accés fix sense fils alimentats per xarxa elèctrica que donen connectivitat a seus aïllades."]
        D3["3. Dispositius 5G RedCap (Reduced Capability / NR-Light - 3GPP Rel. 17)<br/>Xips simplificats per a sensors municipals, càmeres i telemesura amb anys de durada de bateria."]
    end
```

> 🌟 **Concepte clau: 5G RedCap (*Reduced Capability* / *NR-Light*):**  
> Estandarditzat a la *Release 17* del 3GPP, **RedCap** soluciona el problema de consum i cost del 5G en sensors municipals (control d'aigua, càmeres de trànsit, enllumenat): redueix l'amplada de banda a 20 MHz, utilitza 1 o 2 antenes i retalla la complexitat del mòdem, aconseguint que un sensor IoT pugui operar durant anys amb bateria gaudint de la cobertura i baixa latència del 5G.

---

## 6. Quadre Resum i Preguntes Típiques d'Examen Tipus Test

| Pregunta / Concepte Clau | Resposta Correcta / Trampa Freqüent |
| :--- | :--- |
| **Quin protocol s'utilitza per a la senyalització en Telefonia IP?** | El protocol **SIP (Session Initiation Protocol - RFC 3261)**. |
| **Quin protocol transporta la veu digitalitzada en temps real?** | El protocol **RTP (Real-time Transport Protocol - sobre UDP)**. |
| **Quina és la funció d'un Troncal SIP (*SIP Trunk*)?** | Connectar la centraleta IP municipal amb l'operador de telefonia mitjançant una **connexió IP de dades**. |
| **Quina generació mòbil va introduir una arquitectura 100% basada en IP?** | El **4G (LTE - Long Term Evolution)**, eliminant els circuits tradicionals. |
| **Quina latència ofereix el pilar URLLC del 5G?** | Una latència de fins a **1 mil·lisegon (1 ms)** amb fiabilitat del 99,999%. |
| **Què és el Network Slicing en 5G?** | La creació de **xarxes virtuals dedicades sobre la mateixa infraestructura física** per garantir serveis crítics. |
| **Com és l'eficiència energètica del 5G respecte al 4G per Gigabyte?** | El 5G és **fins a un 90% més eficient en Joules per bit**, tot i que té pics de consum instantani més alts en canals amples. |
| **Què és el 5G RedCap (NR-Light - 3GPP Rel. 17)?** | Una versió optimitzada del 5G per a **sensors i dispositius IoT de baix cost i baix consum**. |
| **Com redueix el consum de bateria la funció BWP (*Bandwidth Part*)?** | Ajustant l'amplada de banda del mòdem a **subbandes estretes quan no es requereix màxima velocitat**. |
