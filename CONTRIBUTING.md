# Contribuer au site de RECARN-UQAM

Merci de votre intérêt ! Ce guide explique, étape par étape, comment proposer une modification au site [www.recarn-uqam.org](https://www.recarn-uqam.org). Aucune expérience préalable de GitHub n'est nécessaire.

Le site est construit avec [Jekyll](https://jekyllrb.com) et publié par GitHub Pages : chaque page est un fichier `.md` à la racine du dépôt, et le site est reconstruit automatiquement après chaque fusion dans `main`.

## Avant de commencer

1. Choisissez une [issue ouverte](https://github.com/RECARN-UQAM/recarn-uqam.github.io/issues). Celles marquées `good first issue` sont idéales pour débuter.
2. Laissez un commentaire sur l'issue pour indiquer que vous la prenez, en mentionnant la personne qui l'a créée avec `@` suivi de son identifiant GitHub (il figure en haut de l'issue, à côté de « opened »). La mention lui envoie une notification.
3. Attendez qu'on vous assigne l'issue avant de commencer. Cela évite que deux personnes travaillent sur la même chose.

Commentaire à copier, en remplaçant `<auteur-de-l-issue>` :

```
@<auteur-de-l-issue> Bonjour, j'aimerais me charger de cette issue. Pouvez-vous me l'assigner ?
```

Une issue = une branche = une pull request.

## Étapes

### 1. Forker le dépôt

Sur la page du dépôt, cliquez sur **Fork**. Une copie du dépôt est créée sur votre compte GitHub.

### 2. Cloner votre fork

```bash
git clone https://github.com/<votre-compte>/recarn-uqam.github.io.git
cd recarn-uqam.github.io
```

Ajoutez aussi le dépôt d'origine, pour pouvoir récupérer les dernières modifications :

```bash
git remote add upstream https://github.com/RECARN-UQAM/recarn-uqam.github.io.git
```

Enfin, activez le hook qui vérifie vos messages de commit (voir [Messages de commit](#messages-de-commit)) :

```bash
git config core.hooksPath .githooks
```

### 3. Créer une branche

Partez toujours de la dernière version de `main` :

```bash
git fetch upstream
git checkout -b feat/ma-modification upstream/main
```

Nommez la branche d'après le type de modification et son sujet : `feat/page-404`, `docs/contributing`, `fix/lien-casse`…

### 4. Faire la modification

Modifiez les fichiers concernés. Pour voir le résultat avant de l'envoyer, consultez [Prévisualiser le site en local](#prévisualiser-le-site-en-local).

### 5. Signer le commit

Tous les commits doivent être signés avec l'option `-s` :

```bash
git add <fichiers modifiés>
git commit -s -m "feat: ajouter une page 404 personnalisée"
```

L'option `-s` ajoute à la fin du message une ligne `Signed-off-by: Votre Nom <votre@courriel>`, par laquelle vous certifiez avoir le droit de proposer cette contribution.

Vous avez oublié `-s` ? Corrigez le dernier commit :

```bash
git commit --amend -s --no-edit
```

### 6. Pousser sur votre fork

```bash
git push -u origin feat/ma-modification
```

### 7. Ouvrir une pull request

Sur GitHub, ouvrez une pull request de votre branche (sur votre fork) vers **`RECARN-UQAM:main`**. La description est pré-remplie par un modèle : complétez ce que vous avez changé et comment vous l'avez vérifié.

### 8. Lier l'issue

Le modèle de pull request se termine par `Closes #`. Ajoutez-y le numéro de l'issue :

```
Closes #38
```

À la fusion de la pull request, GitHub fermera automatiquement l'issue. Pour que cela fonctionne :

- la phrase doit être dans la **description** de la pull request, pas dans son titre ;
- un seul numéro par phrase : pour plusieurs issues, écrivez `Closes #38` et `Closes #39` sur deux lignes ;
- `Fixes #38` et `Resolves #38` fonctionnent aussi.

## Messages de commit

Un message de commit se compose d'un titre, d'une ligne vide, puis d'un corps facultatif :

```
docs: ajouter le guide de contribution

Explique les étapes pour contribuer : fork, branche, commit signé,
pull request et fermeture automatique de l'issue.

Closes #38

Signed-off-by: Votre Nom <votre@courriel>
```

Règles :

- le titre commence par un **type** suivi de `: ` (voir le tableau), en minuscules ;
- le titre fait **50 caractères au plus** ;
- une **ligne vide** sépare le titre du corps ;
- chaque ligne du corps fait **72 caractères au plus** (les liens et la ligne `Signed-off-by` ne comptent pas) ; le corps explique le pourquoi plutôt que le comment ;
- le commit est **signé** avec `git commit -s`.

Types autorisés :

| Préfixe | Usage | Exemple |
|---|---|---|
| `feat:` | nouvelle fonctionnalité ou nouvelle page | `feat: ajouter robots.txt` |
| `fix:` | correction d'une erreur | `fix: corriger le lien d'inscription` |
| `docs:` | documentation (README, guides) | `docs: ajouter le guide de contribution` |
| `content:` | texte des pages du site | `content: mettre à jour les membres de l'équipe` |
| `style:` | apparence (CSS, mise en page) | `style: agrandir les photos de l'équipe` |
| `ci:` | workflows GitHub Actions | `ci: corriger le notificateur Matrix` |
| `refactor:` | réorganisation sans changement visible | `refactor: regrouper les icônes` |
| `chore:` | maintenance (dépendances, configuration) | `chore: mettre à jour le Gemfile` |
| `test:` | tests et vérifications | `test: vérifier les liens du site` |
| `revert:` | annulation d'un commit précédent | `revert: retirer la bannière` |

Le hook `.githooks/commit-msg` vérifie ces règles à chaque commit et refuse le commit s'il ne les respecte pas, en indiquant quoi corriger. Activez-le une fois par clone avec `git config core.hooksPath .githooks`. Les messages générés par git (`Merge …`, `Revert …`) ne sont pas vérifiés.

## Après une revue

Si on vous demande des changements, modifiez les fichiers sur la même branche, puis faites un nouveau commit signé et poussez-le : la pull request se met à jour toute seule.

Si vous avez modifié un commit déjà poussé (avec `--amend`), poussez avec :

```bash
git push --force-with-lease
```

Si `main` a avancé pendant ce temps, mettez votre branche à jour :

```bash
git fetch upstream
git rebase upstream/main
git push --force-with-lease
```

## Prévisualiser le site en local

C'est facultatif, mais utile pour vérifier une modification de mise en page. Il faut Ruby 3.x (Ruby 3.3 recommandé ; Jekyll 3 ne fonctionne pas avec Ruby 3.4 et plus) et Bundler.

```bash
bundle install
bundle exec jekyll serve --livereload
```

Le site est alors disponible sur <http://localhost:4000>.

- Sur macOS, si la construction échoue avec `Invalid US-ASCII character`, lancez la commande précédée de `LANG=en_US.UTF-8`.
- Si les pages s'affichent sans mise en forme, c'est que le gem `github-pages` applique son thème par défaut en local. Créez un fichier `_config.local.yml` contenant `theme: null` (ne le commitez pas), puis lancez :

  ```bash
  bundle exec jekyll serve --config _config.yml,_config.local.yml --livereload
  ```

## Une question ?

Posez-la directement dans l'issue concernée, ou écrivez-nous à [support@recarn-uqam.org](mailto:support@recarn-uqam.org).
