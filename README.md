# Ahime Sales Assistant — Releases (Android)

Ce dépôt **public** sert uniquement à **construire et publier les APK Android** de l'application mobile *Ahime Sales Assistant*.
Le **code source reste privé** : il est récupéré au moment du build depuis le dépôt privé `nahim-salami/Ahime-Sales-Assistant` via une **clé de déploiement en lecture seule**.

## 📥 Télécharger l'application
Les APK sont publiés dans l'onglet **[Releases](../../releases)** :
- `...-release.apk` → version de production (à installer sur les téléphones des prospecteurs / sideload).
- `...-debug.apk` → version de débogage.

> Ces APK sont destinés au **sideload** (installation hors Play Store). Activez « Sources inconnues » sur le téléphone pour installer.

## ⚙️ Comment ça marche
Le workflow [`.github/workflows/build-apk.yml`](.github/workflows/build-apk.yml) :
1. Clone le code source du dépôt privé (clé SSH read-only, secret `SOURCE_REPO_SSH_KEY`).
2. Installe Flutter + JDK 17.
3. Construit les APK **debug** et **release** avec les paramètres injectés (`API_BASE_URL`, `AHIME_WHATSAPP`).
4. Téléverse les APK en *artifacts* et crée une **Release**.

## ▶️ Lancer un build
Onglet **Actions** → *Build Android APK (Ahime Sales)* → **Run workflow**, puis renseigner :
- **api_base_url** : l'URL de votre backend déployé (Vercel), ex. `https://votre-app.vercel.app`.
- **ahime_whatsapp** : numéro WhatsApp officiel (défaut `22952533792`).
- **source_ref** : branche/tag à construire (défaut `main`).
- **make_release** : publier une Release (défaut activé).

Ou via la CLI :
```bash
gh workflow run build-apk.yml -R nahim-salami/Ahime-Sales-Assistant-Releases \
  -f api_base_url=https://votre-app.vercel.app \
  -f ahime_whatsapp=22952533792 \
  -f source_ref=main
```

## 🔐 Secrets requis
| Secret | Rôle |
|--------|------|
| `SOURCE_REPO_SSH_KEY` | Clé privée de déploiement (read-only) du dépôt source. **Déjà configurée.** |

La clé publique correspondante est enregistrée comme *Deploy key* (lecture seule) sur le dépôt privé.

## 🔁 Déclenchement à distance (optionnel)
Le workflow accepte aussi un `repository_dispatch` de type `build-apk`, pour être déclenché automatiquement depuis le dépôt source après un push :
```bash
gh api repos/nahim-salami/Ahime-Sales-Assistant-Releases/dispatches \
  -f event_type=build-apk
```
