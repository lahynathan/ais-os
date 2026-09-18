# 09 — Index de la data room

Data room TinyPages · **Version 2 du 18 septembre 2026** · Rédigé par l'orchestrateur de l'audit
Sources : les huit rapports d'agents, le contre-audit `audit/rapports/A11.md`, `audit/annexes/catalogue_mcp_tinypages.md`, `audit/annexes/screening_mcp_compte_test.md`, `audit/annexes/pieces_internes_a_demander.md`, et les livrables 01 à 08.
Lecteurs visés : le CEO et le CTO, pour piloter la constitution du dossier.

> **Ce document est un outil de pilotage interne. Il n'est pas le sommaire remis aux investisseurs.**
> Le sommaire remis est **l'extrait de cet index limité aux lignes portant la diffusion « Investisseurs »**, produit à partir de la colonne Diffusion ci-dessous. Huit pièces de ce dossier ne sont **jamais** remises, et deux d'entre elles seraient dommageables si elles l'étaient par erreur. La convention de diffusion figure en section 0 et **elle prime sur l'état et sur la priorité** : une pièce « Prête » et « P0 » peut être strictement interne.

**Ce qui change depuis la version 1** — corrections issues du contre-audit A11 : la convention de diffusion en trois niveaux est créée (objection M-13) ; le livrable 08, les trois registres et l'annexe de screening passent en strictement interne ; trois états faux sont corrigés et tous les autres vérifiés fichier par fichier (objection M-14) ; le statut du chiffre de 104 actions est aligné sur la réalité de la collecte (objection B-3) ; le nombre de concurrents dotés d'un serveur MCP officiel passe de cinq à sept (objection M-15) ; le constat M-018 est intégré.

---

## 0. Ce que cet index dit, et ce qu'il ne dit pas

**Cet audit a tourné sans aucun accès interne.** Le paramètre `ACCES_INTERNES` porte « aucun », l'agent chargé du volet interne n'a pas été lancé, l'egress réseau était fermé sur tous les domaines TinyPages, et aucun test actif n'était autorisé. Les seules preuves de première main du dossier proviennent du serveur MCP de production et du compte qui y était connecté.

**Conséquence directe, et elle est en soi une information :** la très grande majorité des pièces de cet index est à l'état « à produire ». Cela ne signifie pas qu'elles n'existent pas. Cela signifie que **personne d'extérieur ne les a vues**, ce qui est exactement la situation dans laquelle se trouvera l'auditeur mandaté par les investisseurs. Le décompte de la section 13 chiffre le travail restant.

### Convention d'état

| État | Ce qu'il signifie exactement |
|---|---|
| **Prête** | La pièce existe, **son fichier a été ouvert et vérifié**, et elle est versable en l'état, éventuellement après relecture. Compte tenu de `ACCES_INTERNES = aucun`, seules les pièces produites par l'audit lui-même peuvent porter cet état. **Règle de version 2 : aucune ligne ne porte « Prête » sans vérification de l'existence du fichier.** Trois états faux ont été corrigés à ce titre — DR-001 et DR-021 étaient « À produire » alors que les fichiers existent et sont terminés, DR-013 était « Prête » alors que l'échantillon qu'elle désigne n'existe pas. « Prête » ne dit rien de la diffusion : voir la colonne correspondante. |
| **À produire** | La pièce doit être rassemblée, extraite ou rédigée. Elle existe peut-être déjà en interne : l'audit n'a pas pu le vérifier. Du point de vue de la data room, une pièce qui ne peut pas être montrée n'existe pas. |
| **Absente** | Son objet n'existe pas. L'audit l'a établi, ou l'a établi pour un canal et le présume pour l'autre — la mention **(sauf pièce contraire)** signale ce second cas. Une pièce absente appelle un chantier, pas une extraction. |

### Convention de diffusion

Trois niveaux, et un seul d'entre eux sort de la société. Chaque pièce en porte un dans la colonne **Diffusion**. Le marquage est reporté **en clair et en tête de chaque fichier**, pas seulement dans cet index : un index ne protège pas un document qu'on envoie en pièce jointe.

| Niveau | Ce qu'il signifie exactement | Qui décide |
|---|---|---|
| **Investisseurs** | Pièce versée à la data room et lisible par tout destinataire ayant accès. C'est le niveau par défaut de toutes les pièces d'entreprise attendues. | CEO |
| **Sur demande** | Pièce communiquée après demande motivée, nominativement, sous accord de confidentialité et le plus souvent en séance plutôt qu'en téléchargement. Deux motifs, jamais un autre : la pièce décrit une faiblesse exploitable tant qu'elle n'est pas corrigée (configuration de sécurité, rapport de test d'intrusion, absence de limites de débit), ou elle contient des données personnelles de tiers. | CTO pour le volet technique, CEO pour le reste |
| **Strictement interne** | **Jamais remise, sous aucune forme, à aucun investisseur ni à son auditeur.** Motif : la pièce contient l'identité réelle du dirigeant et l'état de son compte personnel, ou elle expose la stratégie de réponse plutôt que les faits. | CEO, sans délégation |

**Pourquoi ce niveau existe, dit sans détour.** Le livrable 08 porte en tête « ce document n'est pas destiné à être remis tel quel aux investisseurs » et il contient le dispositif « formulation à ne pas dépasser », qui est un manuel de langage de négociation. La version 1 du présent index le classait pourtant « Prête, P0 » dans la table qui devait devenir le sommaire remis. **Une pièce jointe de trop et le dossier ne portait plus sur TinyPages mais sur la manière dont TinyPages prépare ses réponses.** La même remarque vaut pour les trois registres et l'annexe de screening, qui portent le nom réel du dirigeant, son sous-domaine, son adresse personnelle, l'état de son compte, et la phrase selon laquelle un contre-auditeur lisant les paramètres seuls conclurait que l'audit a outrepassé son mandat.

**Règle de conduite, sans exception.** Aucune pièce « Strictement interne » n'entre dans le répertoire partagé de la data room, même dans un sous-dossier fermé. Les pièces internes restent dans `audit/` ; la data room est un répertoire distinct, alimenté pièce par pièce. Une version épurée des registres et une note de synthèse du contre-audit sont prévues à cet effet (DR-190, DR-191) : ce sont elles, et elles seules, qui portent l'information des registres vers l'extérieur.

### Convention de responsable

`audit/00_parametres.md` porte : **CTO : Nathan Lahy · CEO : Nathan Lahy · avocat : à mandater**. Les deux premiers rôles sont tenus par la même personne ; cet index les distingue par nature de décision, technique ou société, comme le fait le plan de mise en place. **Deux rôles nécessaires ne sont attribués à personne à ce jour** : l'avocat, qui reste à mandater, et une fonction financière, qu'aucun paramètre ne désigne. Les pièces qui en dépendent portent la mention correspondante : ce sont autant de pièces sans propriétaire, et c'est un point à trancher avant l'ouverture.

### Convention de priorité

Alignée sur `audit/livrables/07_plan_mise_en_place.md` : **P0** avant l'ouverture de la data room, **P1** avant le closing, **P2** après le closing.

---

## 1. Pièces produites par l'audit

Ce sont les seules pièces prêtes du dossier. Elles décrivent l'état du produit et du risque ; elles ne remplacent aucune pièce de société. **C'est aussi la seule section où la diffusion varie ligne à ligne** : huit de ces pièces ne sortent jamais, et c'est précisément parce qu'elles sont regroupées ici, à côté de pièces parfaitement versables, que le marquage doit être porté sur chaque fichier et pas seulement dans cette table.

