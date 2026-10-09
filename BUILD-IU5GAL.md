# Greenlight — build di IU5GAL (`iu5gal-build`)

🇬🇧 [English version](BUILD-IU5GAL.en.md)

Questo branch è la **mia build personale** di [Greenlight](https://github.com/unknownskl/greenlight) 2.4.2:
`main-v2` dell'originale più le correzioni che ho proposto al maintainer e che non sono ancora state integrate.
**Non è un fork separato e non si propone upstream**: ogni modifica nasce in un branch dedicato, diventa una
pull request verso `unknownskl/greenlight` e viene poi fusa qui.

## Cosa contiene

| PR | Branch | Contenuto |
|----|--------|-----------|
| [#1705](https://github.com/unknownskl/greenlight/pull/1705) | `chore/remove-dsstore` | `.DS_Store` non più tracciato |
| [#1706](https://github.com/unknownskl/greenlight/pull/1706) | `feature/italian-language` | Traduzione italiana (it-IT) |
| [#1707](https://github.com/unknownskl/greenlight/pull/1707) | `fix/webui-express5` | La Web UI non partiva con Express 5 |
| [#1709](https://github.com/unknownskl/greenlight/pull/1709) | `fix/i18n-keys` | Chiavi di traduzione allineate tra codice e file di lingua |
| [#1717](https://github.com/unknownskl/greenlight/pull/1717) | `feature/languages` | Francese, portoghese brasiliano, turco, cinese semplificato, giapponese, coreano; lo spagnolo diventa `es-ES.json` |
| [#1718](https://github.com/unknownskl/greenlight/pull/1718) | `fix/settings-polish` | Rifiniture alle impostazioni: ripristino dei default, porta Web UI validata, descrizioni |
| [#1719](https://github.com/unknownskl/greenlight/pull/1719) | `feature/i18n-system-language` | Lingua salvata applicata subito, finestre native tradotte, lingua di sistema al primo avvio |
| [#1720](https://github.com/unknownskl/greenlight/pull/1720) | `fix/macos-controller` | Controller e vibrazione su macOS dalla 2.4.2 |
| [#1721](https://github.com/unknownskl/greenlight/pull/1721) | `fix/ui-polish` | Voce di menu evidenziata in base alla pagina aperta; conto alla rovescia della coda corretto, con barra di avanzamento |
| [#1682](https://github.com/unknownskl/greenlight/pull/1682) | (di vishalrao8) | Numerazione dei controller in Impostazioni → Input. Fusa solo qui |

Lo stato aggiornato di ogni PR è nella pagina della PR stessa: quando il maintainer ne integra una, qui
diventa semplicemente codice già presente in `main-v2`.

## Compilare e avviare

Servono Node 24 (CI) o 22 (`.nvmrc`) e Yarn 1.22.

```bash
git switch iu5gal-build
yarn
yarn desktop build --no-pack      # compila senza creare il pacchetto
yarn desktop electron . --user-data-dir="$HOME/greenlight-test-profile"
```

- In sviluppo: `yarn desktop dev --electron-options="--user-data-dir=$HOME/greenlight-test-profile"`
  (percorso senza spazi).
- **Usa un profilo di prova**: senza `--user-data-dir` le impostazioni sono quelle dell'app installata.
  Una cartella nuova equivale a un primo avvio. Accesso in sviluppo e in produzione sono distinti.
- `yarn desktop build` (senza `--no-pack`) crea il `.dmg` in `packages/desktop/dist`; con questa build
  non è stato verificato.
- Non ci sono test automatici: `yarn desktop lint` e `yarn desktop build --no-pack` sono il minimo.

## Limiti noti su macOS (non risolti)

- **Pulsante Xbox**: macOS lo intercetta (apre l'app Giochi) e, con le scorciatoie del controller disattivate,
  non arriva all'app. Premere **View + Menu** insieme funziona come pulsante Xbox.
- **Vibrazione via cavo USB-C**: il controller funziona ma non vibra (`playEffect()` risponde `not-supported`
  su macOS). Via Bluetooth vibra, ma in modo debole: la console invia valori bassi e il controller espone
  solo due motori.
- **Due controller**: il secondo è annunciato alla console come controller 2 e gioca solo nei titoli
  che prevedono un secondo giocatore.
- Il "Fixes" della #1720 per #1704 copre il controller non rilevato in 2.4.2; il caso 2.4.1 su macOS 27
  potrebbe essere un problema a parte.

## Release

Le release le compila GitHub Actions su richiesta, non in automatico:

```bash
gh workflow run build-iu5gal.yml --repo iu5gal/greenlight --ref iu5gal-build
```

Compila macOS, Linux e Windows e pubblica una release chiamata `v<versione>-build-iu5gal.<AAAAMMGG>`
(per esempio `v2.4.2-build-iu5gal.20261009`) con gli stessi file di quelle ufficiali, tranne il Flatpak.
Se la release di quel giorno esiste già non fa nulla. Le build non sono firmate: su macOS la prima volta
si apre con clic destro → Apri. L'app controlla gli aggiornamenti nelle release di questo fork.

## Tenere aggiornata la build

```bash
git switch iu5gal-build
git merge --no-ff origin/<nuovo-branch>   # ogni nuova miglioria
git fetch upstream && git merge upstream/main-v2   # ogni tanto, per allinearsi all'originale
git push
```

Meglio da riga di comando che dal sito GitHub: i merge con rinomine di file (lo spagnolo) vanno in
conflitto sul web. Mai aprire una pull request verso upstream **da** questo branch.

## Licenza e avvertenza

Greenlight è software libero (vedi [LICENSE](LICENSE)). *Greenlight non è affiliato a Microsoft, Xbox o
Moonlight; tutti i diritti e i marchi appartengono ai rispettivi proprietari.*
