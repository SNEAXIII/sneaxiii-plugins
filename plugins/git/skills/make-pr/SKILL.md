---
name: make-pr
description: >
  Ouvre la PR d'une branche de feature sur la branche par défaut : commit, push, `gh pr create`.
  Jamais pour promouvoir la branche par défaut vers une branche de release ou de staging.
user-invocable: true
---

# Make PR

Workflow complet pour ouvrir une PR sur la branche par défaut depuis la branche courante.
`<base>` = `gh repo view --json defaultBranchRef --jq .defaultBranchRef.name`.

## Steps

1. **Branche** — sur `<base>`, créer d'abord une branche de feature (`<type>/<slug>`) :
   le skill commit s'arrête sur la branche par défaut.

2. **Commit et push** via le skill `git:commit` avec l'argument `push` — jamais un commit
   à la main. Il découpe les changements en commits par responsabilité, puis pousse la
   branche (`git push -u origin HEAD`) une fois tous les commits faits.

3. **Créer la PR** via le CLI `gh` :
   ```bash
   gh pr create --base <base> --head <branch> --title "<titre>" --body ""
   ```
   - Body vide ou minimal — pas de sections Summary / Test plan.
   - Si `gh` répond `HTTP 401: Bad credentials`, demander à l'utilisateur de lancer
     `gh auth login` puis relancer la commande.

## Args

Si des args sont fournis (ex: `/make-pr fix login redirect`), les utiliser comme titre de PR.  
Sinon, dériver le titre depuis les commits de la branche (`git log <base>..HEAD --oneline`).

## Notes

- Si rien n'est à commiter et que la branche n'a aucun commit d'avance sur `<base>`,
  signaler à l'utilisateur et s'arrêter.
- Ne jamais forcer un push (`--force`).
- Pas de lint ni de format à la main : les hooks pre-commit du repo s'en chargent au commit.