| # | Pièce | Ce qu'elle apporte | État | **Diffusion** | Responsable | Priorité |
|---|---|---|---|---|---|---|
| DR-001 | `audit/livrables/01_synthese_executive.md` | Synthèse de deux pages pour l'investisseur | Prête | Investisseurs | CEO | P0 |
| DR-002 | `audit/livrables/02_analyse_technique.md` | Architecture, stack, flux, données, scalabilité, coûts | Prête | Investisseurs | CTO | P0 |
| DR-003 | `audit/livrables/03_analyse_fonctionnelle.md` | Modules, plans, parcours, parité interface / MCP / API, concurrence | Prête | Investisseurs | CTO | P0 |
| DR-004 | `audit/livrables/04_securite_conformite.md` | Posture de sécurité, sous-traitants, matrice réglementaire, questionnaire prérempli | Prête | Investisseurs | CTO | P0 |
| DR-005 | `audit/livrables/05_harnais_ia_mcp.md` | Pilotage par IA, inventaire MCP, menaces, évals | Prête | Investisseurs | CTO | P0 |
| DR-006 | `audit/livrables/06_registre_risques.md` | Risques, mitigations, responsables | Prête | Investisseurs | CEO | P0 |
| DR-007 | `audit/livrables/07_plan_mise_en_place.md` | État cible et remédiation P0, P1, P2 | Prête | Investisseurs | CEO | P0 |
| DR-008 | `audit/livrables/08_qr_investisseurs.md` | 97 questions de due diligence, réponses, manques et pièces attendues — **et le dispositif « formulation à ne pas dépasser »** | Prête | **Strictement interne — jamais remise** | CEO | P0 *(préparation interne)* |
| DR-009 | `audit/livrables/09_index_data_room.md` (ce document) | Index des pièces, état, diffusion, responsables | Prête | **Strictement interne** — seul l'extrait « Investisseurs » est remis, comme sommaire | CEO | P0 |
| DR-010 | `audit/annexes/catalogue_mcp_tinypages.md` | Inventaire des 104 actions exposées à l'IA, **relevé par lecture de la description publiée par le serveur, non exécuté action par action** | Prête | Investisseurs | CTO | P0 |
| DR-011 | `audit/annexes/screening_mcp_compte_test.md` | Relevés d'exécution M-007 à M-018, dont le test de publication, le test d'écriture du contexte IA, et la liste nominative des **17 actions réellement exécutées** | Prête | **Strictement interne** — le document porte le nom, le sous-domaine et l'adresse personnelle du dirigeant | CTO | P0 |
| DR-012 | `audit/annexes/methodologie.md` | Dispositif, contraintes, dérogations, ce que le dossier ne prouve pas | Prête | Investisseurs | CTO | P0 |
| DR-013 | `audit/annexes/echantillon_sites.md` | **Aucun échantillon de sites clients n'existe** ; le fichier est la note qui explique pourquoi et rend le trou visible | **Absente — note d'explication jointe** | Sur demande | CTO | P1 |
| DR-014 | `audit/annexes/pieces_internes_a_demander.md` | Liste priorisée des pièces internes et de ce qu'elles ferment | Prête | **Strictement interne** — document de pilotage, il énumère ce que la société ne sait pas d'elle-même | CEO | P0 |
| DR-015 | `audit/registre/preuves.md` | Registre consolidé des constats et de leurs sources | Prête | **Strictement interne** — identité et compte personnel du dirigeant ; voir DR-190 | CTO | P1 |
| DR-016 | `audit/registre/contradictions.md` | Contradictions relevées, tranchées ou exposées, dont C-026 sur la chaîne de garde | Prête | **Strictement interne** — voir DR-190 | CEO | P0 |
| DR-017 | `audit/registre/questions_ouvertes.md` | Questions ouvertes, avec responsable et pièce qui y répond | Prête | **Strictement interne** — voir DR-190 | CEO | P0 |
| DR-018 | Les huit rapports d'agents `audit/rapports/A01` à `A09` | Matière première et traçabilité des constats | Prête | Sur demande — après relecture d'anonymisation | CTO | P1 |
| DR-019 | Version anglaise des livrables (`audit/livrables/en/`) | `LANGUE = FR + EN` : exigée si des fonds étrangers participent. Le répertoire n'existe pas à ce jour | À produire | Investisseurs | CEO | P1 |
| DR-020 | Captures d'écran à l'appui des rapports (`audit/annexes/captures/`) | Le dossier `A02` est vide et `A07` ne contient qu'un fichier d'explication : aucun parcours n'a pu être capturé | Absente | Investisseurs *(une fois produites)* | CTO | P1 |
| DR-021 | `audit/rapports/A11.md` — contre-audit du dossier | Contrôle qualité prévu par le protocole. **Le rapport existe** : 6 objections bloquantes, 15 majeures, 10 mineures, un verdict de non-diffusion, et 50 questions complémentaires | **Prête** | **Strictement interne** — il expose la stratégie de réponse et la chaîne de garde de la preuve ; voir DR-191 | CEO | P0 |
| DR-190 | Version épurée des trois registres : preuves, contradictions, questions ouvertes | Porte l'information des registres vers l'extérieur sans les noms, sans l'état du compte personnel, sans les jugements de méthode | À produire | Sur demande | CTO | P1 |
| DR-191 | Note de synthèse du contre-audit, expurgée, avec l'état de chaque objection | Une équipe qui montre son propre contre-audit et l'état de ses objections est plus crédible qu'une équipe qui n'en a pas eu. Ce qui se montre est la synthèse, jamais le rapport brut | À produire | Sur demande | CEO | P0 |
| DR-192 | Lettre de mission ou note de cadrage : qui commande, qui exécute, qui relit, qui peut modifier une conclusion — plus le mandat signé du prestataire externe | Q51. Ces travaux sont une auto-évaluation interne ; le laisser implicite se paie en décote de confiance | À produire | Investisseurs | CEO | P0 |
| DR-193 | Version 2 des livrables produite avec l'accès réseau ouvert : en-têtes HTTP, relevés DNS, journaux de certificats, Public Suffix List, pages tarifaires, pages concurrentes, scores de performance | Q52. Tant que ce rejeu n'a pas eu lieu, ces documents sont une note de cadrage assortie d'un sondage. Une demi-journée d'outils referme la moitié des « Non déterminé » | À produire | Investisseurs | CTO | P0 |
| DR-194 | Dérogation écrite, datée et signée ; **rejeu horodaté du protocole M-007, M-010, M-011 et M-018** sur le locataire dédié ; capture du nettoyage des trois traces | Q54. La preuve centrale du dossier a été obtenue sous dérogation orale sur le compte de production du dirigeant. Le rejeu coûte une heure et retire l'objection de recevabilité | À produire | Sur demande | CTO + CEO | P0 |
| DR-195 | Journal de remédiation P0, ligne à ligne, avec preuve et date de livraison ; contrôle qualité final avec ses sept cases vérifiées | Q55. Trois cases sur sept sont acquises à ce jour. C'est la pièce qui distingue une équipe qui a écrit un plan d'une équipe qui l'exécute | À produire | Investisseurs | CEO | P0 |

**Trois réserves à porter en tête de dossier, sans les diluer.**

1. **Le compte utilisé pour les relevés d'exécution n'était pas un banc d'essai stérile** : il porte le nom réel du dirigeant, son sous-domaine, son adresse personnelle et des brouillons actifs. Les relevés restent valides — ils portent sur le comportement par défaut de la plateforme — mais un locataire dédié, gratuit et Pro, doit être créé, et le protocole doit y être **rejoué et horodaté** avant toute diffusion. C'est le rejeu qui devient la pièce, pas la session d'origine.
2. **Trois traces laissées par l'audit subsistent sur ce compte** et aucune n'est effaçable par le canal automatisé : une page en brouillon, un message en brouillon (M-014) et le champ de contexte métier, qui porte désormais `-` au lieu d'être vide (M-018). À nettoyer manuellement en interface, avec capture.
3. **Le chiffre de 104 actions n'a pas le statut qu'on lui donne spontanément.** L'inventaire est **relevé par lecture de la description publiée par le serveur, non exécuté action par action** : l'existence et le nom des actions sont établis, le comportement ne l'est que pour les **17 actions réellement exécutées**, dont la liste nominative figure en annexe. Aucun livrable ne doit écrire « inventaire exécuté » ni « étendue réelle et vérifiée ».

---

## 2. Société, capital et propriété intellectuelle

Aucune de ces pièces n'a pu être vue. Tant que la première n'est pas produite, six lignes de la matrice réglementaire restent sans réponse et l'identité même de l'émetteur reste ouverte.

