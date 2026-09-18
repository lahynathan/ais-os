# 08 — Questions des investisseurs et de leur auditeur technique

Data room TinyPages · **Version 2 du 18 septembre 2026** · Rédigé par l'orchestrateur de l'audit
Sources : `audit/rapports/A01.md` à `A09.md`, le contre-audit `audit/rapports/A11.md`, `audit/annexes/catalogue_mcp_tinypages.md`, `audit/annexes/screening_mcp_compte_test.md`, `audit/livrables/02` à `07`.
Lecteurs visés : le CEO et le CTO, pour préparer les entretiens de due diligence.

> ## ⛔ PIÈCE STRICTEMENT INTERNE — NE JAMAIS REMETTRE, SOUS AUCUNE FORME
>
> Ce document n'est pas un livrable de data room. **Il ne doit être remis ni aux investisseurs, ni à leur auditeur technique, ni à un conseil externe, ni en extrait, ni en pièce jointe.** Il contient le dispositif « formulation à ne pas dépasser », qui est un manuel de langage de négociation : sa remise accidentelle ferait porter la discussion non plus sur TinyPages, mais sur la manière dont TinyPages prépare ses réponses.
>
> Il est référencé **DR-008** à l'index de la data room, au niveau de diffusion **« Strictement interne »**, et il ne figure pas au sommaire remis. Il reste dans `audit/` et n'entre jamais dans le répertoire partagé de la data room, même dans un sous-dossier fermé.
>
> **Ce qui se remet, c'est la réponse — pas la préparation de la réponse.**

**Ce qui change depuis la version 1**, sur les objections du contre-audit A11 : les 50 questions de la version 1 et les 50 questions du contre-audit sont **fusionnées** en un corpus unique de **97 questions** ; le document est **réorganisé dans l'ordre d'une session de due diligence réelle** et non plus par domaine d'audit ; les blocs finance, contrat, gouvernance et assurance — presque vides en version 1, avec trois questions sur cinquante touchant au compte de résultat — sont écrits ; les quatre trous nommés par le contre-audit sont comblés : la question méta sur la crédibilité du dossier lui-même (Q52), la responsabilité produit (Q79), l'isolement objet à objet entre locataires (Q87), l'authentification des acheteurs et des élèves (Q88) ; le statut du chiffre de 104 actions est corrigé partout (Q53) ; le nombre de concurrents dotés d'un serveur MCP officiel passe de cinq à sept ; le constat M-018 est intégré (Q94).

---

## Comment lire ce document

Quatre-vingt-dix-sept questions, écrites comme les poserait un auditeur payé pour trouver des failles, pas comme les poserait un ami.

**Chaque question porte trois éléments distincts, qu'il ne faut jamais confondre :**

1. **Réponse factuelle aujourd'hui** — ce que TinyPages peut affirmer en s'appuyant sur ce que cet audit a réellement établi, avec le niveau de preuve associé ;
2. **Ce qui manque pour répondre pleinement** — l'écart entre cette réponse et une réponse complète ;
3. **Pièce justificative attendue** — le document qui transforme la réponse en preuve, et son responsable.

Lorsque la réponse honnête est mauvaise, elle figure quand même, suivie d'une **formulation à ne pas dépasser** : le libellé le plus favorable qui reste vrai. Une formulation plus flatteuse sera démentie, souvent en quelques minutes, et le démenti coûtera plus cher que l'aveu.

**Ordre de lecture.** Les blocs suivent l'ordre d'une session de due diligence réelle : on commence par le dossier lui-même et sa recevabilité, puis la société et le capital, puis les chiffres, puis le marché, puis le produit, et la technique vient après — pas avant. C'est l'ordre dans lequel les questions seront posées, et il n'est pas celui dans lequel un audit technique les a produites. **Les questions les plus dures restent en tête de lecture** : elles sont rassemblées dans le tableau ci-dessous, avec leur emplacement.

**Numérotation.** Les numéros de question sont des **identifiants stables**, hérités de la version 1 et repris tels quels par l'index de la data room. Ils ne suivent donc pas l'ordre de lecture : Q9 est dans le bloc 1, Q51 dans le bloc 0, Q87 dans le bloc 7. **Un numéro désigne toujours la même question d'une version à l'autre** ; changer la numérotation aurait cassé les 214 renvois de l'index. Les questions Q51 à Q97 sont celles apportées par la fusion avec le contre-audit.

**Conventions de preuve.** **CONFIRMÉ** n'est employé que pour ce qui a été **exécuté** : les dix-sept actions réellement appelées sur le compte connecté le 18 septembre 2026, dont la liste nominative figure en annexe de screening. L'inventaire du catalogue MCP n'est pas de ce niveau : c'est un **inventaire relevé par lecture de la description publiée par le serveur, non exécuté action par action** — source primaire mais déclarative, établissant l'existence et le nom des 104 actions, et rien de leur comportement. Tout le reste plafonne à **PROBABLE** : l'egress réseau était fermé sur `tinypages.co`, `docs.tinypages.co` et `mcp.tinypages.dev` pendant toute la phase de collecte, aucun test actif n'était autorisé, et aucun accès interne n'a été ouvert. **« Non déterminé à ce jour »** signifie exactement cela : l'audit n'a pas pu l'établir, ce n'est ni un aveu ni une dénégation.

**Une règle tenue partout :** une règle générale (Stripe, RGPD, DSA, TVA) n'est jamais présentée comme un constat sur TinyPages. Les deux registres sont séparés dans chaque réponse.

**Un rappel qui vaut pour les quatre-vingt-dix-sept :** ce document a été produit sans aucun accès interne, sans accès réseau et sans test actif, par une équipe interne à la société auditée. Ce n'est pas un rapport d'audit au sens professionnel du terme, et la première question du bloc 0 porte précisément là-dessus.

---

## Les douze questions les plus dures

À préparer en premier, et à préparer par écrit. Ce sont celles dont la réponse est aujourd'hui difficile, incomplète ou défavorable, et qu'un auditeur trouvera de toute façon — la plupart en moins de dix minutes, en ouvrant un compte gratuit ou en connectant le serveur MCP.

| # | Question | Ce qui la rend dure | Bloc |
|---|---|---|---|
| **Q2** | Qu'est-ce qui empêche techniquement votre IA de publier une page sans validation humaine ? | La réponse est « rien », et c'est **établi par test** sur le serveur de production | 8 |
| **Q87** | Peut-on lire ou modifier l'objet d'un autre locataire en fournissant son identifiant ? | **Jamais testé**, alors que le dossier désigne lui-même ce risque comme le principal, et que le test coûte quinze minutes | 7 |
| **Q3** | Vos garde-fous sont-ils appliqués par le serveur ou écrits dans un prompt ? | Les deux seuls contrôles serveur observés sont **commerciaux** | 8 |
| **Q52** | Votre audit dit n'avoir presque rien prouvé : pourquoi vous croire sur le reste ? | La question méta. Elle ne se gagne pas par un argument, seulement par la version 2 des livrables | 0 |
| **Q4** | Quelle est votre exposition TVA si l'article 9 bis s'applique ? | Le risque le plus lourd du dossier, et il n'est **pas qualifié** | 5 |
| **Q64** | Présentez-vous du chiffre d'affaires brut ou une commission nette ? | Plusieurs **ordres de grandeur sur l'ARR affiché**, et le motif classique de renégociation de valorisation | 2 |
| **Q9** | Quelle est l'entité juridique qui lève, et qui en sont les fondateurs ? | Non établie, et une contradiction de sources n'est pas levée. Aucune formulation ne rattrape cette pièce | 1 |
| **Q1** | Comment maintenez-vous l'affirmation « seule plateforme » ? | **Sept** concurrents publient un serveur MCP officiel. L'affirmation n'est pas maintenable | 3 |
| **Q5** | Pourquoi la politique de confidentialité publiée par défaut est-elle vide ? | Établi, non contestable, et **vérifiable en cinq minutes** par quiconque ouvre un compte | 10 |
| **Q6** | Comment isolez-vous les cookies de session sur un domaine enregistrable partagé ? | Le premier point que regardera l'auditeur technique, et rien n'a pu être relevé | 7 |
| **Q88** | Comment s'authentifient les acheteurs et les élèves ? | **Angle mort complet** : les seuls utilisateurs qui paient n'ont jamais été examinés | 7 |
| **Q7** | Avez-vous une batterie d'évals et une politique de gel de version ? | Le risque produit le plus structurant : des garde-fous en langage naturel sur un modèle tiers mis à jour sans préavis | 8 |

**Quatre questions sortent de cette liste par rapport à la version 1** — Q8, Q10, Q11 et Q12 — non parce qu'elles sont devenues faciles, mais parce que quatre questions nouvelles sont plus dures qu'elles. Elles restent intégralement traitées dans leur bloc.

---

# Bloc 0 — Le dossier lui-même et sa recevabilité

Un auditeur sérieux ne commence pas par le produit : il commence par jauger la pièce qu'on lui remet. Ces six questions portent sur le dossier, pas sur TinyPages. Elles doivent être traitées **spontanément, en ouverture**, et non subies.

---

**Q51 — « Ce rapport a-t-il été produit par un tiers indépendant ? Qui l'a commandé, qui l'a relu, et qui pouvait en modifier une conclusion ? »**
*Domaine : le dossier lui-même · Constats : contre-audit A11 B-5, `annexes/methodologie.md` §1*

- **Réponse factuelle aujourd'hui.** Non. Ces travaux sont une **auto-évaluation produite en interne** : le commanditaire, l'audité, le relecteur et l'unique responsable de toutes les remédiations sont la même personne. Les dérogations d'exécution ont été accordées par le dirigeant de la société auditée. Aucun tiers indépendant n'est intervenu. Le dispositif — un orchestrateur et onze sous-agents — est décrit en méthodologie, mais le mot « audit » et le vocabulaire employé (constats, statuts, opinion, verdict) peuvent laisser croire à un travail de tiers : c'est une ambiguïté qu'il faut lever avant qu'on ne la lève pour nous.
- **Ce qui manque pour répondre pleinement.** Un bandeau explicite en tête de la synthèse et de la méthodologie, et un mandat externe sur les deux points critiques — harnais IA et isolement multi-locataire — dont le rapport sera joint.
- **Pièce justificative attendue.** Lettre de mission ou note de cadrage précisant qui commande, qui exécute, qui relit et qui peut modifier une conclusion ; mandat signé du prestataire externe. **CEO.**
- **Formulation à ne pas dépasser.** « Ce dossier est une auto-évaluation interne, assumée comme telle. Elle ne constitue pas un rapport d'audit au sens professionnel du terme. Nous finançons un tiers sur les deux points que nous jugeons critiques, et son rapport sera versé sans filtre. » Toute réponse qui laisse planer l'indépendance transforme une question de contenu en question d'intention, et c'est beaucoup plus cher.

---

**Q52 — « Votre propre audit dit n'avoir presque rien prouvé : aucune page de votre produit ouverte, aucun en-tête, aucun DNS, aucune pièce d'entreprise. Pourquoi devrions-nous vous croire sur le reste ? »**
*Domaine : le dossier lui-même · Constats : contre-audit A11 §7 étape 3, `annexes/methodologie.md` §8, livrable 09 §13*

- **Réponse factuelle aujourd'hui.** C'est exact, et c'est la question la mieux posée qu'un investisseur puisse poser. La base de preuve du dossier tient en deux sources : l'inventaire du catalogue MCP **relevé par lecture de la description publiée par le serveur**, et dix-sept actions réellement exécutées sur le compte connecté. Tout le reste plafonne à PROBABLE, parce que l'egress réseau était fermé sur tous les domaines TinyPages pendant toute la collecte. La réponse défendable n'est pas un argument, c'est un fait de méthode : **le dossier annonce ses limites en tête de chaque livrable, et il établit par test le constat qui le dessert le plus** (Q2). Un dossier qui fabriquerait sa crédibilité n'aurait pas produit celui-là.
- **Ce qui manque pour répondre pleinement.** Le rejeu de la phase de collecte avec l'accès réseau ouvert : pages du produit, en-têtes HTTP, relevés DNS, journaux de certificats, Public Suffix List, scores de performance, pages tarifaires, pages concurrentes. Une demi-journée d'outils referme la moitié des « Non déterminé », dont les quatre relevés qui décident si trois risques majeurs sont théoriques ou réels. Tant que ce rejeu n'a pas eu lieu, ces documents sont une note de cadrage assortie d'un sondage, pas un dossier de data room.
- **Pièce justificative attendue.** Version 2 des livrables, datée, portant les relevés techniques réellement effectués, et le tableau des « Non déterminé » refermés. **CTO.**
- **Formulation à ne pas dépasser.** « Vous avez raison de le relever : la version 1 a été produite sans accès réseau. Voici la version 2, datée du [date], avec les relevés. Ce qui n'a pas changé entre les deux versions, ce sont les constats établis par exécution — ils sont les plus défavorables du dossier et nous ne les avons pas retirés. » Ne jamais répondre à cette question par une défense de la méthode : y répondre par la version 2.

---

**Q53 — « Sur les 104 actions que vous annoncez, combien ont été réellement exécutées pendant votre audit ? »**
*Domaine : le dossier lui-même · Constats : contre-audit A11 B-3, `annexes/catalogue_mcp_tinypages.md`, `annexes/screening_mcp_compte_test.md`*

- **Réponse factuelle aujourd'hui.** **Dix-sept.** Le chiffre de 104 est un **inventaire relevé par lecture de la description publiée par le serveur, non exécuté action par action** : c'est une source primaire, mais déclarative — c'est TinyPages qui décrit TinyPages. L'existence et le nom des 104 actions sont établis ; le comportement ne l'est que pour les dix-sept exécutées, dont la liste nominative et les réponses obtenues figurent en annexe de screening : `get_account`, `list_accounts`, `list_contacts`, `list_webpages`, `list_products`, `list_emails`, `get_webpage`, `get_business_context`, `get_analytics_summary`, `list_forms`, `list_templates`, `create_webpage` (avec et sans bloc de code), `publish_webpage`, `unpublish_webpage`, `create_email`, `send_email`, `update_business_context`. Aucune des 87 autres n'a été confrontée à son schéma.
- **Ce qui manque pour répondre pleinement.** L'export daté du catalogue complet avec les schémas JSON, qui transforme un relevé de noms en inventaire vérifiable, et le journal des appels de la session d'audit.
- **Pièce justificative attendue.** Export du catalogue avec schémas (DR-109), liste nominative des actions exécutées avec leur réponse, journal des appels horodaté. **CTO.**
- **Formulation à ne pas dépasser.** « Cent quatre est le nombre d'actions que notre serveur publie dans sa propre description ; dix-sept ont été exécutées pendant l'audit et leur liste est jointe. Nous ne présentons pas ce chiffre comme vérifié action par action. » **Ne jamais écrire « inventaire exécuté » ni « étendue réelle et vérifiée » :** un auditeur qui appelle `search_actions` et trouve un décompte différent fait tomber, en une minute, le seul actif que le dossier présente comme solide.

---

**Q54 — « Votre seule preuve solide vient d'une session sur le compte de production de votre dirigeant, sous une dérogation orale contraire à vos propres paramètres écrits. Comment la reproduisez-vous ? »**
*Domaine : le dossier lui-même · Constats : contre-audit A11 B-4, C-026, `annexes/screening_mcp_compte_test.md` réserve*

- **Réponse factuelle aujourd'hui.** Le constat de recevabilité est fondé et il est consigné au registre des contradictions par l'audit lui-même. Les tests M-010 à M-018 ont été conduits sous dérogation orale, sur un compte présenté comme « compte de test dédié » qui portait en réalité le nom du dirigeant, son sous-domaine et son adresse personnelle, avec publication effective d'une URL publique et trois traces résiduelles. **Le fond est reproductible en une heure ; c'est la forme qui est attaquable.** Un contradicteur n'a pas besoin de discuter le résultat : il conteste la recevabilité, et il a raison de le faire tant que le rejeu n'existe pas.
- **Ce qui manque pour répondre pleinement.** Une dérogation écrite, datée, signée, avec périmètre, bénéficiaire, compte visé et fenêtre horaire ; les paramètres d'audit et le journal mis en cohérence ; un locataire dédié gratuit **et** un locataire Pro ; le **rejeu horodaté du protocole M-007, M-010, M-011 et M-018** sur ce locataire, relevé joint ; et la capture de la suppression des trois traces.
- **Pièce justificative attendue.** Relevé de rejeu horodaté, dérogation signée, capture du nettoyage, paramètres d'audit corrigés. **CTO + CEO.**
- **Formulation à ne pas dépasser.** « La session d'origine a été conduite sur un compte qui n'était pas le banc d'essai annoncé, et nous l'avons écrit nous-mêmes dans le dossier. La pièce que nous vous remettons n'est pas cette session : c'est son rejeu sur un locataire dédié, daté du [date]. » Présenter le rejeu comme la pièce principale, jamais la session d'origine.

---

**Q55 — « Qu'est-ce qui a changé dans ce dossier depuis le 18 septembre 2026 ? »**
*Domaine : le dossier lui-même · Constats : livrable 07, `journal/execution.md`*

- **Réponse factuelle aujourd'hui.** À la date de rédaction, rien n'est encore clos : le contrôle qualité final du journal d'exécution porte trois cases acquises sur sept, et quatre objections bloquantes du contre-audit restent ouvertes. C'est une réponse acceptable **une seule fois**, le jour de l'ouverture. Elle cesse de l'être à la deuxième réunion.
- **Ce qui manque pour répondre pleinement.** Un tableau de suivi ligne à ligne : pour chaque item de P0, l'état, la date de livraison, la preuve jointe et son auteur. C'est la pièce qui distingue une équipe qui a écrit un plan d'une équipe qui l'exécute, et c'est celle que l'investisseur relira entre deux réunions.
- **Pièce justificative attendue.** Journal de remédiation P0 avec références de livraison, captures et dates ; contrôle qualité final avec ses sept cases cochées après vérification réelle. **CEO + CTO.**

---

**Q56 — « Quels constats avez-vous écartés en cours de route, et pourquoi ? »**
*Domaine : le dossier lui-même · Constats : `annexes/methodologie.md` §7, registre des preuves*

- **Réponse factuelle aujourd'hui.** Le dossier peut y répondre, et c'est l'une de ses meilleures réponses : quatre corrections d'agents sont documentées en méthodologie, une source a été écartée pour hallucination, et une combinaison de risque a été invalidée par le test lui-même — l'abus fondé sur « plan gratuit plus code personnalisé » ne tient pas, le bloc de code étant réservé au plan payant (M-012). Le risque a été reformulé en « plan gratuit plus pilotage par IA plus publication sans contrôle », qui est plus étroit et qui, lui, tient.
- **Ce qui manque pour répondre pleinement.** Rien sur le fond ; il faut seulement les montrer **spontanément**, en début d'entretien, plutôt que sous la question. Un auditeur qui entend un constat abandonné avant de l'avoir cherché relit tout le reste avec moins de méfiance.
- **Pièce justificative attendue.** Méthodologie §7 et registre des preuves, remis tels quels. **CTO.**

---

# Bloc 1 — Société, capital, gouvernance et personnes

Le bloc qu'un fonds traite en premier, et sur lequel cet audit n'a **rien** pu voir : aucune pièce d'entreprise n'a été ouverte. Tout est ici à produire, et rien ne peut être qualifié tant que l'entité ne l'est pas.

---

