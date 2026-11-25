# Watchdog Certificati TLS/SSL

[![Ambito di Verifica Perimetrale](https://img.shields.io/badge/TAVARDT-Diagnostica%20Tecnica-5b3cf5.svg)](https://tools.tavardt.com/it/analisi-header-sicurezza/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Monitoraggio automatizzato della scadenza dei certificati SSL/TLS, integrità della catena crittografica e sicurezza in Italia.

***

## 1. Ambito di Verifica Perimetrale

Questa utilità verifica la validità temporale, i protocolli di cifratura e l'ente di certificazione dei server web:

* **Avviso Preventivo di Scadenza:** Calcola i giorni residui prima della scadenza del certificato con soglie configurabili.
* **Audit di Cifrari e Protocolli:** Verifica il supporto a TLS 1.3 e la disattivazione di standard obsoleti.
* **Convalida Autorità Emittente (CA):** Ispeziona l'ente certificatore e l'integrità dei Subject Alternative Names (SAN).
* **Zero Dipendenze Esterne:** Costruito interamente sui moduli standard ssl e socket di Python.

***

## 2. Guida Rapida all'Esecuzione

Esegui lo strumento direttamente tramite Python senza necessità di librerie esterne:

```bash
python watchdog.py esempio.it
```

***

## 3. Strumenti Diagnostici Online (Italia)

Verifica la sicurezza perimetrale delle tue applicazioni web tramite gli strumenti ufficiali TAVARDT:

* [Validatore Consent Mode v2 e GDPR](https://tools.tavardt.com/it/validatore-consent-mode-v2-gdpr/)
* [Verifica Protocollo DMARC ed Email](https://tools.tavardt.com/it/verifica-dmarc-email/)
* [Scansione File e Segreti Esposti](https://tools.tavardt.com/it/scansione-file-esposti/)
* [Suite Diagnostica Completa Italia](https://tools.tavardt.com/it/)

***

## 4. Consulenza e Ciberdifesa Aziendale

Per l'adeguamento dei protocolli crittografici, automazione di rinnovo e sicurezza informatica in Italia, contatta **TAVARDT**.

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