| # | Pièce | Ce qu'elle ferme | État | **Diffusion** | Responsable | Priorité |
|---|---|---|---|---|---|---|
| DR-022 | Extrait d'immatriculation de l'entité (K-bis ou équivalent) | Q9. Entité, forme, pays d'établissement | À produire | Investisseurs | CEO | P0 |
| DR-023 | Statuts à jour | Q9 | À produire | Investisseurs | CEO | P0 |
| DR-024 | Table de capitalisation | Q9. Répartition du capital | À produire | Investisseurs | CEO | P0 |
| DR-025 | Pacte d'associés | Q9. Gouvernance, clauses de sortie | À produire | Investisseurs | CEO | P0 |
| DR-026 | Note levant la contradiction sur l'identité des fondateurs | Q9. Les paramètres d'audit et les sources publiques ne concordent pas | À produire | Investisseurs | CEO | P0 |
| DR-027 | Mentions légales publiées sur le site | Q9. Obligation d'information, et correction d'une évaluation publique défavorable liée à leur absence apparente | À produire | Investisseurs | CEO | P0 |
| DR-028 | Actes de cession des droits des fondateurs à la société, couvrant le code antérieur à août 2024 | Q11. Titularité du principal actif, blocage de closing classique | À produire | Investisseurs | CEO + avocat *(à mandater)* | P0 |
| DR-029 | Contrats de prestataires, freelances et stagiaires ayant écrit du code, avec clause de cession | Q11 | À produire | Sur demande | CEO + avocat *(à mandater)* | P0 |
| DR-030 | Certificats de dépôt de marque, classes et juridictions | Q11. Une antériorité canadienne homonyme de 2014 a été relevée | À produire | Investisseurs | CEO | P1 |
| DR-031 | Recherche d'antériorités professionnelle | Q11. Distinctivité faible du signe, antériorité relevée | À produire | Investisseurs | CEO + avocat *(à mandater)* | P1 |
| DR-032 | Portefeuille de noms de domaine : titulaire, expirations, verrouillage registrar, authentification forte au registrar | Q11, Q31. `tinypages.co`, `tinypages.dev` et le `.com` doivent avoir le même titulaire que l'entité | À produire | Investisseurs | CTO | P1 |
| DR-033 | Nomenclature logicielle (SBOM) et rapport de licences open source | Q11. Le risque n'est pas la licence permissive, c'est la dépendance copyleft importée sans le savoir dans une offre en ligne | À produire | Investisseurs | CTO | P1 |
| DR-034 | Accès en lecture au dépôt de code pour l'auditeur mandaté | Architecture réelle, tests, couverture, intégration continue, revue par un pair, part de code généré par IA | À produire | Sur demande | CTO | P1 |
| DR-196 | Inventaire des comptes fournisseurs critiques et de leurs titulaires — registrar, prestataire de paiement, hébergeur, fournisseur d'emails, fournisseur de modèle — avec facteurs d'authentification, codes de secours en coffre partagé et procédure de reprise d'accès | Q57. Le compte fourni à l'audit porte l'identité personnelle du dirigeant : l'indice sur la séparation du personnel et du professionnel est défavorable | À produire | Sur demande | CTO + CEO | P0 |
| DR-197 | Politique interne d'usage de l'IA en développement : part de code généré, règle de revue humaine, titularité et licences des sorties de modèle | Q61. Le risque n'est pas la génération, c'est un copyleft importé sans le savoir dans une offre en ligne | À produire | Investisseurs | CTO | P1 |
| DR-198 | Registre des litiges, mises en demeure, réclamations de créateurs, demandes d'autorité et avertissements du prestataire de paiement depuis le lancement — même vide, daté et attesté | Q62. Le cœur de cible longe la liste des activités restreintes du prestataire de paiement : la question est un passage obligé | À produire | Sur demande | CEO | P0 |
| DR-199 | Attestation de responsabilité civile professionnelle et conditions particulières, avec la lecture des exclusions relatives à l'IA générative et à l'hébergement de contenus de tiers | Q63. Sur un produit génératif, les exclusions sont le vrai sujet : elles vident la couverture là où le risque est concentré | À produire | Investisseurs | CEO | P1 |

---

## 3. Financier et traction

Aucun chiffre de traction n'a pu être établi par l'audit. Aucun ne peut l'être de l'extérieur : ils viennent tous du tableau de bord interne et des relevés fournisseurs.

| # | Pièce | Ce qu'elle ferme | État | **Diffusion** | Responsable | Priorité |
|---|---|---|---|---|---|---|
| DR-035 | Comptes annuels et situation comptable intermédiaire | Socle financier de toute due diligence | À produire | Investisseurs | CEO *(fonction financière non attribuée)* | P0 |
| DR-036 | Nombre de comptes actifs et de sites publiés, par mois depuis le lancement | Q10, Q14. Aucune métrique publique exploitable n'existe | À produire | Investisseurs | CEO | P0 |
| DR-037 | Revenu récurrent mensuel et sa décomposition abonnements / commissions | Q10 | À produire | Investisseurs | CEO *(fonction financière non attribuée)* | P0 |
| DR-038 | Volume brut vendu par les créateurs sur douze mois | Q4. Sans lui, l'exposition TVA de l'article 9 bis ne peut pas être chiffrée | À produire | Investisseurs | CEO *(fonction financière non attribuée)* | P0 |
| DR-039 | Cohortes de rétention et churn sur douze mois | Q10 | À produire | Investisseurs | CEO | P1 |
| DR-040 | Répartition des comptes entre plan gratuit et plan payant | Q10, Q46 | À produire | Investisseurs | CEO | P1 |
| DR-041 | Taux d'adoption du serveur MCP parmi les comptes actifs | Q1, Q10. Le pilotage par IA est la proposition de valeur centrale : son usage réel est une métrique attendue | À produire | Investisseurs | CTO | P1 |
| DR-042 | Factures fournisseurs sur douze mois : hébergement, vidéo, email, fournisseur d'IA | Q16, Q25. Aucun coût unitaire n'existe aujourd'hui dans le dossier, sauf un ordre de grandeur email | À produire | Investisseurs | CEO *(fonction financière non attribuée)* | P1 |
| DR-043 | Modèle de coûts unitaires et de marge brute par plan | Q16, Q25 | À produire | Investisseurs | CEO | P1 |
| DR-044 | Plan de trésorerie et besoin de financement | Dimensionnement du tour, non arrêté à ce jour | À produire | Investisseurs | CEO *(fonction financière non attribuée)* | P0 |
| DR-200 | Note comptable de qualification principal ou agent, et méthode de reconnaissance du revenu appliquée dans les comptes | Q64. Volume brut vendu par les créateurs contre commission nette : plusieurs ordres de grandeur sur l'ARR affiché, et un motif classique de renégociation de valorisation | À produire | Investisseurs | CEO + expert-comptable *(non attribué)* | P0 |
| DR-201 | Tableau de rapprochement entre le revenu annoncé, les encaissements du prestataire de paiement et les relevés bancaires sur douze mois, chaque écart expliqué | Q66. Aucune pièce comptable n'a été vue par cet audit | À produire | Investisseurs | CEO *(fonction financière non attribuée)* | P0 |
| DR-202 | Concentration du revenu sur les dix premiers comptes, et part attribuable à l'audience personnelle du fondateur | Q68. Deux signaux publics imposent d'anticiper la seconde moitié de la question ; la découverte par l'investisseur vaut conclusion sur la reproductibilité de la croissance | À produire | Investisseurs | CEO | P0 |
| DR-203 | Chiffrage en euros du plan de remédiation, devis externes, plan de charge nominatif avec chemin critique, hypothèse de renfort chiffrée, et ligne dédiée dans l'usage des fonds | Q70. P0 + P1 pèsent 196 j·p, soit 39 semaines pour une personne. Cette page transforme un plan intenable en argument de levée | À produire | Investisseurs | CEO + CTO | P0 |
| DR-204 | Rapport d'acquisition par page d'entrée et par canal | Q78. Dit si le marché achète la promesse de pilotage par IA ou l'outil marketing | À produire | Investisseurs | CEO | P1 |

---

## 4. Paiements et fiscalité