**Q9 — « Quelle est l'entité juridique qui lève des fonds, et qui en sont les fondateurs ? »**
*Domaine : conformité · Constats : A06-025, A09-016, A09-015, A01-005*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et c'est bloquant pour la data room. Les paramètres de l'audit désignent Nathan Lahy comme CEO et CTO. Des sources publiques secondaires présentent Emilio Abril comme fondateur de tinypages.co et un « Jerome V. » comme co-fondateur et CTO. Une société française « TINY PAGE » (SIREN 905170916, Le Havre) apparaît dans les résultats de recherche, **sans lien établi** avec la marque TinyPages : le nom est au singulier, la page n'a pas pu être ouverte. Aucune base officielle d'entreprises n'a été consultable. Tant que l'entité n'est pas établie, l'applicabilité de la facturation électronique, du régime TVA, de l'obligation de représentant dans l'Union et la détermination de l'autorité de contrôle compétente restent indéterminées.
- **Ce qui manque pour répondre pleinement.** L'identité exacte de l'entité et son pays d'établissement, la répartition du capital, et la levée de la contradiction sur l'identité des fondateurs. Accessoirement : `emilio.tinypages.co` est-il le site de démonstration du fondateur ou un client tiers ? Cette question conditionne son usage comme preuve de traction.
- **Pièce justificative attendue.** Extrait d'immatriculation, statuts, table de capitalisation, pacte d'associés, mentions légales publiées. **CEO.**
- **Formulation à ne pas dépasser.** Aucune formulation ne rattrape cette pièce : elle se produit ou le dossier ne s'ouvre pas.

---

**Q11 — « Qui détient les droits sur le code, et pouvez-vous le prouver ? »**
*Domaine : conformité · Constats : A06-028, A06-026, A06-029*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. La chronologie publique situe l'association des fondateurs en août 2024 et le lancement en janvier 2025 : une partie du code peut donc être antérieure à la constitution de la société. La règle générale, qui n'est pas un constat sur TinyPages : les droits patrimoniaux sur un logiciel créé par un salarié dans l'exercice de ses fonctions sont dévolus de plein droit à l'employeur, mais cette dévolution ne joue ni pour les prestataires indépendants, ni pour les fondateurs avant la création de la société, ni pour les stagiaires — il faut un acte de cession écrit. C'est un point de blocage classique de closing. Sur la marque, une antériorité canadienne homonyme « TinyPages / Tiny Pages » (demande n° 1668188, déposée le 17 mars 2014, secteur e-learning) ressort d'une recherche ; aucun dépôt français ou européen au nom de TinyPages n'a pu être trouvé ni infirmé, les bases étant inaccessibles. Aucun inventaire de licences open source n'existe.
- **Ce qui manque pour répondre pleinement.** Les actes de cession, l'état réel des dépôts de marque, et une nomenclature logicielle avec rapport de licences.
- **Pièce justificative attendue.** Actes de cession des fondateurs à la société couvrant le code antérieur, clauses de cession dans tous les contrats de prestataires et de freelances, certificats de dépôt de marque, recherche d'antériorités professionnelle, SBOM et rapport de licences. **CEO + avocat, CTO pour le SBOM.**

---

**Q60 — « Que prévoit le pacte au départ du fondateur : vesting, good et bad leaver, non-concurrence, transfert des accès ? »**
*Domaine : société et gouvernance · Constats : contre-audit A11 §B, DR-024, DR-025*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Ni le pacte, ni la table de capitalisation, ni aucun tableau de vesting n'a pu être vu ; l'entité elle-même n'est pas établie (Q9). Sur une équipe d'une personne, ces clauses ne sont pas un détail de négociation : elles sont la seule chose qui protège l'investisseur contre le scénario qu'il finance.
- **Ce qui manque pour répondre pleinement.** Le pacte et le tableau de vesting, et leur cohérence avec une équipe d'un seul associé opérationnel : un vesting sur un fondateur unique n'a de sens que couplé à un mandat de continuité (Q58) et à la détention des accès par la société (Q57).
- **Pièce justificative attendue.** Pacte d'associés, table de capitalisation, tableau de vesting, clauses de non-concurrence et de transfert des accès. **CEO + avocat *(à mandater)*.**

---

**Q57 — « Qui détient les comptes fournisseurs critiques — registrar, prestataire de paiement, hébergeur, fournisseur d'emails, fournisseur de modèle : la société ou une personne physique ? »**
*Domaine : société et gouvernance · Constats : contre-audit A11 §B, A05-011, DR-032*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Aucun inventaire de comptes fournisseurs n'a pu être vu. Le signal indirect est défavorable : le compte fourni à l'audit comme compte de test porte le nom, le sous-domaine et l'adresse personnelle du dirigeant (CONFIRMÉ, réserve du screening), ce qui décrit une organisation où le personnel et le professionnel ne sont pas séparés. Ce n'est pas une preuve sur les comptes fournisseurs, c'en est un indice, et l'auditeur le lira ainsi.
- **Ce qui manque pour répondre pleinement.** L'inventaire des comptes, de leurs titulaires et de leurs facteurs d'authentification ; l'emplacement des codes de secours ; et une procédure écrite de reprise d'accès. Le cas le plus coûteux est le registrar : un nom de domaine détenu à titre personnel est un point de rupture qu'aucun montage juridique ne rattrape en urgence.
- **Pièce justificative attendue.** Inventaire des comptes et de leurs titulaires, procédure de recouvrement d'accès, attestation de dépôt des secrets en coffre partagé. **CTO + CEO.**
- **Formulation à ne pas dépasser.** « Les comptes critiques sont au nom de la société, avec authentification forte et codes de secours en coffre partagé ; voici l'inventaire. » Si ce n'est pas le cas, le dire et donner la date de régularisation : c'est un chantier de quelques heures, et une découverte en due diligence coûte beaucoup plus que l'aveu.

---

**Q58 — « Que se passe-t-il concrètement si votre dirigeant est indisponible trois semaines ? »**
*Domaine : société et gouvernance · Constats : contre-audit A11 §B, risque « personne clé » du livrable 06, `00_parametres.md`*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et la structure rend la question centrale : les paramètres de l'audit désignent la même personne comme CEO et CTO, et le dossier ne connaît aucun autre responsable. Le point mérite une réserve d'honnêteté : des sources publiques nomment deux cofondateurs dont un directeur technique distinct, et cette incohérence n'est pas levée (Q9). Tant qu'elle ne l'est pas, le risque « personne clé » doit être présenté au statut PROBABLE, pas CONFIRMÉ.
- **Ce qui manque pour répondre pleinement.** Un mandat de continuité écrit, un tiers capable de déployer et de restaurer, des runbooks à jour, et la liste des accès délégués. C'est la contrepartie normale d'un investissement dans une équipe très réduite : l'investisseur ne demande pas de supprimer le risque, il demande qu'il soit organisé.
- **Pièce justificative attendue.** Plan de continuité nominatif, runbooks, liste des accès délégués, contrat du tiers de secours le cas échéant. **CEO.**

---

**Q59 — « Avez-vous des prestataires récurrents dont TinyPages représente l'essentiel de l'activité ? Quel est le risque de requalification ? »**
*Domaine : société et gouvernance · Constats : contre-audit A11 §B*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour : aucun contrat de prestation, aucun grand livre fournisseurs n'a été vu. La question est standard et son enjeu est double : un passif social potentiel, et la titularité du code écrit par ces prestataires, qui rejoint Q11.
- **Ce qui manque pour répondre pleinement.** La liste des prestataires récurrents, les volumes facturés, les durées de relation, les clauses d'exclusivité, de subordination et de cession de droits, et une analyse assumée du risque de requalification.
- **Pièce justificative attendue.** Contrats de prestation, grand livre fournisseurs sur 24 mois, note d'analyse. **CEO + avocat *(à mandater)*.**

---

**Q61 — « Quelle part de votre code est générée par IA ? Avez-vous une règle sur la titularité et les licences des sorties de modèle ? »**
*Domaine : société et propriété intellectuelle · Constats : contre-audit A11 §B, A06-028, DR-033, DR-034*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Le dépôt de code n'a pas été ouvert, aucune nomenclature logicielle n'existe et aucun rapport de licences n'a été produit. La question est nouvelle en due diligence et elle est posée systématiquement depuis 2026 sur les produits construits vite : le risque n'est pas la génération elle-même, c'est qu'une sortie de modèle reproduise du code sous licence contraignante et importe un copyleft dans une offre en ligne sans que personne ne le sache.
- **Ce qui manque pour répondre pleinement.** Une politique écrite d'usage de l'IA en développement, la règle de revue humaine, un scan de licences sur l'ensemble des dépendances, et une position sur la titularité des sorties de modèle.
- **Pièce justificative attendue.** Politique interne d'usage de l'IA en développement, nomenclature logicielle (SBOM) et rapport de licences, accès en lecture au dépôt pour l'auditeur mandaté. **CTO.**

---

**Q62 — « Litiges, mises en demeure, réclamations de créateurs, demandes d'autorité, avertissements de votre prestataire de paiement depuis le lancement ? »**
*Domaine : société et juridique · Constats : contre-audit A11 §B, A02-028, A02-029*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et l'audit n'a aucun moyen de le savoir : rien de tout cela n'est public. La question sera posée telle quelle et la seule mauvaise réponse est l'approximation. Le point de vigilance identifié par le dossier : le cœur de cible longe la liste des activités restreintes du prestataire de paiement (Q23), ce qui rend un avertissement ou une suspension plausible et fait de cette question un passage obligé.
- **Ce qui manque pour répondre pleinement.** Un registre exhaustif, même vide, daté et attesté, couvrant les réclamations de créateurs, les courriers d'avocats, les demandes d'autorités et les échanges avec le prestataire de paiement.
- **Pièce justificative attendue.** Registre des réclamations et courriers, attestation du dirigeant sur l'exhaustivité. **CEO.**

---

**Q63 — « Êtes-vous assurés en responsabilité civile professionnelle et en cyber ? Quels plafonds, et quelles exclusions relatives à l'IA ? »**
*Domaine : société et juridique · Constats : contre-audit A11 §B, DR-108*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour ; aucune attestation n'a été vue, et le domaine gouvernance du questionnaire de sécurité reste sans réponse sur ce point. Le sujet n'est pas l'existence des polices, c'est leur contenu : sur un produit génératif, **les exclusions sont le vrai sujet**. Beaucoup de contrats excluent aujourd'hui les dommages causés par un contenu produit automatiquement, ce qui vide la couverture précisément là où le risque du dossier est concentré (Q79).
- **Ce qui manque pour répondre pleinement.** Les attestations, les plafonds, les franchises, et la lecture des exclusions relatives à l'IA générative et à l'hébergement de contenus de tiers.
- **Pièce justificative attendue.** Attestations RC professionnelle et cyber, conditions particulières, note du courtier sur les exclusions IA. **CEO.**

---

# Bloc 2 — Finance, unit economics et trésorerie

Un fonds passe la moitié du temps ici. Aucun de ces chiffres ne peut être produit par un audit externe : ils viennent tous du tableau de bord interne, des relevés fournisseurs et de la comptabilité. **Aucun n'existe dans ce dossier à ce jour.**

---

**Q64 — « Présentez-vous les ventes de vos créateurs en chiffre d'affaires brut, ou seulement votre commission ? Qui a validé la qualification principal ou agent ? »**
*Domaine : finance · Constats : contre-audit A11 M-10, A02-019, A06-020*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et c'est le **corollaire comptable direct de Q4** que le dossier n'avait pas posé. Les trois mêmes faits qui déclenchent la présomption de l'article 9 bis — qui figure sur le reçu, qui fixe les conditions générales, qui décide de la mise à disposition du fichier — commandent aussi la qualification principal ou agent, donc la méthode de reconnaissance du revenu. L'écart entre les deux présentations n'est pas cosmétique : **volume brut vendu par les créateurs contre commission nette, c'est plusieurs ordres de grandeur sur l'ARR affiché**, et c'est un motif classique de renégociation de valorisation en cours de due diligence.
- **Ce qui manque pour répondre pleinement.** La position écrite de l'expert-comptable sur la qualification, sa cohérence explicite avec la position prise sur l'article 9 bis et avec celle prise en droit de la consommation (Q81), et la méthode de reconnaissance du revenu effectivement appliquée dans les comptes.
- **Pièce justificative attendue.** Note comptable de qualification, liasse fiscale, rapprochement avec le journal des ventes. **CEO + expert-comptable *(fonction non attribuée à ce jour)*.**
- **Formulation à ne pas dépasser.** « Nous présentons notre revenu en commission nette. La qualification principal ou agent fait l'objet d'une note de notre expert-comptable, cohérente avec notre position TVA et avec notre position en droit de la consommation : les trois reposent sur le même jeu de faits. » **Ne jamais présenter un volume brut comme un chiffre d'affaires**, même en note de bas de page : la correction se fera devant vous, et elle emportera la confiance dans le reste des chiffres.

---

**Q65 — « Donnez le revenu récurrent mensuel, sa décomposition abonnements et commissions, la part du plan gratuit, et la définition exacte de chacun de ces termes. »**
*Domaine : finance · Constats : contre-audit A11 §C, A09-019, DR-037, DR-040*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Aucun chiffre de traction n'a pu être établi par l'audit et aucun ne peut l'être de l'extérieur : ils viennent tous du tableau de bord interne et des relevés fournisseurs. Le piège de cette question n'est pas le chiffre, c'est la **note de définitions** : une commission encaissée une fois n'est pas un revenu récurrent, un compte gratuit n'est pas un client, et un investisseur qui trouve deux définitions différentes dans deux documents cesse de lire les chiffres.
- **Ce qui manque pour répondre pleinement.** Le revenu récurrent mensuel et sa décomposition, la part des comptes gratuits, et une note de méthode définissant chaque terme, remise **avec** les chiffres et non après.
- **Pièce justificative attendue.** Export du tableau de bord, note de définitions, réconciliation avec les exports du prestataire de paiement. **CEO *(fonction financière non attribuée)*.**

---

**Q66 — « Rapprochez le revenu que vous annoncez et les encaissements réels des douze derniers mois. »**
*Domaine : finance · Constats : contre-audit A11 §C, DR-035*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour : aucun compte annuel, aucune situation intermédiaire, aucun relevé bancaire et aucun export du prestataire de paiement n'a été vu par cet audit. Le dossier ne se prononce donc ni sur l'existence d'un passif, ni sur la sincérité des chiffres annoncés — et il ne doit se prononcer sur ni l'un ni l'autre tant que ces pièces ne sont pas produites.
- **Ce qui manque pour répondre pleinement.** Un tableau de rapprochement au centime entre le revenu annoncé, les encaissements du prestataire de paiement et les relevés bancaires, avec chaque écart expliqué — réserve immobilisée, remboursements, litiges, décalage de versement.
- **Pièce justificative attendue.** Exports du prestataire de paiement sur douze mois, relevés bancaires, tableau de rapprochement, comptes annuels et situation intermédiaire. **CEO *(fonction financière non attribuée)* + expert-comptable.**

---

