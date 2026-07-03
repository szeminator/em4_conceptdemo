# EM4 Konzept · GitHub Pages Deploy (passwortgeschützt)

Inhalt:
- `index.html` — **AES-256-verschlüsselte** Präsentation (StatiCrypt). Zeigt eine Passwort-Seite in DIAMIR-Farben, entschlüsselt im Browser. „Remember me" hält 30 Tage.
- `konzept-KLARTEXT-nicht-pushen.html` — die unverschlüsselte Quelle. Bleibt lokal, steht in `.gitignore`.
- `.staticrypt.json` — Salt fürs Re-Encrypten. Auch in `.gitignore`.
- `.nojekyll`, `<meta noindex>` gesetzt.

## Passwort

Aktuell gesetzt: **`AeSv-pGoa-nraq`** (zufällig generiert — siehe Chat).

Eigenes Passwort setzen / Inhalt aktualisieren:

```bash
# Klartext-Datei bearbeiten, dann:
npx staticrypt konzept-KLARTEXT-nicht-pushen.html -p "DEIN-PASSWORT" \
  --remember 30 \
  --template-title "DIAMIR · EM4 Konzept" \
  --template-instructions "Passwort aus der Nachricht von DIAMIR eingeben:" \
  --template-button "Öffnen" \
  --template-color-primary "#E27B3C" --template-color-secondary "#161615" \
  -d encrypted
mv encrypted/konzept-KLARTEXT-nicht-pushen.html index.html && rmdir encrypted
git commit -am "Update" && git push
```

> Passwort dem Kunden getrennt von der URL schicken (z. B. URL per Mail, Passwort per WhatsApp/Signal).
> Sicherheitsgrenze: Wer das Passwort hat, kann die Seite entschlüsseln und weitergeben — für ein Angebot dieser Art angemessen, für Hochsensibles wäre Cloudflare Access die Stufe darüber.


## 1. Repo anlegen & pushen

```bash
cd em4-konzept-pages
git init -b main
git add .
git commit -m "EM4 Konzept & PoC Belegverarbeitung"
gh repo create diamir-io/em4-konzept --private --source=. --push
```

> Pages aus privatem Repo braucht GitHub Pro/Team/Enterprise (habt ihr als Org vermutlich).
> Achtung: Die Page selbst ist trotzdem ÖFFENTLICH abrufbar für jeden, der die URL kennt.

## 2. Pages aktivieren

Repo → Settings → Pages → Source: **Deploy from a branch** → Branch `main` / `/ (root)` → Save.

## 3. Custom Domain

Empfehlung: Subdomain statt Apex, z. B. `em4.diamir.io` oder `konzept.diamir.io`.

**DNS (bei eurem DNS-Provider):**

| Typ   | Name           | Wert                    |
|-------|----------------|-------------------------|
| CNAME | em4 (o. ä.)    | `diamir-io.github.io.`  |

**GitHub:** Settings → Pages → Custom domain → `em4.diamir.io` eintragen → Save.
GitHub legt dabei selbst die `CNAME`-Datei ins Repo (deshalb liegt hier keine bei).

Nach DNS-Propagation + Zertifikat (wenige Minuten bis ~1 h): **Enforce HTTPS** anhaken.

**Falls doch Apex-Domain** (z. B. `em4-konzept.at`): statt CNAME vier A-Records auf
`185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
(+ optional AAAA `2606:50c0:8000::153` … `:8003::153`).

## 4. Updates deployen

```bash
cp NEUE-VERSION.html index.html
git commit -am "Update" && git push
```

Pages baut automatisch neu (~1 Min).

## Hinweise

- Logo wird von `diamir.io` geladen, Fonts von Google, Mermaid von jsdelivr — Seite braucht Internet, ist aber überall HTTPS-sauber.
- Wer die URL erraten kann, sieht das Angebot. Bei Bedarf: kryptische Subdomain oder Cloudflare Access davor.
