# Nuvio Up

Application **LG webOS** qui installe et met à jour **Nuvio TV** directement depuis la télévision, sans repasser par un ordinateur. Nuvio n'étant pas sur le LG Content Store, Nuvio Up utilise le mode développeur de la TV.

- Installe, met à jour et désinstalle Nuvio TV (barres de téléchargement et d'installation en direct)
- Récupère et mémorise la clé du mode développeur : à faire une seule fois
- Prolonge automatiquement la session développeur
- Affiche les nouveautés de chaque version de Nuvio, en français
- Se met à jour toute seule depuis les releases de ce dépôt

> Projet communautaire non officiel, sans lien avec l'équipe Nuvio ni avec LG.

---

## Ce qu'il faut

- Une TV **LG webOS**. Un avertissement s'affiche dans l'app pour les versions inférieures à webOS 6 (torrent/P2P et plugins fonctionnent, avec des quotas de ressources limités).
- Un compte développeur LG (gratuit) : <https://webostv.developer.lge.com>
- Un ordinateur **Windows** ou **Mac**, sur le **même réseau** que la TV (utilisé une seule fois, pour installer Nuvio Up)
- [Node.js](https://nodejs.org) (version LTS)

---

## 1. Activer le mode développeur sur la TV

1. Sur la TV, ouvre le **LG Content Store** et installe l'application **Developer Mode**.
2. Ouvre-la et connecte-toi avec ton compte développeur LG.
3. Active **Dev Mode Status** puis **Key Server**. La TV redémarre.
4. Rouvre **Developer Mode** et note :
   - l'**adresse IP** de la TV
   - la **passphrase** (code à 6 caractères)

La session développeur dure un temps limité. Nuvio Up la prolonge ensuite automatiquement.

---

## 2. Installer les outils LG sur ton ordinateur

### Windows

Ouvre **PowerShell** :

```powershell
npm install -g @webos-tools/cli
ares --version
```

Si PowerShell refuse d'exécuter le script (l'exécution de scripts est désactivée) :

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

### Mac

Ouvre le **Terminal** :

```bash
npm install -g @webos-tools/cli
ares --version
```

Si npm affiche une erreur de permissions (EACCES) : `sudo npm install -g @webos-tools/cli`

---

## 3. Déclarer la TV

Remplace `192.168.X.X` par l'adresse IP de ta TV. Les commandes sont les mêmes sur Windows (PowerShell) et Mac (Terminal).

```bash
ares-setup-device --add tv -i "host=192.168.X.X" -i "port=9922" -i "username=prisoner" -i "default=true"
ares-novacom --device tv --getkey
```

La deuxième commande demande la **passphrase** de l'étape 1. Vérifie ensuite la connexion :

```bash
ares-device-info --device tv
```

---

## 4. Installer Nuvio Up

1. Télécharge le fichier **`com.nuvioup_X.Y.Z_all.ipk`** le plus récent dans les [**Releases**](../../releases/latest).
2. Dans le terminal, place-toi dans le dossier de téléchargement puis installe.

### Windows (PowerShell)

```powershell
cd $HOME\Downloads
ares-install --device tv .\com.nuvioup_2.9.0_all.ipk
```

### Mac (Terminal)

```bash
cd ~/Downloads
ares-install --device tv ./com.nuvioup_2.9.0_all.ipk
```

Remplace `2.9.0` par la version téléchargée. L'app **Nuvio Up** apparaît dans la liste des applications de la TV.

---

## 5. Premier lancement, sur la TV

1. Ouvre **Nuvio Up**. Les **Réglages** s'affichent au premier lancement.
2. Saisis la **passphrase** avec la télécommande (chiffres) ou le clavier à l'écran.
3. Appuie sur **Capturer la clé**, puis suis les indications : ouvre **Developer Mode** (bouton orange), active **Key Server**, attends une dizaine de secondes et reviens dans Nuvio Up. Le message « Clé capturée » s'affiche.
4. Appuie sur **Installer Nuvio**. Le téléchargement et l'installation se font depuis la TV.

L'ordinateur n'est plus nécessaire ensuite.

---

## Utilisation

| Bouton | Rôle |
|---|---|
| **Vérifier** | Compare la version installée de Nuvio avec la dernière |
| **Mettre à jour / Installer Nuvio** | Télécharge et installe la dernière version |
| **Ouvrir Nuvio** | Lance Nuvio |
| **Nouveautés** | Notes de chaque version de Nuvio, en français |
| **Réglages** | Passphrase, clé, désinstallation |
| **Mettre à jour Nuvio Up** | Apparaît quand une nouvelle version de Nuvio Up est publiée ici |

---

## Dépannage

| Problème | Solution |
|---|---|
| Commande ares introuvable | Ferme et rouvre le terminal. Vérifie que Node.js est installé (`node -v`). |
| La TV ne répond pas | TV et ordinateur sur le même réseau. Session développeur active (app Developer Mode ouverte, Key Server activé). |
| « Mode dév injoignable » dans Nuvio Up | Appuie sur **Ouvrir Dev Mode** et vérifie que le mode développeur est actif. |
| La clé n'est pas capturée | Dans **Réglages**, appuie sur **Recapturer la clé**. Garde l'app Developer Mode ouverte avec Key Server activé. |
| La première désinstallation échoue | Recommence : la deuxième tentative fonctionne. |
| Ancienne icône après réinstallation | Redémarre la TV : le lanceur garde l'icône en cache. |

---

## Communauté

- Site de la communauté francophone : **StremioFr** — <https://stremiofr.me/>
- Discord : **etherial_**