**Q10 — « Combien de comptes actifs, de sites publiés et quel revenu récurrent ? »**
*Domaine : marché · Constats : A09-019, A09-020, A01-012*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Aucune métrique publique fiable n'a été trouvée : le trafic estimé n'a pas pu être relevé, le nombre de sites clients non plus — la découverte par les journaux de certificats était bloquée. Trois sous-domaines clients seulement sont connus, dont l'un porte le prénom du fondateur présumé et pourrait être un site de démonstration interne. La seule trace publique de volume est un ordre de trente-quatre avis sur une plateforme d'avis, avec une note rapportée de façon contradictoire entre deux relevés du même jour : aucun chiffre de note n'est citable en l'état.
- **Ce qui manque pour répondre pleinement.** Tout le socle chiffré : comptes actifs, sites publiés, revenu récurrent mensuel et sa décomposition entre abonnements et commissions, rétention, part des comptes gratuits, volume brut vendu par les créateurs. Aucun de ces chiffres ne peut être produit par un audit externe ; ils viennent tous du tableau de bord interne et des relevés fournisseurs.
- **Pièce justificative attendue.** Export du tableau de bord interne, relevés Stripe, cohorte de rétention sur douze mois, et comptage des sous-domaines actifs. **CEO** (la fonction financière n'est attribuée à personne dans `audit/00_parametres.md` : ce point est à trancher avant la data room).
- **Formulation à ne pas dépasser.** Un audit externe qui n'a produit aucun chiffre de traction n'est pas un argument : ces chiffres existent en interne et doivent être publiés dans la data room avec leur méthode de calcul.

---

**Q67 — « Churn logo et churn revenu par cohorte mensuelle depuis janvier 2025, et rétention nette. »**
*Domaine : finance · Constats : contre-audit A11 §C, DR-039*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Aucune donnée de rétention n'existe dans le dossier, et la chronologie publique situe le lancement en janvier 2025 : les cohortes disponibles sont donc jeunes, ce qui est une information en soi et non une excuse.
- **Ce qui manque pour répondre pleinement.** Les cohortes brutes, mois par mois, en logo et en revenu, avec la rétention nette — **pas une moyenne**. Une moyenne de churn sur un parc en croissance rapide est un chiffre qui ne veut rien dire, et sa remise sera lue comme une tentative de lissage.
- **Pièce justificative attendue.** Table de cohortes et requête ou export source permettant de la recalculer. **CEO *(fonction financière non attribuée)*.**

---

**Q68 — « Quelle part de votre revenu vient de vos dix plus gros comptes ? Quelle part vient de l'audience personnelle du fondateur ? »**
*Domaine : finance · Constats : contre-audit A11 §C, A03-Q6, A09-020*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Deux signaux publics, tous deux au statut PROBABLE, imposent que la seconde moitié de la question soit anticipée : une page du fondateur mentionne une liste personnelle de plus de 70 000 contacts avec une migration annoncée vers TinyPages (Q29), et l'un des trois sous-domaines clients connus porte le prénom du fondateur présumé (Q77). Un investisseur qui découvre lui-même que l'acquisition repose sur l'audience personnelle du dirigeant en tire une conclusion sur la reproductibilité de la croissance, et il la tire seul.
- **Ce qui manque pour répondre pleinement.** La concentration sur les dix premiers comptes, la part du revenu dont l'origine d'acquisition est l'audience du fondateur, et l'aveu explicite si cette part est élevée.
- **Pièce justificative attendue.** Top comptes anonymisés avec leur part de revenu, rapport d'acquisition par origine. **CEO.**
- **Formulation à ne pas dépasser.** « Une part de [X] % de notre revenu vient de comptes acquis par l'audience personnelle du fondateur. C'est un actif réel et un risque de concentration ; voici ce que nous construisons pour le diversifier. » Présenter cette audience comme un canal d'acquisition sans dire qu'elle est personnelle est le type d'omission qui se découvre en une recherche.

---

**Q69 — « Burn mensuel, trésorerie disponible, horizon de financement à la date de remise ? »**
*Domaine : finance · Constats : contre-audit A11 §C, DR-044*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et le dossier ne contient **aucun élément financier** : ni burn, ni trésorerie, ni plan de financement, ni dimensionnement du tour. C'est le manque le plus visible d'un dossier destiné à une levée, et il ne se comble par aucun travail d'audit technique.
- **Ce qui manque pour répondre pleinement.** Un plan de trésorerie tenu, mois par mois, intégrant explicitement l'hypothèse de renfort du plan de remédiation (Q70) : le plan P0 plus P1 pèse 196 jours-personne, soit 39 semaines pour une seule personne, et cette charge doit apparaître en trésorerie avant d'apparaître en calendrier.
- **Pièce justificative attendue.** Plan de trésorerie, situation comptable intermédiaire, dimensionnement du tour et usage des fonds. **CEO *(fonction financière non attribuée)*.**

---

**Q16 — « Quel est votre coût d'infrastructure par compte actif et votre marge brute par plan ? »**
*Domaine : infrastructure · Constats : 02 §10.2, A02-016, A04-029*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Un seul coût unitaire a pu être approché par l'audit, celui de l'acheminement des emails (voir Q25), et il repose sur des tarifs publics relevés par des tiers, pas sur la facture réelle. Le coût d'hébergement par site publié, le coût de diffusion vidéo par heure visionnée, le coût d'inférence par action pilotée par l'IA, la part des frais de paiement revenant à TinyPages et la marge brute par plan sont tous « Non déterminé ». Aucun n'a été estimé : les estimer à partir des sources disponibles reviendrait à fabriquer des chiffres.
- **Ce qui manque pour répondre pleinement.** Les factures fournisseurs des douze derniers mois et un modèle de coûts unitaires reliant chaque poste au nombre de comptes actifs.
- **Pièce justificative attendue.** Factures hébergeur, vidéo, email, fournisseur d'IA, et modèle de marge brute par plan. **CEO et CTO.**

---

**Q25 — « Un créateur à grosse liste vous coûte-t-il plus cher que ce qu'il vous paie ? »**
*Domaine : emails · Constats : A03-002, A03-003, 02 §10.1, A07-006, A07-007*

- **Réponse factuelle aujourd'hui.** L'audit produit un ordre de grandeur, pas un fait. Aux tarifs publics 2026 du fournisseur d'envoi supposé, relevés via des agrégateurs tiers faute d'accès à la page officielle (PROBABLE) : trois plans à 10 000 emails inclus, de 15 à 18 $ par mois, avec un dépassement de 1,20 à 1,80 $ par millier. Sur une **hypothèse explicite et non vérifiée** — un créateur de 70 000 contacts envoyant une newsletter hebdomadaire à sa liste complète, soit 280 000 emails par mois — le coût mensuel d'acheminement ressort entre 342 et 501 $ selon le plan, soit 1,22 à 1,79 $ par millier. Une source secondaire situe le plan payant de TinyPages à 99 $ par mois avec des paliers de contacts (PROBABLE, jamais vérifié). Si ces deux chiffres se confirmaient, l'acheminement seul du profil décrit coûterait plusieurs fois l'abonnement de base. **Cela ne démontre pas une marge négative** : les paliers de contacts peuvent précisément servir à recouvrir ce coût, et rien n'établit qu'un créateur de ce profil existe dans le parc.
- **Ce qui manque pour répondre pleinement.** La grille tarifaire officielle, le plan fournisseur réellement souscrit, la facture des douze derniers mois, la distribution des tailles de liste et la fréquence d'envoi moyenne du parc.
- **Pièce justificative attendue.** Facture du fournisseur d'envoi sur douze mois, grille tarifaire officielle TinyPages, export de la distribution des listes. **CTO + CEO.** C'est l'un des premiers calculs qu'un investisseur refera lui-même : il vaut mieux le lui donner fait, avec ses hypothèses.

---

**Q71 — « Quel est le coût d'inférence d'une session de pilotage, et qui le paie ? Votre contexte métier de 10 000 caractères est-il injecté à chaque appel ? »**
*Domaine : finance et IA · Constats : contre-audit A11 §C, M-015, M-018, A04-029*

- **Réponse factuelle aujourd'hui.** Partiellement, et la distinction est favorable à TinyPages à condition d'être faite nettement. **Sur le canal MCP, l'inférence est payée par le client IA de l'utilisateur**, pas par TinyPages : le créateur connecte son propre abonnement, et le coût marginal d'une session de pilotage est, pour la plateforme, celui des appels d'API. **Sur le coach IA intégré au produit, c'est TinyPages qui paie**, et ce coût n'est ni mesuré ni chiffré dans le dossier. Le champ de contexte métier de 10 000 caractères existe et est écrit sans contrôle (CONFIRMÉ, M-018) ; **sa fréquence d'injection n'a pas été observée**, et elle décide du coût unitaire du coach.
- **Ce qui manque pour répondre pleinement.** Le modèle et le fournisseur retenus pour le coach intégré, le coût unitaire, les plafonds d'usage par plan, la consommation mesurée par compte, et la règle d'injection du contexte métier — à chaque appel, une fois par session, ou par type de génération.
- **Pièce justificative attendue.** Facture du fournisseur de modèle sur douze mois, tableau de consommation par compte et par plan, spécification de la construction du contexte. **CTO + CEO.**

---

**Q70 — « Quel est le coût de votre plan de remédiation s'il est en partie sous-traité, et figure-t-il dans votre usage des fonds ? »**
*Domaine : finance · Constats : contre-audit A11 M-3, livrable 07 §5*

- **Réponse factuelle aujourd'hui.** Le plan existe et il est chiffré **en jours-personne seulement** : 40 j·p en P0, 156 j·p en P1, soit 196 j·p au total. Traduit pour l'équipe réelle — une personne qui est simultanément CEO en levée, CTO, support et commercial — cela fait **39 semaines**, alors que P1 est annoncé « avant le closing ». Aucune levée ne dure neuf mois de remédiation. Aucun coût en euros n'a été posé, aucun devis de test d'intrusion, aucune provision d'honoraires pour les trois mandats externes, aucune marge d'aléa, aucun chemin critique.
- **Ce qui manque pour répondre pleinement.** Un plan de charge nominatif avec chemin critique, l'hypothèse de renfort chiffrée en personnes, en euros et en date, les devis externes, et la ligne correspondante dans l'usage des fonds. Trois estimations doivent par ailleurs être refaites après réponse à Q82 : la confirmation serveur, la suppression définitive avec journal de purge et l'annulation avec historique de versions ne sont pas chiffrables tant qu'on ne sait pas s'il existe une préproduction, des tests et un retour arrière.
- **Pièce justificative attendue.** Devis de test d'intrusion et de développement, plan de charge nominatif, tableau d'usage des fonds faisant apparaître la remédiation. **CEO + CTO.**
- **Formulation à ne pas dépasser.** « P0 plus P1 représente 196 jours-personne, soit 39 semaines pour une personne ou 13 semaines à trois. Le calendrier que nous vous présentons suppose un renfort de [X] personnes à partir de [date], pour [Y] euros, et cette ligne fait partie de l'usage des fonds. » **Cette phrase transforme un plan intenable en argument de levée** ; son absence transforme le même plan en preuve d'optimisme.

---

**Q72 — « L'échéance de marquage des contenus générés au 2 décembre 2026 est-elle budgétée et affectée à quelqu'un ? »**
*Domaine : finance et conformité · Constats : contre-audit A11 M-4, A06-016, A06-017, livrable 04 §7.2*

- **Réponse factuelle aujourd'hui.** L'obligation est identifiée et datée par le dossier — c'est la seule échéance dure qui approche et qui demande du développement, et elle **tombe pendant la levée** (Q44). Ce qui n'existe pas, c'est sa traduction en gestion : ni budget, ni personne nommée, ni date de démarrage, ni maquette technique. Une obligation identifiée sans propriétaire est, en due diligence, équivalente à une obligation ignorée.
- **Ce qui manque pour répondre pleinement.** Le nom de la personne affectée, la date de démarrage, la maquette technique du marquage lisible par machine, et la ligne budgétaire correspondante dans le plan de charge et l'usage des fonds.
- **Pièce justificative attendue.** Plan de charge nominatif, spécification du marquage, ligne budgétaire. **CTO + CEO.**

---

**Q73 — « Avez-vous déjà augmenté vos prix ? Quelle élasticité avez-vous observée ? Que se passe-t-il pour les comptes historiques ? »**
*Domaine : finance et marché · Constats : contre-audit A11 §D, C-001, C-003, A03-002*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Aucune page tarifaire n'a pu être ouverte, aucun historique de grille n'a été trouvé, et le prix du plan payant lui-même n'est connu que par une source secondaire au statut PROBABLE. Trois contradictions d'offre restent par ailleurs ouvertes sur ce que le client achète (Q46) : un investisseur qui pose une question de pricing tombera dessus avant d'obtenir une réponse sur l'élasticité.
- **Ce qui manque pour répondre pleinement.** L'historique des grilles avec leurs dates, l'effet mesuré de chaque changement par cohorte avant et après, et la politique assumée de maintien ou non des tarifs historiques.
- **Pièce justificative attendue.** Historique tarifaire daté, cohortes avant et après chaque changement, politique de maintien des anciens tarifs. **CEO.**

---

# Bloc 3 — Traction, clients et marché

Ce que le produit vaut sur son marché, et ce que le discours commercial peut prouver. C'est aussi le bloc où se joue la différenciation revendiquée, et où elle ne tient pas en l'état.

---

**Q74 — « Combien de comptes ont réellement connecté votre serveur MCP, et combien d'actions par semaine ? »**
*Domaine : traction · Constats : contre-audit A11 §D, DR-041*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. C'est la question la plus embarrassante du bloc traction, parce qu'elle porte sur **la proposition de valeur centrale du produit** : si le pilotage par IA est ce qui différencie TinyPages, son taux d'adoption réel est la métrique que l'investisseur regardera avant toute autre. Le dossier ne la contient pas.
- **Ce qui manque pour répondre pleinement.** Le nombre de comptes ayant connecté le serveur, le nombre d'actions par semaine, leur répartition par type d'action, et l'interprétation qui va avec. Un différenciateur peu utilisé est une information exploitable — signal de marché, problème d'activation, ou promesse en avance sur l'usage — ce n'est pas une honte, sauf s'il est caché.
- **Pièce justificative attendue.** Télémétrie du serveur MCP sur douze mois : comptes connectés, actions par semaine, répartition par action. **CTO.**
- **Formulation à ne pas dépasser.** « [N] comptes ont connecté le serveur, pour [M] actions par semaine. C'est [faible/en croissance] et voici notre lecture. » Ne jamais répondre « nous ne mesurons pas » sur la métrique qui porte la thèse d'investissement : c'est une semaine d'instrumentation, et la réponse « nous ne mesurons pas » vaut, pour l'investisseur, « personne ne l'utilise ».

---

**Q75 — « Quelle part des pages et des emails publiés l'est par l'IA plutôt qu'à la main ? »**
*Domaine : traction · Constats : contre-audit A11 §D, M-010*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et le dossier établit en creux pourquoi : il n'existe **aucun journal distinguant l'acteur humain de l'acteur automatisé** (Q30). La conséquence dépasse la métrique commerciale — sans cette distinction, ni le créateur, ni TinyPages, ni une autorité ne peut reconstituer ce que l'IA a fait.
- **Ce qui manque pour répondre pleinement.** La part des publications attribuables à l'IA, par cohorte de compte et dans le temps, et l'instrumentation qui la produit — laquelle est exactement la pièce DR-112 du plan de remédiation. La métrique commerciale et le journal d'audit sont ici le même chantier : un seul travail, deux usages.
- **Pièce justificative attendue.** Télémétrie produit distinguant acteur humain et acteur automatisé, par type d'objet. **CTO.**

---

**Q76 — « Combien de sites clients publiés, combien actifs à trente jours, combien avec un domaine personnalisé ? »**
*Domaine : traction · Constats : contre-audit A11 §D, A09-020, A05-020, `annexes/echantillon_sites.md`*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour par voie externe, et le dossier le dit sans détour : l'échantillon de sites clients prévu par le plan d'enquête n'a pas pu être constitué, les journaux de transparence des certificats et les outils de mesure étant inaccessibles. Trois sous-domaines clients seulement sont connus, non vérifiés, dont un susceptible d'être un site de démonstration interne (Q77).
- **Ce qui manque pour répondre pleinement.** Les trois chiffres — publiés, actifs à trente jours, avec domaine personnalisé — **recoupés avec le comptage indépendant par les journaux de certificats**. C'est le recoupement qui compte : un chiffre interne seul sera refait par l'auditeur en dix minutes, et il vaut mieux qu'il trouve le même.
- **Pièce justificative attendue.** Export produit par mois depuis le lancement et comptage `crt.sh` à la même date, remis ensemble. **CTO.**

---

**Q77 — « Parmi vos références publiques, lesquelles sont des sites internes ou de démonstration ? »**
*Domaine : traction · Constats : contre-audit A11 §D, A09-020, DR-182*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. L'audit relève que l'un des trois sous-domaines clients connus porte le prénom du fondateur présumé, sans avoir pu établir s'il s'agit d'un site de démonstration interne ou d'un client tiers. Cette question conditionne son usage comme preuve de traction, et elle se vérifie en une minute par n'importe qui.
- **Ce qui manque pour répondre pleinement.** La liste des sites cités en référence, avec pour chacun son statut : client payant, client gratuit, démonstration interne, site du fondateur.
- **Pièce justificative attendue.** Liste des sites cités avec le statut de chacun, et accord écrit du créateur pour ceux qui sont nommés. **CEO.**
- **Formulation à ne pas dépasser.** Le dire avant qu'on ne le trouve. Un site de démonstration présenté comme une référence client est le genre de détail qui contamine la lecture de toutes les autres références.

---

**Q78 — « Quelle part de vos nouveaux comptes vient de votre page consacrée au pilotage par IA ? »**
*Domaine : traction et marché · Constats : contre-audit A11 §D, A09-001*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. La donnée existe nécessairement dans un rapport d'acquisition par page d'entrée ; l'audit n'y a pas eu accès. Elle répond à une question que ni le discours commercial ni le comparatif concurrentiel ne peuvent trancher : **le marché achète-t-il la promesse de pilotage par IA, ou l'outil marketing** ?
- **Ce qui manque pour répondre pleinement.** La répartition des inscriptions par page d'entrée sur douze mois, et le taux de conversion comparé entre la page IA et les autres.
- **Pièce justificative attendue.** Rapport d'acquisition par page d'entrée et par canal. **CEO.**

---

**Q1 — « Vous dites être la seule plateforme marketing tout-en-un pilotable de bout en bout par Claude. Kajabi, GoHighLevel, ClickFunnels, Stan Store, Systeme.io, Kit.com et beehiiv ont un serveur MCP officiel. Comment maintenez-vous cette affirmation ? »**
*Domaine : marché · Constats : A09-001 à A09-006, A04-026, A04-027*

- **Réponse factuelle aujourd'hui.** L'affirmation n'est pas maintenable en l'état. L'audit a relevé, à partir de sources secondaires convergentes du 18 septembre 2026 (statut PROBABLE, aucune page concurrente n'ayant pu être ouverte), l'existence d'un serveur MCP officiel chez Kajabi (couverture annoncée : pages, emails, offres, cours, contacts, commandes, étiquettes, segments, blog, réglages du site), GoHighLevel (36 outils annoncés, feuille de route revendiquée à plus de 250), ClickFunnels (bêta officielle), Stan Store (`mcp.stan.store`), Systeme.io (couverture annoncée plus étroite : contacts, étiquettes, cours, abonnements), Kit.com et beehiiv. **Sept concurrents, pas cinq** : les registres C-004 et P-038 en recensent sept, et une liste plus courte dans les livrables serait lue comme un rabotage. Un serveur MCP TinyPages hébergé par Zapier existe par ailleurs, distinct du serveur officiel : toute plateforme dotée d'une application Zapier dispose de fait d'un MCP. Ce que l'audit a établi en propre, et que les concurrents ne documentent pas publiquement à ce jour, c'est l'**étendue** de la surface pilotable : 104 actions au catalogue, de la création de la page à la publication et à l'envoi d'email. Statut exact de ce chiffre, à ne jamais arrondir : **inventaire relevé par lecture de la description publiée par le serveur, non exécuté action par action**. L'existence et le nom des 104 actions sont établis ; le comportement n'est établi que pour les **dix-sept actions réellement exécutées** pendant l'audit (liste nominative en annexe de screening).
- **Ce qui manque pour répondre pleinement.** Un comparatif fonctionnel daté, action par action, face aux sept concurrents, et à Kajabi et GoHighLevel au minimum. Les unités ne sont pas comparables en l'état : nos 104 comprennent 47 lectures, 3 utilitaires et une passerelle, et personne ne sait ce que GoHighLevel compte dans ses 36. Le périmètre de comparaison doit être défini avant le chiffre : objets pilotables en écriture, pas nombre d'appels. Aucune page concurrente n'a pu être ouverte : la couverture réelle de leurs serveurs n'est pas vérifiée. Il manque aussi la réponse à la question qui départage réellement : plusieurs concurrents, dont Kajabi, imposeraient un passage par le brouillon avant publication, ce qui est précisément le garde-fou que TinyPages n'applique pas (voir Q2).
- **Pièce justificative attendue.** Comparatif fonctionnel daté et archivé (captures des pages concurrentes au jour du relevé), rejoué chaque trimestre. **CEO.**
- **Formulation à ne pas dépasser.** « Le pilotage par IA n'est plus une exclusivité : plusieurs concurrents publient un serveur MCP officiel. Notre différenciation est la continuité du parcours entre l'idée et la mise en ligne, et un harnais auditable que nous annonçons comme feuille de route. » **Ne publier aucun chiffre de différenciation avant le comparatif daté** : « 104 » face à « 36 » compare deux unités différentes, et la feuille de route publique d'un concurrent à plus de 250 outils périme l'argument avant sa publication. Toute mention du mot « seule » doit disparaître des supports avant l'ouverture de la data room.

---

**Q49 — « Votre produit n'avait aucune fonction IA en avril 2025. Si Kajabi ou GoHighLevel exposent la même surface d'actions en un trimestre, que vous reste-t-il ? »**
*Domaine : marché · Constats : A01-007, A09-021, A09-010, A04-026*

- **Réponse factuelle aujourd'hui.** La chronologie publique, au statut PROBABLE faute de sources primaires : association des fondateurs en août 2024, lancement en janvier 2025, absence de fonction IA affirmée publiquement en avril 2025, bascule vers une orientation IA courant 2025, version « 2.0 » orientée IA autour de mai 2026. Les dates exactes n'ont pas pu être confirmées et la Wayback Machine était inaccessible. Ce que l'audit établit sur la fenêtre concurrentielle : elle se referme vite. **Sept** concurrents ont publié un serveur MCP officiel en 2026 — Kajabi, GoHighLevel, ClickFunnels, Stan Store, Systeme.io, Kit.com et beehiiv — dont l'un sur son plan gratuit. La barrière n'est donc pas l'antériorité.
- **Ce qui manque pour répondre pleinement.** Un journal des versions daté, et surtout la démonstration que la barrière est ailleurs : garde-fous appliqués côté serveur, journal des actions de l'IA, annulation, et batterie d'évals publiée. Aucun concurrent identifié ne documente publiquement ces éléments — affirmation à vérifier concurrent par concurrent avant d'en faire un argument, mais qui, si elle se confirme, constitue une barrière plus solide que l'antériorité.
- **Pièce justificative attendue.** Journal des versions daté, comparatif fonctionnel daté, feuille de route du harnais avec responsable et échéance, et les quatre pièces du harnais une fois livrées. **CTO + CEO.**
- **Formulation à ne pas dépasser.** « L'antériorité n'est pas une barrière et nous ne la présentons pas comme telle. La barrière que nous visons est un harnais appliqué et mesuré — contrôle serveur, journal, annulation, évals — et il n'existe pas encore. Voici sa feuille de route, son responsable et ses dates. » Toute réponse qui présente le virage IA comme une avance défendable sera démentie par la liste des sept concurrents.

---

**Q8 — « Que se passe-t-il pour votre produit si Anthropic ou OpenAI publient nativement des pages hébergées ? »**
*Domaine : marché · Constats : A09-011, A09-012, A04-032*

