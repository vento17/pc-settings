# Windows

## Account e accesso

- Usare un account principale con nome riconoscibile.
- Configurare PIN Windows Hello.
- Creare un account amministratore locale di emergenza, con password diversa e conservata offline.

## Windows Update

Uso normale:

- Tenere Windows aggiornato.
- Riavviare e verificare stabilità dopo aggiornamenti importanti.

Prima di un live:

- Mettere Windows Update in pausa per almeno una settimana.
- Non installare update driver/BIOS nelle 24-48 ore prima dello show.

## Alimentazione

Con alimentatore collegato:

- Modalità alimentazione: Prestazioni migliori.
- Sospensione: Mai.
- Spegnimento schermo: a scelta; per show meglio Mai o tempo lungo.
- PCI Express Link State Power Management: Disattivato.
- Disco: non spegnere quando collegato alla rete elettrica.

## Avvio automatico

Disattivare dall'avvio:

- Teams
- Xbox
- Phone Link / Collegamento al telefono
- Dispositivi mobili, se non usati
- Browser Assistant
- launcher e updater non necessari

Lasciare attivi:

- Sicurezza Windows
- driver audio
- servizi ASUS/MSI necessari a ventole, profili e GPU
- driver NVIDIA/Intel essenziali

## Defender

Non disattivare Defender completamente.

Aggiungere esclusioni per:

- cartelle progetti TouchDesigner / MadMapper
- media show
- cache/render
- SSD esterni usati per contenuti 4K

Esempi:

```text
D:\SHOWS
E:\MEDIA
C:\TD_PROJECTS
```

## OneDrive

Usare OneDrive per documenti leggeri, preventivi, PDF, note.

Non usare OneDrive per:

- media show
- cache
- video 4K
- progetti live pesanti

Prima di uno show: mettere OneDrive in pausa o chiuderlo.
