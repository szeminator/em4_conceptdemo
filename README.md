# EM4 Konzept · GitHub Pages Deploy (passwortgeschützt)

## Passwort


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