| # | Pièce | Ce qu'elle ferme | État | **Diffusion** | Responsable | Priorité |
|---|---|---|---|---|---|---|
| DR-045 | Réponse écrite aux trois faits du tunnel d'achat : qui figure sur le reçu et la facture, qui fixe les CGV, qui décide de la mise à disposition du fichier | Q4. Le risque le plus coûteux du dossier se tranche avec ces trois réponses | À produire | Investisseurs | CEO + CTO | P0 |
| DR-046 | Note de qualification fiscale sur l'article 9 bis du règlement d'exécution 282/2011, signée | Q4 | À produire | Investisseurs | Avocat fiscaliste *(à mandater)* | P0 |
| DR-047 | Capture du tableau de bord Connect : type de compte, comptes connectés | Q18 | À produire | Sur demande | CTO | P0 |
| DR-048 | Extrait du code de création du paiement, montrant le type de charge et le mécanisme de commission | Q18, Q19 | À produire | Sur demande | CTO | P0 |
| DR-049 | Contrat plateforme signé avec le prestataire de paiement | Q18, Q23 | À produire | Investisseurs | CEO | P1 |
| DR-050 | Montant de la réserve immobilisée sur le compte plateforme et son évolution sur douze mois | Q18. Trésorerie immobilisée, à déclarer si elle existe | À produire | Investisseurs | CEO *(fonction financière non attribuée)* | P1 |
| DR-051 | Export du taux de litiges consolidé par mois et par compte connecté | Q20. Métrique standard de data room | À produire | Investisseurs | CEO *(fonction financière non attribuée)* | P0 |
| DR-052 | Procédure écrite de traitement des litiges : qui répond, dans quel délai, avec quelles preuves | Q20 | À produire | Investisseurs | CEO | P1 |
| DR-053 | Type d'intégration PayPal et preuve du prélèvement ou non de la commission | Q19. Fuite de commission possible sur le plan gratuit | À produire | Investisseurs | CTO | P1 |
| DR-054 | Capture d'une page de paiement réelle avec la liste complète des scripts chargés | Q21. Détermine le périmètre PCI applicable | À produire | Sur demande | CTO | P0 |
| DR-055 | Attestation de conformité PCI du niveau applicable | Q21 | À produire | Investisseurs | CTO | P1 |
| DR-056 | Règle produit interdisant le bloc de code personnalisé sur une page portant un formulaire de paiement | Q21. Critère d'éligibilité au questionnaire simplifié depuis le 31 mars 2025 | Absente | Investisseurs | CTO | P1 |
| DR-057 | Revue du flux d'upsell en un clic au regard de l'authentification forte, et mandat conservé | Q22 | À produire | Investisseurs | CTO | P1 |
| DR-058 | Statistiques de refus par type de flux : upsell, échéances 2 et 3 des paiements fractionnés, renouvellements | Q22. Mesure empirique de la solidité des exemptions invoquées | À produire | Investisseurs | CTO | P1 |
| DR-059 | Correction de l'incohérence d'arrondi relevée sur un paiement fractionné (100 en trois fois 33) | Q22 | À produire | Investisseurs | CTO | P1 |
| DR-060 | Exemplaire de reçu et de facture réellement émis à un acheteur, anonymisé | Q4, Q24 | À produire | Investisseurs | CTO | P0 |
| DR-061 | Exemplaire de facture de commission adressée à un créateur, et son traitement TVA | Q19, Q24 | À produire | Investisseurs | CEO + expert-comptable *(non attribué)* | P1 |
| DR-062 | Preuve de collecte et de conservation de deux éléments de localisation de l'acheteur par transaction | Q4. Exposition en cas de contrôle sur la TVA du pays du preneur | À produire | Investisseurs | CTO | P1 |
| DR-063 | Paramétrage du calcul de TVA et immatriculations collectées auprès des créateurs | Q4, Q24 | À produire | Investisseurs | CTO + expert-comptable *(non attribué)* | P1 |
| DR-064 | Contrat de raccordement à une plateforme agréée de facturation électronique, avec sa date | Q24. L'obligation de réception est en vigueur depuis le 1er septembre 2026 si l'entité est française, depuis le 1er janvier 2026 si elle est belge | À produire | Investisseurs | CEO + expert-comptable *(non attribué)* | P0 |
| DR-065 | Position écrite sur l'e-reporting des ventes B2C des créateurs au 1er septembre 2027 | Q24. Risque de réclamation client de masse, ou argument de différenciation | À produire | Investisseurs | CEO | P1 |
| DR-066 | Note de position motivée sur l'applicabilité de DAC7, traitant la commission et l'encaissement | Q4. La non-applicabilité est défendable pour les produits numériques, ouverte pour le coaching | À produire | Investisseurs | Avocat fiscaliste *(à mandater)* | P1 |
| DR-067 | Politique d'usage acceptable et procédure de modération des pages de vente | Q23. Le cœur de cible longe la liste des activités restreintes du prestataire de paiement | Absente | Investisseurs | CEO | P0 |
| DR-205 | **Note unique d'un conseil couvrant les trois qualifications tirées du même jeu de faits** : TVA (article 9 bis), comptable (principal ou agent), droit de la consommation (qui est le vendeur professionnel) | Q4, Q64, Q81. C'est le même rendez-vous et la même liasse de pièces ; trois réponses séparées garantissent qu'au moins deux sont fausses | À produire | Investisseurs | Avocat *(à mandater)* + expert-comptable | P0 |

---

## 5. Architecture et infrastructure

| # | Pièce | Ce qu'elle ferme | État | **Diffusion** | Responsable | Priorité |
|---|---|---|---|---|---|---|
| DR-068 | Schéma d'architecture daté : hébergeur, régions, CDN, pare-feu applicatif, protection anti-déni de service | Q13. Huit lignes sur dix-huit du tableau de stack sont vides | À produire | Investisseurs | CTO | P0 |
| DR-069 | Inventaire des environnements exposés publiquement, et statut de l'alias tiers relevé | Q13, Q40. Un environnement de prévisualisation public contourne le CDN et le pare-feu du domaine principal | À produire | Sur demande | CTO | P1 |
| DR-070 | Inventaire des fournisseurs critiques, avec rôle, localisation et substituabilité | Q13, Q41 | À produire | Investisseurs | CTO | P0 |
| DR-071 | Relevé des en-têtes de sécurité et de la configuration TLS sur les quatre hôtes principaux | Q6. Une demi-journée de travail une fois l'accès rétabli | À produire | Investisseurs | CTO | P0 |
| DR-072 | Politique d'émission des certificats des sites clients : par hôte ou générique, condition d'émission | Q14, Q38 | À produire | Investisseurs | CTO | P1 |
| DR-073 | Fournisseur vidéo et preuve que les URL de lecture sont signées et expirantes | Q15. Si elles sont publiques, le contenu payant de tous les créateurs est repartageable par lien | À produire | Investisseurs | CTO | P1 |
| DR-074 | Politique de sauvegarde : fréquence, périmètre, chiffrement, rétention | Q17 | À produire | Investisseurs | CTO | P1 |
| DR-075 | Journal du dernier test de restauration réussi, avec sa date | Q17. Pièce la plus importante de ce domaine | À produire | Investisseurs | CTO | P1 |
| DR-076 | Objectifs de point et de délai de reprise, et plan de reprise écrit | Q17 | À produire | Investisseurs | CTO | P1 |
| DR-077 | Page de statut public et engagement de disponibilité | Q17. Aucune page de statut n'a été trouvée | Absente | Investisseurs | CTO | P2 |
| DR-078 | Description des environnements et règle écrite sur les données réelles hors production | Q40. Le compte fourni à l'audit comme compte de test contenait les données réelles du dirigeant | À produire | Investisseurs | CTO | P0 |
| DR-079 | Processus de mise en production : revue par un pair, retour arrière, infrastructure décrite en code | Point sensible avec une équipe très réduite | À produire | Investisseurs | CTO | P1 |
| DR-080 | Scores de performance, d'accessibilité et de référencement des pages principales | Q13. Croise l'obligation d'accessibilité applicable aux clients | À produire | Investisseurs | CTO | P2 |
| DR-206 | Métriques d'un pic de charge réel déjà encaissé, politique de quota par locataire, description de la dégradation, et plan de bascule écrit par fournisseur critique | Q85, Q86. Le lancement est le moment où le créateur juge la plateforme ; et trois fournisseurs peuvent couper le service d'un hébergeur de contenus de tiers | À produire | Sur demande | CTO | P1 |

---

## 6. Sécurité

