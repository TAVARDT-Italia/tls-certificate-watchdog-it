<p align="center">
  <a href="https://ag.tavardt.com/it/">
    <img src="https://ag.tavardt.com/img/tavardt-banner.png" alt="TAVARDT" width="1024" />
  </a>
</p>

# Watchdog Certificati TLS/SSL

[![Ambito di Verifica Perimetrale](https://img.shields.io/badge/TAVARDT-Diagnostica%20Tecnica-5b3cf5.svg)](https://tools.tavardt.com/it/analisi-header-sicurezza/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Monitoraggio automatizzato della scadenza dei certificati SSL/TLS, integrità crittografica e conformità agli standard ACN in Italia.

Ingegnerizzato e gestito dalla divisione di ciberdifesa di **TAVARDT** per le infrastrutture web italiane.

***

## 1. Ambito di Verifica Perimetrale

Questa utilità da terminale analizza il ciclo di vita dei certificati, le suite crittografiche e la catena di attendibilità:

* **Allarme Preventivo di Scadenza:** Calcola i giorni rimanenti prima della scadenza con soglie automatiche (> 30 giorni valido, <= 30 giorni avviso, <= 0 giorni scaduto).
* **Verifica della Versione del Protocollo:** Valuta l'imposizione di TLS 1.3 e TLS 1.2 in allineamento con gli standard di sicurezza ACN e GDPR.
* **Ispezione della Suite Crittografica:** Esamina gli algoritmi di cifratura negoziati e la lunghezza delle chiavi simmetriche (128/256 bit).
* **Validazione dell'Autorità di Certificazione (CA):** Ispeziona l'organizzazione emittente nella catena di fiducia e i Subject Alternative Names (SAN).
* **Zero Dipendenze Esterne:** Opera interamente con i moduli nativi `ssl` e `socket` della libreria standard di Python.

***

## 2. Matrice di Sicurezza Crittografica e Ciclo di Vita

<table>
  <thead>
    <tr>
      <th>Parametro Ispezionato</th>
      <th>Metodo di Valutazione dell'Handshake</th>
      <th>Baseline di Sicurezza Consigliata</th>
      <th>Rischio Operativo Mitigato</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Scadenza del Certificato</strong></td>
      <td>Confronta il timestamp <code>notAfter</code> con l'ora UTC attuale</td>
      <td>Avviso preventivo con &gt; 30 giorni rimanenti</td>
      <td>Interruzione del servizio e avvisi di connessione non sicura</td>
    </tr>
    <tr>
      <td><strong>Versione del Protocolo</strong></td>
      <td>Esamina la versione negoziata tramite <code>ssock.version()</code></td>
      <td><code>TLSv1.3</code> preferito, <code>TLSv1.2</code> minimo</td>
      <td>Attacchi di downgrade e protocolli crittografici obsoleti</td>
    </tr>
    <tr>
      <td><strong>Suite Crittografica e Chiavi</strong></td>
      <td>Estrae i dettagli della suite tramite <code>ssock.cipher()</code></td>
      <td>Cifrari moderni AEAD con chiavi di &gt;= 128 bit</td>
      <td>Intercettazione man-in-the-middle e decifrazione del traffico</td>
    </tr>
    <tr>
      <td><strong>Autorità di Certificazione (CA)</strong></td>
      <td>Verifica l'ente emittente nella catena di fiducia</td>
      <td>CA pubblica accreditata nel trust store di sistema</td>
      <td>Certificati auto-firmati non attendibili e frodi di identità</td>
    </tr>
    <tr>
      <td><strong>Subject Common Name (CN)</strong></td>
      <td>Valida l'hostname rispetto al subject del certificato</td>
      <td>Corrispondenza esatta al FQDN o wildcard SAN</td>
      <td>Errori di mancata corrispondenza del dominio e spoofing</td>
    </tr>
  </tbody>
</table>

***

## 3. Guida Rapida all'Esecuzione e Automazione

Esegui lo strumento direttamente tramite Python senza necessità di librerie esterne:

```bash
python watchdog.py esempio.it
```

Il motore di analisi valuta l'handshake socket in modo deterministico:

* **Libreria Standard Nativa:** Opera esclusivamente tramite la libreria standard di Python (`ssl`, `socket`, `datetime`), eliminando ogni dipendenza di terze parti.
* **Diagnostica a Tre Livelli:** Qualifica lo stato del certificato come PASS (> 30 giorni), WARN (in avvicinamento alla scadenza) o FAIL (scaduto).
* **Integrazione Continua in CI/CD:** Si integra in modo lineare nelle pipeline di deploy e nei flussi di monitoraggio continuo dell'uptime.

***

## 4. Domande Frequenti (FAQ Tecnico)

### 1. Perché la soglia di allerta preventiva a 30 giorni è fondamentale per la continuità operativa?
L'avviso a 30 giorni dalla scadenza garantisce il tempo operativo necessario per il rinnovo automatico ACME o la rotazione manuale dei certificati. Il superamento di questo limite comporta il blocco immediato dell'accesso da parte dei browser con gravi avvisi di sicurezza.

### 2. Come ispeziona watchdog.py l'handshake TLS senza scaricare l'intero payload HTTP?
L'utilità apre una connessione diretta via socket TCP sulla porta 443 incapsulata tramite TLS nativo (`ssl.wrap_socket`). Estrae i parametri crittografici direttamente dal certificato durante l'handshake, garantendo tempi di risposta immediati e consumo minimo di rete.

### 3. Quali rischi di sicurezza comportano le versioni obsolete come TLS 1.0 e TLS 1.1?
Le versioni deprecate di TLS utilizzano algoritmi vulnerabili ad attacchi di downgrade e intercettazione man-in-the-middle. L'adozione esclusiva di TLS 1.2 o TLS 1.3 assicura la segretezza di inoltro (forward secrecy) e cifrari autenticati moderni.

### 4. Come gestisce lo script sia domini semplici sia URL complete con prefisso di protocollo?
L'utilità elabora l'argomento fornito tramite il modulo standard `urllib.parse`. Se la destinazione include `http://` o `https://`, isola con precisione l'hostname per avviare la connessione socket sulla porta 443.

### 5. Questo watchdog può essere integrato in pipeline di CI/CD e monitoraggi periodici (cron)?
Sì, `watchdog.py` funziona nativamente su ambienti Linux, macOS e Windows senza alcuna dipendenza esterna. Il suo comportamento deterministico lo rende ideale per l'inserimento in job cron, GitHub Actions e verifiche automatiche di uptime.

***

## 5. Strumenti Diagnostici Online (Italia)

Verifica la sicurezza perimetrale delle tue applicazioni web tramite gli strumenti ufficiali TAVARDT:

* [Validatore Consent Mode v2 e GDPR](https://tools.tavardt.com/it/validatore-consent-mode-v2-gdpr/)
* [Verifica Protocollo DMARC ed Email](https://tools.tavardt.com/it/verifica-dmarc-email/)
* [Scansione File e Segreti Esposti](https://tools.tavardt.com/it/scansione-file-esposti/)
* [Suite Diagnostica Completa Italia](https://tools.tavardt.com/it/)

***

## 6. Consulenza e Blindaggio Crittografico in Italia

Per audit crittografici specialistici, adeguamento al GDPR e ciberdifesa perimetrale a Roma, Milano o Torino, contatta il team di **TAVARDT**.

***

## Link Utili

* **Portale Aziendale:** https://ag.tavardt.com/it/
* **Suite di Diagnostica:** https://tools.tavardt.com/it/
* **Contatto Tecnico:** https://ag.tavardt.com/it/contatto/

***

### Licenza
Rilasciato sotto licenza MIT.

***

© 2026 TAVARDT. Tutti i diritti riservati.
