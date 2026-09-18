# Passe 1 — exécution par l'orchestrateur

18 septembre 2026, 23:07 à 23:12 UTC. Compte en **plan gratuit**. Tout en brouillon, aucune publication, aucun envoi.

## Objets créés

| Objet | Identifiant | État |
|---|---|---|
| Produit espace membre « POC Cadrage Net » | `85dd81bd-14f5-4850-92a4-ff5ceab3c0c2` | gratuit, commentaires activés |
| Leçon 1 — Voir avant de déclencher | `2896330e-1c52-43e6-8ddf-c28ca70d98b4` | brouillon, ~850 mots |
| Leçon 2 — Où poser le sujet dans le cadre | `b88f270e-383c-46af-8231-becdd3891b69` | brouillon, ~800 mots |
| Leçon 3 — Les lignes conduisent le regard | `cfe35ca3-457d-4e39-9258-d5d41089db85` | brouillon, `dripDelayDays: 7` |
| Leçon 4 — Exercice pratique à compléter | `8ede7e32-78a5-4e81-b822-b37892c9d229` | brouillon, **sans contenu** |
| Page « POC Les 7 erreurs de cadrage » | `94ede07c-a02d-4dd6-968b-6aba321f3e65` | brouillon, `indexed: false` |
| Formulaire « POC Diagnostic cadrage » | `17c02675-ebc6-464d-86e4-5ce641644252` | brouillon, 3 pages |
| Étiquette « POC — prospect diagnostic » | `952b281d-b748-4b3b-8825-adba66f8e7a3` | — |
| Contact fictif | `05447cbc-e5c3-4688-a05b-19b7fc0705ff` | `poc-test-01@example.invalid` |
| Coupon « POCTEST30 » | `4599cff8-9988-4e4e-9b69-0095d3d57d00` | code réel : `XLYK19KN` |

## Ce que cette passe a établi

### G-01 — Quotas de produits du plan gratuit : 1 payant + 1 gratuit

Deux refus distincts, tous deux en `402 UPGRADE_REQUIRED` :

- `The free plan allows 1 paid product.` — déclenché dès le premier produit payant, le compte en ayant déjà un.
- `The free plan allows 1 free product (lead magnet).` — déclenché au second produit gratuit.

**Conséquence opérationnelle.** Un compte de test en plan gratuit ne permet **qu'un seul scénario produit à la fois**. Toute campagne de tests qui suppose plusieurs offres exige un compte Pro, ou la réaffectation du produit existant.

### G-02 — `update_product` permet de changer le type d'accès d'un produit existant

Le produit créé en `download` a été converti en `member` par un seul appel, sans perte ni avertissement. **C'est le contournement du quota** : plutôt que créer un second produit, on réaffecte le seul disponible.

Effet de bord relevé : la **vignette générée automatiquement à la création conserve le slug de l'ancien nom**. Après renommage, le fichier s'appelait toujours `poc-assiette-nette-*.webp` alors que le produit s'appelait « POC Cadrage Net ». La vignette n'est pas régénérée au renommage, et son URL divulgue le nom précédent.

### G-03 — `create_coupon` : le nom n'est pas le code, et c'est un piège sérieux

Le schéma réel attend `name`, `type`, `value` — et non `code`, `discountType`, `discountValue`, qui produisent une erreur de validation.

Surtout : **le champ `name` n'est pas le code de réduction.** Le serveur génère un code indépendant. Ici, `name: "POCTEST30"` a produit `code: "XLYK19KN"`.

**Conséquence, et elle est grave.** La documentation des outils indique que la seule façon d'appliquer un coupon à un bouton est d'ajouter `?coupon=CODE` à l'URL de paiement. Un agent qui reprendrait le nom qu'il vient de choisir produirait un lien dont la remise ne s'applique pas, **sans aucune erreur visible**. Il faut impérativement relire le `code` retourné par la création, ou par `list_coupons`.

### G-04 — `update_modules` a deux exigences non documentées

Deux échecs successifs avant de trouver la forme correcte :

1. `modules.0.id: Invalid input: expected string, received undefined` — chaque module exige un `id` en plus de son `name`. Une chaîne libre convient, le serveur l'accepte comme identifiant.
2. `API error 400: standalone module is required` — **le module `standalone` doit figurer dans la charge utile même vide.** C'est le conteneur des leçons non affectées ; l'omettre invalide l'appel entier.

`get_modules` sur un cours neuf renvoie un unique module `standalone` contenant toutes les leçons dans leur ordre de création.

### G-05 — Le contrôle de plan s'applique aussi au contenu des leçons

Une leçon contenant un bloc `codeHtmlBlock` est refusée en `402 PRO_PLAN_REQUIRED`, exactement comme une page. **Le contrôle porte sur le type de bloc, pas sur le type d'objet qui le contient.** C'est cohérent, et c'est une bonne nouvelle pour la prévisibilité.

### G-06 — Le champ `content` d'une leçon est réellement optionnel

Une leçon créée sans aucun contenu est acceptée et retourne un brouillon valide. Utile pour créer un squelette de cours avant d'écrire, ou pour tester l'arborescence sans produire de texte.

### G-07 — `create_form` retourne les `nodeId` des champs

La création d'un formulaire renvoie, pour chaque page, la liste de ses champs avec un `nodeId` et, pour les choix, les identifiants d'options tels qu'on les a fournis. **Ces `nodeId` sont nécessaires pour toute règle de destination ultérieure** et ne sont retournés qu'à la création : les relever immédiatement, ou les retrouver par `get_form`.

### G-08 — Le contact créé est abonné d'office

`create_contact` retourne `isSubscribed: true` sans étape de confirmation, cohérent avec le `doubleOptin: false` du compte. À connaître avant tout test de délivrabilité : un contact créé par API est considéré comme ayant consenti.

## Pour le manuel

Les cinq points à retenir de cette passe, par ordre d'importance pour un administrateur :

1. Relire systématiquement le `code` d'un coupon, jamais réutiliser son `name`.
2. `update_modules` : toujours inclure `standalone`, toujours donner un `id` à chaque module.
3. Les quotas du plan gratuit rendent impossible plus d'un scénario produit : prévoir un compte Pro pour les campagnes UAT.
4. Le schéma annoncé par un outil et le schéma réellement attendu par le serveur divergent sur plusieurs actions du catalogue. Lire l'erreur de validation, elle nomme exactement les champs attendus.
5. Tout objet créé porte un préfixe `POC` : c'est le seul moyen de les retrouver, **le canal automatisé ne permettant de supprimer ni page, ni leçon, ni produit, ni contact**.