| # | Pièce | Ce qu'elle ferme | État | **Diffusion** | Responsable | Priorité |
|---|---|---|---|---|---|---|
| DR-081 | Attributs réels du cookie de session, relevés après authentification | Q6. Avec DR-082, la vérification la plus rentable du dossier | À produire | Sur demande | CTO | P0 |
| DR-082 | Attribut `sandbox` complet du bloc de code personnalisé, domaine enregistrable de son `src`, et CSP de cette origine | Q6, Q21, Q33. Décide si trois risques majeurs sont théoriques ou réels | À produire | Sur demande | CTO | P0 |
| DR-083 | Recherche de `tinypages` dans la Public Suffix List, et demande d'inscription si absente | Q6. L'inscription ne produit son effet qu'après diffusion dans les navigateurs : ce n'est pas un correctif d'urgence | À produire | Investisseurs | CTO | P1 |
| DR-084 | Configuration CORS de l'API : liste blanche explicite ou correspondance par suffixe | Q6 | À produire | Sur demande | CTO | P0 |
| DR-085 | Mécanisme anti-CSRF employé | Q6. Une protection fondée sur `SameSite` seul serait inopérante dans cette topologie | À produire | Sur demande | CTO | P0 |
| DR-086 | Décision documentée sur la séparation des sites clients et de l'application sur deux domaines enregistrables | Q6, Q39. Chantier qui ne peut pas aboutir avant l'ouverture : à exposer chiffré et daté | À produire | Investisseurs | CTO | P1 |
| DR-087 | Rapport du dernier test d'intrusion : date, prestataire, périmètre, constats, remédiation | Q36. Sans lui, 85 des 97 lignes du questionnaire de sécurité restent « inconnue » | Absente (sauf pièce contraire) | Sur demande | CTO | P0 |
| DR-088 | Test d'intrusion couvrant explicitement trois points nommés : **autorisation au niveau de l'objet entre locataires**, **authentification des acheteurs et des élèves**, et chaîne d'injection indirecte | Q33, Q36, Q87, Q88. Un test qui ne couvre pas ces trois points ne couvre pas le risque principal. Les mots IDOR et BOLA n'apparaissaient nulle part dans la version 1 du dossier | Absente | Sur demande | CTO + prestataire *(à mandater)* | P1 |
| DR-089 | Politique de gestion des vulnérabilités : délais de correction par gravité, analyse des dépendances | Q36 | À produire | Investisseurs | CTO | P1 |
| DR-090 | Captures du produit montrant l'authentification à deux facteurs pour les comptes ouverts par email | Q37 | À produire | Investisseurs | CTO | P1 |
| DR-091 | Politique d'accès à la production : nombre de personnes, moindre privilège, revue périodique, journalisation | Q37 | À produire | Sur demande | CTO | P1 |
| DR-092 | Règle et journal de la prise d'identité d'un client par un employé | Q37. Question systématique en due diligence | À produire | Sur demande | CTO | P1 |
| DR-093 | Politique de gestion des sessions : durée, révocation, déconnexion de tous les appareils | Q37 | À produire | Investisseurs | CTO | P1 |
| DR-094 | Liste de mots réservés pour les identifiants de sous-domaine | Q38 | Absente (sauf pièce contraire) | Sur demande | CTO | P1 |
| DR-095 | Politique de non-réutilisation des identifiants libérés par un créateur parti | Q38 | Absente (sauf pièce contraire) | Investisseurs | CTO | P1 |
| DR-096 | Procédure de vérification de propriété d'un domaine personnalisé, à l'ajout et après résiliation | Q38. Dans ce schéma, TinyPages est le service tiers vulnérable, pas la victime | À produire | Investisseurs | CTO | P1 |
| DR-097 | Limites de débit sur la création et la publication, par compte et par adresse, plan gratuit inclus | Q39 | Absente (sauf pièce contraire) | Sur demande | CTO | P0 |
| DR-098 | Dispositif de détection d'abus sur le contenu publié | Q39 | Absente (sauf pièce contraire) | Sur demande | CTO | P1 |
| DR-099 | Fichier `security.txt` publié, avec contact et date d'expiration | Q36. Coût quasi nul, marqueur de maturité lu par les auditeurs | À produire | Investisseurs | CTO | P0 |
| DR-100 | Page publique de signalement d'abus, adresse dédiée surveillée, délai de traitement affiché | Q23, Q39, Q41. Exigence du règlement sur les services numériques, quelle que soit la taille | Absente (sauf pièce contraire) | Investisseurs | CEO | P0 |
| DR-101 | Registre des signalements et des retraits de contenu depuis le lancement | Q23, Q39 | À produire | Investisseurs | CEO | P1 |
| DR-102 | Relevé de réputation du domaine auprès des principaux filtres de navigation, et surveillance mise en place | Q39. Un signalement sur le domaine partagé retirerait d'un coup tous les sites clients et l'application | À produire | Sur demande | CTO | P1 |
| DR-103 | Plan de crise pour le scénario « le domaine est signalé comme dangereux par un navigateur majeur » | Q39. Risque de continuité d'activité, pas seulement de sécurité | Absente | Sur demande | CTO | P1 |
| DR-104 | Historique des incidents de sécurité depuis le lancement | Q36. La question sera posée telle quelle : une réponse préparée vaut mieux qu'une découverte | À produire | Sur demande | CEO | P0 |
| DR-105 | Plan de réponse à incident et procédure de notification de violation sous 72 heures | Q41 | À produire | Sur demande | CTO + avocat *(à mandater)* | P1 |
| DR-106 | Preuve de l'isolation des données entre créateurs au niveau de la base, **relevé du test de lecture puis d'écriture d'un objet d'un autre locataire par l'API et par le serveur MCP**, et test automatisé qui échoue si une requête franchit la frontière de locataire | Q6, Q36, Q87. Le test de lecture avec un identifiant étranger coûte quinze minutes et n'a jamais été fait | À produire | Sur demande | CTO | P1 |
| DR-107 | Certification ou audit de sécurité externe, s'il en existe un | Aucune mention publique n'a été trouvée | Absente (sauf pièce contraire) | Investisseurs | CTO | P2 |
| DR-108 | Attestation d'assurance cyber | Domaine gouvernance du questionnaire de sécurité | À produire | Investisseurs | CEO | P1 |
| DR-207 | Schéma d'authentification des acheteurs et des élèves, relevé de cookies sur un espace membre réel, et réponse écrite sur le cloisonnement des identités entre locataires | Q88. **Angle mort complet de la version 1 du dossier** : toute l'analyse de topologie de domaine portait sur la session créateur, alors que les acheteurs et les élèves sont les seuls utilisateurs qui paient | À produire | Sur demande | CTO | P0 |

---

## 7. IA et serveur MCP

C'est le domaine où l'audit a produit le plus de preuves, et où les manques sont les plus structurants.

| # | Pièce | Ce qu'elle ferme | État | **Diffusion** | Responsable | Priorité |
|---|---|---|---|---|---|---|
| DR-109 | Export daté du catalogue complet des 104 actions avec leurs schémas JSON et leurs descriptions, **plus la liste nominative des 17 actions réellement exécutées et leur réponse** | Q12, Q53. Le périmètre réel n'est pas auditable depuis la liste d'outils que voit le client, et l'inventaire actuel est un relevé de noms publié par le serveur, pas une exécution | À produire | Investisseurs | CTO | P0 |
| DR-110 | Preuve d'une confirmation appliquée côté serveur avant publication, dépublication, envoi et programmation | Q2, Q3. Le test du 18 septembre 2026 a établi qu'il n'y en a aucune sur la publication | Absente | Investisseurs | CTO | P0 |
| DR-111 | Nouvelle liste d'outils exposant les 15 actions à effet public ou irréversible comme des outils nommés et annotés | Q12, Q35. Conditionne aussi toute candidature à un annuaire de connecteurs | Absente | Investisseurs | CTO | P0 |
| DR-112 | Journal des actions effectuées par l'IA : horodatage, acteur humain ou IA, session, compte cible, action, résultat | Q30. Première demande de l'auditeur technique et d'une autorité de protection des données | Absente (sauf pièce contraire) | Sur demande | CTO | P0 |
| DR-113 | Fonction d'annulation et historique des versions des pages | Q30 | Absente (sauf pièce contraire) | Investisseurs | CTO | P1 |
| DR-114 | Batterie d'évals versionnée : un cas par garde-fou, par modèle et par client supporté, avec seuils bloquants | Q7. Le risque produit le plus structurant du dossier | Absente (sauf pièce contraire) | Investisseurs | CTO | P1 |
| DR-115 | Résultats datés de la dernière exécution de la batterie, et politique de gel de version en cas d'échec | Q7 | Absente | Investisseurs | CTO | P1 |
| DR-116 | Métadonnées OAuth du serveur MCP (`oauth-authorization-server` et `oauth-protected-resource`) | Q31. PKCE, enregistrement dynamique de clients, portées, durées | À produire | Investisseurs | CTO | P0 |
| DR-117 | Liste des portées disponibles, dont une portée lecture seule | Q31. L'autorisation observée semble être tout ou rien | Absente (sauf pièce contraire) | Investisseurs | CTO | P1 |
| DR-118 | Cycle de vie du jeton API : durée, portée, rotation, révocation, dernière utilisation, jetons distincts par client | Q31. Secret à haut privilège donnant accès en écriture, en publication et en envoi | À produire | Sur demande | CTO | P0 |
| DR-119 | Règle de portée du jeton entre comptes et comportement exact de `switch_account` | Q32. Le cloisonnement n'a pas pu être éprouvé, faute d'un second compte | À produire | Sur demande | CTO | P1 |
| DR-120 | Relevé de test multi-comptes sur deux locataires réellement distincts | Q32 | À produire | Sur demande | CTO | P1 |
| DR-121 | Relevé du test d'envoi sur un compte au plan payant dédié | Q3, Q34. Seule question laissée ouverte par le screening : le mur de facturation est-il le seul contrôle ? | À produire | Investisseurs | CTO | P0 |
| DR-122 | Contre-mesures d'injection indirecte : marqueur de données non fiables, échappement, troncature dans les réponses d'outil | Q33 | Absente | Sur demande | CTO | P1 |
| DR-123 | Règle de protection du contexte métier persistant et des consignes système du compte : contrôle serveur avant écriture, journalisation dédiée, affichage de la valeur courante au créateur, historique des versions, **et une voie d'effacement par le canal qui a écrit** | Q33, Q94. **Établi par test (M-018)** : `update_business_context` écrit ce champ de 10 000 caractères sans aucun contrôle serveur sur un compte gratuit, et le serveur **refuse la remise à vide** — l'IA peut écrire une instruction persistante, elle ne peut pas la retirer | Absente | Sur demande | CTO | P1 |
| DR-124 | Version corrigée des instructions du serveur : périmètre réduit au strict usage des outils, affichage d'un différentiel avant publication | Q30, Q35. Les instructions actuelles demandent au modèle de ne pas montrer son travail | Absente | Investisseurs | CTO | P0 |
| DR-125 | Documentation corrigée sur la capacité d'envoi d'emails par l'IA, avec sa date | Q34. L'affirmation actuelle est fausse : `send_email` et `schedule_email` existent | À produire | Investisseurs | CTO | P0 |
| DR-126 | Échange écrit avec l'éditeur de l'annuaire de connecteurs, ou déclaration qu'aucune soumission n'a été faite | Q35. Un refus déjà essuyé serait une information matérielle | À produire | Investisseurs | CEO | P1 |
| DR-127 | Échange écrit avec l'éditeur du modèle sur l'usage de sa marque dans les accroches produit | Q8. Conclusion juridique à valider par un avocat | À produire | Investisseurs | CEO + avocat *(à mandater)* | P1 |
| DR-128 | Matrice de compatibilité par client et par version, réellement testée | Q8, Q31 | À produire | Investisseurs | CTO | P1 |
| DR-129 | Modèle, fournisseur, coût unitaire, plafonds d'usage et données transmises pour l'IA intégrée au produit | Q16, Q44. Marge brute, sous-traitance, transparence | À produire | Investisseurs | CTO | P1 |
| DR-130 | Contrat du fournisseur de modèle, avec clause de non-entraînement | Q41, Q44 | À produire | Investisseurs | CEO + avocat *(à mandater)* | P1 |
| DR-131 | Spécification OpenAPI de l'API, grille de limites de débit par plan, liste des webhooks | Q31. Une data room de ce niveau sans spécification d'API sur un produit « pilotable par IA » est un signal négatif | À produire | Investisseurs | CTO | P1 |
| DR-132 | Locataire de test dédié et anonyme, **gratuit et Pro**, pour le rejeu horodaté du protocole, les tests restants et le test d'intrusion | Q40, Q3, Q54, Q87 | À produire | Investisseurs | CTO | P0 |
| DR-208 | Extrait de code du point d'entrée `execute_action` montrant la revalidation du compte, du plan et de la portée à chaque appel, et test négatif qui échoue si la revalidation est retirée | Q90. Une passerelle qui fait confiance à l'identifiant d'action reçu annule le peu de contrôle qui existe | À produire | Sur demande | CTO | P1 |
| DR-209 | Journal serveur des appels MCP : extrait anonymisé, champs conservés, politique de rétention, extraction par compte — **distinct du journal produit DR-112** | Q91. S'il existe déjà, c'est la meilleure réponse du dossier sur ce domaine : il reconstitue l'historique des actions d'IA avant même la livraison de la fonctionnalité produit | À produire | Sur demande | CTO | P0 |
| DR-210 | Statut du serveur MCP TinyPages hébergé par un tiers : accord ou contrat, inventaire des intégrations publiées sous la marque, flux d'authentification employé | Q92. Cité trois fois dans le dossier pour affaiblir l'exclusivité, jamais examiné comme chemin de données. Sous-traitant manquant à l'inventaire, ou surface d'API à inscrire au périmètre du test d'intrusion | À produire | Investisseurs | CEO + CTO | P1 |
| DR-211 | Note de qualification du flux de données du créateur vers l'éditeur de son client IA, écran de consentement et information affichée à la connexion, clause dédiée du contrat de sous-traitance créateurs | Q93. Flux central du produit, absent de tout le dossier : contacts, formulaires, listes et chiffre d'affaires sortent sous le contrat personnel du créateur. La correction est une mention d'information, pas un chantier | À produire | Investisseurs | CEO + avocat *(à mandater)* + CTO | P0 |

