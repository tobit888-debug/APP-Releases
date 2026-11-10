# APP-Releases

Öffentliches Update-Repo für meine privaten Android-Apps. Die Apps prüfen beim Start (mit Internet) die `version.json` ihres Ordners und bieten ein Update an.

## Struktur
```
<app>/version.json      {"versionCode": 24, "versionName": "2.4", "apk": "<Datei>.apk", "notes": {"de": "...", "en": "..."}}
<app>/<Datei>.apk
```
| Ordner | App | APK |
|---|---|---|
| `herbs/` | Heilkräuter (de.tobit.heilkraeuter) | `Heilkraeuter.apk` |

## Neue Version veröffentlichen (Herbs)
1. `versionCode`/`versionName` in `android/app/build.gradle` und `www/version.js` erhöhen, APK bauen → `Final/Heilkraeuter.apk`.
2. Im Herbs-Repo: `scripts/release_to_apps_repo.sh "Hinweistext"` (kopiert APK + schreibt `version.json` hierher und pusht).

## Neue App an das System anschließen
1. Android: `UpdaterPlugin.java` (Capacitor-Plugin „Updater“, lädt die APK und startet den Installer), in `MainActivity` mit `registerPlugin(UpdaterPlugin.class)`, Permission `REQUEST_INSTALL_PACKAGES`, FileProvider (cache-path) – siehe Herbs-Repo `android/app/src/main/java/de/tobit/heilkraeuter/`.
2. Web: Update-Prüfung wie in `www/app.js` (Abschnitt „Update-Prüfung“): `UPD.app` auf den neuen Ordner setzen, Versionsnummer in `www/version.js`.
3. Hier neuen Ordner `<app>/` mit `version.json` + APK anlegen und diese Tabelle ergänzen.

## Hinweise
- Updates funktionieren nur, wenn die neue APK mit **demselben Schlüssel** signiert ist wie die installierte; `versionCode` muss steigen.
- Das Repo ist öffentlich (nötig für die Prüfung ohne Anmeldung): keine geheimen oder fremd lizenzierten Inhalte ablegen.
