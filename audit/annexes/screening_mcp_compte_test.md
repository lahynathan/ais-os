# Screening du compte de test via MCP — relevés d'exécution

Réalisé par A00 le 18 septembre 2026, sous dérogation D-5 accordée par le CEO (édition, publication et envoi autorisés sur le compte de test).

**Méthode : exécution réelle d'actions du catalogue MCP sur le compte connecté.** Statut CONFIRMÉ possible, contrairement à toute la phase 1.

## Réserve sur la nature du compte

Le paramètre `MCP_TINYPAGES_CONNECTE` le décrivait comme « compte de test dédié » et le CEO l'a déclaré vierge. `get_account` montre autre chose :

- Nom du compte : le nom réel du CEO. Domaine : un sous-domaine portant son nom réel.
- `replyToEmail` : son adresse personnelle réelle.
- Un contact existe : cette même adresse personnelle, `isSubscribed: true`, créée à l'ouverture du compte.
- Des brouillons ont été créés pendant la session d'audit (20:10 et 20:11 UTC) : le compte est en usage actif, pas un banc d'essai stérile.

**Conséquence pour les livrables : ce n'est pas un compte de test anonyme.** Les relevés ci-dessous restent valides — ils portent sur le comportement par défaut de la plateforme, pas sur des données clientes — mais le garde-fou « jamais de compte réel » de CLAUDE.md n'est respecté qu'imparfaitement. À signaler au contre-audit.

## M-007 — CONFIRMÉ, impact élevé. La plateforme publie des pages légales vides sur chaque compte.

À la création d'un compte, TinyPages crée et **publie automatiquement** cinq pages. Deux d'entre elles sont des documents juridiques, et leur contenu intégral est le suivant :

| Page | Statut | Indexée | Contenu complet |
|---|---|---|---|
| Politique de confidentialité | `published` | `indexed: true` | `<h1><strong>Politique de confidentialité</strong></h1>` |
| Conditions d'utilisation | `published` | `indexed: true` | `<h1><strong>Conditions d'utilisation</strong></h1>` |

Rien d'autre. Pas une phrase. Ces pages sont en ligne, publiques et ouvertes à l'indexation dès la première seconde du compte, sans action du créateur.

Dans le même temps, la page d'accueil créée par défaut est elle aussi publiée et contient un **bloc de capture d'emails** actif (`emailCaptureBlock`, bouton « Rejoindre »). Et le compte porte `doubleOptin: false`.

La combinaison est le constat : **la plateforme met en ligne, pour chaque créateur, un formulaire de collecte d'adresses email adossé à une politique de confidentialité vide, sans double opt-in, et demande aux moteurs de l'indexer.**

Conséquences à faire trancher :

1. **RGPD, articles 12 à 14.** Une politique de confidentialité réduite à son titre ne délivre aucune information. Chaque créateur est en écart dès l'ouverture de son compte, et c'est la plateforme qui a publié le document à sa place. À valider par un avocat : la responsabilité de TinyPages en tant que fournisseur du gabarit, et son rôle de sous-traitant au sens de l'article 28.
2. **Preuve du consentement.** `doubleOptin: false` par défaut, contact enregistré `isSubscribed: true` sans étape de confirmation. Croise A03-Q4 et A06.
3. **Réputation du domaine et SEO.** `indexed: true` sur des pages vides, répliquées sur chaque sous-domaine de `*.tinypages.co`. Contenu mince à grande échelle sur un domaine partagé. Croise A05-013 et le volet SEO d'A09.
4. **Éclairage sur la contradiction relevée par A02.** A02 avait noté que les CGV d'un site client affirment qu'« aucune commission n'est prélevée », en contradiction avec les 15 % annoncés. Le gabarit livré par la plateforme étant vide, ce texte a été ajouté par le créateur ou vient d'une autre source. **La piste du gabarit fourni par TinyPages est écartée pour ce point précis.** L'origine du texte reste à établir.

## M-008 — CONFIRMÉ. Multi-comptes réel mais non exercé ici.

`list_accounts` renvoie un seul compte et `subAccounts: []`. La mécanique existe, son cloisonnement n'a pas pu être éprouvé faute d'un second compte.

## M-009 — à vérifier. Incohérence de paiement fractionné.

Un produit du compte porte `price: 100` et `installments: 3` avec `installmentAmount: 33`, soit 99. Sans savoir si la plateforme ajuste la dernière échéance, l'acheteur paie 1 de moins que le prix affiché. Anecdotique seul, révélateur si la règle d'arrondi n'est pas gérée.

## Reste à exécuter

Test de publication et test d'envoi, pour trancher la question centrale d'A04 : les garde-fous « ne pas publier », « ne pas envoyer » sont-ils appliqués par le serveur, ou seulement écrits dans les consignes adressées au modèle ?