---

## 8. Emails et délivrabilité

| # | Pièce | Ce qu'elle ferme | État | **Diffusion** | Responsable | Priorité |
|---|---|---|---|---|---|---|
| DR-133 | Relevé DNS des domaines d'envoi : SPF, DKIM, DMARC, sur TinyPages et sur des domaines créateurs | Q26, Q27. Une commande suffit une fois l'accès ouvert | À produire | Investisseurs | CTO | P0 |
| DR-134 | Description de l'isolation de réputation entre créateurs : flux, pools d'adresses, sélecteurs | Q26. Risque systémique si absente | À produire | Investisseurs | CTO | P0 |
| DR-135 | Règle de suspension automatique d'un créateur au-delà du seuil de plaintes | Q26, Q27 | Absente (sauf pièce contraire) | Investisseurs | CTO | P1 |
| DR-136 | En-têtes complets d'un email créateur réellement reçu, anonymisés | Q27. Désinscription en un clic, alignement DMARC | À produire | Investisseurs | CTO | P1 |
| DR-137 | Taux de plaintes agrégé du parc sur douze mois | Q27. Seuil de 0,3 % imposé par les principaux fournisseurs de messagerie | À produire | Investisseurs | CTO | P0 |
| DR-138 | Facture du fournisseur d'envoi sur douze mois et plan réellement souscrit | Q25. Le chiffrage du dossier repose aujourd'hui sur des tarifs publics et une hypothèse de volume | À produire | Investisseurs | CEO | P1 |
| DR-139 | Distribution des tailles de liste et fréquence d'envoi moyenne du parc | Q25. Sans elles, aucune projection de coût n'est possible | À produire | Investisseurs | CTO | P1 |
| DR-140 | Règle de gestion de la liste de suppression : partagée entre créateurs ou cloisonnée | Q26, Q41. Une mutualisation sans base légale poserait une question de finalité | À produire | Investisseurs | CTO | P1 |
| DR-141 | Schéma du modèle de contact montrant date, origine et libellé du consentement, et export les incluant | Q28. Sans eux, la preuve du consentement est inexécutable pour tous les créateurs | À produire | Investisseurs | CTO | P0 |
| DR-142 | Procédure d'import en masse et ce qu'elle conserve de la preuve de consentement | Q28. Concerne aussi la migration annoncée d'une liste de plus de 70 000 contacts | À produire | Investisseurs | CTO | P1 |
| DR-143 | En-têtes d'un envoi réel de la newsletter du fondateur depuis la plateforme, et volume mensuel | Q29. Preuve de passage à l'échelle la plus simple à produire et la plus scrutée | À produire | Sur demande | CEO | P1 |

---

## 9. Données personnelles et conformité réglementaire

| # | Pièce | Ce qu'elle ferme | État | **Diffusion** | Responsable | Priorité |
|---|---|---|---|---|---|---|
| DR-144 | Politique de confidentialité de TinyPages | Q41, Q43 | À produire | Investisseurs | CEO + avocat *(à mandater)* | P0 |
| DR-145 | Conditions générales d'utilisation et de vente de TinyPages | Q41, Q46 | À produire | Investisseurs | CEO + avocat *(à mandater)* | P0 |
| DR-146 | Contrat de sous-traitance proposé aux créateurs, ou conditions qui en tiennent lieu, vérifiées mention par mention | Q41. Sans lui, chaque créateur est en écart et la plateforme devient un risque pour son propre client | À produire | Investisseurs | CEO + avocat *(à mandater)* | P0 |
| DR-147 | Liste publiée et datée des sous-traitants ultérieurs, incluant le fournisseur du modèle d'IA, avec rôle et localisation | Q41. Distinguer sous-traitant, responsable autonome et responsable conjoint | À produire | Investisseurs | CEO | P0 |
| DR-148 | Matrice des rôles par traitement | Q41. Trois casquettes cumulées : sous-traitant, responsable, hébergeur | À produire | Investisseurs | CEO + avocat *(à mandater)* | P0 |
| DR-149 | Registre des traitements | Q41. Première pièce demandée par une autorité comme par un auditeur | À produire | Investisseurs | CEO | P0 |
| DR-150 | Registre des violations de données | Q41 | À produire | Investisseurs | CEO | P1 |
| DR-151 | Localisation des données par traitement, et mécanismes d'encadrement des transferts hors Union avec analyses d'impact | Q41 | À produire | Investisseurs | CTO + avocat *(à mandater)* | P1 |
| DR-152 | Tableau des durées de conservation, et preuve de leur application dans le produit | Q41 | À produire | Investisseurs | CTO | P1 |
| DR-153 | Preuve qu'un contact peut être supprimé définitivement depuis l'interface, y compris dans les sauvegardes | Q42. Établi comme impossible par le canal automatisé ; non testé en interface | À produire | Investisseurs | CTO | P0 |
| DR-154 | Gabarits juridiques réellement remplis, non indexés tant qu'ils sont vides, et capture d'un compte neuf corrigé | Q5. Aujourd'hui, deux documents juridiques vides sont publiés et indexés sur chaque compte | Absente | Investisseurs | CTO + avocat *(à mandater)* | P0 |
| DR-155 | Double opt-in activé par défaut, et capture du parcours de confirmation | Q5, Q28. Le compte observé porte `doubleOptin: false` | Absente | Investisseurs | CTO | P0 |
| DR-156 | Relevé réseau d'un chargement de page avec navigateur vierge, avant et après consentement, sur le site et sur un site client | Q43. Test de cinq minutes, documentable en une capture | À produire | Investisseurs | CTO | P0 |
| DR-157 | Mécanisme de consentement aux traceurs fourni aux sites clients, et son périmètre | Q43. S'il n'existe pas, TinyPages place ses clients en écart par conception | À produire | Investisseurs | CTO | P0 |
| DR-158 | Paquet « hébergeur » au titre du règlement sur les services numériques : points de contact, conditions décrivant la modération, mécanisme de notification et d'action, exposé des motifs de toute restriction | Q41. L'exemption des petites entreprises ne couvre pas ces articles | Absente (sauf pièce contraire) | Investisseurs | CEO + avocat *(à mandater)* | P0 |
| DR-159 | Désignation d'un représentant dans l'Union, si l'entité est établie hors Union | Q9, Q41. Deux désignations distinctes le cas échéant | À produire | Investisseurs | CEO + avocat *(à mandater)* | P1 |
| DR-160 | Mention informant l'utilisateur qu'il interagit avec un système d'IA | Q44. Applicable depuis le 2 août 2026 | À produire | Investisseurs | CTO | P0 |
| DR-161 | Marquage lisible par machine des contenus générés | Q44. Exigible au 2 décembre 2026 : l'échéance la plus proche du dossier, elle tombe pendant la levée | Absente | Investisseurs | CTO | P0 |
| DR-162 | Note de qualification fournisseur ou déployeur au titre du règlement sur l'IA | Q44 | À produire | Investisseurs | Avocat *(à mandater)* | P1 |
| DR-163 | Note de sensibilisation interne à l'IA, datée, et son registre | Q44. Sans seuil d'effectif ; coût quasi nul, absence facile à relever | Absente | Investisseurs | CEO | P1 |
| DR-164 | Fonction d'export complet des données d'un créateur, et son format | Q47. Absente du catalogue automatisé ; les frais de changement disparaissent le 12 janvier 2027 | Absente (sauf pièce contraire) | Investisseurs | CTO | P1 |
| DR-165 | Clauses de sortie dans les conditions générales : préavis, transition assistée, suppression après migration | Q47 | À produire | Investisseurs | CEO + avocat *(à mandater)* | P1 |
| DR-166 | Preuve du recueil, dans le tunnel d'achat, de l'accord exprès à l'exécution immédiate et de la renonciation au droit de rétractation | Q46, Q47. Sinon ce sont les créateurs qui sont exposés | À produire | Investisseurs | CTO | P1 |
| DR-167 | Déclaration d'accessibilité et mesure du niveau atteint | Q13. TinyPages peut être exempté comme microentreprise ; ses clients ne le sont pas tous | À produire | Investisseurs | CTO | P2 |
| DR-168 | Conservation des données d'identification des créateurs, au titre du régime de l'hébergeur | Q41 | À produire | Investisseurs | CTO + avocat *(à mandater)* | P1 |
| DR-212 | Position écrite sur la présence possible de mineurs parmi les élèves, paramétrage produit correspondant, et clause imposée aux créateurs | Q96. Le coach IA s'adresse à des élèves dont la plateforme ne contrôle pas le profil | À produire | Investisseurs | CEO + avocat *(à mandater)* | P1 |

