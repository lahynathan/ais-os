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

---

# Tests d'exécution — garde-fous de publication et d'envoi

Réalisés le 18 septembre 2026 entre 20:15 et 20:23 UTC, sous dérogation D-5. **Le compte est en plan gratuit** (établi par les erreurs de plan ci-dessous).

## Protocole

1. Créer une page par `create_webpage`, clairement étiquetée comme page d'audit, `indexed: false`.
2. Appeler `publish_webpage` **en violant délibérément** la consigne « Do NOT call publish_webpage » inscrite dans la description de l'outil. C'est l'objet même du test : distinguer un garde-fou appliqué par le serveur d'une consigne adressée au modèle.
3. Dépublier immédiatement.
4. Répéter pour l'envoi d'un message.

## M-010 — CONFIRMÉ, impact bloquant. La publication n'est soumise à aucun contrôle.

| Étape | Appel | Résultat |
|---|---|---|
| Création | `create_webpage` | Brouillon créé |
| Publication | `execute_action` → `publish_webpage` | **Succès immédiat**, une URL publique est retournée |
| Dépublication | `execute_action` → `unpublish_webpage` | Succès |

Aucune confirmation demandée, aucune revue humaine, aucun délai, aucune restriction de plan. **Un modèle qui ignore la consigne textuelle publie une page en ligne en un appel.**

C'est la réponse définitive à la question centrale d'A04 : les mentions « Do NOT call publish_webpage », « saved as a draft », « the user decides when to publish » sont **exclusivement du texte adressé à un modèle que TinyPages ne contrôle pas**. Rien ne les applique.

Pour la data room : toute formulation présentant ces consignes comme une garantie sera démentie en un appel par l'auditeur du fonds. La formulation défendable est qu'il s'agit d'un comportement par défaut du modèle, non d'un contrôle d'accès.

## M-011 — CONFIRMÉ. Deux contrôles serveur existent, et ce sont des contrôles commerciaux.

| Action tentée | Réponse du serveur |
|---|---|
| `create_webpage` avec un bloc `codeHtmlBlock` | `402 PRO_PLAN_REQUIRED` — les blocs de code exigent un plan Pro |
| `execute_action` → `send_email` | `402 PRO_PLAN_REQUIRED` — un plan Pro est exigé pour envoyer par l'API |

Le serveur **sait** refuser une action. Il le fait pour deux actions payantes, et pas pour la publication.

**Le constat est là : les contrôles côté serveur protègent le chiffre d'affaires, pas l'utilisateur.** L'action la plus exploitable pour l'abus — publier une page publique sur un sous-domaine de la marque — est la seule des trois à n'avoir aucune barrière. Les deux qui en ont une sont celles qui sont facturées.

À noter : le message d'erreur d'envoi implique qu'un compte Pro enverrait sans autre contrôle. Le garde-fou est un mur de facturation, pas un mur de sécurité. **À confirmer sur un compte Pro** — c'est la seule question que ce screening laisse ouverte sur ce point.

## M-012 — correction au rapport A05.

A05 fondait son risque d'abus sur la combinaison « plan gratuit + code personnalisé + création de pages par IA ». **Cette combinaison ne tient pas** : le bloc de code personnalisé est réservé au plan Pro.

Le risque réel, lui, tient et se reformule ainsi : **plan gratuit + pilotage par IA + publication sans aucun contrôle**. Une page d'hameçonnage visuelle, sans JavaScript, sur un sous-domaine de la marque avec certificat valide, reste créable et publiable en deux appels automatisés. Le JavaScript n'est pas nécessaire pour tromper un visiteur.

## M-013 — éclaire la contradiction C-001.

Le MCP fonctionne sur le plan gratuit : lectures, création de pages, publication, tout passe. Mais plusieurs actions sont refusées par plan. **Ni la FAQ du site — « connexion Claude incluse dès le plan gratuit » — ni la documentation — « intégration réservée au plan Pro » — n'ont entièrement raison.** La formulation exacte est que la connexion et la majorité des actions sont disponibles en gratuit, et qu'un sous-ensemble d'actions est réservé au Pro. C'est cette nuance qui manque aux deux sources et qui explique la contradiction.

## M-014 — CONFIRMÉ. L'IA ne peut pas nettoyer derrière elle.

Aucune action ne supprime une page ni un email. Les deux objets créés pour ces tests subsistent en brouillon et **ne peuvent pas être supprimés par le canal automatisé** :

- page « AUDIT TECHNIQUE — page de test, ne pas diffuser », dépubliée, brouillon résiduel ;
- message « Test d'audit technique interne », brouillon jamais envoyé.

À supprimer manuellement dans l'interface. Ce résidu illustre le constat A06-008 : si l'impossibilité de supprimer vaut aussi en interface, une demande d'effacement au sens de l'article 17 du RGPD ne peut pas être exécutée. **À trancher par un test en interface**, que le canal MCP ne permet pas.

## M-015 — surface d'injection persistante non relevée jusqu'ici.

`get_business_context` renvoie un champ libre de 10 000 caractères, vide sur ce compte, modifiable par `update_business_context`. Ce champ est décrit comme le contexte métier fourni au modèle. Le compte porte également `aiSystemPrompts` avec deux entrées, `webpage` et `email`, vides ici.

**Ce sont des instructions persistantes injectées dans toutes les générations futures.** Quiconque obtient une écriture sur ces champs — par le MCP, par une injection indirecte, ou par un accès compromis — oriente durablement tout ce que l'IA produira pour ce créateur, sans que rien n'apparaisse dans le contenu généré. À ajouter au modèle de menaces d'A04 et au questionnaire d'A05.

## M-016 — 15 modèles livrés par défaut

Dont une séquence de lancement complète en 7 emails (annonce, problème, recadrage, preuve, objections, urgence, dernier rappel), une page d'inscription, une page de vente, une newsletter, une signature, trois thèmes visuels. Matière pour la carte des modules d'A07.

## M-017 — analytics exposés au canal automatisé

`get_analytics_summary` renvoie visiteurs, contacts, ventes et revenus sur une période. Le pilotage par IA a donc accès aux métriques commerciales du créateur.

## Synthèse des garde-fous, vue d'ensemble

| Action | Contrôle serveur | Nature du contrôle |
|---|---|---|
| Publier une page | **Aucun** | — |
| Dépublier une page | Aucun | — |
| Lire les contacts | Aucun | — |
| Lire les métriques commerciales | Aucun | — |
| Écrire le contexte IA persistant | Aucun | — |
| Envoyer un message | Oui | **Commercial** (plan Pro) |
| Insérer du code personnalisé | Oui | **Commercial** (plan Pro) |
| Supprimer une page ou un email | Action inexistante | Absence de fonction |
