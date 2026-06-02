# Kiro Releases

Ce dépôt public héberge uniquement les binaires de Kiro. Le code source reste dans le dépôt privé.

## Comment publier une nouvelle version

### 1. Builder et signer le .exe

Dans le dépôt source (`Kiro`), configure la variable d'environnement avec ta clé privée :

```powershell
$env:TAURI_SIGNING_PRIVATE_KEY = (Get-Content "C:<chemin-vers-ta-clé>" -Raw)
npx tauri build
```

Tauri génère automatiquement le `.nsis.zip` signé dans :
```
src-tauri/target/release/bundle/nsis/
```

### 2. Créer une GitHub Release

1. Va sur https://github.com/Killian8-arch/Kiro_releases/releases/new
2. Tag : `v0.6.0` (adapte le numéro)
3. Upload les fichiers depuis `src-tauri/target/release/bundle/nsis/` :
   - `kiro_0.6.0_x64-setup.exe`
   - `kiro_0.6.0_x64-setup.nsis.zip`
   - `kiro_0.6.0_x64-setup.nsis.zip.sig` ← la signature

### 3. Mettre à jour latest.json

Édite `latest.json` avec :
- Le nouveau numéro de version
- Le contenu du fichier `.sig` dans `"signature"`
- L'URL du `.nsis.zip` sur GitHub Releases

```json
{
  "version": "0.6.0",
  "notes": "Description des changements...",
  "pub_date": "2026-06-01T00:00:00Z",
  "platforms": {
    "windows-x86_64": {
      "signature": "CONTENU_DU_FICHIER_.SIG",
      "url": "https://github.com/Killian8-arch/Kiro_releases/releases/download/v0.6.0/kiro_0.6.0_x64-setup.nsis.zip"
    }
  }
}
```

### 4. Committer latest.json

```powershell
git add latest.json
git commit -m "Release v0.6.0"
git push
```

Kiro détectera automatiquement la nouvelle version au prochain démarrage.