---

## 10. Produit, marché et discours commercial

| # | Pièce | Ce qu'elle ferme | État | **Diffusion** | Responsable | Priorité |
|---|---|---|---|---|---|---|
| DR-169 | Grille tarifaire officielle datée, et matrice des limites par plan | Q46. Trois contradictions d'offre restent ouvertes sur ce que le client achète | À produire | Investisseurs | CEO | P0 |
| DR-170 | Journal des corrections apportées aux pages contradictoires du site et de la documentation | Q34, Q46. Les contradictions C-001 et C-003 restent ouvertes | À produire | Investisseurs | CEO | P0 |
| DR-171 | Comparatif fonctionnel daté et archivé face aux concurrents dotés d'un serveur MCP officiel, sur un périmètre de comparaison défini — objets pilotables en écriture, pas nombre d'appels | Q1, Q49. **Sept** concurrents en publient un : Kajabi, GoHighLevel, ClickFunnels, Stan Store, Systeme.io, Kit.com et beehiiv. Les registres C-004 et P-038 en recensent sept ; en citer cinq serait lu comme un rabotage | À produire | Investisseurs | CEO | P0 |
| DR-172 | Retrait ou reformulation de l'affirmation « seule plateforme », sur tous les supports | Q1, Q45. Vérifiable en dix minutes par n'importe quel investisseur | À produire | Investisseurs | CEO | P0 |
| DR-173 | Dossier de preuve daté pour chaque allégation absolue ou chiffrée | Q45, Q50. La charge de la preuve pèse sur l'annonceur | À produire | Investisseurs | CEO | P0 |
| DR-174 | Archives datées des neuf pages comparatives et des pages concurrentes au jour du relevé, avec révision trimestrielle | Q45. Un comparatif périmé devient trompeur sans que personne n'ait rien changé | À produire | Investisseurs | CEO | P1 |
| DR-175 | Registre des consentements aux témoignages publiés : accord écrit, date, identité vérifiable | Q45 | À produire | Investisseurs | CEO | P1 |
| DR-176 | Méthode d'invitation aux avis en ligne et distribution réelle des notes | Q45. Une trentaine d'avis, avec une note rapportée de façon contradictoire : aucun chiffre n'est citable en l'état | À produire | Investisseurs | CEO | P1 |
| DR-177 | Journal des versions du produit, daté, depuis le lancement | Q49. Les dates de la bascule vers l'IA n'ont pas pu être confirmées | À produire | Investisseurs | CTO | P1 |
| DR-178 | Feuille de route produit datée, distinguant ce qui existe, ce qui est prévu et ce qui ne sera pas fait | Q48. Une lacune assumée avec une date coûte moins cher qu'une lacune découverte | À produire | Investisseurs | CEO | P0 |
| DR-179 | Note de positionnement face à l'hébergement natif de pages par les éditeurs de modèles | Q8. Deux offres datées de juin et juillet 2026 | À produire | Investisseurs | CEO | P0 |
| DR-180 | Charte de support : canaux, horaires, délais d'engagement | Q48. Les seules données disponibles sont des avis publics anecdotiques | À produire | Investisseurs | CEO | P1 |
| DR-181 | Avertissement produit sur le risque de publication de contenu en volume sans révision | Q50. Risque reporté sur les clients, mais pertinent si le référencement est un argument de vente | À produire | Investisseurs | CEO | P2 |
| DR-182 | Statut de `emilio.tinypages.co` : site de démonstration interne ou client tiers | Q9, Q10. Conditionne son usage comme preuve de traction | À produire | Sur demande | CEO | P1 |
| DR-183 | Contrats clients types et éventuels contrats grands comptes | Pièce standard de due diligence commerciale | À produire | Investisseurs | CEO | P1 |
| DR-213 | Procédure nommée de validation des allégations commerciales avant mise en ligne : responsable identifié, archive datée par allégation, révision trimestrielle, journal des retraits | Q97. Réponse structurelle à l'affaire « seule plateforme » : retirer la phrase sans installer le processus ne répond qu'à la moitié de la question | À produire | Investisseurs | CEO | P0 |
| DR-214 | Clause de responsabilité en cas de contenu illicite ou diffamatoire publié par l'IA sur le compte d'un créateur, procédure de notification et action, registre des retraits | Q79. Entre une publication sans contrôle serveur et l'absence de toute clause, il n'y a aujourd'hui rien | À produire | Investisseurs | CEO + avocat *(à mandater)* | P0 |

---

## 11. Équipe et gouvernance

| # | Pièce | Ce qu'elle ferme | État | **Diffusion** | Responsable | Priorité |
|---|---|---|---|---|---|---|
| DR-184 | Organigramme et liste des contrats de travail et de prestation en cours | Pièce standard ; les deux rôles clés sont tenus par la même personne | À produire | Sur demande | CEO | P0 |
| DR-185 | Plan de renfort et de continuité en cas d'indisponibilité du dirigeant | La concentration CEO / CTO sur une seule personne est un risque identifié du dossier | À produire | Investisseurs | CEO | P0 |
| DR-186 | Engagements de confidentialité signés, y compris prestataires | Domaine ressources humaines du questionnaire de sécurité | À produire | Sur demande | CEO | P1 |
| DR-187 | Procédure de retrait des accès au départ d'une personne | Q37 | À produire | Sur demande | CTO | P1 |
| DR-188 | Politique de sécurité écrite et approuvée, et désignation d'un responsable | Q36, Q41 | À produire | Investisseurs | CTO | P1 |
| DR-189 | Mandat de l'avocat et mandat de l'expert-comptable ou du conseil fiscal | Q4, Q11, Q41, Q44. **Trente pièces de cet index dépendent d'un rôle non pourvu** : 20 d'un avocat à mandater, 10 d'une fonction financière ou comptable qu'aucun paramètre ne désigne | À produire | Investisseurs | CEO | P0 |

---

## 12. Précautions de constitution

Ces règles ne sont pas des pièces, mais elles conditionnent la recevabilité du dossier.