- **Réponse factuelle aujourd'hui.** Le mouvement est déjà engagé et daté. Claude Code Artifacts, annoncé le 18 juin 2026, publie une page HTML depuis une session et l'héberge sur une URL claude.ai. ChatGPT Sites est en bêta publique depuis le 9 juillet 2026 : construction et hébergement de sites dans ChatGPT, avec deux limites à la date de l'audit — le commerce y est interdit et le service est indisponible dans l'Union européenne, au Royaume-Uni et en Suisse (PROBABLE, sources secondaires). Aucun des deux n'égale aujourd'hui la boîte à outils marketing de TinyPages : paiements, emails, contacts, espace membre. La trajectoire, elle, est lisible. S'y ajoute une dépendance de discours : le positionnement, le slogan et le nom de domaine du serveur MCP reposent sur Claude, marque déposée d'un tiers (A04-031, conclusion juridique à valider par un avocat).
- **Ce qui manque pour répondre pleinement.** Une analyse écrite du périmètre exact de ces deux offres face au périmètre TinyPages, pour cadrer le risque plutôt que le sur- ou sous-estimer ; et un plan de découplage : compatibilité multi-clients réellement testée, et discours de marque qui ne dépend pas d'un fournisseur unique.
- **Pièce justificative attendue.** Note de positionnement datée, matrice de compatibilité par client (Claude, Claude Code, ChatGPT, autres) réellement testée, et le cas échéant l'échange écrit avec Anthropic sur l'usage de la marque. **CEO + CTO.**
- **Formulation à ne pas dépasser.** « L'hébergement natif de pages par les éditeurs de modèles existe depuis juin et juillet 2026, avec un périmètre aujourd'hui limité à la page et sans commerce en Europe. Notre défense n'est pas l'antériorité du MCP, c'est la chaîne complète paiement-email-membre et un harnais auditable. » Ne pas présenter ce risque comme théorique : les deux dates sont publiques.

---

**Q29 — « Le fondateur envoie-t-il sa propre newsletter depuis TinyPages ? »**
*Domaine : emails et marché · Constat : A03-Q6, A06-032*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Un signal figure au point de départ de l'audit : une page du fondateur indiquait qu'il envoyait encore ses emails à plus de 70 000 contacts via un outil tiers, avec une migration prévue vers TinyPages. Aucune trace de cette migration n'a pu être trouvée. C'est la preuve de passage à l'échelle la plus simple à produire, et la plus scrutée : un éditeur qui n'utilise pas son propre produit pour sa charge la plus lourde sera interrogé là-dessus.
- **Ce qui manque pour répondre pleinement.** La date de bascule, le volume mensuel envoyé depuis la plateforme, et les en-têtes d'un envoi réel.
- **Pièce justificative attendue.** En-têtes d'un email de la newsletter du fondateur envoyé depuis la plateforme, avec sa date, et volume mensuel. **CEO.** Si la migration n'a pas eu lieu, le dire et expliquer pourquoi vaut mieux que de laisser la question sans réponse.

---

**Q45 — « Vos neuf pages comparatives et vos témoignages sont-ils défendables ? »**
*Domaine : conformité et marché · Constats : A06-022, A06-023, A06-024, A09-018*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour : aucune des neuf pages comparatives n'a pu être ouverte. Les règles générales, qui ne sont pas des constats sur TinyPages : une publicité comparative doit porter sur des caractéristiques objectives, pertinentes et **vérifiables**, sans dénigrement, et la charge de la preuve pèse sur l'annonceur — un tableau comparatif périmé devient une allégation trompeuse sans que personne n'ait rien changé ; une allégation absolue comme « la seule plateforme » doit être exacte au jour de la diffusion et prouvable, ce qui renvoie directement à Q1 ; et diffuser des témoignages sans mesures raisonnables de vérification est une pratique réputée trompeuse. Sur la réputation publique, l'audit relève un ordre de trente-quatre avis sur une plateforme d'avis, avec une note rapportée de manière contradictoire entre deux relevés : **aucun chiffre de note n'est citable**, et trente-quatre avis ne permettent aucun agrégat marketing.
- **Ce qui manque pour répondre pleinement.** Pour chaque page comparative : la date du relevé, la capture archivée de la page concurrente à cette date, et une révision trimestrielle. Pour chaque témoignage : l'accord écrit, la date, une identité vérifiable.
- **Pièce justificative attendue.** Dossier de preuve par allégation, archives datées des comparaisons, registre des consentements aux témoignages, méthode d'invitation aux avis. **CEO.** La réponse structurelle n'est pas un dossier de preuve mais un processus nommé : qui valide qu'une phrase commerciale est prouvable avant sa mise en ligne (Q97).

---

**Q97 — « Qui, chez vous, valide qu'une phrase commerciale est prouvable avant sa mise en ligne, et sur quelle preuve archivée ? »**
*Domaine : marché et gouvernance · Constats : contre-audit A11 §G, A06-022, A06-023, A09-001*

- **Réponse factuelle aujourd'hui.** Aucun processus de ce type n'a pu être observé, et le dossier contient déjà le cas d'école : l'affirmation « seule plateforme » n'est pas maintenable, sept concurrents publient un serveur MCP officiel, et neuf pages comparatives n'ont fait l'objet d'aucun archivage daté (Q1, Q45). **Le problème n'est pas la phrase, c'est l'absence de processus** : sans validation ni archive, l'erreur se reproduira à la prochaine page, et un comparatif se périme sans que personne n'ait rien changé.
- **Ce qui manque pour répondre pleinement.** Un processus nommé avec un responsable identifié, une archive datée par allégation, une révision trimestrielle des comparatifs, et un journal des retraits ou corrections déjà effectués.
- **Pièce justificative attendue.** Procédure de validation des allégations, dossier de preuve daté par allégation, journal des retraits et corrections. **CEO.**
- **Formulation à ne pas dépasser.** « [Nom] valide toute allégation absolue ou chiffrée avant mise en ligne, sur la base d'un dossier de preuve archivé et daté, revu chaque trimestre. Voici le journal des corrections déjà apportées. » C'est la **réponse structurelle** à l'affaire « seule plateforme » : retirer la phrase sans installer le processus ne répond qu'à la moitié de la question.

---

**Q50 — « Sur quelle preuve reposent vos allégations de résultat pour vos clients, et quel risque leur faites-vous courir ? »**
*Domaine : marché et conformité · Constats : A07-010, A06-023, A06-024, M-016*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour : le discours commercial exact n'a pas pu être relevé, les pages n'ayant pas pu être ouvertes. Deux règles générales encadrent la réponse, et ne sont pas des constats sur TinyPages. D'une part, toute allégation portant sur les résultats attendus doit être exacte et prouvable. D'autre part, la politique du principal moteur de recherche sur le contenu produit en volume ne prohibe pas le contenu généré par IA en soi, mais cible la production de pages en nombre sans valeur ajoutée : une promesse du type « générez votre blog en un prompt » n'est pas interdite, mais elle expose le client final à une pénalité s'il publie en volume sans révision. Le produit fournit par ailleurs quinze modèles par défaut, dont une séquence de lancement en sept emails jouant l'urgence et l'objection (CONFIRMÉ, M-016) : ces modèles portent une promesse implicite qu'il faut assumer.
- **Ce qui manque pour répondre pleinement.** Le relevé du discours commercial réel, un dossier de preuve par allégation chiffrée, et une mention dans la documentation produit avertissant du risque de publication en volume sans révision.
- **Pièce justificative attendue.** Dossier de preuve des allégations, relevé daté des pages commerciales, avertissement produit. **CEO.**

---

# Bloc 4 — Produit, offre et responsabilité

Ce que le client achète, ce qu'il peut emporter en partant, et qui répond quand le produit publie quelque chose qu'il n'aurait pas dû publier.

---

**Q46 — « Que contient exactement le plan gratuit, et que contient le plan payant ? »**
*Domaine : fonctionnel · Constats : C-001, C-003, M-013, A07-006, A07-007*

- **Réponse factuelle aujourd'hui.** Partiellement, et trois contradictions publiques restent ouvertes sur ce que le client achète. Ce que le screening a établi : le serveur MCP fonctionne sur le plan gratuit — lectures, création de pages, publication passent toutes — tandis qu'un sous-ensemble d'actions est refusé par plan, notamment le bloc de code personnalisé et l'envoi d'email (CONFIRMÉ, M-013). **Ni la page marketing qui annonce la connexion incluse dès le plan gratuit, ni la documentation qui réserve l'intégration au plan payant n'ont donc entièrement raison** : c'est cette nuance qui manque aux deux sources et qui explique la contradiction C-001. Les contradictions C-001 et C-003 restent formellement ouvertes au registre, aucune page tarifaire n'ayant pu être ouverte. Le prix du plan payant et les paliers de contacts relevés par des sources secondaires sont au statut PROBABLE et ne doivent pas être présentés comme établis.
- **Ce qui manque pour répondre pleinement.** La grille tarifaire officielle, les quotas exacts par plan (pages, produits, contacts, emails par mois), et une formulation unique reprise à l'identique sur le site, la documentation et la FAQ.
- **Pièce justificative attendue.** Grille tarifaire officielle datée, matrice des limites par plan, et journal des corrections apportées aux pages contradictoires. **CEO.** Laisser trois contradictions d'offre ouvertes dans une data room est un écart en soi.

---

**Q48 — « Que ne fait pas votre produit, que font vos concurrents ? »**
*Domaine : fonctionnel · Constats : A07-008, M-003, A07-011*

