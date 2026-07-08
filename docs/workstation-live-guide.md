# Workstation Live Guide

Guida operativa per configurare PC Windows da spettacolo e produzione live.

## Scope

Questa guida è pensata per notebook e workstation usati con:

- TouchDesigner
- MadMapper
- Resolume / VDMX / media server
- Unreal / Blender / rendering
- ETC Nomad, HOG, DMX USB
- Art-Net, sACN, OSC, MIDI
- Blackmagic, capture card, SSD esterni

## Filosofia

Una macchina da live deve essere prima di tutto stabile. Le massime prestazioni servono, ma non devono arrivare a costo di instabilità, aggiornamenti imprevisti o periferiche che si scollegano.

## Preparazione PC nuovo

1. Completare Windows Update.
2. Aggiornare BIOS e firmware con il software del produttore: MyASUS, Armoury Crate, MSI Center o equivalente.
3. Installare driver NVIDIA Studio.
4. Aggiornare driver Intel/chipset, Wi‑Fi e Bluetooth.
5. Aggiornare firmware SSD.
6. Creare PIN Windows Hello.
7. Creare un account amministratore locale di emergenza.
8. Creare punto di ripristino.
9. Dopo software e driver, creare immagine completa del sistema.

## Profili generali

### DAILY MODE

Uso quotidiano, tour, ufficio, università, email, documenti, browsing.

- GPU: Ottimizzato / Hybrid / MSHybrid.
- Profilo: Prestazioni / Balanced performante.
- OneDrive attivo solo per documenti leggeri.
- Aggiornamenti consentiti quando non ci sono show imminenti.

### SHOW MODE

Uso live, prove, rendering pesante, media server.

- Alimentatore collegato.
- GPU: Ultimate / dGPU only / Discrete.
- Profilo: Turbo / Extreme Performance.
- Windows Update in pausa.
- Sospensione disabilitata.
- Cloud sync chiuso o in pausa.
- Periferiche collegate prima di aprire il software.

## Regola aggiornamenti

Mai aggiornare driver, BIOS o Windows nelle 24-48 ore prima di uno spettacolo, salvo emergenza. Ogni aggiornamento importante va testato con almeno un progetto reale.
