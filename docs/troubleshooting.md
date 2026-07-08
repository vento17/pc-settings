# Troubleshooting

## Windows vuole aggiornare prima dello show

1. Mettere in pausa Windows Update.
2. Riavviare solo se già richiesto e c'è tempo per test.
3. Non installare driver o BIOS nuovi prima dello show.
4. Se l'update è già partito, attendere completamento: non spegnere forzatamente salvo blocco reale.

## GPU non riconosciuta o prestazioni basse

1. Verificare alimentatore collegato.
2. Verificare profilo produttore: Turbo / Extreme Performance.
3. Verificare modalità GPU: Ultimate / Discrete / dGPU only.
4. Riavviare dopo cambio MUX.
5. Controllare driver NVIDIA Studio.
6. Controllare che l'app usi la GPU NVIDIA.

## TouchDesigner scende a 30 fps

Controllare:

- VSync e refresh monitor/proiettore.
- Display esterni a 60 Hz o refresh coerente.
- GPU in modalità Ultimate/dGPU.
- Power management NVIDIA su prestazioni massime.
- media su SSD veloce, non cloud.
- Defender esclusioni su media/cache/progetti.

## Art-Net/sACN non arriva

1. Controllare IP/subnet.
2. Disattivare Wi‑Fi se crea ambiguità di routing.
3. Verificare firewall/Defender.
4. Verificare che la scheda Ethernet non abbia risparmio energia attivo.
5. Verificare switch/cavi.
6. Controllare universe e indirizzamento 0-based/1-based.

## Periferica USB sparisce

1. Cambiare porta.
2. Evitare hub non alimentati.
3. Disattivare risparmio energia sugli USB Root Hub se disponibile.
4. Collegare periferiche prima di aprire software live.
5. Verificare cavo USB.
6. Per show importanti, usare alimentatore/hub USB industriale o affidabile.

## Black screen su proiettore/monitor

1. Windows + P e selezionare Estendi o Duplica.
2. Controllare refresh e risoluzione.
3. Controllare cavo/adattatore.
4. Riavviare software video dopo collegamento monitor.
5. Se possibile, usare sempre la stessa porta/adattatore per ogni show.