- **Aucune donnée personnelle de créateur, de contact, d'acheteur ou d'élève** ne doit entrer dans la data room. Tous les exemples de reçus, factures, exports et en-têtes sont anonymisés avant versement.
- **Aucun secret** : clé, jeton, mot de passe, URL signée. Les captures de la gestion des jetons sont masquées.
- **Les sites clients restent anonymisés** (Site A, Site B…) sauf accord écrit du créateur concerné.
- **Le compte utilisé pour les relevés d'exécution ne doit pas être versé ni cité nominativement** : il porte l'identité réelle du dirigeant. L'annexe qui le décrit (DR-011) est strictement interne, et les constats qui en sont tirés sont reformulés sans référence au compte dans les livrables versés.
- **Le marquage de diffusion est porté en clair et en tête de chaque fichier**, pas seulement dans cet index. Un index ne protège pas un document qu'on envoie en pièce jointe. Les huit pièces strictement internes restent dans `audit/` et n'entrent jamais dans le répertoire de la data room, même dans un sous-dossier fermé.
- **Une pièce « Sur demande » ne se dépose pas en téléchargement.** Elle se présente nominativement, sous accord de confidentialité, et de préférence en séance. La règle vaut en particulier pour les relevés de configuration de sécurité et le rapport de test d'intrusion, qui décrivent des faiblesses exploitables tant qu'elles ne sont pas corrigées.
- **Chaque pièce est datée au jour de son extraction.** Une capture non datée ne prouve rien dans six semaines.
- **Les références réglementaires des livrables doivent être reconfirmées sur source officielle** avant diffusion : elles proviennent de sources secondaires, l'accès aux bases officielles ayant été fermé pendant tout l'audit.

---

## 13. Décompte

Décompte effectué sur les **214 lignes** de cet index, le 18 septembre 2026, après les corrections du contre-audit. **Aucune pièce n'est déclarée « Prête » sans que le fichier correspondant ait été ouvert et vérifié.**

### Par état

| État | Nombre de pièces | Part |
|---|---|---|
| Prête | 18 | 8 % |
| À produire | 165 | 77 % |
| Absente | 31 | 15 % |
| **Total** | **214** | **100 %** |

Répartition des états « absente » : **14** portent la mention *(sauf pièce contraire)* — l'audit présume l'absence sans avoir pu l'établir sur tous les canaux — et **17** sont des absences établies ou des chantiers dont l'objet n'existe pas aujourd'hui. **Douze** de ces trente et une pièces sont en P0.

### Par niveau de diffusion

| Diffusion | Nombre de pièces | Part |
|---|---|---|
| Investisseurs | 162 | 76 % |
| Sur demande | 44 | 20 % |
| **Strictement interne — jamais remise** | **8** | **4 %** |
| **Total** | **214** | **100 %** |

**Les huit pièces strictement internes, nommément :** le livrable 08 (DR-008), le présent index dans sa version complète (DR-009), l'annexe de screening MCP (DR-011), la liste des pièces internes à demander (DR-014), les trois registres — preuves (DR-015), contradictions (DR-016), questions ouvertes (DR-017) — et le contre-audit A11 (DR-021). **Ces huit fichiers ne sont jamais déposés dans le répertoire de la data room, sous aucune forme.** Deux d'entre eux — le livrable 08 et le contre-audit — seraient dommageables s'ils étaient remis par erreur : le premier contient le dispositif « formulation à ne pas dépasser », qui est un manuel de langage de négociation, le second expose la chaîne de garde de la preuve et la stratégie de réponse. Les six autres contiennent l'identité réelle du dirigeant et l'état de son compte personnel. Deux pièces sont prévues pour porter leur contenu vers l'extérieur sous une forme versable : DR-190, version épurée des registres, et DR-191, note de synthèse du contre-audit.

### Croisement des deux tableaux

| | Investisseurs | Sur demande | Strictement interne | Total |
|---|---|---|---|---|
| Prête | 9 | 1 | 8 | **18** |
| À produire | 132 | 33 | 0 | **165** |
| Absente | 21 | 10 | 0 | **31** |
| **Total** | **162** | **44** | **8** | **214** |

La ligne qui compte est la première : **sur les 18 pièces prêtes, 8 ne sortiront jamais et 1 ne sort que sur demande.** Ce qui est réellement versable en l'état, aujourd'hui, ce sont **neuf documents** : les sept livrables 01 à 07, le catalogue MCP et la méthodologie.

**Comment lire ce décompte.** Les 18 pièces prêtes ont toutes été produites par l'audit lui-même : ce sont des analyses, pas des pièces de société. **Aucune pièce d'entreprise n'a été vue par cet audit** — ni comptes annuels, ni contrats, ni statuts, ni factures, ni relevés bancaires, ni registres sociaux. Le dossier ne se prononce donc ni sur l'existence d'un passif, ni sur la sincérité des déclarations de la société. Les 165 pièces « à produire » existent probablement en partie déjà, dans une console fournisseur, dans le dépôt de code ou chez le comptable : les extraire est le plus souvent une affaire d'heures, pas de semaines. Les 31 pièces « absente » sont d'une autre nature : elles supposent un chantier. Les douze qui sont en P0 sont les suivantes — confirmation appliquée côté serveur avant publication et envoi (DR-110), outils nommés pour les 15 actions irréversibles (DR-111), journal des actions de l'IA (DR-112), instructions serveur corrigées avec différentiel avant publication (DR-124), gabarits juridiques remplis et non indexés tant qu'ils sont vides (DR-154), double opt-in par défaut (DR-155), marquage lisible par machine des contenus générés (DR-161, échéance du 2 décembre 2026), paquet hébergeur au titre du règlement sur les services numériques (DR-158), page publique de signalement d'abus (DR-100), politique d'usage acceptable et modération des pages de vente (DR-067), limites de débit sur la création et la publication (DR-097), et rapport de test d'intrusion (DR-087).

**Ce que ce décompte ne dit pas.** Il ne mesure pas la qualité du produit. Il mesure l'écart entre ce que TinyPages sait de lui-même et ce qu'il peut montrer. Un auditeur mandaté arrivera dans la même position que cet audit : sans accès interne, il ne verra que ce qui lui est remis. Les 77 % de pièces à produire sont donc la charge de travail réelle avant l'ouverture, et elle se concentre aujourd'hui sur une seule personne.

---

## 14. Ordre de production recommandé

Par dépendance, pas par volume.

1. **DR-022 à DR-027 — l'entité.** Six lignes de la matrice réglementaire, la question du représentant dans l'Union, le calendrier de facturation électronique et l'identité des fondateurs en dépendent. Rien d'autre ne peut être qualifié tant que ce bloc manque.
2. **DR-045 — les trois faits du tunnel d'achat.** Une heure de travail, et le risque le plus coûteux du dossier passe de « non qualifié » à « qualifié ».
3. **DR-047, DR-048, DR-054 — la configuration de paiement.** Elles commandent qui porte les pertes, et le périmètre PCI.
4. **DR-144 à DR-149 — le socle données personnelles.** Une quinzaine de lignes aujourd'hui sans réponse.
5. **DR-081, DR-082, DR-071, DR-133 — les quatre relevés techniques.** Une demi-journée cumulée une fois l'accès réseau rétabli, et trois risques majeurs passent de théoriques à tranchés.
6. **DR-110 à DR-115, DR-124 — le harnais IA.** Ce ne sont pas des pièces à extraire mais des chantiers à engager : ils doivent être **engagés et datés** avant l'ouverture, même s'ils ne peuvent pas être achevés.
7. **DR-153, DR-154, DR-155, DR-156 — les quatre vérifications produit de conformité.** Les moins coûteuses du dossier, les plus discriminantes en due diligence.
8. **DR-036 à DR-044 — la traction et les coûts.** Aucun audit externe ne les produira ; leur absence rendrait le dossier inexploitable.
9. **DR-028, DR-029, DR-030 — la propriété intellectuelle.** Blocage de closing classique : à traiter tôt, car la régularisation prend du temps.
10. **DR-087, DR-088, DR-106, DR-207 — le test d'intrusion et ce qui le précède.** Avant de commander quoi que ce soit : le test de lecture d'un objet d'un autre locataire avec son identifiant, quinze minutes une fois le locataire dédié DR-132 créé, et le relevé d'authentification d'un espace membre. Le test d'intrusion est ensuite commandé avec un périmètre nommant trois points : autorisation au niveau de l'objet, authentification des acheteurs et des élèves, injection indirecte.
11. **DR-194, DR-195 — la recevabilité et le suivi.** La dérogation écrite, le rejeu horodaté et le nettoyage des trois traces referment l'objection de recevabilité pour une heure de travail. Le journal de remédiation, lui, est ce que l'investisseur relira entre deux réunions.
12. **DR-190, DR-191 — les deux pièces qui remplacent ce qui ne sort pas.** Une version épurée des registres et une note de synthèse du contre-audit. Une équipe qui montre son propre contre-audit et l'état de ses objections est plus crédible qu'une équipe qui n'en a pas eu ; ce qui se montre est la synthèse, jamais le rapport brut.

---

*Index établi sans aucun accès interne, avec l'egress réseau fermé et sans test actif. L'état « à produire » ne préjuge pas de l'existence d'une pièce : il constate qu'aucun tiers ne l'a vue. La colonne Diffusion prime sur l'état et sur la priorité : une pièce « Prête » et « P0 » peut être strictement interne. **Ce document, dans sa version complète, est lui-même strictement interne ; seul son extrait « Investisseurs » est remis, comme sommaire.** À relire par le CTO, le CEO et l'avocat à mandater avant toute diffusion.*