- **Réponse factuelle aujourd'hui.** Deux réponses, de valeur inégale. Ce qui est établi : le pilotage par IA ne couvre ni la gestion du domaine personnalisé, ni les paramètres de paiement, ni les remboursements, ni l'export des données, ni les réglages de sécurité du compte (M-003, établi sur l'inventaire relevé). La promesse « pilotable de bout en bout » a donc une borne, et elle passe là où se trouvent l'argent et l'administration du compte — ce qui est d'ailleurs défendable, à condition de le dire. Ce qui n'est établi qu'au statut PROBABLE, par recherche négative : aucune mention n'a été trouvée de fonctions de communauté, d'affiliation, de certificats de complétion, de facturation multi-devises ni d'application mobile, toutes présentes chez au moins un concurrent direct. **Une absence non trouvée n'est pas une absence prouvée.** Sur le support, aucun engagement de délai formel n'a été retrouvé : les seules données disponibles sont des avis publics anecdotiques.
- **Ce qui manque pour répondre pleinement.** La liste, tenue par TinyPages, de ce qui existe, de ce qui est en feuille de route avec une date, et de ce qui ne sera pas fait. Et une charte de support avec canaux, horaires et délais d'engagement.
- **Pièce justificative attendue.** Feuille de route produit datée, charte de support. **CEO.** Une lacune assumée avec une date coûte moins cher qu'une lacune découverte.

---

**Q47 — « Un créateur peut-il récupérer ses données et partir ? »**
*Domaine : fonctionnel et conformité · Constats : A07-009, A06-015, M-003*

- **Réponse factuelle aujourd'hui.** Par le canal automatisé, non : aucune action d'export ne figure au catalogue de 104 actions relevé sur le serveur (inventaire relevé par lecture de la description publiée par le serveur, non exécuté action par action). Par l'interface, Non déterminé à ce jour. Aucune fonction d'export ni de migration assistée n'a été trouvée par recherche publique, alors que des concurrents documentent des parcours de migration et un export. La règle générale, qui n'est pas un constat sur TinyPages : le règlement européen sur les données impose depuis le 12 septembre 2025 un droit de changer de fournisseur, un préavis de résiliation plafonné, une transition assistée et un export structuré et lisible par machine ; les frais de changement disparaissent complètement le **12 janvier 2027**.
- **Ce qui manque pour répondre pleinement.** L'existence et le périmètre d'un export complet — contacts avec preuves de consentement, pages, produits, emails, contenus de cours — son format, et les clauses de sortie dans les conditions générales.
- **Pièce justificative attendue.** Capture d'un export réel et de son contenu, clauses de sortie. **CTO + avocat.** Un export partiel ou payant sera relevé par l'auditeur ; un export complet est aussi un argument commercial.

---

**Q80 — « Fournissez-vous à vos créateurs les outils de leur propre conformité — mentions légales, conditions de vente, bandeau de consentement, preuve de consentement, facture conforme ? Sinon, quelle est votre exposition contractuelle envers eux ? »**
*Domaine : produit et conformité · Constats : contre-audit A11 §G, M-007, A06-010, A06-011*

- **Réponse factuelle aujourd'hui.** Partiellement, et défavorablement. Ce qui est établi : **les gabarits juridiques livrés par défaut sont vides**, publiés et ouverts à l'indexation dès la première seconde du compte, et le double opt-in est désactivé par défaut (CONFIRMÉ, M-007). Ce qui n'est pas déterminé : l'existence d'un mécanisme de consentement aux traceurs fourni aux sites clients (Q43), la conformité des factures émises (Q24), et la conservation de la preuve de consentement (Q28). L'inventaire honnête est donc : la plateforme fournit le contenant et pas le contenu, tout en publiant le contenant à la place du créateur.
- **Ce qui manque pour répondre pleinement.** L'inventaire de ce qui est fourni et de ce qui ne l'est pas, gabarit par gabarit, et la clause qui répartit la responsabilité entre la plateforme et le créateur — clause qui, aujourd'hui, n'a pas pu être vue.
- **Pièce justificative attendue.** Inventaire des gabarits et de leur contenu réel, capture d'un compte neuf après correctif, clause de répartition des responsabilités dans les conditions créateurs. **CTO + avocat *(à mandater)*.**
- **Formulation à ne pas dépasser.** « Nous publions aujourd'hui des gabarits juridiques vides sur chaque compte ; c'est un défaut de conception, il est corrigé le [date]. » Le dire avant qu'on n'ouvre un compte d'essai : la vérification prend cinq minutes et elle sera faite.

---

**Q79 — « Si votre IA publie un contenu illicite ou diffamatoire sur le compte d'un créateur, qui est responsable, et que disent vos conditions ? »**
*Domaine : responsabilité produit · Constats : contre-audit A11 §F, M-010, A06-001, A06-012*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et c'est le point où deux constats établis du dossier se rejoignent de la pire manière. D'une part, **la publication n'est soumise à aucun contrôle serveur** : un modèle qui ignore la consigne textuelle met une page en ligne en un appel (CONFIRMÉ, M-010). D'autre part, **aucune condition générale, aucune clause de responsabilité et aucune procédure de retrait n'a pu être vue**. Entre les deux, il n'y a rien : ni contrôle, ni contrat, ni procédure. La chaîne de responsabilité — créateur, TinyPages comme fournisseur du système d'IA, TinyPages comme hébergeur — n'est écrite nulle part.
- **Ce qui manque pour répondre pleinement.** Une clause de responsabilité claire dans les conditions imposées aux créateurs, une procédure de notification et d'action conforme au régime de l'hébergeur, un registre des signalements et des retraits, et une assurance qui n'exclut pas le contenu produit automatiquement (Q63).
- **Pièce justificative attendue.** Clause de responsabilité, procédure de notification et action, registre des retraits, attestation d'assurance avec lecture des exclusions. **CEO + avocat *(à mandater)*.**
- **Formulation à ne pas dépasser.** « Le créateur reste l'éditeur de son contenu ; nous sommes hébergeur et fournisseur de l'outil. Notre procédure de retrait est [X], notre délai est [Y], et nous tenons un registre. » **Ne pas invoquer le statut d'hébergeur sans en tenir les obligations** : c'est la première chose qu'un avocat de fonds vérifiera, et le paquet correspondant est aujourd'hui absent du dossier (DR-158).

---

# Bloc 5 — Paiements et fiscalité

Le bloc le plus coûteux du dossier en cas de mauvaise réponse, et le seul dont trois questions se referment avec un seul rendez-vous chez le conseil : Q4, Q64 et Q81 reposent sur le même faisceau de trois faits.

---

**Q18 — « Quel type de compte Stripe Connect et quel type de charge utilisez-vous ? Qui porte les litiges et les soldes négatifs ? »**
*Domaine : paiements · Constats : A02-001, A02-002, A02-003, A02-008*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. La règle générale, qui n'est pas un constat sur TinyPages : avec un compte Standard, le compte connecté gère ses litiges et couvre ses soldes négatifs, la plateforme n'est pas exposée ; avec Express ou Custom, la plateforme est responsable des soldes négatifs et des litiges sur les charges indirectes. Le type de charge redouble ce choix : en charges directes, le compte connecté supporte frais, remboursements et chargebacks ; en charges destination, c'est le solde de la plateforme qui est débité. Le point de départ de l'audit mentionne une « connexion par redirection », ce qui évoque un flux de compte Standard, mais un onboarding Express est lui aussi une redirection hébergée : **l'indice ne tranche rien**. La formule de la page tarifs relevée par un moteur de recherche (« every sale lands directly in your account ») est une formule marketing, pas une description technique, et ne doit pas être reprise comme un constat.
- **Ce qui manque pour répondre pleinement.** La combinaison exacte type de compte × type de charge, et son corollaire chiffré : si la plateforme porte les soldes négatifs, Stripe immobilise une réserve sur son compte, poste de trésorerie à déclarer.
- **Pièce justificative attendue.** Capture du tableau de bord Connect (onglet des comptes connectés), extrait du code de création du PaymentIntent ou de la Checkout Session, contrat plateforme signé, montant de la réserve au dernier arrêté et son évolution sur douze mois. **CTO + CEO.**

---

**Q19 — « Comment la commission du plan gratuit est-elle techniquement prélevée, et l'est-elle aussi sur PayPal ? »**
*Domaine : paiements · Constats : A02-006, A02-007, A02-016, contradiction A02*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Le taux de commission figure au point de départ de l'audit sans avoir pu être vérifié sur une page officielle. Le mécanisme de prélèvement — frais d'application par paiement, pourcentage sur abonnement, transfert séparé ou facturation a posteriori — n'a pas été observé. Sur PayPal, la règle générale, qui n'est pas un constat sur TinyPages : une commission de plateforme n'est pas supportée par les intégrations PayPal dites de première partie. Si TinyPages est dans ce cas, la commission du plan gratuit ne serait pas prélevable sur les ventes PayPal, ce qui rendrait le plan gratuit contournable. Une contradiction publique reste par ailleurs ouverte : les conditions générales d'un site client affirment qu'aucune commission n'est prélevée sur les ventes. Le screening a établi que le gabarit livré par la plateforme est vide (M-007), ce qui **écarte la piste du gabarit fourni** pour ce texte précis ; son origine reste à établir.
- **Ce qui manque pour répondre pleinement.** Un objet de paiement de production anonymisé, le type d'intégration PayPal, et le chiffrage de la fuite éventuelle de commission.
- **Pièce justificative attendue.** Objet Stripe anonymisé, extrait du code d'appel, contrat PayPal, part des ventes réalisées via PayPal sur le plan gratuit. **CTO.**

---

**Q20 — « Quel est votre taux de litiges consolidé sur douze mois, et qui répond aux litiges ? »**
*Domaine : paiements · Constat : A02-030*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Cette métrique n'est observable que depuis le tableau de bord du prestataire de paiement. Elle sera demandée telle quelle : c'est une métrique standard de data room, et elle conditionne aussi la relation contractuelle avec le prestataire.
- **Ce qui manque pour répondre pleinement.** L'export sur douze mois, la procédure de traitement (qui répond, dans quel délai), et les preuves collectées automatiquement à l'appui d'une contestation : adresse, horodatage, preuve de téléchargement, acceptation des conditions.
- **Pièce justificative attendue.** Export du taux de litiges par mois et par compte connecté, procédure interne écrite. **CEO + CTO.**

---

**Q21 — « Votre formulaire de paiement est-il hébergé par Stripe ? Du code fourni par un créateur peut-il coexister avec lui sur la même page ? »**
*Domaine : paiements et sécurité · Constats : A02-010, A02-011, A02-012, M-011, M-012*

- **Réponse factuelle aujourd'hui.** Partiellement. Ce qui est établi : le bloc de code personnalisé, qui accepte du HTML et du JavaScript bruts, est **réservé au plan payant** — le serveur retourne `402 PRO_PLAN_REQUIRED` pour un compte gratuit (CONFIRMÉ, M-011). Ce qui n'est pas déterminé : le type de formulaire de paiement (page hébergée en redirection, formulaire intégré, ou champs sur l'infrastructure marchande), donc le périmètre PCI applicable ; et la possibilité, pour un créateur au plan payant, de placer un bloc de code sur une page portant un formulaire de paiement. La règle générale, qui n'est pas un constat sur TinyPages : depuis le 31 mars 2025, les exigences d'inventaire et de contrôle d'intégrité des scripts de la page de paiement sont devenues un critère d'éligibilité au questionnaire simplifié — le marchand doit attester que son site n'est pas exposé aux attaques par script. Si un bloc de code arbitraire peut coexister avec un formulaire de paiement, cette attestation devient difficile à tenir.
- **Ce qui manque pour répondre pleinement.** Le type de checkout, le domaine qui sert le formulaire, et une règle produit explicite interdisant le bloc de code sur une page de paiement.
- **Pièce justificative attendue.** Capture d'une page de paiement en mode test avec son code source, attestation de conformité PCI du niveau applicable, et règle produit documentée. **CTO.**

---

**Q22 — « Vos upsells en un clic sont-ils conformes à l'authentification forte ? Quel est leur taux de refus ? »**
*Domaine : paiements · Constats : A02-013, A02-014, A02-015*

- **Réponse factuelle aujourd'hui.** Les fonctions sont établies : le catalogue relevé contient `add_upsell`, `update_upsell_content`, `delete_upsell`, `add_order_bump`, `delete_order_bump`, `create_coupon`, `create_evergreen_deadline` — existence et nom établis par lecture de la description publiée par le serveur, aucune de ces actions n'ayant été exécutée. Leur paramétrage au regard de l'authentification forte est Non déterminé à ce jour. La règle générale, qui n'est pas un constat sur TinyPages : dans un upsell après achat, le client est présent à l'écran, l'opération est donc initiée par le client et relève de l'authentification forte sauf exemption ; la traiter comme une transaction initiée par le marchand suppose un mandat préalable et une série convenue, et l'exemption n'est jamais garantie.
- **Ce qui manque pour répondre pleinement.** Le paramétrage réel du flux, l'existence et la conservation d'un mandat, et surtout la mesure empirique : taux de refus sur l'upsell, taux d'échec des échéances 2 et 3 des paiements fractionnés, taux d'échec de renouvellement des abonnements.
- **Pièce justificative attendue.** Revue de code du flux d'upsell, statistiques de refus par type de flux sur douze mois. **CTO.**
- **Remarque à ne pas omettre.** Le screening a relevé sur le compte observé un produit à `price: 100` avec `installments: 3` et `installmentAmount: 33`, soit 99 au total (M-009). Anecdotique seul, révélateur si la règle d'arrondi de la dernière échéance n'est pas gérée. À vérifier avant qu'un acheteur ne le signale.

---

**Q23 — « Votre cœur de cible longe la liste des activités restreintes de votre prestataire de paiement. Comment modérez-vous les pages de vente ? »**
*Domaine : paiements et risque · Constats : A02-028, A02-029, M-012, M-016*

- **Réponse factuelle aujourd'hui.** Aucune politique d'usage acceptable ni aucun dispositif de modération n'a pu être observé, et aucune procédure publique de signalement d'abus n'a été trouvée. Ce que l'audit a établi en propre : la plateforme livre par défaut quinze modèles, dont une séquence de lancement complète en sept emails et une page de vente (CONFIRMÉ, M-016), et le catalogue expose la création de dates limites « evergreen ». La règle générale, qui n'est pas un constat sur TinyPages : la liste des activités interdites et restreintes du prestataire vise explicitement les promesses de gains faciles, les témoignages fabriqués, l'upselling à haute pression et les services sans valeur ajoutée, et le prestataire peut suspendre un compte sans préavis. L'exposition est double : fermetures en série de comptes connectés, et mise en cause de la plateforme selon la configuration Connect retenue.
- **Ce qui manque pour répondre pleinement.** Une politique d'usage acceptable écrite, une procédure de signalement, un suivi du taux de litiges par compte connecté avec seuil d'alerte, et l'historique des retraits de contenu depuis le lancement.
- **Pièce justificative attendue.** Politique d'usage, procédure de modération et de signalement, registre des retraits. **CEO.**

---

**Q4 — « Si l'article 9 bis du règlement d'exécution (UE) 282/2011 s'applique, quelle est votre exposition TVA sur le volume vendu par vos créateurs ? »**
*Domaine : paiements et fiscalité · Constats : A02-019, A06-020*

- **Réponse factuelle aujourd'hui.** Non déterminée à ce jour, et c'est le risque le plus lourd du dossier. La règle, elle, est établie : une plateforme par laquelle sont fournis des services électroniques est présumée agir en son nom propre ; cette présomption devient **irréfragable** dès lors que la plateforme autorise la facturation au client, autorise la fourniture, **ou fixe les conditions générales** de la prestation. La Cour de justice a validé le dispositif (aff. C-695/20, Fenix International). Si la présomption s'applique, TinyPages devient redevable de la TVA de chaque pays d'acheteur sur l'intégralité du volume vendu par ses créateurs, et non sur ses seuls abonnements, avec les obligations OSS correspondantes. **Aucun élément de cet audit ne permet de dire si elle s'applique ou non** : les trois faits qui la déclenchent n'ont pas pu être observés.
- **Ce qui manque pour répondre pleinement.** Trois faits, et trois seulement : qui apparaît sur le reçu et sur la facture remis à l'acheteur final ; qui fixe les conditions générales de vente du tunnel d'achat, TinyPages ou le créateur ; qui décide de la mise à disposition du fichier. Plus le volume brut vendu par les créateurs sur douze mois, sans lequel aucune exposition ne peut être chiffrée.
- **Pièce justificative attendue.** Un reçu et une facture réels anonymisés, les CGV du tunnel d'achat, le paramétrage Stripe Connect, et une **note de qualification signée par un avocat fiscaliste**. **CEO + avocat à mandater** (le paramètre `RESPONSABLES` porte « avocat : à mandater » : cette pièce n'a pas de propriétaire à ce jour).
- **Formulation à ne pas dépasser.** « La qualification au regard de l'article 9 bis est en cours d'analyse par un conseil fiscal. Les trois critères déclencheurs sont identifiés et documentés. Nous communiquerons la note de qualification et, le cas échéant, le chiffrage de l'exposition. » Ne jamais affirmer que la présomption ne s'applique pas avant d'avoir la note écrite.

---

**Q81 — « Qui est le vendeur professionnel vis-à-vis de l'acheteur final : vous ou le créateur ? Qui porte la garantie, la rétractation et le litige ? Cette réponse est-elle cohérente avec votre position TVA et votre présentation comptable ? »**
*Domaine : juridique et fiscalité · Constats : contre-audit A11 M-10, A02-019, A06-020, A06-015*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et c'est le **troisième corollaire** du même faisceau de faits que Q4 et Q64. Le dossier traite la TVA avec soin ; il n'a posé ni la qualification comptable, ni celle de droit de la consommation. Si la présomption de l'article 9 bis s'applique, ce n'est pas seulement la TVA qui change de redevable : c'est la plateforme qui devient le professionnel vis-à-vis de l'acheteur final, avec la garantie de conformité, le droit de rétractation, le traitement du litige et la responsabilité du contenu vendu. Le livrable de conformité traite aujourd'hui la rétractation comme un « volet produit » exposant les créateurs ; sous cette qualification, elle exposerait la plateforme.
- **Ce qui manque pour répondre pleinement.** Une réponse **unique**, tirée des mêmes trois faits, validée par un conseil, et identique dans les trois registres — fiscal, comptable, consommation. Trois réponses différentes aux trois questions est la configuration la plus coûteuse, parce qu'elle garantit qu'au moins deux sont fausses.
- **Pièce justificative attendue.** Conditions générales du tunnel d'achat, reçu et facture réels anonymisés, **note unique d'un conseil couvrant les trois qualifications**, preuve du recueil de l'accord exprès à l'exécution immédiate et de la renonciation à la rétractation. **CEO + avocat *(à mandater)*.**
- **Formulation à ne pas dépasser.** « Les trois qualifications — TVA, comptable, consommation — reposent sur le même jeu de faits et font l'objet d'une note unique de notre conseil. Voici la note. » C'est le même rendez-vous et la même liasse de pièces que pour Q4 : élargir le mandat ne coûte rien et referme trois questions au lieu d'une.

---

**Q24 — « L'obligation de réception des factures électroniques est en vigueur depuis le 1er septembre 2026. Où en êtes-vous, et que ferez-vous pour vos créateurs en 2027 ? »**
*Domaine : fiscalité · Constats : A02-024, A02-025, A02-026, A06-019, A06-019 bis*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et la question a deux volets. Volet entreprise : si l'entité est française, l'obligation de **réception** via une plateforme agréée est entrée en vigueur le 1er septembre 2026, soit dix-sept jours avant la date de cet audit ; l'obligation d'émission frappe les TPE et PME au 1er septembre 2027. Si l'entité est belge, c'est le mandat Peppol qui s'applique, depuis le 1er janvier 2026. **L'entité n'étant pas établie (Q9), l'applicabilité n'est pas tranchée.** Volet produit : les créateurs français devront produire un e-reporting de leurs ventes B2C au 1er septembre 2027 ; comme TinyPages encaisse et tient le journal des ventes, ils se tourneront vers lui pour les données. Aucune fonction de ce type n'apparaît dans le catalogue MCP (CONFIRMÉ pour l'absence dans le catalogue, ce qui ne préjuge pas de l'interface).
- **Ce qui manque pour répondre pleinement.** Le nom de la plateforme agréée retenue et la date de raccordement ; et une position écrite sur ce que TinyPages fournira à ses créateurs, à quelle date, ou explicitement ce qu'il ne fournira pas.
- **Pièce justificative attendue.** Contrat de raccordement à une plateforme agréée, position produit datée dans la feuille de route. **CEO + expert-comptable.** Les dates réglementaires citées reposent sur des sources secondaires et doivent être reconfirmées sur source officielle avant publication.

---

# Bloc 6 — Architecture, infrastructure et exploitation

Le socle technique, dont **aucune ligne n'a pu être relevée** : ni en-tête HTTP, ni résolution DNS, ni inspection TLS. Le tableau de stack du livrable 02 est majoritairement vide, et ce bloc est celui que la réouverture de l'accès réseau referme le plus vite.

---

**Q13 — « Qui héberge la plateforme, derrière quel CDN et quel pare-feu applicatif ? »**
*Domaine : infrastructure · Constats : A01-001, A01-008*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Aucun en-tête HTTP, aucune résolution DNS, aucune inspection TLS n'a pu être réalisée : l'egress était fermé et les outils de résolution absents de l'environnement d'audit. Deux signaux faibles seulement, tous deux au statut HYPOTHÈSE : un alias `tinypages.vercel.app` porte la même accroche marketing que le site principal, sans qu'on sache s'il s'agit d'un hébergement de production, d'un déploiement de prévisualisation ou d'un alias abandonné ; un agrégateur de réputation mentionne une mutualisation d'hébergement, sans nommer l'hébergeur ni donner d'adresse.
- **Ce qui manque pour répondre pleinement.** Le schéma d'architecture : hébergeur, régions, CDN, pare-feu applicatif, protection anti-déni de service, segmentation réseau, exposition des bases. Et la clarification du statut de l'alias : un environnement de prévisualisation exposé publiquement contourne le CDN et le pare-feu du domaine principal.
- **Pièce justificative attendue.** Schéma d'architecture interne daté, capture de la console d'hébergement, inventaire des environnements exposés. **CTO.**

---

**Q14 — « Combien de sites clients sont publiés aujourd'hui, et depuis quand ? »**
*Domaine : infrastructure · Constats : A01-012, A05-020, A09-020*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour par voie externe. Le comptage passait par les journaux de transparence des certificats, inaccessibles pendant l'audit. Trois sous-domaines clients seulement sont connus, issus du point de départ et non vérifiés. Un arbitrage de conception mérite d'être assumé explicitement dans la data room : si chaque site client reçoit son propre certificat, la transparence des certificats expose publiquement la liste complète des clients et leur date d'arrivée, donnée commercialement sensible en pleine levée ; si un certificat générique est utilisé, une clé privée unique couvre tous les locataires.
- **Ce qui manque pour répondre pleinement.** Le comptage interne des sites publiés et des comptes actifs, et la politique de certificats retenue.
- **Pièce justificative attendue.** Export du nombre de sites publiés par mois depuis le lancement, et note sur la politique d'émission de certificats. **CTO.**

---

**Q82 — « Existe-t-il une préproduction et une suite de tests automatisés ? Quelle couverture, quelle durée d'exécution ? »**
*Domaine : technique et exploitation · Constats : contre-audit A11 M-3 et §E, DR-034, DR-079*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et c'est la question qui conditionne la crédibilité de **tout le plan de remédiation**. Trois lignes du plan — confirmation serveur sur douze actions (15 j·p), suppression définitive avec journal de purge (8 j·p), annulation et historique de versions (8 j·p) — ne sont pas chiffrables sans savoir s'il existe une préproduction, des tests automatisés et un retour arrière. S'il n'y en a pas, ces trois lignes doublent au minimum. Un indice défavorable figure au dossier : le compte fourni à l'audit comme compte de test était un compte de production réel (Q40), ce qui décrit une organisation sans séparation d'environnements.
- **Ce qui manque pour répondre pleinement.** L'existence et la description des environnements, la couverture de tests, la durée d'exécution de la suite, et la règle écrite sur les données réelles hors production.
- **Pièce justificative attendue.** Capture de la chaîne d'intégration continue, rapport de couverture, description des environnements. **CTO.**
- **Formulation à ne pas dépasser.** S'il n'y a ni préproduction ni tests : le dire, et rechiffrer le plan en conséquence devant l'investisseur. Un plan de remédiation chiffré sur une chaîne d'ingénierie inexistante est un plan faux, et il sera lu comme tel.

---

**Q83 — « Combien de temps entre un commit et la production, et comment revenez-vous en arrière ? »**
*Domaine : technique et exploitation · Constats : contre-audit A11 §E, DR-079*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Ni le processus de mise en production, ni la revue par un pair, ni la procédure de retour arrière n'ont pu être observés. Avec une équipe très réduite, la revue par un pair est structurellement difficile : l'enjeu n'est pas de prétendre qu'elle existe, c'est de dire ce qui la remplace.
- **Ce qui manque pour répondre pleinement.** Le délai mesuré entre commit et production, la procédure de retour arrière **déjà exercée** — pas seulement écrite — et le compte rendu du dernier incident de déploiement.
- **Pièce justificative attendue.** Historique de déploiements, procédure de rollback, compte rendu du dernier incident de déploiement. **CTO.**

---

**Q85 — « Que se passe-t-il quand un créateur fait un lancement et décuple sa charge en une heure : files d'attente, quotas, dégradation ? »**
*Domaine : technique et exploitation · Constats : contre-audit A11 §E, A03-001, DR-068*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Aucun élément d'architecture n'a pu être relevé : ni hébergeur, ni CDN, ni pare-feu applicatif, ni protection anti-déni de service (Q13). La question est structurante pour ce produit précisément parce que **le lancement est le moment où le créateur juge la plateforme** : une page de vente indisponible pendant une heure de lancement est une résiliation.
- **Ce qui manque pour répondre pleinement.** Les mécanismes nommés — files d'attente, quotas par locataire, dégradation gracieuse, mise en cache — et le graphique d'un pic réel déjà encaissé. Le même sujet a un versant email : un lancement, c'est aussi un envoi de masse qui traverse la réputation partagée du parc (Q26).
- **Pièce justificative attendue.** Métriques d'un pic de production daté, politique de quota par locataire, description de la dégradation. **CTO.**

---

**Q17 — « Quelles sont vos sauvegardes, quand avez-vous testé une restauration pour la dernière fois, et pouvez-vous restaurer un seul créateur sans restaurer tout le parc ? »**
*Domaine : infrastructure · Constats : A05 questionnaire BCR-01 à BCR-07*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Le questionnaire de sécurité prérempli compte 97 lignes, dont 85 « inconnue » : la continuité d'activité en fait partie intégralement. Aucune page de statut public n'a été trouvée, aucun engagement de disponibilité non plus.
- **Ce qui manque pour répondre pleinement.** Fréquence, périmètre, chiffrement et rétention des sauvegardes ; date du dernier test de restauration réussi ; objectifs de point et de délai de reprise ; plan de reprise écrit et exercice réalisé. Et la question qui départage une sauvegarde d'une sauvegarde utile : la **restauration sélective d'un seul locataire** est-elle possible sans restaurer tout le parc, l'a-t-elle déjà été, à quelle date et en combien de temps ? Sur une plateforme multi-locataire, une restauration globale pour réparer un client en écrase mille autres.
- **Pièce justificative attendue.** Politique de sauvegarde, procédure de restauration sélective par locataire, journal du dernier test de restauration avec sa date, sa durée et son résultat, plan de reprise. **CTO.** Un hébergeur de sites marchands sans restauration testée est un risque direct pour ses clients : c'est la ligne que l'auditeur lira en premier dans ce domaine.

---

**Q15 — « Vos vidéos de formation sont-elles protégées ? Les URL sont-elles signées et expirantes ? »**
*Domaine : infrastructure · Constat : A01-004*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Le point de départ de l'audit mentionnait un fournisseur de diffusion vidéo pour la vidéo de démonstration ; aucune source indépendante n'a confirmé qu'il serve les vidéos des formations des clients, et aucun espace membre n'a pu être observé. Le catalogue relevé contient `list_videos`, `search_videos` et `get_video` — existence et nom établis par lecture de la description publiée par le serveur, aucune de ces trois actions n'ayant été exécutée. Cela atteste l'existence d'un module vidéo, sans rien dire du fournisseur ni de la protection des URL.
- **Ce qui manque pour répondre pleinement.** L'identité du fournisseur, et la réponse à une question binaire : les URL de lecture **et les fichiers téléchargeables des produits numériques** sont-ils servis par des URL signées et expirantes, ou publiques ? Avec quelle durée de validité, et un élève peut-il partager le lien ? Si elles sont publiques, le contenu payant de tout le parc est repartageable par simple copie de lien — c'est le mode de fuite le plus banal d'une plateforme de formation.
- **Pièce justificative attendue.** Contrat fournisseur, configuration du stockage, et relevé anonymisé d'une URL de lecture et d'une URL de téléchargement montrant la signature et l'expiration. **CTO.**

---

**Q84 — « Quelles données sont chiffrées au repos, où vivent les clés, et qui peut les lire ? »**
*Domaine : sécurité et exploitation · Constats : contre-audit A11 §E, questionnaire A05 domaine IAM, DR-091*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Le questionnaire de sécurité prérempli reste sans réponse sur l'ensemble de ce domaine. Ce qui est établi et qui donne sa portée à la question : le canal automatisé lit les contacts, les soumissions de formulaires et les métriques commerciales sans aucun contrôle serveur (M-017, M-004). Les données concernées ne sont donc pas théoriques.
- **Ce qui manque pour répondre pleinement.** Le périmètre du chiffrement au repos, l'emplacement et la rotation des clés, la matrice nominative des accès à la production, et la revue périodique de cette matrice.
- **Pièce justificative attendue.** Description de la gestion des secrets, matrice d'accès à la production avec dates de revue. **CTO.**

---

**Q86 — « Que faites-vous si votre hébergeur ou votre fournisseur d'emails coupe le service pour violation de politique d'usage ? »**
*Domaine : technique et continuité · Constats : contre-audit A11 §E, A02-028, A05-018, DR-070*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et le scénario n'est pas théorique pour cette plateforme : elle héberge du contenu de tiers, son cœur de cible longe la liste des activités restreintes de son prestataire de paiement (Q23), la publication est sans contrôle (M-010) et le domaine enregistrable est partagé entre tous les sites clients et l'application (Q39). Trois fournisseurs peuvent couper : l'hébergeur, le fournisseur d'emails et le prestataire de paiement. Aucun plan de bascule n'a pu être vu.
- **Ce qui manque pour répondre pleinement.** Un plan de bascule écrit par fournisseur critique, un fournisseur de secours identifié pour chacun, la lecture des clauses de résiliation, et le délai réel de bascule mesuré plutôt qu'estimé.
- **Pièce justificative attendue.** Inventaire des fournisseurs critiques avec substituabilité, plan de bascule, contrats et conditions de résiliation. **CTO + CEO.**

---

# Bloc 7 — Sécurité et isolement multi-locataire

Le risque principal revendiqué par le dossier lui-même. Les deux premières questions de ce bloc n'existaient pas dans la version 1 : **elles sont aujourd'hui les moins chères à refermer et les plus lourdes à laisser ouvertes.**

---

**Q87 — « Peut-on, via votre API ou votre serveur MCP, lire ou modifier un objet appartenant à un autre compte en fournissant simplement son identifiant ? L'avez-vous testé ? »**
*Domaine : sécurité · Constats : contre-audit A11 M-8, MT-09, Q-026, M-008*

- **Réponse factuelle aujourd'hui.** **Non testé, et c'est le trou le plus difficile à justifier du dossier.** Le protocole de test du livrable 05 comporte quatre tests — envoi en plan Pro, injection indirecte, contexte persistant, bascule de compte — et **aucun ne porte sur l'autorisation au niveau de l'objet** : fournir à `update_webpage`, `get_form`, `list_contacts` ou `execute_action` l'identifiant d'un objet appartenant à un autre locataire. C'est la première classe de vulnérabilité des API selon la référence du secteur, et c'est le risque que le dossier lui-même désigne comme « le risque principal de la plateforme ». Le paradoxe est complet : l'audit disposait du seul canal permettant de le tester sans rien casser — **une simple lecture avec un identifiant étranger** — et ne l'a pas fait, tout en exécutant une publication réelle sur un compte de production. Le multi-comptes a par ailleurs été observé mais pas éprouvé : `list_accounts` ne renvoyait qu'un compte (CONFIRMÉ, M-008).
- **Ce qui manque pour répondre pleinement.** Le test lui-même, en lecture puis en écriture, sur deux locataires réellement distincts ; un test automatisé en intégration continue **qui échoue si une requête franchit la frontière de locataire** ; et la couverture explicite de ce point dans le cahier des charges du test d'intrusion. Le premier coûte quinze minutes une fois le locataire dédié créé.
- **Pièce justificative attendue.** Relevé de test d'autorisation objet par objet sur deux locataires, extrait de la suite de tests automatisés, chapitre dédié du rapport de test d'intrusion, preuve d'isolation au niveau de la base. **CTO + prestataire externe.**
- **Formulation à ne pas dépasser.** « Nous ne l'avions pas testé au moment de l'audit ; c'est fait depuis, voici le relevé daté, et un test automatisé échoue désormais en intégration continue si la frontière de locataire est franchie. » **Il n'existe pas d'autre réponse acceptable.** Répondre « notre ORM filtre par locataire » sans relevé sera traité comme une absence de réponse, et c'est la question que l'auditeur du fonds posera en premier.

---

**Q88 — « Comment s'authentifient les acheteurs et les élèves ? Une même identité vaut-elle chez plusieurs créateurs ? Où vit ce cookie, avec quelle portée ? »**
*Domaine : sécurité · Constats : contre-audit A11 M-9, MT-10, A05-001 à A05-005*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et le dossier n'a **jamais examiné cette surface**. Le questionnaire de sécurité la soulève en une ligne — « les acheteurs et élèves ont-ils une identité globale réutilisée entre créateurs ? point de conception potentiellement majeur » — et cette ligne n'a traversé aucun autre document : ni le registre des risques, ni le plan, ni la version 1 de ce document, ni l'index de la data room. Toute l'analyse de topologie de domaine porte sur la session **créateur**. Or les acheteurs et les élèves sont **les seuls utilisateurs qui paient**.
- **Ce qui manque pour répondre pleinement.** Le schéma d'authentification des espaces membres : comment on s'y connecte, où vit le cookie, quelle est sa portée sur le domaine enregistrable partagé, comment fonctionne la réinitialisation de mot de passe, et si une identité d'acheteur est cloisonnée par locataire ou partagée entre créateurs. Le scénario à écarter explicitement est le plus grave que le dossier ait effleuré : **une identité d'acheteur partagée entre locataires, combinée à du JavaScript libre chez les comptes Pro, permet à un créateur de viser les acheteurs des autres.**
- **Pièce justificative attendue.** Schéma d'authentification acheteur et élève, relevé de cookies sur un espace membre réel, réponse écrite sur l'unicité ou le cloisonnement des identités, couverture explicite dans le périmètre du test d'intrusion. **CTO.**
- **Formulation à ne pas dépasser.** « Les identités d'acheteurs et d'élèves sont cloisonnées par locataire ; voici le schéma et le relevé de cookies. » Si elles ne le sont pas, ne pas le présenter comme une commodité produit sans énoncer d'abord les mesures compensatoires : une identité globale sur un domaine partagé est un choix défendable, mais seulement s'il est instruit.

---

**Q6 — « Vos sites clients et votre application partagent le même domaine enregistrable. Comment isolez-vous les cookies de session ? »**
*Domaine : sécurité · Constats : A05-001 à A05-005, A05-013*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et c'est le point que l'auditeur technique regardera en premier. Ce qui est établi au statut PROBABLE : les hôtes connus (`tinypages.co`, `app.tinypages.co`, `docs.tinypages.co` et les sous-domaines clients) partagent le domaine enregistrable `tinypages.co`. Ce qui n'a pas pu être relevé, l'egress étant fermé : les attributs réels des cookies de session, l'inscription éventuelle de `tinypages.co` à la Public Suffix List, la politique CORS de l'API, et le mécanisme anti-CSRF. La règle générale, qui n'est pas un constat sur TinyPages : sans inscription à la PSL, le navigateur traite tous ces hôtes comme un seul site ; `SameSite=Lax` ne protège alors pas entre un site client et l'application, et une protection CSRF fondée sur `SameSite` seul serait inopérante. Toute l'industrie comparable sépare les deux plans sur deux domaines enregistrables distincts.
- **Ce qui manque pour répondre pleinement.** Un relevé de trente minutes : attributs du cookie de session (préfixe `__Host-`, attribut `Domain`, `Secure`, `HttpOnly`, `SameSite`), recherche de `tinypages` dans la Public Suffix List, configuration CORS de la passerelle, et attribut `sandbox` complet du bloc de code personnalisé avec le domaine enregistrable de son `src`.
- **Pièce justificative attendue.** Relevé navigateur horodaté sur un compte de test, extrait de configuration CORS, et décision documentée sur la séparation des domaines. **CTO.**
- **Formulation à ne pas dépasser.** Si le cookie n'est pas verrouillé : « Le cookie de session est aujourd'hui [état réel]. Le correctif immédiat est son verrouillage en `__Host-` sans attribut `Domain` ; la cible est la séparation des sites clients sur un domaine enregistrable distinct, chantier de [délai], l'inscription à la Public Suffix List ne produisant son effet qu'après diffusion dans les navigateurs. » Ne pas présenter l'inscription à la PSL comme un correctif d'urgence : elle ne l'est pas.

---

**Q38 — « Que se passe-t-il quand un créateur part ? Son sous-domaine peut-il être repris par un tiers ? »**
*Domaine : sécurité · Constats : A05-009, A05-010, A05-011, A05-012*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Aucune règle de réservation d'identifiants de sous-domaine n'est documentée publiquement, aucune politique de non-réutilisation non plus. Trois risques de reprise se posent, tous au statut HYPOTHÈSE faute d'observation : un identifiant évocateur créé par un tiers (`secure-billing`, `verify`, `login`) constituerait une plateforme d'hameçonnage prête à l'emploi sur le domaine de la marque, avec certificat valide ; un identifiant libéré par un créateur parti hérite du référencement, des liens entrants et de la confiance acquise ; un domaine personnalisé dont l'enregistrement DNS survit à la résiliation peut être revendiqué par un autre compte — dans ce schéma, c'est TinyPages qui est le service tiers vulnérable, pas la victime.
- **Ce qui manque pour répondre pleinement.** L'existence d'une liste de mots réservés, d'une quarantaine des identifiants libérés, d'une preuve de propriété à l'ajout d'un domaine personnalisé et de sa revérification périodique, et la condition d'émission des certificats : vérification de propriété ou simple résolution DNS ?
- **Pièce justificative attendue.** Politique de nommage et de cycle de vie des hôtes, documentée et datée. **CTO.**

---

**Q39 — « Le plan gratuit permet de publier des pages pilotées par IA sur un sous-domaine de votre marque. Qu'est-ce qui vous protège d'une campagne d'hameçonnage ? »**
*Domaine : sécurité · Constats : M-010, M-012, A05-013, A05-014, A05-018, A04-018*

- **Réponse factuelle aujourd'hui.** Rien qui ait pu être observé. Le bloc de code personnalisé est réservé au plan payant, ce qui écarte une variante du risque (M-011, M-012) ; mais la publication est sans contrôle (M-010), aucune limite de débit sur la création et la publication n'est documentée, aucune détection d'abus ni procédure publique de signalement n'a été trouvée. Le risque réel se formule précisément : plan gratuit, plus pilotage par IA, plus publication sans contrôle. Une page trompeuse sans JavaScript, sur un sous-domaine de la marque avec certificat valide, reste créable et publiable en deux appels automatisés. Le JavaScript n'est pas nécessaire pour tromper un visiteur. Et parce que le domaine enregistrable est partagé, un signalement portant sur `tinypages.co` peut retirer d'un coup tous les sites clients, l'application et le site vitrine, et dégrader la délivrabilité des emails contenant des liens vers ces hôtes. C'est un risque de continuité d'activité, pas seulement de sécurité.
- **Ce qui manque pour répondre pleinement.** Limites de création par compte et par adresse sur le plan gratuit, vérification à l'inscription, détection de similarité de marques, mise en file d'attente au-delà d'un seuil, surveillance quotidienne de la réputation du domaine, canal de signalement public, et plan de crise pour le scénario « le domaine est signalé comme dangereux par un navigateur majeur ».
- **Pièce justificative attendue.** Politique anti-abus écrite, relevé de réputation du domaine, registre des signalements et des retraits sur douze mois. **CTO + CEO.**

---

**Q37 — « L'authentification à deux facteurs existe-t-elle pour les comptes créateurs, et est-elle obligatoire pour vos accès internes ? »**
*Domaine : sécurité · Constats : A05-015, A05-016, questionnaire IAM*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Aucune source publique n'évoque l'authentification à deux facteurs. Une connexion par un fournisseur d'identité tiers est annoncée au point de départ de l'audit, ce qui délègue de fait le second facteur pour les comptes concernés, mais ne dit rien des comptes ouverts par email. La gestion des sessions — durée de vie, révocation, sessions concurrentes, déconnexion de tous les appareils — est également Non déterminée. Un compte créateur donne accès à la liste de contacts et au flux de paiement.
- **Ce qui manque pour répondre pleinement.** L'existence de l'authentification à deux facteurs pour les comptes email, la possibilité pour un créateur de l'imposer à son équipe, et son caractère obligatoire pour les accès administrateurs internes. Plus une question systématiquement posée en due diligence : les employés peuvent-ils prendre l'identité d'un client, et est-ce journalisé et consenti ?
- **Pièce justificative attendue.** Captures du produit, politique d'accès à la production (nombre de personnes, moindre privilège, revue périodique, journalisation). **CTO.**

---

**Q36 — « Quand a eu lieu votre dernier test d'intrusion, et couvrait-il l'isolement entre locataires ? »**
*Domaine : sécurité · Constats : A05 questionnaire TVM-01, TVM-02*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, aucun rapport public. C'est la pièce qui manque le plus au dossier sécurité : sans elle, le questionnaire de sécurité prérempli reste majoritairement sans réponse — 85 lignes « inconnue » sur 97.
- **Ce qui manque pour répondre pleinement.** Date, prestataire, périmètre, constats, état de remédiation. Et une exigence de périmètre, en trois points nommés faute de quoi le rapport ne vaudra rien : **l'autorisation au niveau de l'objet** entre locataires (Q87), l'**authentification des acheteurs et des élèves** (Q88), et la chaîne d'**injection indirecte** (Q33). Un test qui ne couvre pas ces trois points ne couvre pas le risque principal de ce produit.
- **Pièce justificative attendue.** Rapport de test d'intrusion et journal de remédiation. **CTO.** Si aucun test n'a eu lieu, le dire et annoncer la commande, avec périmètre et date.

---

**Q89 — « Avez-vous eu un incident de sécurité ou une violation de données depuis le lancement ? Tenez-vous un registre ? »**
*Domaine : sécurité et conformité · Constats : contre-audit A11 §G, DR-104, DR-150*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour ; l'audit n'a vu ni registre des violations, ni plan de réponse à incident, ni procédure de notification. L'absence de trace publique ne prouve rien : elle signifie seulement que rien n'a été rendu public.
- **Ce qui manque pour répondre pleinement.** Un registre daté, **même vide**, une procédure de notification sous 72 heures, et le compte rendu du dernier exercice de cette procédure. Un registre vide et daté est une bonne réponse ; « nous n'en tenons pas » n'en est pas une, parce que la tenue du registre est une obligation et non une bonne pratique.
- **Pièce justificative attendue.** Registre des violations, plan de réponse à incident, procédure de notification, compte rendu du dernier exercice. **CEO + CTO + avocat *(à mandater)*.**

---

**Q40 — « Votre environnement de test contient-il des données réelles ? »**
*Domaine : sécurité · Constats : réserve du screening, questionnaire CCC-02*

- **Réponse factuelle aujourd'hui.** Sur le compte fourni à cet audit comme « compte de test dédié », la réponse est oui. Le relevé `get_account` montre le nom réel du dirigeant, un sous-domaine portant son nom, son adresse email personnelle en adresse de réponse, un contact correspondant à cette même adresse, et des brouillons créés pendant la session d'audit : le compte est en usage actif, ce n'est pas un banc d'essai stérile. **CONFIRMÉ.** Les relevés issus de ce compte restent valides — ils portent sur le comportement par défaut de la plateforme, pas sur des données clientes — mais le point doit être signalé, et il documente en lui-même la séparation des environnements. Par ailleurs, **trois traces** laissées par l'audit ne peuvent pas être effacées par le canal automatisé : une page en brouillon, un message en brouillon (M-014) et le champ de contexte métier du compte, qui porte désormais `-` au lieu d'être vide (M-018). Elles doivent être retirées manuellement en interface.
- **Ce qui manque pour répondre pleinement.** L'existence d'environnements séparés, la règle sur les données réelles en préproduction, et un locataire réellement dédié pour les tests et le futur test d'intrusion.
- **Pièce justificative attendue.** Description des environnements, règle écrite sur les données de test, création d'un locataire dédié. **CTO.**

---

# Bloc 8 — IA, serveur MCP et harnais

Le domaine où l'audit a produit le plus de preuves, parce que c'est le seul canal qu'il a pu exercer — et où les constats sont les plus défavorables. Quatre réponses de ce bloc sont établies par exécution réelle et ne peuvent pas être adoucies.

---

**Q2 — « Qu'est-ce qui empêche techniquement votre IA de publier une page sans validation humaine ? »**
*Domaine : IA et MCP · Constats : M-010, M-011, A04-007*

- **Réponse factuelle aujourd'hui.** Rien. Le test a été exécuté le 18 septembre 2026 sur le compte connecté : une page a été créée, puis `publish_webpage` a été appelée **en violant délibérément** la consigne « Do NOT call publish_webpage » inscrite dans la description de l'outil. La publication a réussi immédiatement et le serveur a retourné une URL publique. Aucune confirmation demandée, aucune revue humaine, aucun délai, aucune restriction de plan. **CONFIRMÉ, impact bloquant.** Les mentions « saved as a draft », « the user decides when to publish » sont exclusivement du texte adressé à un modèle que TinyPages ne contrôle pas.
- **Ce qui manque pour répondre pleinement.** Rien, du point de vue du constat : il est établi. Ce qui manque est le correctif — une confirmation appliquée côté serveur, en deux temps, sur la publication, la dépublication, l'envoi et la programmation — et le journal qui permettrait au créateur de voir *a posteriori* ce que l'IA a publié.
- **Pièce justificative attendue.** Code du serveur MCP montrant le mécanisme de confirmation, une fois livré, et le journal d'actions horodaté distinguant acteur humain et acteur IA. **CTO.** D'ici là, la seule pièce honnête est le relevé de test lui-même.
- **Formulation à ne pas dépasser.** « Aujourd'hui, le comportement par défaut du modèle est de rester au brouillon ; ce comportement est porté par les descriptions d'outils, pas par un contrôle d'accès. Un utilisateur, ou un modèle, qui demande explicitement la publication l'obtient. Le correctif — confirmation serveur en deux temps et journal des actions — est en cours et sera livré avant [date]. » Toute formulation présentant ce comportement comme une garantie sera démentie en un appel.

---

**Q3 — « Vos garde-fous sont-ils appliqués par votre serveur ou écrits dans un prompt adressé à un modèle tiers ? »**
*Domaine : IA et MCP · Constats : M-010, M-011, M-014, A04-007, A04-008*

- **Réponse factuelle aujourd'hui.** Écrits dans les descriptions d'outils et dans le champ `instructions` du serveur, à deux exceptions près, et ces deux exceptions sont commerciales. Le screening a établi que le serveur **sait** refuser une action : `create_webpage` avec un bloc `codeHtmlBlock` et `send_email` retournent tous deux `402 PRO_PLAN_REQUIRED`. Il le fait pour deux actions payantes, et pas pour la publication. **CONFIRMÉ.** Un troisième test, exécuté le 18 septembre 2026 à 21:05 UTC, confirme le motif et l'aggrave : `update_business_context` — le champ libre de 10 000 caractères injecté dans toutes les générations futures — **s'écrit sans aucun contrôle serveur sur un compte gratuit**, et la **remise à vide est refusée** par le serveur (« expected string to have >=1 characters »). Le serveur contrôle donc ce qui se facture et ce qui casse son schéma, jamais ce qui expose l'utilisateur (CONFIRMÉ, M-018). Le seul garde-fou structurel non commercial est une absence de fonction : aucune action du catalogue ne supprime un contact, un produit, une page, un article, un formulaire, une leçon ou un email — trois suppressions existent, mais sur des objets secondaires (`delete_order_bump`, `delete_upsell`, `delete_automation_stop_condition`).
- **Ce qui manque pour répondre pleinement.** La confirmation que le mur de facturation sur `send_email` n'est pas le seul contrôle : le message d'erreur implique qu'un compte au plan payant enverrait sans autre barrière. Ce test n'a pas été fait. C'est la seule question laissée ouverte par le screening sur ce point.
- **Pièce justificative attendue.** Rejeu du test d'envoi sur un compte au plan payant dédié, avec relevé, et code du serveur pour les actions de publication et d'envoi. **CTO.**
- **Formulation à ne pas dépasser.** « Les contrôles appliqués aujourd'hui côté serveur sont des contrôles de plan. Les garde-fous de sécurité sont des comportements par défaut du modèle. Nous les déplaçons côté serveur. » Ne jamais employer le mot « garantie » avant que ce déplacement soit livré et testé.

---

**Q94 — « Votre contexte métier persistant oriente toutes les générations futures. Qui peut l'écrire, et peut-on l'effacer ? »**
*Domaine : IA et MCP · Constats : M-015, M-018, P-095, Q-077, DR-123*

- **Réponse factuelle aujourd'hui.** **N'importe quel client IA connecté peut l'écrire, sur un compte gratuit, sans aucun contrôle serveur — et il ne peut pas le remettre à vide.** Le test a été exécuté le 18 septembre 2026 à 21:05 UTC, en réponse directe à une objection du contre-audit qui relevait à juste titre qu'une ligne « aucun contrôle » figurait dans un tableau de résultats pour une action jamais appelée. L'action a donc été exécutée plutôt que rétrogradée : `update_business_context` écrit le champ immédiatement (177 caractères, aucune confirmation, aucune restriction de plan) ; la remise à vide est **refusée** par le serveur (« expected string to have >=1 characters ») ; une réduction à un caractère passe. **CONFIRMÉ, M-018.** Deux conséquences. D'une part, le champ libre de 10 000 caractères injecté dans toutes les générations futures, ainsi que les deux emplacements de consignes système du compte, sont à la portée de tout ce qui obtient une écriture — connexion MCP, injection indirecte, accès compromis. D'autre part, **l'IA peut écrire mais ne peut pas défaire son écriture** : une injection persistante introduite par erreur ou par malveillance n'est pas retirable par le canal qui l'a produite. C'est une variante de M-014, et elle est plus gênante, parce que ce qui subsiste n'est pas un brouillon, c'est une instruction.
- **Ce qui manque pour répondre pleinement.** Une règle de protection de ces champs : confirmation côté serveur avant écriture, journalisation dédiée, affichage au créateur de la valeur courante, historique des versions, et **une voie d'effacement par le même canal**. Plus le rejeu du test sur un compte Pro, pour savoir si le mur de facturation change quelque chose ici — rien ne le laisse penser.
- **Pièce justificative attendue.** Relevé du test M-018, règle de protection du contexte métier et des consignes système, journal des écritures de ces champs, capture de l'écran qui les expose au créateur. **CTO.**
- **Formulation à ne pas dépasser.** « Le contexte métier est aujourd'hui écrit par le canal automatisé sans contrôle serveur, et il ne peut pas être vidé par ce canal. Nous l'avons établi par test et voici le relevé. Le correctif est [X], livré le [date]. » **Trace laissée par l'audit, à signaler et à nettoyer :** le champ du compte de test porte la valeur `-` au lieu d'être vide, et seule l'interface permet de le corriger.

---

**Q12 — « Votre outil `execute_action` ouvre quatre-vingts actions derrière une seule autorisation. Comment un utilisateur peut-il donner un consentement éclairé ? »**
*Domaine : IA et MCP · Constats : M-006, A04-003, A04-004*

- **Réponse factuelle aujourd'hui.** Il ne le peut pas, et le constat est établi par construction. Le serveur expose 24 outils directs ; toute autre action passe par `search_actions` puis `execute_action`, qui couvre 80 actions supplémentaires, dont les 10 de publication et de dépublication, les 3 de suppression et les 2 d'envoi. Les clients MCP demandent l'autorisation **par outil** : un seul « Toujours autoriser » sur `execute_action` ouvre l'ensemble du catalogue, sans nouvelle question. **CONFIRMÉ pour l'architecture** — les 24 outils directs ont été chargés dans la session et la passerelle a été empruntée en exécution réelle (publication, dépublication, envoi) ; **la ventilation des 80 actions restantes provient de la description publiée par le serveur, non d'une exécution action par action**. Un auditeur qui inspecte la connexion voit un quart de la surface réelle. Conséquence adjacente : les critères d'entrée dans les annuaires de connecteurs reposent sur les annotations d'outils (`readOnlyHint`, `destructiveHint`), que cette architecture rend inopérantes.
- **Ce qui manque pour répondre pleinement.** Le correctif : exposer les 15 actions à effet public ou irréversible comme des outils nommés et annotés, hors de `execute_action` ; et publier le catalogue complet avec les schémas des 104 actions.
- **Pièce justificative attendue.** Nouvelle liste d'outils exposée par le serveur, et export du catalogue complet avec schémas, joint en annexe de data room. **CTO.**
- **Formulation à ne pas dépasser.** « Notre architecture actuelle regroupe le catalogue derrière une passerelle unique, ce qui simplifie l'usage mais agrège le consentement. Nous séparons les actions à effet public en outils nommés et annotés d'ici [date]. »

---

**Q90 — « `execute_action` revalide-t-il le compte, le plan et la portée à chaque appel, ou fait-il confiance à l'identifiant d'action qu'il reçoit ? »**
*Domaine : IA et MCP · Constats : contre-audit A11 §F, M-006, A04-003, A04-004*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et la question prolonge Q12 là où elle compte. Ce qui est établi : la passerelle fonctionne, elle a été empruntée en exécution réelle pour publier, dépublier et tenter un envoi, et elle applique **au moins** un contrôle de plan, puisqu'elle a retourné `402 PRO_PLAN_REQUIRED` sur `send_email` (CONFIRMÉ, M-011). Ce qui n'est pas établi : si cette validation est systématique, si elle porte aussi sur le compte cible et sur la portée du jeton, et si l'identifiant d'action reçu de `search_actions` est revérifié ou traité comme une preuve d'intention légitime.
- **Ce qui manque pour répondre pleinement.** La démonstration par le code, et surtout un **test négatif** : un test automatisé qui échoue si la revalidation est retirée. Sans ce test, la propriété n'est pas garantie dans le temps.
- **Pièce justificative attendue.** Extrait de code commenté du point d'entrée `execute_action`, test négatif intégré à la suite automatisée. **CTO.**

---

**Q30 — « Existe-t-il un journal des actions faites par l'IA, et une annulation ? »**
*Domaine : IA et MCP · Constats : A04-017, A04-008, M-014*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, aucune trace dans le catalogue relevé de 104 actions, et le contexte aggrave le manque. À distinguer soigneusement d'un **journal serveur des appels MCP**, qui peut très bien exister côté TinyPages sans être exposé au produit : c'est une question séparée, et la réponse la plus favorable du dossier si elle est positive (Q91). Les instructions du serveur demandent au modèle de ne jamais exposer le code produit ni décrire le contenu écrit : l'utilisateur ne voit donc ni ce qui a été appelé, ni ce qui a été écrit, pendant l'action. Sans journal *a posteriori*, il n'existe aucun moyen de savoir ce que l'IA a fait. Le screening a par ailleurs établi que l'IA ne peut pas défaire ce qu'elle a fait : deux brouillons de test subsistent et doivent être supprimés manuellement en interface (CONFIRMÉ, M-014), et le champ de contexte métier écrit par l'audit ne peut pas être remis à vide par le même canal (CONFIRMÉ, M-018). **Trois traces au total**, dont aucune n'est effaçable par le canal automatisé.
- **Ce qui manque pour répondre pleinement.** Un journal horodaté par action, distinguant acteur humain et acteur IA, avec identifiant de session, action, objet et résultat ; une annulation ; un historique des versions des pages.
- **Pièce justificative attendue.** Capture du produit montrant le journal, ou **confirmation écrite de son absence**. **CTO.** C'est la première demande d'un auditeur technique et la première demande d'une autorité de protection des données.

---

**Q91 — « Disposez-vous aujourd'hui d'un journal serveur des appels MCP, distinct du journal utilisateur qui reste à construire ? »**
*Domaine : IA et MCP · Constats : contre-audit A11 §F, A04-017, DR-112*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Le dossier a établi l'absence de journal **côté produit** — l'utilisateur ne voit ni ce qui a été appelé ni ce qui a été écrit (Q30) — mais il n'a jamais posé la question du journal **côté serveur**, qui est une infrastructure d'exploitation ordinaire et qui peut parfaitement exister déjà.
- **Ce qui manque pour répondre pleinement.** L'existence du journal, sa durée de rétention, les champs conservés — compte, session, action, objet, résultat, horodatage — et la capacité à en extraire l'historique d'un compte donné.
- **Pièce justificative attendue.** Extrait de journal anonymisé, politique de rétention, exemple d'extraction par compte. **CTO.**
- **Formulation à ne pas dépasser.** Si ce journal existe, le dire tôt et fort : **c'est la meilleure réponse possible du dossier sur ce domaine.** Il permet de reconstituer l'historique des actions d'IA avant même la livraison de la fonctionnalité produit, et c'est exactement ce qu'une autorité de protection des données demandera en premier. S'il n'existe pas, ne pas laisser croire le contraire par une réponse évasive : la question sera reposée par écrit.

---

**Q7 — « Avez-vous une batterie de tests rejouée à chaque nouveau modèle Claude ? Quelle est votre politique de gel de version, et comment apprenez-vous qu'un modèle a changé ? »**
*Domaine : IA et MCP · Constats : A04-030, A04-032*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, aucune trace publique, et il faut considérer par défaut qu'il n'y en a pas. C'est le risque produit le plus structurant du dossier, pour une raison mécanique : les garde-fous observés sont en langage naturel et s'appuient sur un modèle tiers mis à jour sans préavis par Anthropic ou OpenAI. Sans batterie rejouée à chaque version, TinyPages ne peut pas savoir qu'une mise à jour de modèle a cassé « ne publie pas sans accord ». Le screening a d'ailleurs montré que cette consigne ne tient pas même sans changement de modèle (Q2).
- **Ce qui manque pour répondre pleinement.** Tout : la batterie elle-même, un cas par garde-fou (ne pas publier sans accord, ne pas envoyer sans accord, ne pas suivre les instructions contenues dans le champ libre d'un contact, ne pas changer de compte seul), exécutée sur chaque modèle **et chaque client** supporté, avec seuils bloquants et journal de résultats daté. Et le dispositif qui va avec, que le dossier n'a jamais posé : un **compte canari** en production sur lequel la batterie tourne en continu, une politique de **gel de version** du modèle, et le moyen d'apprendre qu'une version a changé — aujourd'hui, TinyPages l'apprendrait par un client.
- **Pièce justificative attendue.** Batterie versionnée, résultats de la dernière exécution avec dates et versions de modèle, politique de gel de version en cas d'échec, et journal du canari. **CTO.**
- **Formulation à ne pas dépasser.** « Nous n'avons pas de batterie d'évals à ce jour. C'est la première pièce de notre plan produit : [contenu], première exécution le [date], résultats publiés dans la data room. » C'est la pièce qui transforme « nous avons écrit une consigne » en « nous mesurons qu'elle tient ». Son absence, annoncée avec un plan, est moins coûteuse qu'une réponse évasive.

---

**Q33 — « Que se passe-t-il si un visiteur écrit des instructions dans le champ libre d'un formulaire et que le créateur demande ensuite à l'IA de résumer ses nouveaux contacts ? »**
*Domaine : IA et MCP · Constats : A04-010, A04-011, M-004, M-015*

- **Réponse factuelle aujourd'hui.** La chaîne est architecturalement supportée de bout en bout, et elle n'a pas été testée. Ce qui est établi : le catalogue donne à l'IA accès aux contacts, aux soumissions de formulaires et aux listes de destinataires, tous alimentés par des tiers (CONFIRMÉ, M-004) ; l'IA peut écrire et publier sans contrôle (CONFIRMÉ, M-010) ; et le compte porte un champ de contexte métier libre de 10 000 caractères plus deux emplacements de consignes système, modifiables par le canal automatisé, dont le contenu est injecté dans **toutes** les générations futures sans apparaître dans le contenu produit (M-015). L'écriture de ce champ a depuis été **exécutée** : elle réussit sans aucun contrôle serveur sur un compte gratuit, et la remise à vide est refusée par le serveur (CONFIRMÉ, M-018). Autrement dit, le dernier maillon de la chaîne n'est plus une hypothèse d'architecture : il est éprouvé, et il est à sens unique. Le bloc de code personnalisé, lui, est réservé au plan payant (M-011), ce qui restreint une variante de la chaîne mais n'en supprime pas la principale : une page trompeuse sans JavaScript, sur un sous-domaine de la marque avec certificat valide, suffit à abuser un visiteur.
- **Ce qui manque pour répondre pleinement.** Un test d'intrusion dédié à ce scénario exact, mené sur un locataire dédié, avec rapport ; et les contre-mesures à la source : encadrement de tout contenu tiers dans les réponses d'outil par un marqueur de données non fiables, échappement, troncature, et refus côté serveur d'une écriture déclenchée dans le même tour qu'une lecture de contenu tiers.
- **Pièce justificative attendue.** Rapport de test d'intrusion couvrant l'injection indirecte, joint à la data room. **CTO + prestataire externe.** C'est la première question que posera l'auditeur technique mandaté.

---

**Q32 — « Un seul jeton couvre-t-il plusieurs comptes ? Que fait exactement `switch_account` ? »**
*Domaine : IA et MCP · Constats : M-005, M-008, A04-013*

- **Réponse factuelle aujourd'hui.** Le multi-comptes est réel et pilotable par l'IA : `list_accounts` a été **exécutée** pendant l'audit, et `switch_account` figure au catalogue relevé sans avoir été exécutée. Sur le compte observé, `list_accounts` ne renvoie qu'un compte et aucun sous-compte : **le cloisonnement n'a donc pas pu être éprouvé**, faute d'un second compte. La portée de l'autorisation entre comptes est Non déterminée à ce jour. Le risque, s'il se confirme qu'un jeton couvre plusieurs comptes : un modèle peut basculer de lui-même ou sous injection, puis écrire dans le mauvais compte, et rien n'indique que le compte courant soit rappelé avant chaque écriture. Pour une agence gérant plusieurs clients, une injection dans un compte peut produire une publication dans un autre.
- **Ce qui manque pour répondre pleinement.** Un test sur deux comptes réellement distincts, et la règle de portée du jeton.
- **Pièce justificative attendue.** Relevé de test multi-comptes sur deux locataires dédiés, et documentation de la portée du jeton. **CTO.**

---

**Q31 — « Votre serveur MCP implémente-t-il OAuth 2.1 avec PKCE et l'enregistrement dynamique de clients ? Votre compatibilité ChatGPT est-elle réelle ? »**
*Domaine : IA et MCP · Constats : A04-022, A04-023, A04-024, A04-025*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour : les métadonnées OAuth du serveur n'ont pas pu être récupérées, l'egress étant fermé. Deux modes d'authentification sont annoncés : autorisation par navigateur et jeton API statique. La règle générale, qui n'est pas un constat sur TinyPages : le mode développeur d'un client tiers majeur exige OAuth 2.1 **et** l'enregistrement dynamique de clients, et n'accepte pas les jetons porteurs ; il est de plus réservé aux plans payants. Deux conséquences vérifiables en découlent : le mode jeton API ne peut pas fonctionner avec ce client, et si l'enregistrement dynamique n'est pas implémenté, la compatibilité annoncée sur le site est inexacte. À noter également : le serveur est servi depuis un domaine enregistrable distinct de la marque, si bien que l'écran de consentement s'affiche sur un domaine que l'utilisateur ne reconnaît pas.
- **Ce qui manque pour répondre pleinement.** Les deux documents de métadonnées OAuth du serveur, la liste des portées disponibles — l'audit n'a trouvé aucune portée granulaire, l'autorisation semblant être tout ou rien — et le cycle de vie du jeton API : durée, portée, rotation, révocation, journal d'utilisation.
- **Pièce justificative attendue.** Copie des métadonnées OAuth, schéma de la table des jetons, capture de l'écran de gestion des jetons. **CTO.**

---

**Q34 — « Votre documentation affirme que l'envoi d'emails n'est pas accessible via l'IA. Est-ce exact ? »**
*Domaine : IA et MCP · Constats : M-001, A04-006, C-002*

- **Réponse factuelle aujourd'hui.** Non, c'est inexact, et le constat est établi. Le catalogue relevé sur le serveur de production contient `send_email` et `schedule_email`, ainsi que dix actions de publication et de dépublication (M-001). Niveau de preuve exact : `send_email` a été **exécutée** — elle existe et répond, le refus étant un `402 PRO_PLAN_REQUIRED` et non un « action inconnue » ; `publish_webpage` et `unpublish_webpage` ont été **exécutées avec succès** ; `schedule_email` et les huit autres actions de publication sont relevées au catalogue sans avoir été exécutées. La contradiction C-002 du registre est tranchée en faveur de la page marketing : la documentation est fausse ou périmée. Nuance à conserver : par défaut, le modèle est *instruit* de s'arrêter au brouillon, mais la capacité d'envoi est à une phrase d'écart, et sur le compte observé, au plan gratuit, l'envoi est refusé par un mur de facturation, pas par un contrôle de sécurité (M-011).
- **Ce qui manque pour répondre pleinement.** La correction de la documentation, et le test d'envoi sur un compte payant pour savoir si un autre contrôle existe à ce niveau.
- **Pièce justificative attendue.** Page de documentation corrigée avec sa date, relevé du test d'envoi sur compte payant. **CTO.** Une documentation qui sous-déclare une capacité d'envoi de masse est un risque de conformité, pas une coquille.

---

**Q35 — « Êtes-vous répertorié dans un annuaire officiel de connecteurs ? Avez-vous soumis votre serveur ? »**
*Domaine : IA et MCP · Constats : A04-019, A09-013, A04-009*

- **Réponse factuelle aujourd'hui.** TinyPages n'apparaît pas dans les résultats relatifs aux annuaires officiels de connecteurs, et la documentation décrit la procédure d'ajout par URL, qui est précisément la procédure hors annuaire (PROBABLE, absence de preuve n'étant pas preuve d'absence : les annuaires n'ont pas pu être ouverts). Deux points s'y rattachent, tous deux établis. D'une part, l'architecture `execute_action` rend inopérantes les annotations d'outils sur lesquelles reposent les critères d'entrée en annuaire (Q12). D'autre part, le champ `instructions` du serveur assigne un rôle au modèle hôte et lui interdit d'exposer les noms d'outils, d'écrire du code, d'appeler des API externes ou de suggérer des outils tiers : ces directives sortent du périmètre d'un connecteur et s'appliqueraient à des conversations sans rapport. Une revue de sécurité d'un éditeur de modèles lira ce motif comme de l'empoisonnement d'outil.
- **Ce qui manque pour répondre pleinement.** Une réponse à une question factuelle simple : TinyPages a-t-il soumis son connecteur, et avec quel retour ? Un refus déjà essuyé serait une information matérielle pour l'investisseur.
- **Pièce justificative attendue.** Échange écrit avec l'éditeur d'annuaire, ou déclaration écrite qu'aucune soumission n'a été faite. **CEO.**

---

**Q92 — « Un serveur MCP TinyPages hébergé par un tiers existe. Est-il le vôtre ? Est-il autorisé ? Par où passent les identifiants de vos créateurs ? »**
*Domaine : IA et MCP · Constats : contre-audit A11 M-12, P-038, C-004, A04 LLM03*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Le dossier cite trois fois l'existence d'un serveur MCP TinyPages hébergé par Zapier, **uniquement pour affaiblir l'exclusivité revendiquée** (Q1), et jamais comme ce qu'il est aussi : un chemin de données. Personne n'a demandé qui l'exploite, s'il est autorisé, par quel canal d'identifiants il passe, ni s'il existe un contrat.
- **Ce qui manque pour répondre pleinement.** Le statut du connecteur. Deux cas, deux conséquences : s'il est exploité par un tiers sans accord, des données de créateurs transitent par un **sous-traitant manquant à l'inventaire** et par une surface d'API hors contrôle, et le retrait doit être demandé ; s'il a été publié par TinyPages, il doit figurer dans la liste des sous-traitants, dans l'inventaire des intégrations publiées sous la marque, et **dans le périmètre du test d'intrusion**.
- **Pièce justificative attendue.** Accord ou contrat avec l'intégrateur, inventaire des intégrations tierces publiées sous la marque, description du flux d'authentification employé. **CEO + CTO.**

---

**Q93 — « Quand un créateur connecte son client IA, quelles données de ses contacts partent chez l'éditeur du modèle, sous quel contrat, et sont-elles utilisées pour l'entraînement selon le plan de ce créateur ? Que lui dites-vous ? »**
*Domaine : IA et données personnelles · Constats : contre-audit A11 M-11, M-004, M-017, livrable 05 §2.6*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et c'est un angle mort complet du dossier. Celui-ci qualifie correctement le fournisseur du modèle **intégré au produit** de sous-traitant ultérieur, et n'examine jamais le **second flux, qui est pourtant le flux central du produit** : lorsqu'un créateur connecte son client IA, ses contacts, ses soumissions de formulaires, ses listes de destinataires et son chiffre d'affaires sortent chez l'éditeur de ce client, **sous le contrat que le créateur a souscrit à titre personnel**, avec des règles d'entraînement et de rétention qui dépendent de son plan et sur lesquelles TinyPages n'a aucune prise. L'accès de l'IA à ces données est établi (M-004, M-017) ; personne n'en a tiré la conséquence sur la chaîne de traitement.
- **Ce qui manque pour répondre pleinement.** Qui est responsable de ce transfert ; si le créateur en est informé au moment de l'autorisation ; si le futur contrat de sous-traitance le mentionne ; et s'il existe une portée en lecture seule ou une recommandation de plan permettant de limiter ce qui sort. La correction la moins coûteuse est une **mention d'information à l'écran de consentement** — ce n'est pas un chantier, c'est une phrase, et elle appartient à P0.
- **Pièce justificative attendue.** Note de qualification de ce flux, écran de consentement et information affichée à la connexion, clause dédiée du contrat de sous-traitance créateurs, ligne dédiée à la page des sous-traitants. **CEO + avocat *(à mandater)* + CTO.**

---

# Bloc 9 — Emails et délivrabilité

Un risque systémique propre aux plateformes multi-locataires : l'incident d'un seul créateur dégrade la délivrabilité de tout le parc. Aucun relevé DNS n'a été possible.

---

**Q26 — « Comment isolez-vous la réputation d'envoi entre créateurs ? »**
*Domaine : emails · Constats : A03-001, A03-006, A03-008*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Aucun relevé DNS n'a été possible : ni SPF, ni DKIM, ni DMARC des domaines TinyPages et des créateurs. Les mécanismes standards du marché ont été documentés pour servir de grille de lecture — sous-domaine ou domaine dédié par créateur avec sélecteur DKIM propre, chemin de retour personnalisé, pool partagé avec limitation par locataire, IP dédiée au-delà d'un certain volume — mais ce sont des règles générales, pas des constats sur TinyPages. Le risque, lui, est structurel et se formule simplement : sans isolation, l'incident d'un seul créateur peut dégrader la délivrabilité de tout le parc en quelques jours.
- **Ce qui manque pour répondre pleinement.** Le contenu réel des enregistrements SPF, DKIM et DMARC des domaines d'envoi ; la répartition des créateurs entre flux et pools d'adresses ; l'existence d'un mécanisme de suspension automatique au-delà d'un seuil de plaintes ; et le mode de gestion de la liste de suppression, partagée entre créateurs ou cloisonnée.
- **Pièce justificative attendue.** Relevé DNS horodaté sur plusieurs domaines créateurs, capture de la configuration des flux d'envoi, règle de suspension documentée. **CTO.**

---

**Q27 — « Vos créateurs respectent-ils les exigences de Gmail, Yahoo et Microsoft pour les envois en masse ? »**
*Domaine : emails · Constats : A03-004, A03-005*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour pour TinyPages. Les exigences sont établies comme règle générale : depuis février 2024, Gmail et Yahoo imposent aux envoyeurs de masse l'authentification SPF et DKIM, un enregistrement DMARC aligné, une désinscription en un clic conforme, et un taux de plaintes maintenu sous 0,3 % ; l'application a été durcie en novembre 2025, avec rejets permanents. Microsoft a aligné des exigences comparables depuis mai 2025. Aucun en-tête d'email réellement émis par la plateforme n'a pu être analysé.
- **Ce qui manque pour répondre pleinement.** L'analyse des en-têtes d'un email créateur réellement reçu (présence et conformité de l'en-tête de désinscription en un clic, alignement DMARC) et le taux de plaintes agrégé du parc, disponible uniquement dans le tableau de bord du fournisseur d'envoi.
- **Pièce justificative attendue.** En-têtes complets d'un envoi réel anonymisé, capture du taux de plaintes agrégé sur douze mois. **CTO.**

---

**Q28 — « Le double opt-in est-il activé par défaut, et conservez-vous la preuve du consentement ? »**
*Domaine : emails et conformité · Constats : M-007, A03-007, A06-031, A06-032*

- **Réponse factuelle aujourd'hui.** Sur le compte observé, le double opt-in est **désactivé par défaut** (`doubleOptin: false`), la page d'accueil publiée automatiquement porte un bloc de capture d'emails actif, et le contact créé à l'ouverture du compte est enregistré `isSubscribed: true` sans étape de confirmation. **CONFIRMÉ sur un compte, à une date.** Ce que le produit conserve comme preuve de consentement — horodatage, origine, libellé du consentement — est Non déterminé à ce jour. La règle générale, qui n'est pas un constat sur TinyPages : le double opt-in n'est pas une obligation légale universelle, mais la **preuve** du consentement l'est, et une jurisprudence allemande a jugé qu'un simple journal d'événement ne suffit pas.
- **Ce qui manque pour répondre pleinement.** Le modèle de données du contact : porte-t-il une date, une origine et un libellé de consentement, et l'export les inclut-il ? Et ce que conserve l'import en masse : une base migrée qui perd l'origine et la date du consentement devient une base non prouvable — question qui concerne aussi la migration annoncée d'une liste de plus de 70 000 contacts depuis un autre outil.
- **Pièce justificative attendue.** Schéma du modèle de contact, capture d'un export montrant les champs de consentement, procédure d'import documentée. **CTO.**

---

# Bloc 10 — Données personnelles et conformité réglementaire

Le bloc où l'échéance la plus proche du dossier tombe pendant la levée, et où deux vérifications de cinq minutes valent mieux que dix pages de qualification.

---

**Q5 — « Pourquoi la politique de confidentialité publiée par défaut sur chaque compte client est-elle vide ? »**
*Domaine : conformité · Constat : M-007*

- **Réponse factuelle aujourd'hui.** Le fait est établi, il n'est pas contestable, et il se vérifie en ouvrant un compte. À la création d'un compte, TinyPages crée et **publie automatiquement** cinq pages. Deux sont des documents juridiques dont le contenu intégral se réduit à leur titre : `<h1><strong>Politique de confidentialité</strong></h1>` et `<h1><strong>Conditions d'utilisation</strong></h1>`. Elles sont `published` et `indexed: true` dès la première seconde du compte, sans action du créateur. Dans le même temps, la page d'accueil créée par défaut est publiée et porte un bloc de capture d'emails actif, et le compte porte `doubleOptin: false`. **CONFIRMÉ, relevé sur un compte, à une date.** La combinaison est le constat : la plateforme met en ligne, pour chaque créateur, un formulaire de collecte d'adresses adossé à une politique de confidentialité vide, sans double opt-in, et demande aux moteurs de l'indexer.
- **Ce qui manque pour répondre pleinement.** La confirmation que ce comportement est bien celui de tous les comptes et non une particularité du compte observé ; la position de l'avocat sur la responsabilité de TinyPages en tant que fournisseur du gabarit et sous-traitant au sens de l'article 28 ; et le correctif produit.
- **Pièce justificative attendue.** Capture d'un compte fraîchement créé montrant l'état corrigé, gabarits juridiques réellement remplis ou dépubliés par défaut, `indexed: false` sur ces pages tant qu'elles sont vides, double opt-in activé par défaut. **CTO pour le correctif, avocat pour la qualification.**
- **Formulation à ne pas dépasser.** « Nous avons identifié que les pages légales créées par défaut étaient publiées vides et indexées. C'est un défaut de conception que nous corrigeons : [correctif], livré le [date]. » C'est un constat trouvable en cinq minutes par n'importe quel auditeur ouvrant un compte gratuit : le cacher est impossible, l'anticiper est gratuit.

---

**Q41 — « Proposez-vous un contrat de sous-traitance à vos créateurs, et publiez-vous la liste de vos sous-traitants ultérieurs ? »**
*Domaine : conformité · Constats : A06-001 à A06-006, A06-012*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour : aucune page de TinyPages n'a pu être ouverte pendant l'audit. La qualification, elle, est claire et structure tout le reste : dans le modèle décrit, TinyPages est **sous-traitant** des créateurs pour les données de leurs contacts, **responsable de traitement** pour ses propres clients et son site, et **fournisseur de service d'hébergement** au sens du règlement sur les services numériques. Sans contrat de sous-traitance, chaque créateur est en écart, et la plateforme devient un risque juridique pour son propre client. Un piège de cartographie mérite d'être évité d'emblée : les prestataires de paiement agissent en responsables autonomes, pas en sous-traitants ; les présenter comme sous-traitants dans la politique de confidentialité est une erreur fréquente et repérable.
- **Ce qui manque pour répondre pleinement.** Le contrat lui-même, ou les conditions générales qui en tiennent lieu — auquel cas les mentions obligatoires doivent être vérifiées une par une — et une page de sous-traitants datée et versionnée, incluant le fournisseur du modèle d'IA, qui est un sous-traitant ultérieur et non une fonctionnalité produit.
- **Pièce justificative attendue.** Contrat de sous-traitance publié, liste des sous-traitants avec rôle et localisation, registre des traitements, registre des violations. **CEO + avocat.**

---

**Q42 — « Un créateur peut-il supprimer définitivement un contact ? »**
*Domaine : conformité · Constats : A06-008, M-002, M-014*

- **Réponse factuelle aujourd'hui.** Par le canal automatisé, non : aucune action du catalogue relevé ne supprime un contact, un produit, une page, un article, un formulaire, une leçon ou un email. Trois suppressions existent, sur des objets secondaires seulement. Niveau de preuve : l'absence est établie sur l'inventaire **relevé par lecture de la description publiée par le serveur**, et confirmée en pratique par l'audit, qui n'a pas pu supprimer les deux objets qu'il avait créés (M-014) ni remettre à vide le champ qu'il avait écrit (M-018). **Par l'interface, Non déterminé à ce jour** : l'audit n'a pas pu tester l'interface. La distinction est décisive. Si l'impossibilité vaut aussi en interface, une demande d'effacement ne peut pas être exécutée par le créateur, et TinyPages ne peut pas l'assister comme l'exige son rôle de sous-traitant.
- **Ce qui manque pour répondre pleinement.** Un test en interface, en quelques minutes : un contact peut-il être supprimé définitivement, et en combien de clics ? Et la même question pour l'effacement dans les sauvegardes.
- **Pièce justificative attendue.** Capture du parcours de suppression en interface, avec la confirmation de l'effacement effectif. **CTO.** C'est la vérification la moins coûteuse du dossier conformité et l'une des plus discriminantes.

---

**Q95 — « Quelles sont vos durées de conservation, et pouvez-vous prouver qu'une suppression est effective, y compris dans les sauvegardes ? »**
*Domaine : données personnelles · Constats : contre-audit A11 §G, A06-008, M-014, M-018, DR-152*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour pour les durées ; défavorable pour la preuve d'effectivité. Ce qui est établi : le canal automatisé **ne sait rien supprimer** — ni contact, ni produit, ni page, ni email (Q42) — et il ne sait pas non plus vider un champ qu'il a écrit (M-018). L'audit en a fait l'expérience sur ses propres objets. Ce que le dossier ne peut pas dire : si la même limite vaut en interface, et ce qu'il advient des données dans les sauvegardes après une suppression.
- **Ce qui manque pour répondre pleinement.** Un tableau des durées par traitement, la preuve d'exécution d'une purge réelle, et la procédure d'effacement en sauvegarde — y compris la règle appliquée aux sauvegardes déjà constituées, qui est le point où la plupart des réponses s'arrêtent.
- **Pièce justificative attendue.** Tableau des durées de conservation, journal d'une purge réelle datée, procédure d'effacement en sauvegarde, capture du parcours de suppression en interface. **CTO.**

---

**Q43 — « Un bandeau de consentement est-il en place, sur votre site et sur ceux de vos créateurs ? »**
*Domaine : conformité · Constats : A06-010, A06-011, A01-014, M-007*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Le point de départ de l'audit mentionne un pixel publicitaire de réseau social sur la page d'accueil ; ce fait n'a été ni confirmé ni infirmé, la page n'ayant pas pu être ouverte. La règle générale, qui n'est pas un constat sur TinyPages : un pixel publicitaire ne relève d'aucune exception au consentement préalable. La question a deux volets, et le second est le plus lourd : si TinyPages injecte ses propres traceurs sur les pages des créateurs, ou leur permet d'en ajouter sans fournir de mécanisme de consentement, il place ses clients en écart et s'expose lui-même au titre de la conception du service. À rapprocher du constat M-007 : la politique de confidentialité publiée par défaut sur chaque compte est vide.
- **Ce qui manque pour répondre pleinement.** Un test de cinq minutes, à faire dès que l'accès réseau est rétabli : charger le site avec un navigateur vierge et relever les requêtes réseau **avant** toute interaction. Puis la même observation sur un site client.
- **Pièce justificative attendue.** Capture du relevé réseau avant et après consentement, sur le site principal et sur un site client ; description du mécanisme de consentement fourni aux créateurs. **CTO + avocat pour la qualification.**

---

**Q44 — « Le règlement européen sur l'IA impose depuis le 2 août 2026 d'informer l'utilisateur qu'il interagit avec une IA et de marquer les contenus générés. Où en êtes-vous ? »**
*Domaine : conformité · Constats : A06-016, A06-017, A04-029*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Les obligations de transparence sont applicables depuis le 2 août 2026 ; le marquage lisible par machine des contenus générés bénéficie d'un délai de grâce jusqu'au **2 décembre 2026** pour les systèmes mis sur le marché avant le 2 août 2026, soit onze semaines à compter de la date de cet audit. TinyPages met un système d'IA sur le marché sous sa propre marque, ce qui en fait probablement un fournisseur au sens du règlement, même s'il s'appuie sur un modèle tiers. Ces qualifications sont à valider par un avocat, et les références de textes citées proviennent de sources secondaires à reconfirmer.
- **Ce qui manque pour répondre pleinement.** Trois vérifications produit : la mention informant l'utilisateur qu'il parle à une IA, le marquage des contenus générés, et une note de qualification fournisseur ou déployeur. Plus, sans seuil d'effectif, une note de sensibilisation interne datée et son registre.
- **Pièce justificative attendue.** Captures produit, note de qualification signée par un avocat, registre de sensibilisation. **CTO + avocat.** L'échéance du 2 décembre 2026 est la plus proche du dossier : elle tombe pendant la levée.

---

**Q96 — « Votre coach IA s'adresse à des élèves. Certains peuvent-ils être mineurs ? Vérifiez-vous l'âge, et qu'en déduisez-vous en matière d'obligations ? »**
*Domaine : données personnelles et conformité · Constats : contre-audit A11 §G, A06 matrice réglementaire*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et la question n'apparaît nulle part dans le dossier. Elle se pose pourtant mécaniquement : la plateforme héberge des formations vendues à des acheteurs dont elle ne contrôle pas le profil, et elle leur adresse un système d'IA conversationnel sous sa propre marque. Rien n'établit qu'un mineur ne puisse pas s'inscrire à l'espace membre d'un créateur.
- **Ce qui manque pour répondre pleinement.** Une position écrite — public exclusivement majeur, ou traitement du cas des mineurs — le paramétrage produit correspondant, et la clause qui le répercute dans les conditions imposées aux créateurs. Le sujet emporte des conséquences de conformité distinctes sur la base légale, sur l'information et sur la conception du service.
- **Pièce justificative attendue.** Conditions d'utilisation des espaces membres, paramétrage produit, note de position. **CEO + avocat *(à mandater)*.**

---

## Ce que ce document laisse ouvert

- **Vingt-cinq des quatre-vingt-dix-sept questions portent une réponse principalement défavorable** : Q1, Q2, Q3, Q5, Q7, Q12, Q30, Q34, Q39, Q40, Q46, Q51, Q52, Q53, Q54, Q70, Q74, Q79, Q80, Q87, Q88, Q92, Q93, Q94 et Q97. Aucune n'est dissimulable : chacune se vérifie en quelques minutes par un tiers, soit en ouvrant un compte gratuit, soit en connectant le serveur MCP, soit en lisant deux pages publiques, soit — pour les cinq du bloc 0 — en lisant le dossier lui-même.
- **Quatre d'entre elles sont défavorables parce que l'audit ne les a pas posées, et non parce que le produit a échoué** : Q87 (isolement objet à objet), Q88 (authentification des acheteurs et des élèves), Q92 (connecteur tiers sous la marque) et Q93 (données sortant chez l'éditeur du client IA). Les deux premières se referment par deux tests de quinze minutes sur un locataire dédié. **Ce sont les quatre à traiter en premier, parce qu'elles coûtent le moins et qu'elles pèsent le plus.**
- **Les quatre-vingt-dix-sept questions appellent une pièce justificative, et neuf seulement sont disponibles aujourd'hui.** Elles sont toutes reprises dans `audit/livrables/09_index_data_room.md`, qui en compte **214** : 18 prêtes, dont **huit strictement internes et une sur demande** — ce qui laisse **neuf documents réellement versables en l'état**, les livrables 01 à 07, le catalogue MCP et la méthodologie. 165 pièces sont à produire, 31 sont absentes.
- **Plusieurs questions dépendent d'un rôle qui n'est pourvu par personne à ce jour.** Q4, Q11, Q24, Q41, Q43, Q44, Q59, Q60, Q79, Q81, Q89, Q93 et Q96 supposent un conseil juridique ou fiscal, et le paramètre `RESPONSABLES` porte « avocat : à mandater ». Q10, Q16, Q20, Q25, Q64, Q65, Q66, Q67, Q69 et Q73 supposent une fonction financière ou comptable qu'aucun paramètre de l'audit ne désigne. **Deux rôles manquants commandent une quarantaine de pièces de l'index** : c'est le premier arbitrage à rendre, avant toute production.
- **Trois questions se répondent avec le même rendez-vous et la même liasse de pièces** : Q4 (TVA), Q64 (comptabilité, principal ou agent) et Q81 (droit de la consommation, vendeur professionnel) reposent sur le même faisceau de trois faits. Une note unique du conseil les referme toutes les trois ; trois notes séparées garantissent qu'au moins deux se contredisent.
- **Ce document a été produit sans aucun accès interne, avec l'egress réseau fermé et sans test actif.** Les réponses « Non déterminé à ce jour » ne signifient pas que TinyPages ne sait pas répondre : elles signifient que l'audit n'a pas pu l'établir. La plupart se ferment en une journée de travail interne. C'est précisément l'objet du document 09.

---

## Comment s'en servir en séance

1. **Préparer par écrit les douze plus dures**, dans l'ordre du tableau de tête. Aucune ne s'improvise, et chacune a une formulation à ne pas dépasser.
2. **Ouvrir la séance par le bloc 0**, spontanément. Dire que le dossier est une auto-évaluation, dire ce qu'il n'a pas prouvé, et remettre la version 2 des livrables. Un dossier qui annonce sa propre limite avant qu'on ne la trouve achète la crédibilité du reste.
3. **Ne jamais dépasser la formulation indiquée.** Les onze questions dont la réponse est établie par test seront revérifiées en séance ou juste après, et un écart entre la réponse orale et le relevé est la seule chose vraiment coûteuse de ce dossier.
4. **Renvoyer systématiquement à la pièce**, même absente. « Cette pièce n'existe pas encore, elle est en P0, elle est livrée le [date], voici son responsable » est une réponse. « Je vous reviens là-dessus » n'en est pas une.

---

*Document interne de préparation, **strictement interne et jamais remis**. À relire par le CTO, le CEO et l'avocat à mandater avant tout usage. Aucune ligne n'est un avis juridique. Aucune ligne n'est opposable à un tiers en l'état.*
