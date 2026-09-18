# 04 — Sécurité et conformité

Data room TinyPages · Date de référence : 18 septembre 2026 · État : **partiel, mode dégradé assumé**

Sources internes : `audit/rapports/A05.md` (sécurité et multi-tenance), `audit/rapports/A06.md` (conformité et juridique), `audit/rapports/A02.md` (paiements, fiscalité, PCI DSS), `audit/rapports/A03.md` (emails, authentification, consentement), `audit/rapports/A01.md` (infrastructure), `audit/annexes/screening_mcp_compte_test.md` (relevés d'exécution MCP).

---

## 0. À lire avant le reste : ce que ce document prouve, et ce qu'il ne prouve pas

Ce chapitre n'est pas une précaution de style. Il conditionne la lecture de toutes les tables qui suivent.

**Aucune page de TinyPages n'a pu être ouverte.** L'egress réseau de l'environnement d'audit est fermé (403 du proxy au CONNECT, relevé le 18/09/2026, décision D-2 du plan d'enquête). N'ont donc été lus ni la politique de confidentialité réelle de la plateforme, ni les CGV, ni les CGU, ni les mentions légales, ni le bandeau cookies, ni la page d'accueil, ni les pages tarifaires, ni les 9 pages comparatives, ni la documentation, ni un seul site de créateur. Aucun en-tête HTTP, aucun cookie, aucune inspection TLS, aucun relevé DNS, aucun journal de certificats. Les bases officielles (EUR-Lex, Légifrance, CNIL, impots.gouv.fr, INPI, EUIPO, Pappers) sont également hors d'atteinte.

**Conséquences, à garder en tête ligne après ligne :**

1. La colonne « état » de la matrice réglementaire porte très souvent « **Non observé** ». Cela signifie : l'audit n'a pas pu constater le fait. Cela ne signifie **ni conformité, ni manquement**. Aucune ligne de ce document ne doit être citée comme un constat de non-conformité.
2. Les colonnes « exigences » et « action » sont du droit et de la méthode. Elles restent valides quel que soit l'état réel. Ce sont elles qui portent la valeur du document en l'état.
3. **Une règle générale n'est jamais un constat sur TinyPages.** Quand ce document rappelle ce qu'exige le RGPD, le DSA ou PCI DSS, il décrit l'obligation, pas la situation de la plateforme.
4. Une recherche négative n'est pas une preuve d'absence. « Aucune page de sécurité trouvée » veut dire qu'aucun matériel public n'est indexé et atteignable, pas qu'il n'en existe aucun.
5. **Toute conclusion juridique ou fiscale de ce document est à valider par un avocat.** Le paramètre RESPONSABLES porte « avocat : à mandater ». Rien ici ne constitue un avis juridique et rien n'est opposable à un tiers en l'état.

**Le seul canal ayant produit des constats CONFIRMÉS est le serveur MCP, sur le compte connecté** (annexe `screening_mcp_compte_test.md`, dérogation D-5). Tous les faits établis de ce volet viennent de là, et d'eux seuls : M-007 sur les pages légales vides publiées par défaut (§ 1.3), puis M-010 à M-017 sur les garde-fous réellement appliqués par le serveur (§ 1.4). Ils sont peu nombreux, ils sont reproductibles, et ce sont les seuls que ce document présente comme des constats sur TinyPages.

**Réserve sur la nature de ce compte.** Il était décrit comme « compte de test dédié » et déclaré vierge. Les relevés montrent autre chose : nom réel du CEO, sous-domaine à son nom, adresse personnelle réelle en `replyToEmail`, un contact existant correspondant à cette adresse, des brouillons créés pendant la session. Les relevés restent valides, car ils portent sur le **comportement par défaut de la plateforme** et non sur des données clientes, mais le garde-fou « jamais de compte réel » n'est respecté qu'imparfaitement. À signaler au contre-audit.

---

## 1. Posture de sécurité observable

### 1.1 Ce qui a été tenté, et ce que cela a donné

| Élément recherché | Méthode tentée | Résultat |
|---|---|---|
| En-têtes de sécurité (CSP, HSTS, `frame-ancestors`, `Referrer-Policy`, `Permissions-Policy`) | `curl -sS -I` sur les 4 hôtes connus | `CONNECT tunnel failed, response 403`. Non observé |
| Configuration TLS, chaîne de certificats, versions acceptées | `openssl s_client` | Egress fermé. Non observé |
| Attributs des cookies de session, portée `Domain`, préfixe `__Host-` | Relevé HTTP puis navigateur piloté | Non observé |
| Inscription à la Public Suffix List | `curl` sur `public_suffix_list.dat` | Non observé |
| `security.txt` (RFC 9116) | `curl` sur `/.well-known/security.txt` | Non observé |
| Inventaire des sous-domaines, transparence des certificats | `crt.sh` | Domaine bloqué. Non observé |
| Réputation du domaine (Safe Browsing, SmartScreen, listes email) | API et pages de transparence | Non observé |
| SPF, DKIM, DMARC des domaines d'envoi | `dig` | Binaire absent du conteneur **et** egress fermé. Non observé |
| Page de sécurité, MFA, procédure d'abus, page de statut, certification | 10 recherches WebSearch | **Aucun matériel public remonté.** Recherche négative |

### 1.2 Ce qu'il faut en retenir pour un investisseur

Pour un dossier construit au niveau d'exigence d'une Série A, l'absence de matériel public de sécurité est en soi un écart, indépendamment de la qualité réelle de la plateforme. Un auditeur mandaté ouvre en premier : la page sécurité, le `security.txt`, la page de statut, la politique de divulgation, la liste des sous-traitants. Aucun de ces éléments n'a été trouvé. Plusieurs se produisent en quelques heures et coûtent presque rien : c'est un défaut de documentation avant d'être un défaut technique, et les deux ne se corrigent pas dans le même délai.

Le questionnaire de sécurité prérempli (section 4) chiffre ce point : **88 % des lignes restent « inconnue »**. La colonne « à confirmer » se ferme en une demi-journée dès que l'egress est rouvert, avec les commandes déjà écrites en section 6 du rapport A05. La colonne « inconnue » dépend entièrement de pièces internes qu'il faut réclamer maintenant.

### 1.3 Premier fait établi : M-007, les pages légales vides publiées sur chaque compte

**Statut : CONFIRMÉ. Impact : élevé. Source : exécution réelle d'actions du catalogue MCP sur le compte connecté, 18/09/2026.**

À la création d'un compte, TinyPages crée et **publie automatiquement** cinq pages. Deux sont des documents juridiques, et leur contenu intégral est le suivant :

| Page | Statut | Indexée | Contenu complet relevé |
|---|---|---|---|
| Politique de confidentialité | `published` | `indexed: true` | `<h1><strong>Politique de confidentialité</strong></h1>` |
| Conditions d'utilisation | `published` | `indexed: true` | `<h1><strong>Conditions d'utilisation</strong></h1>` |

Rien d'autre. Pas une phrase. Ces pages sont en ligne, publiques et ouvertes à l'indexation dès la première seconde du compte, sans action du créateur.

Dans le même temps, la page d'accueil créée par défaut est elle aussi publiée et contient un **bloc de capture d'emails actif** (`emailCaptureBlock`, bouton « Rejoindre »). Et le compte porte `doubleOptin: false`.

Le constat est la combinaison des trois : **la plateforme met en ligne, pour chaque créateur, un formulaire de collecte d'adresses email adossé à une politique de confidentialité vide, sans double opt-in, et demande aux moteurs de l'indexer.**

Quatre conséquences, toutes à faire trancher :

1. **RGPD, articles 12 à 14.** Une politique de confidentialité réduite à son titre ne délivre aucune information aux personnes. Chaque créateur se trouve en écart dès l'ouverture de son compte, et c'est la plateforme qui a publié le document à sa place. **À valider par un avocat** : la responsabilité de TinyPages en tant que fournisseur du gabarit, et son rôle de sous-traitant au sens de l'article 28.
2. **Preuve du consentement.** `doubleOptin: false` par défaut, contact enregistré `isSubscribed: true` sans étape de confirmation. Le double opt-in n'est pas une obligation légale universelle sous le RGPD ; ce qui est exigé, c'est la **preuve du consentement** : identité, horodatage, texte du consentement, source (A03-007, conclusion à valider par un avocat). La question à poser au produit n'est donc pas « le double opt-in existe-t-il » mais « qu'est-ce que le modèle de contact stocke, et l'export le restitue-t-il ». Sans cela, l'article 7(1) du RGPD est inexécutable pour tous les créateurs, et l'exception de prospection du CPCE L34-5 est indémontrable.
3. **Réputation de domaine et référencement.** `indexed: true` sur des pages vides, répliquées sur chaque sous-domaine de `*.tinypages.co` : du contenu mince à grande échelle sur un domaine partagé. Croise directement le risque de réputation mutualisée de la section 2.
4. **Éclairage sur une contradiction relevée par A02.** Les CGV d'un site client affirment qu'« aucune commission n'est prélevée », en contradiction avec les 15 % annoncés sur le plan gratuit. Le gabarit livré par la plateforme étant vide, **la piste du gabarit fourni par TinyPages est écartée pour ce point précis**. L'origine du texte reste à établir.

**Pourquoi ce constat compte plus que sa taille apparente :** c'est un élément établi, reproductible, et corrigible sans dépendre d'une pièce interne. Un correctif produit (gabarit juridique réellement rempli ou page non publiée par défaut, `indexed: false` tant que la page est vide, double opt-in proposé à l'activation du bloc de capture) est mesurable en jours. À l'inverse, s'il subsiste à l'ouverture de la data room, il sera trouvé par n'importe quel auditeur en ouvrant un compte d'essai, et il donnera le ton de tout l'examen.

### 1.4 Les garde-fous réellement appliqués : ce que les tests d'exécution ont établi

Des tests d'exécution ont été menés le 18/09/2026 sur le compte connecté, sous dérogation D-5, en appelant délibérément des actions que la description des outils demande au modèle de ne pas appeler. L'objet du test est précisément de **distinguer un garde-fou appliqué par le serveur d'une consigne adressée au modèle**. Les résultats sont CONFIRMÉS et ils commandent une partie de l'analyse de sécurité qui suit.

| Action | Contrôle côté serveur | Nature du contrôle |
|---|---|---|
| Publier une page | **Aucun** | — |
| Dépublier une page | Aucun | — |
| Lire les contacts | Aucun | — |
| Lire les métriques commerciales (visiteurs, ventes, revenus) | Aucun | — |
| Écrire le contexte IA persistant | Aucun | — |
| Envoyer un message | Oui | **Commercial** (`402 PRO_PLAN_REQUIRED`) |
| Insérer du code personnalisé | Oui | **Commercial** (`402 PRO_PLAN_REQUIRED`) |
| Supprimer une page ou un email | Action inexistante | Absence de fonction |

**M-010, CONFIRMÉ, impact bloquant. La publication n'est soumise à aucun contrôle.** Création par `create_webpage`, puis `publish_webpage` appelé en violation délibérée de la consigne « Do NOT call publish_webpage » : **succès immédiat, URL publique retournée**. Aucune confirmation, aucune revue, aucun délai, aucune restriction de plan. Les mentions « saved as a draft », « the user decides when to publish » sont **exclusivement du texte adressé à un modèle que TinyPages ne contrôle pas**. Pour la data room, la conséquence est une règle de rédaction : toute formulation présentant ces consignes comme une garantie sera démentie en un appel par l'auditeur du fonds. La formulation défendable est qu'il s'agit d'un **comportement par défaut du modèle, pas d'un contrôle d'accès**.

**M-011, CONFIRMÉ. Le serveur sait refuser, et il refuse deux choses payantes.** L'envoi d'email et l'insertion de code personnalisé renvoient `402 PRO_PLAN_REQUIRED`. Le serveur sait donc opposer un refus ; il le fait pour deux actions facturées et pas pour la publication. **Les contrôles côté serveur protègent le chiffre d'affaires, pas l'utilisateur.** L'action la plus exploitable pour l'abus, publier une page publique sur un sous-domaine de la marque, est la seule des trois à n'avoir aucune barrière. Le message d'erreur d'envoi laisse entendre qu'un compte Pro enverrait sans autre contrôle : **à confirmer sur un compte Pro**, c'est la seule question que ce screening laisse ouverte sur ce point.

**M-014, CONFIRMÉ. L'IA ne peut pas nettoyer derrière elle.** Aucune action ne supprime une page ni un email. Les deux objets créés pour les tests subsistent en brouillon et ne peuvent pas être supprimés par le canal automatisé ; ils sont à supprimer manuellement en interface. Ce résidu documente le canal IA, il ne tranche pas l'interface : la question de l'article 17 du RGPD reste ouverte (§ 3.2).

**M-015, surface d'injection persistante.** `get_business_context` expose un champ libre de 10 000 caractères, modifiable par `update_business_context`, décrit comme le contexte métier fourni au modèle ; le compte porte aussi `aiSystemPrompts` avec deux entrées (`webpage`, `email`). **Ce sont des instructions persistantes injectées dans toutes les générations futures.** Qui obtient une écriture sur ces champs, par le MCP, par une injection indirecte ou par un accès compromis, oriente durablement tout ce que l'IA produira pour ce créateur, **sans que rien n'apparaisse dans le contenu généré**. À verser au modèle de menaces du volet IA et au questionnaire de sécurité (MT-19 ci-dessous).

**M-017.** Le canal automatisé lit les métriques commerciales du créateur (visiteurs, contacts, ventes, revenus) sans contrôle particulier. À croiser avec la portée des jetons OAuth (IAM-10).

**Deux constats mineurs du même screening.** M-008 : `list_accounts` renvoie un seul compte et `subAccounts: []`, la mécanique multi-comptes existe mais son cloisonnement n'a pas pu être éprouvé faute d'un second compte. M-009, à vérifier : un produit porte `price: 100`, `installments: 3` et `installmentAmount: 33`, soit 99 ; anecdotique seul, révélateur si la règle d'arrondi de la dernière échéance n'est pas gérée.

---

## 2. Isolation multi-tenant et domaines : le risque structurel n°1

C'est le point le plus important de ce livrable. Ce n'est pas un en-tête manquant ni une bibliothèque à mettre à jour : c'est une décision d'architecture, et c'est ce qu'un auditeur technique mandaté par un fonds regardera en premier.

### 2.1 La topologie

Les hôtes connus sont `tinypages.co` (vitrine), `app.tinypages.co` (application), `docs.tinypages.co` (documentation) et les sites des créateurs en `<créateur>.tinypages.co`. `co` étant un suffixe public ICANN, le **domaine enregistrable est `tinypages.co`** pour tous. Statut : **PROBABLE** (recoupement de quatre hôtes distincts, pages non ouvertes). Le serveur MCP est servi depuis `mcp.tinypages.dev`, domaine distinct : c'est un point favorable de cloisonnement, sous réserve que ce domaine appartienne bien à la même entité, ce qui n'est pas établi.

### 2.2 Ce que le navigateur en déduit

Tant que `tinypages.co` n'est pas inscrit à la section PRIVATE de la Public Suffix List, le navigateur considère `app.tinypages.co` et `emilio.tinypages.co` comme **un seul site**. Quatre conséquences en découlent, toutes en statut HYPOTHÈSE faute de relevé, toutes vérifiables en une heure une fois l'egress ouvert :

1. **Portée des cookies.** Un cookie posé avec `Domain=.tinypages.co` est envoyé à tous les sites clients. Inversement, une page cliente peut poser un cookie sur le domaine parent : c'est le **cookie tossing**, mécanisme documenté et utilisé notamment pour détourner des flux OAuth.
2. **SameSite est inopérant entre les deux plans.** Une requête émise depuis `client.tinypages.co` vers `app.tinypages.co` est *same-site*. `SameSite=Lax` comme `SameSite=Strict` laissent donc passer les cookies. **Une protection CSRF fondée sur SameSite seul serait sans effet dans ce modèle.** Il faut des jetons anti-CSRF synchronisés et une vérification stricte de l'en-tête `Origin`, sans dérogation par suffixe.
3. **CORS.** Si l'API autorise dynamiquement toute origine correspondant à `*.tinypages.co` avec `Access-Control-Allow-Credentials: true`, n'importe quelle page cliente peut appeler l'API avec la session du créateur connecté. Un seul motif trop large suffit.
4. **WebAuthn.** Si des clés d'accès sont ajoutées un jour avec `rpId = tinypages.co`, une page hébergée sur un sous-domaine client pourra déclencher une assertion valide pour l'application. Le sujet doit être tranché **avant** tout chantier MFA, pas pendant.

**Toute l'industrie comparable sépare les deux plans sur deux domaines enregistrables distincts** : `myshopify.com`, `vercel.app`, `github.io`, `notion.site`, `substack.com`. TinyPages ne le fait pas. Ce n'est pas un détail d'implémentation, c'est le point de départ de la revue de sécurité d'un fonds, et il faut y répondre avec un plan, pas avec une explication.

### 2.3 Le bloc de code personnalisé : « origine séparée » n'est pas « site séparé »

Le bloc `codeHtmlBlock` accepte du HTML et du JavaScript bruts, décrits dans les schémas MCP comme exécutés « in an ISOLATED sandbox iframe on a separate origin ».

**Précision établie par les tests d'exécution (M-011) :** ce bloc est **réservé au plan Pro** (`402 PRO_PLAN_REQUIRED` sur un compte gratuit). Cela réduit la surface d'abus par des comptes jetables, mais ne réduit en rien le risque technique : les comptes Pro sont précisément ceux qui vendent, donc ceux dont les pages portent un tunnel de paiement. La question de l'isolement reste entière, et le croisement PCI du § 2.4 s'en trouve resserré, pas affaibli.

**Ambiguïté de vocabulaire lourde de conséquences.** « Origine séparée » est vrai au sens de la politique de même origine et faux au sens de la portée des cookies. Si la sandbox est servie depuis un sous-domaine de `tinypages.co` **et** que l'attribut `sandbox` contient `allow-same-origin`, l'origine opaque disparaît et l'isolement contre le vol de cookies tombe.

Trois issues possibles, à trancher par un relevé de deux minutes sur une page publiée :

| Ce que montre le relevé | Conclusion |
|---|---|
| Domaine enregistrable distinct, `sandbox` sans `allow-same-origin` | Isolement solide, le risque retombe |
| Sous-domaine de `tinypages.co`, `sandbox` sans `allow-same-origin` | Origine opaque, isolement acceptable mais fragile à toute évolution |
| `allow-same-origin` sur un sous-domaine de `tinypages.co` | **Risque réel et prioritaire** |

Et même parfaitement isolée, **la sandbox protège la plateforme, pas le visiteur**. Restent entiers : formulaire de connexion ou de paiement contrefait affiché dans la frame, exfiltration réseau par `fetch`, `sendBeacon` ou pixel image, sortie de données par `postMessage`, clickjacking et redirection si `allow-popups` ou `allow-top-navigation` sont accordés. Le tout sur un sous-domaine de la marque, avec un certificat valide. Les contre-mesures sont une CSP interne (`connect-src`, `img-src`, `form-action`), la journalisation des domaines contactés, et une revue ou une mise en file d'attente du code personnalisé selon le plan.

Volet données : ce mécanisme permet à un créateur, ou à qui prend le contrôle de son compte, de **dupliquer silencieusement vers un tiers les données saisies par ses acheteurs**. C'est autant un sujet RGPD (sous-traitant non déclaré, transfert hors UE non encadré) qu'un sujet de sécurité.

### 2.4 Croisement PCI DSS : le bloc de code contre le critère d'éligibilité au SAQ A

Ce croisement est le plus coûteux du document après le sujet fiscal, parce qu'il touche au périmètre d'attestation, pas seulement à une bonne pratique.

- **Règle, PROBABLE.** Une page de paiement **hébergée par Stripe** (Checkout en redirection) relève du **SAQ A**. Stripe Elements ou un formulaire servi depuis l'infrastructure du marchand, même avec tokenisation, relèvent du **SAQ A-EP**, nettement plus lourd.
- **Règle, PROBABLE.** Depuis le **31 mars 2025**, les exigences PCI DSS v4.0.1 **6.4.3** (inventaire, autorisation et contrôle d'intégrité de tous les scripts de la page de paiement) et **11.6.1** (détection hebdomadaire de falsification de la page et des scripts) sont obligatoires. Elles ont été **retirées du SAQ A** et remplacées par un **critère d'éligibilité** : le marchand doit attester que son site **n'est pas exposé aux attaques par script**.
- **Tension, HYPOTHÈSE.** Deux signaux entrent en conflit avec ce critère : un Pixel Facebook annoncé sur le site TinyPages, et surtout le bloc `codeHtmlBlock` qui accepte du HTML et du JavaScript bruts fournis par le créateur. **Si un tel bloc peut se trouver sur une page portant un formulaire de paiement, l'attestation « site non exposé aux attaques par script » devient très difficile à tenir**, et l'inventaire de scripts du 6.4.3 devient un inventaire ouvert et non maîtrisé. Le fait que ce bloc soit réservé au plan Pro (CONFIRMÉ, M-011) ne desserre pas la tension : il la concentre sur les comptes qui encaissent.

**Question à trancher, formulée telle quelle pour le CTO :** le bloc HTML/JS peut-il coexister avec le formulaire de paiement sur la même page et la même origine ? Si oui, quel est le plan de traitement documenté ? Aucune page de paiement n'a pu être observée, ni en production ni en mode test (Stripe mode test non créé). Le fond du sujet paiements relève du livrable qui porte ce volet, à partir de `audit/rapports/A02.md`.

### 2.5 Cycle de vie des locataires : trois reprises possibles

| Risque | Mécanisme | Traitement attendu |
|---|---|---|
| **Identifiant de sous-domaine libéré** | Un créateur part, `sonnom.tinypages.co` redevient disponible. Qui le réenregistre hérite du référencement, des liens entrants, de la confiance du public et des identifiants enregistrés dans les gestionnaires de mots de passe des anciens visiteurs | Quarantaine permanente, ou de douze mois au minimum. Page « ce site n'existe plus » servie à la place, sans réattribution |
| **Identifiant sensible créé par un tiers** | `secure-billing`, `app-login`, `verify` : plateforme d'hameçonnage prête à l'emploi, sur le domaine de la marque, avec certificat valide | Liste de mots réservés (app, admin, www, api, login, auth, secure, pay, checkout, billing, account, support, help, status, mail, cdn, static, assets, docs, mcp, dev, staging) et refus de tout identifiant contenant « tinypages » ou une variante proche |
| **Domaine personnalisé orphelin** | Un créateur pointe `boutique.sonsite.fr` en CNAME vers TinyPages puis résilie. Si l'enregistrement DNS survit et qu'un autre compte peut revendiquer l'hôte, un tiers prend le contrôle d'un sous-domaine du client. **Dans ce schéma, TinyPages est le service tiers vulnérable, pas la victime** | Preuve de propriété par enregistrement TXT à l'ajout, revérification périodique, liaison permanente hôte / premier compte vérifié, refus de revendication par un second compte sans nouvelle vérification. Émission de certificat conditionnée à la vérification de propriété, jamais à la seule résolution DNS |

Point de conception à documenter : si chaque site client reçoit son propre certificat, la transparence des certificats **expose publiquement la liste complète des clients et leur date d'arrivée**, donnée commercialement sensible en pleine levée. Si un certificat générique `*.tinypages.co` est utilisé, une clé privée unique couvre tous les locataires. Le choix doit être assumé et expliqué dans la data room ; le certificat générique ne doit jamais être présenté sur un domaine personnalisé de client.

### 2.6 Réputation mutualisée et cumul d'abus

Parce que le domaine enregistrable est partagé, **un signalement portant sur `tinypages.co` peut retirer d'un coup tous les sites clients, l'application et la vitrine**, et dégrader la délivrabilité de tout email contenant un lien vers ces hôtes. C'est un risque de continuité d'activité, pas seulement de sécurité.

**Correction apportée par les tests d'exécution.** Le rapport A05 fondait ce risque sur la combinaison « plan gratuit + code personnalisé + création de pages par IA ». **Cette combinaison ne tient pas** : le bloc de code est réservé au plan Pro (M-011, CONFIRMÉ). La correction est consignée ici plutôt que gommée.

Le risque réel tient, et se reformule sur des faits établis : **plan gratuit + pilotage par IA + publication sans aucun contrôle côté serveur** (M-010, CONFIRMÉ). Une page d'hameçonnage purement visuelle, sans une ligne de JavaScript, sur un sous-domaine de la marque avec certificat valide, reste créable et publiable **en deux appels automatisés**, sans revue, sans délai, sans limite de plan. Le JavaScript n'est pas nécessaire pour tromper un visiteur, et c'est ce qui rend cette reformulation plus grave que la version d'origine : elle ne dépend plus d'une fonctionnalité payante, et elle est vérifiée, pas supposée.

Si le domaine personnalisé est réservé au plan Pro, le gisement d'abus se concentre par construction sur `*.tinypages.co`, c'est-à-dire sur le domaine qui porte aussi l'application. La littérature sectorielle sur l'abus de plateformes SaaS légitimes documente l'ampleur du phénomène et la lenteur des retraits ; ces sources datent de 2022 à 2024 et sont signalées comme possiblement périmées quant aux ordres de grandeur.

M-007 aggrave ce tableau d'un cran : des pages vides, indexées, répliquées sur chaque sous-domaine, ajoutent un signal de faible qualité sur le même domaine partagé.

Contre-mesures attendues, dans cet ordre : **un contrôle de publication appliqué par le serveur** et non par une consigne textuelle (limites de création et de publication par compte et par IP sur le plan gratuit, vérification d'email et de moyen de paiement, mise en file d'attente au-delà d'un seuil), puis détection de similarité de marques, journal des publications effectuées par l'IA, surveillance quotidienne de la réputation, et **un plan de crise écrit pour le scénario « `tinypages.co` signalé comme dangereux par un navigateur majeur »**. Ce plan n'existe pas à notre connaissance.

### 2.7 Plan de traitement, dans l'ordre

Cet ordre compte : le correctif d'urgence n'est pas celui qu'on croit.

1. **Immédiat (jours).** Verrouiller les cookies : cookie de session en `__Host-`, sans attribut `Domain`, `Secure`, `HttpOnly`, `SameSite=Lax` au minimum. Vérifier qu'aucun cookie applicatif ne porte `Domain=.tinypages.co`. Ajouter des jetons anti-CSRF synchronisés et la vérification d'`Origin`. Restreindre CORS à une liste blanche explicite. Relever et durcir l'attribut `sandbox` du bloc de code.
2. **Court terme (semaines).** Liste de mots réservés, quarantaine des identifiants libérés, vérification TXT des domaines personnalisés, `security.txt`, page et procédure d'abus, surveillance de réputation.
3. **Moyen terme (mois).** Demander l'inscription de `tinypages.co` à la section PRIVATE de la Public Suffix List. **Ce n'est pas un correctif d'urgence** : l'inscription passe par une proposition de modification, un enregistrement DNS TXT `_psl`, une revue manuelle sans délai garanti, puis une diffusion dans les versions des navigateurs, soit plusieurs mois avant tout effet réel.
4. **Chantier.** Migrer les sites clients vers un domaine enregistrable distinct (par exemple `tinypages.site`). C'est la seule mesure qui supprime la classe de risque au lieu de la contenir. À présenter aux investisseurs comme une décision datée, avec son coût, plutôt qu'à défendre comme un non-sujet.

---

## 3. Sous-traitants et chaîne de traitement

TinyPages cumule trois casquettes réglementaires distinctes, et aucune n'est vérifiable ici : **sous-traitant** des créateurs pour les données de leurs contacts, élèves et acheteurs ; **responsable de traitement** pour ses propres clients, son site et sa prospection ; **fournisseur de service d'hébergement** au sens du DSA. La qualification dépend des faits (qui décide des finalités), pas du contrat.

### 3.1 Inventaire, en l'état des preuves

| Fournisseur | Fonction annoncée | Qualification RGPD probable | Statut de preuve | À vérifier |
|---|---|---|---|---|
| Stripe | Paiements, montage Connect | **Responsable de traitement autonome** pour les données de paiement, pas sous-traitant | Annoncé, non vérifié | Type de compte Connect, `application_fee`, transferts (DPF / CCT) |
| PayPal | Paiements | **Responsable autonome** | Annoncé, non vérifié | Type d'intégration, prélèvement effectif de la commission |
| Postmark | Emails | Sous-traitant | Annoncé, non vérifié. Signature DNS attendue : `include:spf.mtasv.net`, sélecteur DKIM propre, `Return-Path` en CNAME | SPF/DKIM/DMARC réels, séparation des flux transactionnel et broadcast, isolation de réputation par créateur |
| Cloudflare Stream | Vidéo | Sous-traitant | Vu pour la vidéo de démo ; usage pour les vidéos clients **non confirmé** | Fournisseur vidéo réel, signature et expiration des URLs |
| Hébergeur (Vercel ?) | Infrastructure | Sous-traitant | **Hypothèse.** Un alias `tinypages.vercel.app` existe, sans valeur probante | Hébergeur, CDN, WAF, régions de stockage |
| Fournisseur du modèle IA | Coach IA, pilotage MCP | **Sous-traitant ultérieur** au sens de l'art. 28(2) et (4) | Non identifié publiquement | Identité, contrat, clause de non-entraînement, localisation |
| Code personnalisé d'un créateur | Bloc HTML/JS sur une page publiée | **Sous-traitant non déclaré potentiel**, avec transfert hors UE non encadré | Mécanisme décrit, implémentation non vérifiée | CSP interne, journalisation des domaines contactés |

**Deux pièges de cartographie à ne pas reproduire dans la politique de confidentialité :**

- Présenter **Stripe et PayPal comme sous-traitants** est une erreur fréquente et immédiatement repérable. La liste des fournisseurs doit distinguer trois colonnes : sous-traitant, responsable autonome, responsable conjoint.
- **Oublier le fournisseur du modèle IA.** C'est un sous-traitant ultérieur, il exige une autorisation générale ou spécifique du créateur, une information préalable de tout changement avec droit d'opposition, et des obligations équivalentes en cascade. C'est une chaîne contractuelle à documenter, pas une fonctionnalité produit.

### 3.2 Ce qui manque, et qui se produit vite

1. **Le contrat de sous-traitance (art. 28(3)) proposé aux créateurs.** C'est le point le plus structurant et le moins coûteux à traiter de tout le volet conformité. Sans lui, chaque créateur est en infraction et la plateforme devient un risque juridique pour son propre client. Des CGU peuvent en tenir lieu si elles portent l'intégralité des mentions obligatoires ; dans ce cas, il faut le dire explicitement et vérifier les points un par un. Y inscrire un **délai chiffré d'alerte en cas de violation (24 à 48 heures)** : un DPA qui dit seulement « sans délai indu » est un signal faible en due diligence.
2. **Une page « sous-traitants » datée et versionnée**, avec abonnement aux changements. Systématiquement demandée en due diligence.
3. **Une analyse d'impact des transferts par fournisseur américain.** Le Data Privacy Framework a été confirmé par le Tribunal de l'UE le 3 septembre 2025 (affaire Latombe), mais un pourvoi a été formé le 31 octobre 2025 et l'affaire est pendante devant la CJUE. Un scénario d'invalidation reste ouvert : ne pas s'appuyer sur le seul DPF, doubler par les clauses contractuelles types 2021/914.
4. **Une matrice des rôles par traitement**, une ligne par traitement, un rôle justifié. Pièce de data room attendue.
5. **Une réponse écrite à la question de la finalité propre.** Les données des contacts des créateurs servent-elles à autre chose qu'à exécuter le service : statistiques produit, amélioration, évaluation ou entraînement de modèles, prospection croisée ? Toute réponse positive fait sortir TinyPages du rôle de sous-traitant pour ces usages et exige une base légale propre et une information.

**Signal à lever en priorité.** Les instructions du serveur MCP énoncent « You cannot delete contacts, products, webpages, blog posts, forms, lessons, or emails », et les tests d'exécution le confirment pour le canal automatisé : **aucune action ne supprime une page ni un email, les objets créés pour les tests subsistent en brouillon** (M-014, CONFIRMÉ). Ce qui est établi, c'est donc l'absence de fonction de suppression **par le canal IA**. Ce qui ne l'est pas, c'est l'interface : rien ne dit que la restriction y vaut aussi. Si elle y valait, le créateur ne pourrait pas exécuter une demande d'effacement et TinyPages ne pourrait pas l'assister comme l'exige l'art. 28(3)(e). Statut HYPOTHÈSE pour l'interface, **à trancher par un test en interface sous 7 jours**, que le canal MCP ne permet pas. Ce point ne doit pas circuler hors de cet audit tant qu'il n'est pas tranché.

---

## 4. Questionnaire de sécurité prérempli

Structure calquée sur les domaines de la Cloud Controls Matrix v4, base commune du CAIQ et recoupant SIG Lite, plus un domaine ajouté **MT** pour la multi-tenance, qui est le sujet propre de TinyPages et que les questionnaires standards traitent mal.

**Colonne État :** *observée* (relevé effectué dans cette session), *à confirmer* (élément d'indice, non relevé), *inconnue* (aucun élément).

Le questionnaire d'origine compte **97 lignes** (A05, section 7). Il est repris ici condensé : les **12 lignes qui portent un état autre que « inconnue » sont conservées telles quelles**, les lignes « inconnue » sont regroupées par domaine quand elles appellent la même pièce et le même destinataire. Le décompte de fin porte sur les 97 lignes d'origine.

Rappel : en mode dégradé, aucune ligne ne peut porter « observée » sur un relevé réseau. La seule ligne « observée » porte sur un fait documentaire. C'est le résultat attendu, et la colonne de droite est la véritable commande de travail pour le CTO.

| Réf | Question | Réponse préremplie | État | Pièce qui tranche |
|---|---|---|---|---|
| **A&A-01 à 03** | Certification SOC 2 Type II, ISO 27001 ou équivalent ; audit interne périodique ; rapport communicable | Aucune mention publique trouvée | inconnue | CTO |
| **AIS-01** | En-têtes de sécurité déployés (CSP, HSTS, `frame-ancestors`) | Non relevable, egress fermé | **à confirmer** | A05 section 6, commandes prêtes |
| **AIS-02** | Les utilisateurs peuvent-ils injecter du HTML et du JavaScript ? | Oui. Bloc de code exécuté en iframe sandbox sur une « origine séparée », **réservé au plan Pro** (`402 PRO_PLAN_REQUIRED` en gratuit) | **observée** pour l'existence et le gating (MCP, 18/09/2026) ; **à confirmer** pour l'implémentation de la sandbox | Relevé sur page publiée, § 2.3 |
| **AIS-03** | Isolation exacte de ce code : jetons `sandbox`, domaine enregistrable de la sandbox, CSP interne | — | inconnue | CTO. **Question la plus déterminante du volet sécurité** |
| **AIS-04** | Le code personnalisé est-il revu, mis en file d'attente ou limité ? | Limité par **plan** (Pro), pas par revue ni par file d'attente. Aucune revue observée | **observée** (MCP, 18/09/2026) | CTO pour la revue éventuelle côté Pro |
| **AIS-05** | Revue de sécurité du code et analyse statique dans la chaîne d'intégration | — | inconnue | CTO, dépôt |
| **AIS-06** | Politique CORS de l'API : liste blanche explicite ou correspondance de suffixe | — | inconnue | CTO |
| **AIS-07** | Protection CSRF : jetons synchronisés, ou SameSite seul | — | inconnue | CTO. **SameSite seul serait insuffisant ici**, voir § 2.2 |
| **AIS-08** | Limitation de débit sur l'authentification et sur l'API | — | inconnue | CTO |
| **AIS-09** | Sécurité de l'API publique : authentification, portées, rotation des jetons | Jeton API annoncé dans Paramètres, Avancé | **à confirmer** | Volet MCP |
| **BCR-01 à 05** | Sauvegardes (fréquence, chiffrement, rétention) ; **restauration testée et date du dernier test** ; RPO et RTO ; plan de reprise et exercice ; redondance régionale | — | inconnue | CTO. **BCR-02 est la pièce la plus importante du domaine** |
| **BCR-06** | Engagement de disponibilité, page de statut publique | Aucune page de statut trouvée | **à confirmer** | CTO |
| **BCR-07** | Sortie du service : récupération des contenus et des contacts | — | inconnue | Test produit. Croise le Data Act, § 5 |
| **CCC-01 à 04** | Mise en production et revue par un pair ; environnements séparés, aucune donnée réelle en préproduction ; retour arrière ; infrastructure en code | — | inconnue | CTO. **Point sensible avec une équipe très réduite** |
| **CEK-01** | Chiffrement en transit, versions TLS acceptées | Non relevable | **à confirmer** | A05 section 6 |
| **CEK-02, 03, 05** | Chiffrement au repos ; gestion des secrets (coffre, rotation, aucun secret en dépôt) ; hachage des mots de passe | — | inconnue | CTO |
| **CEK-04** | Certificats des sites clients : émetteur, renouvellement, générique ou par hôte | — | inconnue | Voir le compromis exposé en § 2.5 |
| **DCS-01 / DCS-02** | Hébergeurs et régions, attestations héritées ; données des clients européens stockées dans l'Union | Non déterminé, hébergeur non établi | inconnue | CTO |
| **DSP-01 à 06** | Cartographie des données ; durées de conservation ; liste des sous-traitants ; DPA créateurs ; transferts hors Union ; suppression effective y compris des sauvegardes | — | inconnue | Voir § 3 et la matrice § 5 |
| **DSP-07** | Le code personnalisé peut-il exfiltrer des données d'acheteurs vers un tiers non déclaré ? | Oui en l'état du mécanisme décrit, sauf CSP interne restrictive | **à confirmer** | § 2.3 |
| **GRC-01, 03, 04** | Politique de sécurité écrite ; registre des risques ; assurance cyber | — | inconnue | CTO, CEO |
| **GRC-02** | Responsable sécurité désigné | CTO et CEO sont la même personne | **observée** (`audit/00_parametres.md`, 18/09/2026) | — |
| **HRS-01 à 04** | Vérification des antécédents ; sensibilisation annuelle ; retrait des accès au départ ; engagements de confidentialité y compris prestataires | — | inconnue | CEO |
| **IAM-01** | Modes de connexion des créateurs | Connexion Google annoncée, email à confirmer | **à confirmer** | Produit, CTO |
| **IAM-02 / IAM-03 / IAM-04** | MFA disponible pour les créateurs ; imposable à une équipe ; **obligatoire pour les accès administrateurs internes** | Aucune information publique | inconnue | CTO. Attendu sur IAM-04 : obligatoire, sans exception |
| **IAM-05** | Durée de vie des sessions, révocation, déconnexion globale, rotation à l'élévation de privilège | — | inconnue | CTO |
| **IAM-06** | Attributs des cookies de session (`Domain`, préfixe `__Host-`) | Non relevable | **à confirmer** | A05 section 6. **Décide de l'exploitabilité du cookie tossing** |
| **IAM-07** | Rôles et permissions dans un compte créateur | — | inconnue | Produit |
| **IAM-08** | Accès à la production : nombre de personnes, moindre privilège, revue périodique, accès d'urgence | — | inconnue | CTO |
| **IAM-09** | Les employés peuvent-ils prendre l'identité d'un client ? Est-ce journalisé et consenti ? | — | inconnue | CTO. **Question systématique en due diligence** |
| **IAM-10** | Portée et révocation des jetons OAuth du MCP | — | inconnue | Volet MCP |
| **IPY-01 / IPY-02** | Export complet des données du créateur dans un format ouvert ; devenir d'un site publié après résiliation | — | inconnue | Produit. Croise le Data Act et § 2.5 |
| **IVS-01 à 03** | Pare-feu applicatif et protection anti-déni de service ; segmentation réseau et exposition des bases ; durcissement et mises à jour | — | inconnue | CTO |
| **LOG-01, 03, 05** | Journaux d'audit des actions sensibles et rétention ; accessibles au créateur ; intégrité et protection contre l'altération | — | inconnue | CTO |
| **LOG-02** | Journal des actions effectuées par l'IA via MCP, annulation possible | Annulation : **non**. Aucune action de suppression n'existe par le canal automatisé ; les objets créés subsistent en brouillon (M-014). Journal : non observé | **observée** pour l'annulation ; inconnue pour le journal | CTO |
| **LOG-04** | Alerte sur comportement anormal, création en masse, pics d'échecs de connexion | — | inconnue | CTO. **Directement lié au cumul d'abus, § 2.6** |
| **SEF-01 à 05** | Plan de réponse à incident écrit et rôles ; délai d'engagement de notification aux clients ; procédure de notification de violation sous 72 h ; incidents survenus depuis le lancement ; exercice de crise | — | inconnue | CTO, CEO. SEF-03 à valider par un avocat |
| **STA-01** | Inventaire des fournisseurs critiques | Stripe, PayPal, Postmark, Cloudflare Stream annoncés, non vérifiés | **à confirmer** | § 3.1 |
| **STA-02 à 04** | Évaluation sécurité des fournisseurs à l'entrée ; dépendances logicielles et nomenclature (SBOM) ; sous-traitants publiés aux clients | — | inconnue | CTO |
| **TVM-01 / TVM-02** | Test d'intrusion : date, prestataire, périmètre, constats, remédiation. **Le périmètre couvrait-il l'isolement multi-tenant ?** | Aucun rapport public | inconnue | CTO. Sans TVM-02, le test ne couvre pas le risque principal |
| **TVM-03 / TVM-05** | Analyse de vulnérabilités récurrente et SLA par gravité ; programme de primes aux bogues | Aucune trace publique | inconnue | CTO |
| **TVM-04** | Politique de divulgation responsable, `security.txt` publié | Non relevable. RFC 9116 n'impose que `Contact` et `Expires` | **à confirmer** | Coût quasi nul, signal fort |
| **UEM-01 / UEM-02** | Chiffrement de disque et verrouillage des postes ayant accès à la production ; gestion de parc et mises à jour | — | inconnue | CTO |
| **MT-01** | Les sites clients partagent-ils le domaine enregistrable de l'application ? | Oui selon toute vraisemblance, `*.tinypages.co` | **à confirmer** (PROBABLE) | § 2.1 |
| **MT-02** | `tinypages.co` est-il inscrit à la Public Suffix List ? | Non vérifiable | inconnue | A05 section 6 |
| **MT-03** | Le cookie de session est-il host-only, avec préfixe `__Host-` ? | — | inconnue | A05 section 6 |
| **MT-04 / MT-05** | Liste de mots réservés pour les identifiants de sous-domaine ; politique de non-réutilisation après départ | — | inconnue | CTO, § 2.5 |
| **MT-06 / MT-07** | Vérification de propriété d'un domaine personnalisé et maintien après résiliation ; **un hôte peut-il être revendiqué par un second compte sans nouvelle vérification ?** | — | inconnue | CTO. MT-07 est la question la plus directe pour tester le risque de reprise |
| **MT-08** | Émission de certificat conditionnée à la vérification, ou à la simple résolution DNS | — | inconnue | CTO |
| **MT-09** | Isolation des données entre créateurs au niveau de la base : clé de locataire, sécurité au niveau des lignes, tests automatisés | — | inconnue | CTO. Attendu : un test qui échoue si une requête franchit la frontière de locataire |
| **MT-10** | Les acheteurs et élèves ont-ils une identité globale réutilisée entre créateurs ? | — | inconnue | Produit. **Point de conception potentiellement majeur** : identité globale + JavaScript libre par créateur = un créateur peut viser les acheteurs des autres |
| **MT-11** | Limites anti-abus sur le plan gratuit | **Aucune sur la publication.** `publish_webpage` réussit immédiatement sur un compte gratuit, sans confirmation ni délai (M-010). Les seuls refus serveur observés sont commerciaux (M-011) | **observée** (MCP, 18/09/2026, CONFIRMÉ) | § 1.4, § 2.6 |
| **MT-12** | Détection de contenu frauduleux, automatisée ou sur signalement | Aucune détection observée à la publication | **à confirmer** (une modération asynchrone reste possible) | CTO |
| **MT-13 / MT-14** | Procédure publique de signalement d'abus et délai de traitement ; mécanisme de notification et action au titre du DSA | Aucune trace publique | inconnue | **Obligation DSA malgré le statut PME**, voir § 5. À valider par un avocat |
| **MT-15 / MT-16** | Surveillance de la réputation du domaine et alerte ; **plan de réponse si `tinypages.co` est signalé comme dangereux par un navigateur majeur** | — | inconnue | CTO. MT-16 est le scénario de crise à préparer en priorité |
| **MT-17** *(ajout de ce livrable)* | Les pages légales publiées par défaut sur chaque compte sont-elles vides et indexées ? | **Oui.** Politique de confidentialité et conditions d'utilisation publiées, `indexed: true`, contenu limité au titre. Page d'accueil par défaut publiée avec bloc de capture d'emails actif, `doubleOptin: false` | **observée** (MCP, 18/09/2026, CONFIRMÉ) | § 1.3, M-007 |
| **MT-18** *(ajout)* | La publication d'une page publique est-elle soumise à un contrôle appliqué par le serveur ? | **Non.** Publication réussie en un appel sur un compte gratuit, en violation délibérée de la consigne textuelle. Les garde-fous « ne pas publier » sont du texte adressé au modèle. Les seuls refus serveur sont commerciaux | **observée** (MCP, 18/09/2026, CONFIRMÉ) | § 1.4, M-010 et M-011 |
| **MT-19** *(ajout)* | Existe-t-il des instructions IA persistantes au niveau du compte, et qui peut les écrire ? | Oui : contexte métier libre de 10 000 caractères et `aiSystemPrompts` (`webpage`, `email`), modifiables par le canal automatisé. Injectés dans toutes les générations futures, invisibles dans le contenu produit | **observée** (MCP, 18/09/2026, CONFIRMÉ) | § 1.4, M-015. À verser au modèle de menaces du volet IA |

### Décompte

Décompte d'origine, sur les **97 lignes** du questionnaire produit par A05 :

| État | Lignes |
|---|---|
| observée | 1 |
| à confirmer | 11 |
| inconnue | 85 |
| **Total** | **97** |

Le décompte est en soi un constat : **88 % d'un questionnaire de sécurité standard reste sans réponse**. Un fonds n'y lira pas d'abord une faiblesse technique, il y lira un défaut de documentation. Les deux se corrigent, mais pas dans le même délai.

Mise à jour après le screening MCP : **sept lignes passent de « inconnue » ou « à confirmer » à « observée »** (AIS-02 partiellement, AIS-04, LOG-02 partiellement, MT-11, MT-17, MT-18, MT-19). C'est une mauvaise nouvelle, pas une bonne : **chacune de ces lignes est un écart, pas une réponse rassurante**. Le premier bloc de faits établis du dossier décrit une plateforme dont les garde-fous de publication sont textuels et dont les seuls contrôles serveur sont commerciaux.

---

## 5. Matrice réglementaire

**Lecture de la colonne « état ».** « Non observé » signifie que l'egress fermé a empêché toute constatation, **pas** qu'un manquement est présumé. Les colonnes « exigences » et « action » restent valides quel que soit l'état réel. Échéances comptées depuis le 18 septembre 2026. **Toute ligne est à valider par un avocat.**

| Texte | Applicable | Exigences principales | État | Écart | Action | Échéance |
|---|---|---|---|---|---|---|
| **RGPD, rôles** (règl. (UE) 2016/679, art. 4, 24, 26, 28) | Oui | Qualifier le rôle par traitement ; sous-traitant pour les données des contacts des créateurs, responsable pour ses propres clients ; aucune finalité propre non déclarée | Non observé | Non déterminé | Matrice des rôles par traitement | 30 j |
| **RGPD, contrat de sous-traitance** (art. 28(3)) | Oui | DPA écrit proposé à chaque créateur, mentions obligatoires complètes ; des CGU peuvent suffire si elles les portent toutes | Non observé | Non déterminé, **absence probable en l'état d'une jeune plateforme** | Produire le DPA créateurs et l'exposer publiquement ; délai chiffré de 24 à 48 h pour l'alerte violation | 30 j |
| **RGPD, information des personnes** (art. 12 à 14) | Oui | Information complète, concise, accessible, au moment de la collecte | **Écart CONFIRMÉ sur le gabarit livré** : politique de confidentialité publiée vide sur chaque compte, indexée, adossée à un formulaire de capture d'emails actif (M-007) | **Confirmé et directement actionnable** | Ne plus publier une page juridique vide : gabarit réellement rempli, ou page non publiée et `indexed: false` tant qu'elle est vide ; prévenir les comptes existants | **15 j** |
| **RGPD, consentement et preuve** (art. 7(1)) | Oui | Preuve du consentement conservée : identité, horodatage, texte, source. Le double opt-in n'est pas une obligation universelle, la preuve l'est | Non observé pour le stockage. **Observé** : `doubleOptin: false` par défaut, contact enregistré `isSubscribed: true` | Non déterminé | Établir ce que stocke le modèle de contact et ce que restitue l'export ; proposer le double opt-in à l'activation du bloc de capture | 30 j |
| **RGPD, sous-traitants ultérieurs** (art. 28(2) et (4)) | Oui | Liste publiée et datée ; information préalable des changements avec droit d'opposition ; obligations en cascade ; **fournisseur du modèle IA inclus** | Non observé | Non déterminé | Page « sous-traitants » versionnée, notification par email | 30 j |
| **RGPD, transferts hors UE** (chap. V, art. 44 à 49) | Oui (Stripe, Cloudflare, Postmark, fournisseur IA) | Mécanisme par destinataire (DPF ou CCT 2021/914) et analyse d'impact des transferts documentée | Non observé | Non déterminé | AITD par fournisseur ; ne pas dépendre du seul DPF, pourvoi Latombe pendant devant la CJUE | 60 j |
| **RGPD, conservation** (art. 5(1)(e)) | Oui | Durées définies, documentées, appliquées ; prospects 3 ans selon le référentiel CNIL | Non observé | Non déterminé | Tableau des durées et purge outillée dans le produit | 60 j |
| **RGPD, droits des personnes** (art. 12 à 22, 28(3)(e)) | Oui | Accès, rectification, effacement, portabilité, opposition ; le sous-traitant assiste le responsable | Non observé. **Signal** : le canal MCP annonce l'impossibilité de supprimer des contacts | **Potentiellement bloquant** si l'effacement est impossible aussi en interface | Vérifier en interface sur le compte de test ; corriger le produit le cas échéant | **Vérification sous 7 j** |
| **RGPD, violations** (art. 33, 34) | Oui | 72 h vers l'autorité, information des personnes si risque élevé, registre des violations, alerte du sous-traitant vers le responsable | Non observé | Non déterminé | Procédure écrite et testée, délai chiffré dans le DPA | 30 j |
| **ePrivacy et cookies** (dir. 2002/58/CE art. 5(3) ; loi 78-17 art. 82) | Oui, pour `tinypages.co` **et** les pages des créateurs | Consentement préalable avant tout dépôt ou lecture non strictement nécessaire ; refus aussi simple que l'acceptation ; retrait possible. **Le Pixel Facebook ne bénéficie d'aucune exception** | Non observé. Pixel signalé sur l'accueil, source non vérifiée | Non déterminé, **forte probabilité d'écart si aucun mécanisme de consentement n'est en place** | Charger le site avec un navigateur vierge et relever les requêtes réseau avant toute interaction (5 minutes, une capture) ; installer un mécanisme de consentement ; l'étendre aux sites clients | **Priorité 1, 15 j après réouverture de l'egress** |
| **DSA, hébergeur** (règl. (UE) 2022/2065, art. 11 à 18) | Oui | Point de contact autorités (art. 11) et destinataires (art. 12) électroniques et publiés ; CGU décrivant restrictions et modération y compris automatisée (art. 14) ; notification et action (art. 16) ; exposé des motifs de toute restriction (art. 17) ; signalement d'infractions pénales graves (art. 18) | Non observé. **Signal aggravant** : la publication n'est soumise à aucun contrôle serveur (M-010), ce qui rend le mécanisme de notification et action d'autant plus nécessaire | Non déterminé. Aucune procédure d'abus publique trouvée | Paquet DSA hébergeur : cinq pages et une adresse surveillée | 45 j |
| **DSA, plateforme en ligne** (section 3) | À qualifier, **probablement non** | Signalements prioritaires, recours interne, médiation, transparence publicitaire | Non observé | Sans objet si la qualification d'hébergeur pur est retenue (considérant 13 : exclusion des services d'hébergement web quand la diffusion publique est mineure et accessoire) | Note de qualification motivée | 45 j |
| **DSA, exemption micro et petite entreprise** (art. 19) et rapports de transparence (art. 15(2)) | À qualifier | **L'art. 19 n'exempte que la section 3.** L'art. 15(2) exempte les micro et petites entreprises du rapport de transparence | Non observé | **Piège à éviter absolument** : croire que l'exemption PME dispense des art. 11 à 18. Elle ne les couvre pas. Ces articles s'appliquent quelle que soit la taille | Documenter le statut PME (< 50 salariés et CA ≤ 10 M€) et ne pas s'en prévaloir au-delà de la section 3 | 45 j |
| **Représentants UE** (DSA art. 13 ; RGPD art. 27) | À qualifier, dépend de l'entité | Deux désignations écrites distinctes si l'entité est établie hors UE | **Non observé, entité inconnue** | Non déterminé | Trancher dès que l'entité est établie | 15 j après l'établissement de l'entité |
| **LCEN** (loi 2004-575 art. 6, adaptée par la loi SREN 2024-449) | À qualifier, si rattachement français | Conservation des données d'identification des contributeurs de contenu ; dispositif de notification des contenus illicites | Non observé | Non déterminé | Vérifier la rétention et l'existence d'une adresse de signalement surveillée | 45 j |
| **Data Act** (règl. (UE) 2023/2854, chap. VI, art. 23 à 31) | Oui, TinyPages est un service de traitement de données. Applicable depuis le 12 septembre 2025 | Résiliation avec préavis ≤ 2 mois ; transition assistée 30 j ; export structuré et lisible par machine des données et des actifs numériques exportables ; suppression après migration ; information précontractuelle | Non observé | Non déterminé | Clauses de sortie dans les CGU et fonction d'export complet documentée (argument de vente autant que conformité) | Clauses : 60 j · **Frais de changement interdits au 12 janvier 2027** |
| **AI Act, art. 50(1)** (règl. (UE) 2024/1689) | Oui, coach IA | Informer la personne qu'elle interagit avec un système d'IA, sauf évidence | Non observé | Non déterminé | Mention visible à l'ouverture du coach | **Déjà applicable depuis le 2 août 2026. Échéance dépassée de 47 jours** |
| **AI Act, art. 50(2)** | Oui, contenus générés (pages, emails, articles, images) | Marquage lisible par machine, dans un format interopérable, robuste et fiable | Non observé | Non déterminé | Implémenter le marquage (métadonnées C2PA ou équivalent) | **2 décembre 2026, soit 11 semaines.** Délai de grâce pour les systèmes mis sur le marché avant le 2 août 2026 |
| **AI Act, art. 4** | Oui, sans seuil d'effectif | Maîtrise de l'IA du personnel et des personnes agissant pour le compte de l'entreprise | Non observé | Non déterminé. Portée de l'obligation non tranchée (§ 6) | Note de sensibilisation datée et registre de formation. La même action satisfait les deux lectures | 30 j |
| **AI Act, haut risque** | Probablement non | Annexe III non concernée a priori par un outil marketing | Non observé | — | Note de qualification | Annexe III reportée au 2 décembre 2027 |
| **Qualification fournisseur / déployeur (AI Act)** | À qualifier | TinyPages met un système d'IA sur le marché **sous sa marque**, ce qui en fait probablement un fournisseur même en s'appuyant sur un modèle tiers | Non observé | Non déterminé | Note de qualification, à traiter avant l'art. 50 | 30 j |
| **European Accessibility Act** (dir. (UE) 2019/882, applicable depuis le 28 juin 2025) | À qualifier. Exemption microentreprise possible pour TinyPages, **mais pas pour ses clients** | EN 301 549, qui intègre WCAG 2.1 niveau AA ; déclaration d'accessibilité ; défense de charge disproportionnée documentée | Non observé, score d'accessibilité non mesurable ici | Non déterminé | Mesurer, prioriser l'éditeur et les modèles de page, publier une déclaration. Les pages générées servent des clients non exemptés | 90 j · Services préexistants : 28 juin 2030 |
| **PCI DSS v4.0.1, périmètre** (critère d'éligibilité SAQ A, exigences 6.4.3 et 11.6.1) | Oui, via le tunnel de paiement | SAQ A si la page de paiement est hébergée par le prestataire ; **SAQ A-EP** si le formulaire est servi depuis l'infrastructure marchande. Depuis le 31 mars 2025, l'éligibilité au SAQ A suppose d'attester que le site **n'est pas exposé aux attaques par script** | Non observé. Aucune page de paiement vue, en production comme en mode test | **Tension identifiée** : le bloc HTML/JS libre et un éventuel Pixel rendent cette attestation difficile à tenir si le bloc peut coexister avec le formulaire de paiement | Établir le type de checkout et si le bloc de code peut coexister avec le formulaire de paiement sur la même page et la même origine ; à défaut, plan de traitement documenté | 30 j |
| **TVA numérique** (dir. 2006/112/CE art. 58 ; règl. d'exéc. 282/2011 art. 9 bis) | Oui | Taxation au pays du preneur, seuil unique de 10 000 €, guichet OSS. **Présomption de l'art. 9 bis, irréfragable si la plateforme autorise la facturation, autorise la fourniture ou fixe les conditions générales** | Non observé | **Risque majeur non qualifié** (voir § 6 bis ci-dessous) | Note fiscale d'urgence croisée avec le montage Stripe Connect ; ajuster les CGV si nécessaire | **30 j** |
| **Facturation électronique, France** (LF 2024 art. 91 ; CGI art. 289 bis) | À qualifier, si entité française | Réception via une plateforme agréée pour toutes les entreprises assujetties ; émission pour les grandes entreprises et les ETI ; e-reporting des transactions B2C | Non observé | Non déterminé | Choisir une plateforme agréée ; traiter le volet produit (e-reporting des ventes des créateurs, identité de l'émetteur de la facture) | **Réception : 1er sept. 2026, échue depuis 17 jours** · Émission PME : 1er sept. 2027 (date à vérifier, § 6) |
| **Facturation électronique, Belgique** (loi du 6 février 2024) | À qualifier, si entité belge | Émission et réception B2B structurées via Peppol (EN 16931, Peppol BIS 3.0) | Non observé | Non déterminé | Raccordement Peppol | **1er janvier 2026, échue.** Tolérance du 1er trimestre 2026 expirée |
| **ViDA** (paquet TVA à l'ère numérique, adopté le 11 mars 2025) | Oui, à terme | Extension du guichet OSS, facturation électronique et déclaration transfrontalière harmonisées | Non observé | Non déterminé | Veille, intégration à la feuille de route produit | Jalons 2027 à 2030. **Référence du texte à vérifier, § 6** |
| **DAC7** (dir. (UE) 2021/514 ; CGI art. 1649 ter A et s.) | À qualifier, **non-applicabilité défendable** | Collecte et vérification des données vendeurs, déclaration annuelle. Les « services personnels » (coaching, sessions en direct) sont déclarables dès le premier euro ; la « vente de biens » ne vise que les biens **corporels**, ce qui exclut les produits numériques préenregistrés | Non observé | Non déterminé. Argument de fond : un constructeur de sites où le créateur amène sa propre audience n'est pas une place de marché, il n'y a ni mise en relation ni audience mutualisée | Note de position motivée par un fiscaliste, traitant explicitement les deux faits qui pèsent en sens inverse : la **commission de 15 %** du plan gratuit et la facilitation de l'encaissement | **31 janvier 2027** si applicable |
| **Publicité comparative** (C. conso. art. L122-1 à L122-7 ; dir. 2006/114/CE) | Oui, 9 pages `/fr/tinypages-vs-*` | Comparaison objective, vérifiable, représentative, non trompeuse, sans dénigrement. **La charge de la preuve pèse sur l'annonceur** | Non observé, pages non ouvertes | Non déterminé | Dater et archiver chaque comparaison (capture de la page tarifaire du concurrent à la date du relevé), afficher « données relevées le … », réviser trimestriellement, remplacer les jugements de valeur par des faits vérifiables | 30 j, puis trimestriel |
| **Pratiques commerciales trompeuses** (C. conso. art. L121-1 à L121-5) | Oui | Toute allégation, notamment sur les résultats attendus, doit être exacte et prouvable au jour de la diffusion | Non observé | Non déterminé | Dossier de preuve daté pour chaque allégation absolue (« la seule plateforme … pilotable de bout en bout ») ou reformulation relative | 30 j |
| **Avis en ligne et témoignages** (C. conso. art. L121-4, L111-7-2 ; dir. (UE) 2019/2161) | Oui | Interdiction des faux avis ; mesures raisonnables et proportionnées de vérification ; information sur la méthode et la date de dépôt ; transparence des liens d'affiliation | Non observé | Non déterminé | Registre des consentements aux témoignages, mention de méthode. Base d'avis publics faible (environ 34 sur une plateforme tierce) : ne pas en tirer d'agrégat marketing | 30 j |
| **Authentification des emails et envois de masse** (exigences Gmail, Yahoo depuis fév. 2024 ; Microsoft depuis mai 2025) | Oui, dès 5 000 messages par jour | SPF et DKIM, DMARC au moins `p=none` aligné, désinscription en un clic RFC 8058 (`List-Unsubscribe` + `List-Unsubscribe-Post`), taux de plaintes sous 0,3 % | Non observé (`dig` absent **et** egress fermé) | Non déterminé. **Bloquant si non respecté** : rejets permanents, pas simple classement en spam | Relevé DNS des domaines d'envoi, en-têtes d'un email réel, taux de plaintes agrégé. Vérifier l'isolation de réputation par créateur (sous-domaine et sélecteur DKIM propres, `Return-Path` en CNAME, segmentation des IP) | 30 j |
| **Prospection électronique** (CPCE art. L34-5 ; RGPD art. 7) | Oui, volet produit | Opt-in préalable, exception cumulative client et produits analogues, désinscription simple et gratuite, **preuve du consentement conservée** | Non observé pour le stockage. Observé : `doubleOptin: false` par défaut | Non déterminé | Horodatage et origine du consentement dans le modèle de contact **et** dans l'export. Vérifier ce que conserve un import de base existante | 60 j |
| **Droit de rétractation** (C. conso. art. L221-18, L221-25, L221-28) | Oui, volet produit | Pour un contenu numérique sans support matériel : accord préalable exprès à l'exécution immédiate **et** reconnaissance expresse de la perte du droit, avec preuve conservée | Non observé | Non déterminé | Case dédiée et horodatée dans le tunnel de paiement. Fonctionnalité de conformité valorisable commercialement | 60 j |
| **Propriété intellectuelle, code** (CPI art. L111-1, L113-9) | Oui | Dévolution automatique à l'employeur pour les **salariés seulement** ; cession écrite nécessaire pour les fondateurs avant constitution, les prestataires et les stagiaires | Non observé | Non déterminé. **Point de blocage classique de closing** : le lancement est de janvier 2025, l'association des fondateurs d'août 2024, donc du code peut être antérieur à la société | Réunir toutes les cessions et régulariser avec effet rétroactif | **Avant l'ouverture de la data room** |
| **Marques** (règl. (UE) 2017/1001 ; CPI livre VII) | Oui | Recherche d'antériorités, dépôt dans les classes utiles, surveillance | Antériorité canadienne homonyme relevée (demande de 2014, secteur e-learning). Dépôts UE et FR **non déterminés**, bases inaccessibles | Non déterminé. Le signe est faiblement distinctif pour un constructeur de pages | Recherche d'antériorités professionnelle (INPI, EUIPO, TMview, WIPO) puis dépôt de marque de l'Union si le champ est libre | **Avant l'ouverture de la data room** |
| **Noms de domaine** | Oui | Titularité au nom de la société, verrouillage registrar, MFA, cohérence `.co` / `.dev` / `.com` | Non observé, WHOIS inaccessible | Non déterminé. Dispersion constatée dans les sources : `.co` pour la marque, `.dev` pour le MCP, un troisième domaine aperçu dans un résultat de recherche | Inventaire du portefeuille, transfert à la société si nécessaire | 30 j |
| **Open source** | Oui | Inventaire des dépendances et des licences, respect des obligations d'attribution, alerte sur le copyleft en SaaS | Non observé, dépôts non accessibles | Non déterminé. Le risque n'est pas le MIT, c'est une dépendance copyleft importée sans le savoir | SBOM et rapport de licences joints à la data room | 45 j |
| **Entité juridique et gouvernance** | Préalable à la moitié des lignes ci-dessus | Immatriculation, statuts, pays d'établissement, mentions légales publiées | **Non établi.** Contradiction non levée sur l'identité du fondateur entre une source interne et un profil public | **Bloquant pour la data room** | Obtenir l'extrait d'immatriculation, les statuts, la table de capitalisation, publier des mentions légales complètes | **Avant tout le reste** |

### 5 bis. Le risque TVA de l'article 9 bis, en une lecture

Il figure dans la matrice, il ne sera pas redéveloppé ici : **le fond appartient au livrable qui porte le volet paiements et fiscalité**, construit à partir de `audit/rapports/A02.md` (constats A02-017 à A02-024, et surtout A02-019). Ce qu'un lecteur du volet conformité doit en retenir, en trois phrases :

L'article 9 bis du règlement d'exécution (UE) n° 282/2011 présume qu'une plateforme par laquelle sont fournis des services électroniques agit **en son nom propre**, et la présomption devient **irréfragable** dès lors que la plateforme autorise la facturation au client, autorise la fourniture, **ou fixe les conditions générales** de la prestation. Si elle s'appliquait, **TinyPages serait redevable de la TVA de chaque pays d'acheteur sur l'intégralité du volume vendu par ses créateurs**, et non sur ses seuls abonnements. Trois faits, et trois seulement, déterminent la réponse : qui apparaît sur le reçu et sur la facture, qui fixe les CGV du tunnel d'achat, qui décide de la mise à disposition du fichier.

Aucun de ces trois faits n'a pu être observé. **À valider par un avocat fiscaliste** : c'est le premier sujet à mettre sur sa table, avant DAC7, avant la facturation électronique.

---

## 6. Divergences de dates : exposées, non tranchées

Trois divergences apparaissent dans les sources secondaires. Aucune source primaire n'a pu être ouverte. **Elles ne sont pas arbitrées ici**, et aucune ne doit être publiée dans la data room avant vérification sur la source officielle indiquée.

| Sujet | Lecture A | Lecture B | Incidence | Source officielle à consulter |
|---|---|---|---|---|
| **AI Act, genèse du règlement modificatif** | Un « accord provisoire trouvé au printemps 2026 » sur le paquet omnibus numérique | Proposition de la Commission du 19 novembre 2025, puis règlement (UE) 2026/1744 publié au JOUE le 24 juillet 2026, en vigueur le 27 juillet 2026 | Les deux récits peuvent se combiner. Ce qui doit être exact dans le dossier, c'est **le numéro et la date du règlement modificatif**, puisque c'est lui qui porte le délai de grâce du 2 décembre 2026 | **EUR-Lex**, notice du règlement modificatif et version consolidée du règl. (UE) 2024/1689 |
| **AI Act, art. 4** | Le règlement modificatif aurait **réécrit** l'art. 4, transformant l'obligation de garantir un niveau suffisant de maîtrise de l'IA en obligation de la **promouvoir** | L'art. 4 serait **resté inchangé** | Obligation de résultat ou de moyens. **Sans incidence pratique** : une note de sensibilisation datée et un registre de formation satisfont les deux lectures | **EUR-Lex**, art. 4 consolidé |
| **Facturation électronique France, position des ETI** | Sources officielles et majoritaires : au 1er septembre 2026, réception pour toutes les entreprises **et émission pour les grandes entreprises et les ETI** | Une source secondaire place les grandes entreprises (> 5 000 salariés) au 1er septembre 2026 et **les ETI (250 à 5 000 salariés) au 1er septembre 2027** | **Sans effet pratique pour TinyPages**, qui n'est ni grande entreprise ni ETI. Mais une date fausse dans un dossier d'investisseurs se paie cher, et ce calendrier a déjà été modifié par le passé | **impots.gouv.fr** et **Légifrance** (LF 2024 art. 91, CGI art. 289 bis) |
| **ViDA, référence du texte** | Un « règlement 2025/1539 » | Une **directive (UE) 2025/516** adoptée le 11 mars 2025 | Référence du texte dans le dossier. Aucun jalon immédiat | **EUR-Lex** |

Deux références reposent sur la mémoire du rédacteur et non sur une recherche, et sont signalées comme telles : l'arrêt Fashion ID (CJUE, C-40/17) sur la responsabilité conjointe liée aux modules sociaux, et la loi n° 2023-451 du 9 juin 2023 sur l'influence commerciale. À vérifier avant publication.

Plus largement : **aucune source primaire réglementaire n'a été ouverte.** Les numéros d'articles et de textes proviennent de sources secondaires rapportées par recherche et doivent être revérifiés sur la source officielle avant que ce livrable ne quitte le périmètre de l'audit. C'est particulièrement vrai du paysage AI Act, qui a bougé en juillet 2026, soit moins de deux mois avant cette analyse.

---

## 7. Échéances dures et écarts les plus coûteux

### 7.1 Le calendrier, sans interprétation

| Date | Texte | État au 18/09/2026 |
|---|---|---|
| 1er janvier 2026 | Facturation électronique B2B, Belgique (Peppol) | **Échue**, si l'entité est belge. Tolérance du 1er trimestre expirée |
| 2 août 2026 | AI Act, art. 50(1), information « vous parlez à une IA » | **Échue depuis 47 jours** |
| 1er septembre 2026 | Facturation électronique France, obligation de **réception** | **Échue depuis 17 jours**, si l'entité est française |
| **2 décembre 2026** | AI Act, art. 50(2), marquage lisible par machine des contenus générés | **11 semaines** |
| 12 janvier 2027 | Data Act, disparition complète des frais de changement de fournisseur | 16 semaines |
| 31 janvier 2027 | DAC7, déclaration annuelle, **si applicable** | 19 semaines |
| 1er septembre 2027 | Facturation électronique France, obligation d'**émission** pour les PME et TPE, donc la quasi-totalité des créateurs | 1 an |
| 2 décembre 2027 | AI Act, annexe III, haut risque | Probablement hors champ |
| 28 juin 2030 | European Accessibility Act, services préexistants | Horizon |

### 7.2 Les cinq écarts les plus coûteux

| # | Écart | Pourquoi il coûte le plus | Échéance |
|---|---|---|---|
| 1 | **TVA, présomption de l'article 9 bis non qualifiée** | Si la présomption s'applique, TinyPages est redevable de la TVA de chaque pays d'acheteur sur l'intégralité du volume vendu par ses créateurs, pas sur ses seuls abonnements. Le montant en jeu dépasse tout le reste du document réuni. Trois faits suffisent à trancher, aucun n'est établi | Note fiscale sous **30 j** |
| 2 | **Publication sans aucun contrôle côté serveur, garde-fous purement textuels, seuls refus serveur commerciaux** (M-010, M-011) | **CONFIRMÉ, reproductible en un appel.** Sur un domaine de marque partagé avec l'application, c'est le scénario d'abus de bout en bout : page publiée par l'IA, sans revue, sans limite de plan. Et toute affirmation de garde-fous dans la data room sera démentie devant l'auditeur | Contrôle serveur de publication et limites de débit sous **15 j** ; réécriture immédiate de toute formulation de garde-fou |
| 3 | **M-007, pages légales vides publiées et indexées, adossées à une capture d'emails sans double opt-in** | **CONFIRMÉ.** Touche chaque compte dès sa création, met les clients en écart RGPD (art. 12 à 14) du fait du gabarit de la plateforme, et se découvre en ouvrant un compte d'essai | Correctif produit sous **15 j** |
| 4 | **Domaine enregistrable partagé entre l'application et les sites clients** | SameSite inopérant, cookie tossing, CORS par suffixe, réputation mutualisée. Premier point regardé par un auditeur technique, et seule la séparation de domaine supprime la classe de risque. Se combine directement avec l'écart n° 2 | Verrouillage des cookies sous **15 j** · PSL : mois · Séparation de domaine : chantier daté |
| 5 | **AI Act, article 50** | 50(1) déjà en retard de 47 jours ; 50(2) à **11 semaines**, avec un marquage lisible par machine à implémenter dans le produit. La seule échéance dure qui approche et qui demande du développement | 50(1) : immédiat · 50(2) : **2 décembre 2026** |

Quatre écarts suivent de près et ne doivent pas être perdus de vue : l'**absence de contrat de sous-traitance (art. 28(3)) et de liste publiée des sous-traitants** (30 j, bloquant en due diligence), le **croisement PCI DSS** (bloc de code libre sur les comptes Pro contre critère d'éligibilité au SAQ A, § 2.4), le **paquet DSA hébergeur** (art. 11 à 18, applicables quelle que soit la taille, § 5), et la **titularité du code** (cessions des fondateurs et des prestataires, blocage de closing classique).

---

## 8. Ce qu'il faut réclamer maintenant

Par ordre de dépendance, parce que les premières conditionnent les suivantes.

1. **L'entité juridique** : extrait d'immatriculation, statuts, pays d'établissement, table de capitalisation, mentions légales. Tant qu'elle n'est pas établie, l'applicabilité de la facturation électronique, du représentant UE et de l'autorité chef de file reste indéterminée, et la contradiction sur l'identité du fondateur reste ouverte.
2. **Les trois faits du tunnel d'achat** : qui apparaît sur le reçu et la facture, qui fixe les CGV, qui décide de la mise à disposition du fichier. Ils commandent le risque le plus lourd du dossier.
3. **Le DPA créateurs**, ou les CGU qui en tiennent lieu, et la liste des sous-traitants avec leur rôle et leur localisation.
4. **Le schéma d'architecture** : hébergeur, CDN, WAF, régions de stockage, isolation des données entre créateurs au niveau de la base.
5. **Le dernier test d'intrusion** : périmètre (couvrait-il l'isolement multi-tenant ?), date, prestataire, constats, état de remédiation. Sans cette pièce, le questionnaire de sécurité reste majoritairement « inconnue ».
6. **Les réponses produit vérifiables sur un compte de test** : suppression définitive d'un contact **depuis l'interface** (le canal IA ne le permet pas, M-014), export complet, MFA, attributs des cookies, attribut `sandbox` du bloc de code. Et, sur un **compte Pro**, la seule question laissée ouverte par le screening : l'envoi d'emails est-il soumis à un contrôle autre que le mur de facturation ?
7. **Les cessions de droits** des fondateurs et des prestataires, et l'état des dépôts de marque.
8. **L'historique d'incidents, de réclamations d'abus et de retraits de contenu depuis le lancement.** La question sera posée telle quelle en due diligence : une réponse préparée vaut mieux qu'une découverte.

---

*Document produit en mode dégradé, à relire par le CTO, le CEO et l'avocat à mandater avant toute diffusion. Aucune ligne n'est un avis juridique. Aucune ligne n'est opposable à un tiers en l'état.*
