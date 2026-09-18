# 09 — Index de la data room

Data room TinyPages · Version 1 du 18 septembre 2026 · Rédigé par l'orchestrateur de l'audit
Sources : les huit rapports d'agents, `audit/annexes/catalogue_mcp_tinypages.md`, `audit/annexes/screening_mcp_compte_test.md`, `audit/annexes/pieces_internes_a_demander.md`, et les livrables 02 à 08.
Lecteurs visés : le CEO et le CTO, pour piloter la constitution du dossier. Une fois les pièces réunies, cet index devient le sommaire remis aux investisseurs.

---

## 0. Ce que cet index dit, et ce qu'il ne dit pas

**Cet audit a tourné sans aucun accès interne.** Le paramètre `ACCES_INTERNES` porte « aucun », l'agent chargé du volet interne n'a pas été lancé, l'egress réseau était fermé sur tous les domaines TinyPages, et aucun test actif n'était autorisé. Les seules preuves de première main du dossier proviennent du serveur MCP de production et du compte qui y était connecté.

**Conséquence directe, et elle est en soi une information :** la très grande majorité des pièces de cet index est à l'état « à produire ». Cela ne signifie pas qu'elles n'existent pas. Cela signifie que **personne d'extérieur ne les a vues**, ce qui est exactement la situation dans laquelle se trouvera l'auditeur mandaté par les investisseurs. Le décompte de la section 13 chiffre le travail restant.

### Convention d'état

| État | Ce qu'il signifie exactement |
|---|---|
| **Prête** | La pièce existe, elle a été produite ou vue par cet audit, et elle est versable en l'état, éventuellement après relecture. Compte tenu de `ACCES_INTERNES = aucun`, seules les pièces produites par l'audit lui-même peuvent porter cet état. |
| **À produire** | La pièce doit être rassemblée, extraite ou rédigée. Elle existe peut-être déjà en interne : l'audit n'a pas pu le vérifier. Du point de vue de la data room, une pièce qui ne peut pas être montrée n'existe pas. |
| **Absente** | Son objet n'existe pas. L'audit l'a établi, ou l'a établi pour un canal et le présume pour l'autre — la mention **(sauf pièce contraire)** signale ce second cas. Une pièce absente appelle un chantier, pas une extraction. |

### Convention de responsable

`audit/00_parametres.md` porte : **CTO : Nathan Lahy · CEO : Nathan Lahy · avocat : à mandater**. Les deux premiers rôles sont tenus par la même personne ; cet index les distingue par nature de décision, technique ou société, comme le fait le plan de mise en place. **Deux rôles nécessaires ne sont attribués à personne à ce jour** : l'avocat, qui reste à mandater, et une fonction financière, qu'aucun paramètre ne désigne. Les pièces qui en dépendent portent la mention correspondante : ce sont autant de pièces sans propriétaire, et c'est un point à trancher avant l'ouverture.

### Convention de priorité

Alignée sur `audit/livrables/07_plan_mise_en_place.md` : **P0** avant l'ouverture de la data room, **P1** avant le closing, **P2** après le closing.

---

## 1. Pièces produites par l'audit

Ce sont les seules pièces prêtes du dossier. Elles décrivent l'état du produit et du risque ; elles ne remplacent aucune pièce de société.

| # | Pièce | Ce qu'elle apporte | État | Responsable | Priorité |
|---|---|---|---|---|---|
| DR-001 | `audit/livrables/01_synthese_executive.md` | Synthèse de deux pages pour l'investisseur | À produire | CEO | P0 |
| DR-002 | `audit/livrables/02_analyse_technique.md` | Architecture, stack, flux, données, scalabilité, coûts | Prête | CTO | P0 |
| DR-003 | `audit/livrables/03_analyse_fonctionnelle.md` | Modules, plans, parcours, parité interface / MCP / API, concurrence | Prête | CTO | P0 |
| DR-004 | `audit/livrables/04_securite_conformite.md` | Posture de sécurité, sous-traitants, matrice réglementaire, questionnaire prérempli | Prête | CTO | P0 |
| DR-005 | `audit/livrables/05_harnais_ia_mcp.md` | Pilotage par IA, inventaire MCP, menaces, évals | Prête | CTO | P0 |
| DR-006 | `audit/livrables/06_registre_risques.md` | Risques, mitigations, responsables | Prête | CEO | P0 |
| DR-007 | `audit/livrables/07_plan_mise_en_place.md` | État cible et remédiation P0, P1, P2 | Prête | CEO | P0 |
| DR-008 | `audit/livrables/08_qr_investisseurs.md` | 50 questions de due diligence, réponses et pièces attendues | Prête | CEO | P0 |
| DR-009 | `audit/livrables/09_index_data_room.md` (ce document) | Index des pièces, état, responsables | Prête | CEO | P0 |
| DR-010 | `audit/annexes/catalogue_mcp_tinypages.md` | Inventaire des 104 actions exposées à l'IA, relevé sur le serveur de production | Prête | CTO | P0 |
| DR-011 | `audit/annexes/screening_mcp_compte_test.md` | Relevés d'exécution M-007 à M-017, dont le test de publication | Prête | CTO | P0 |
| DR-012 | `audit/annexes/methodologie.md` | Dispositif, contraintes, dérogations, ce que le dossier ne prouve pas | Prête | CTO | P0 |
| DR-013 | `audit/annexes/echantillon_sites.md` | Échantillon de sites clients, anonymisé | Prête | CTO | P1 |
| DR-014 | `audit/annexes/pieces_internes_a_demander.md` | Liste priorisée des pièces internes et de ce qu'elles ferment | Prête | CEO | P0 |
| DR-015 | `audit/registre/preuves.md` | Registre consolidé des constats et de leurs sources | Prête | CTO | P1 |
| DR-016 | `audit/registre/contradictions.md` | Contradictions relevées, tranchées ou exposées | Prête | CEO | P0 |
| DR-017 | `audit/registre/questions_ouvertes.md` | Questions ouvertes, avec responsable et pièce qui y répond | Prête | CEO | P0 |
| DR-018 | Les huit rapports d'agents `audit/rapports/A01` à `A09` | Matière première et traçabilité des constats | Prête | CTO | P1 |
| DR-019 | Version anglaise des livrables (`audit/livrables/en/`) | `LANGUE = FR + EN` : exigée si des fonds étrangers participent | À produire | CEO | P1 |
| DR-020 | Captures d'écran à l'appui des rapports (`audit/annexes/captures/`) | Les dossiers `A02` et `A07` sont vides : aucun parcours n'a pu être capturé | Absente | CTO | P1 |
| DR-021 | Contre-audit du dossier, avec objections levées | Contrôle qualité prévu par le protocole, non encore exécuté | À produire | CEO | P0 |

**Deux réserves à porter en tête de dossier, sans les diluer.** D'une part, le compte utilisé pour les relevés d'exécution n'était pas un banc d'essai stérile : il porte le nom réel du dirigeant, son adresse personnelle et des brouillons actifs. Les relevés restent valides — ils portent sur le comportement par défaut de la plateforme — mais un locataire dédié doit être créé pour les tests restants. D'autre part, deux objets de test créés pendant l'audit subsistent en brouillon sur ce compte et doivent être supprimés manuellement, le canal automatisé ne le permettant pas.

---

## 2. Société, capital et propriété intellectuelle

Aucune de ces pièces n'a pu être vue. Tant que la première n'est pas produite, six lignes de la matrice réglementaire restent sans réponse et l'identité même de l'émetteur reste ouverte.

| # | Pièce | Ce qu'elle ferme | État | Responsable | Priorité |
|---|---|---|---|---|---|
| DR-022 | Extrait d'immatriculation de l'entité (K-bis ou équivalent) | Q9. Entité, forme, pays d'établissement | À produire | CEO | P0 |
| DR-023 | Statuts à jour | Q9 | À produire | CEO | P0 |
| DR-024 | Table de capitalisation | Q9. Répartition du capital | À produire | CEO | P0 |
| DR-025 | Pacte d'associés | Q9. Gouvernance, clauses de sortie | À produire | CEO | P0 |
| DR-026 | Note levant la contradiction sur l'identité des fondateurs | Q9. Les paramètres d'audit et les sources publiques ne concordent pas | À produire | CEO | P0 |
| DR-027 | Mentions légales publiées sur le site | Q9. Obligation d'information, et correction d'une évaluation publique défavorable liée à leur absence apparente | À produire | CEO | P0 |
| DR-028 | Actes de cession des droits des fondateurs à la société, couvrant le code antérieur à août 2024 | Q11. Titularité du principal actif, blocage de closing classique | À produire | CEO + avocat *(à mandater)* | P0 |
| DR-029 | Contrats de prestataires, freelances et stagiaires ayant écrit du code, avec clause de cession | Q11 | À produire | CEO + avocat *(à mandater)* | P0 |
| DR-030 | Certificats de dépôt de marque, classes et juridictions | Q11. Une antériorité canadienne homonyme de 2014 a été relevée | À produire | CEO | P1 |
| DR-031 | Recherche d'antériorités professionnelle | Q11. Distinctivité faible du signe, antériorité relevée | À produire | CEO + avocat *(à mandater)* | P1 |
| DR-032 | Portefeuille de noms de domaine : titulaire, expirations, verrouillage registrar, authentification forte au registrar | Q11, Q31. `tinypages.co`, `tinypages.dev` et le `.com` doivent avoir le même titulaire que l'entité | À produire | CTO | P1 |
| DR-033 | Nomenclature logicielle (SBOM) et rapport de licences open source | Q11. Le risque n'est pas la licence permissive, c'est la dépendance copyleft importée sans le savoir dans une offre en ligne | À produire | CTO | P1 |
| DR-034 | Accès en lecture au dépôt de code pour l'auditeur mandaté | Architecture réelle, tests, couverture, intégration continue, revue par un pair, part de code généré par IA | À produire | CTO | P1 |

---

## 3. Financier et traction

Aucun chiffre de traction n'a pu être établi par l'audit. Aucun ne peut l'être de l'extérieur : ils viennent tous du tableau de bord interne et des relevés fournisseurs.

| # | Pièce | Ce qu'elle ferme | État | Responsable | Priorité |
|---|---|---|---|---|---|
| DR-035 | Comptes annuels et situation comptable intermédiaire | Socle financier de toute due diligence | À produire | CEO *(fonction financière non attribuée)* | P0 |
| DR-036 | Nombre de comptes actifs et de sites publiés, par mois depuis le lancement | Q10, Q14. Aucune métrique publique exploitable n'existe | À produire | CEO | P0 |
| DR-037 | Revenu récurrent mensuel et sa décomposition abonnements / commissions | Q10 | À produire | CEO *(fonction financière non attribuée)* | P0 |
| DR-038 | Volume brut vendu par les créateurs sur douze mois | Q4. Sans lui, l'exposition TVA de l'article 9 bis ne peut pas être chiffrée | À produire | CEO *(fonction financière non attribuée)* | P0 |
| DR-039 | Cohortes de rétention et churn sur douze mois | Q10 | À produire | CEO | P1 |
| DR-040 | Répartition des comptes entre plan gratuit et plan payant | Q10, Q46 | À produire | CEO | P1 |
| DR-041 | Taux d'adoption du serveur MCP parmi les comptes actifs | Q1, Q10. Le pilotage par IA est la proposition de valeur centrale : son usage réel est une métrique attendue | À produire | CTO | P1 |
| DR-042 | Factures fournisseurs sur douze mois : hébergement, vidéo, email, fournisseur d'IA | Q16, Q25. Aucun coût unitaire n'existe aujourd'hui dans le dossier, sauf un ordre de grandeur email | À produire | CEO *(fonction financière non attribuée)* | P1 |
| DR-043 | Modèle de coûts unitaires et de marge brute par plan | Q16, Q25 | À produire | CEO | P1 |
| DR-044 | Plan de trésorerie et besoin de financement | Dimensionnement du tour, non arrêté à ce jour | À produire | CEO *(fonction financière non attribuée)* | P0 |

---

## 4. Paiements et fiscalité

| # | Pièce | Ce qu'elle ferme | État | Responsable | Priorité |
|---|---|---|---|---|---|
| DR-045 | Réponse écrite aux trois faits du tunnel d'achat : qui figure sur le reçu et la facture, qui fixe les CGV, qui décide de la mise à disposition du fichier | Q4. Le risque le plus coûteux du dossier se tranche avec ces trois réponses | À produire | CEO + CTO | P0 |
| DR-046 | Note de qualification fiscale sur l'article 9 bis du règlement d'exécution 282/2011, signée | Q4 | À produire | Avocat fiscaliste *(à mandater)* | P0 |
| DR-047 | Capture du tableau de bord Connect : type de compte, comptes connectés | Q18 | À produire | CTO | P0 |
| DR-048 | Extrait du code de création du paiement, montrant le type de charge et le mécanisme de commission | Q18, Q19 | À produire | CTO | P0 |
| DR-049 | Contrat plateforme signé avec le prestataire de paiement | Q18, Q23 | À produire | CEO | P1 |
| DR-050 | Montant de la réserve immobilisée sur le compte plateforme et son évolution sur douze mois | Q18. Trésorerie immobilisée, à déclarer si elle existe | À produire | CEO *(fonction financière non attribuée)* | P1 |
| DR-051 | Export du taux de litiges consolidé par mois et par compte connecté | Q20. Métrique standard de data room | À produire | CEO *(fonction financière non attribuée)* | P0 |
| DR-052 | Procédure écrite de traitement des litiges : qui répond, dans quel délai, avec quelles preuves | Q20 | À produire | CEO | P1 |
| DR-053 | Type d'intégration PayPal et preuve du prélèvement ou non de la commission | Q19. Fuite de commission possible sur le plan gratuit | À produire | CTO | P1 |
| DR-054 | Capture d'une page de paiement réelle avec la liste complète des scripts chargés | Q21. Détermine le périmètre PCI applicable | À produire | CTO | P0 |
| DR-055 | Attestation de conformité PCI du niveau applicable | Q21 | À produire | CTO | P1 |
| DR-056 | Règle produit interdisant le bloc de code personnalisé sur une page portant un formulaire de paiement | Q21. Critère d'éligibilité au questionnaire simplifié depuis le 31 mars 2025 | Absente | CTO | P1 |
| DR-057 | Revue du flux d'upsell en un clic au regard de l'authentification forte, et mandat conservé | Q22 | À produire | CTO | P1 |
| DR-058 | Statistiques de refus par type de flux : upsell, échéances 2 et 3 des paiements fractionnés, renouvellements | Q22. Mesure empirique de la solidité des exemptions invoquées | À produire | CTO | P1 |
| DR-059 | Correction de l'incohérence d'arrondi relevée sur un paiement fractionné (100 en trois fois 33) | Q22 | À produire | CTO | P1 |
| DR-060 | Exemplaire de reçu et de facture réellement émis à un acheteur, anonymisé | Q4, Q24 | À produire | CTO | P0 |
| DR-061 | Exemplaire de facture de commission adressée à un créateur, et son traitement TVA | Q19, Q24 | À produire | CEO + expert-comptable *(non attribué)* | P1 |
| DR-062 | Preuve de collecte et de conservation de deux éléments de localisation de l'acheteur par transaction | Q4. Exposition en cas de contrôle sur la TVA du pays du preneur | À produire | CTO | P1 |
| DR-063 | Paramétrage du calcul de TVA et immatriculations collectées auprès des créateurs | Q4, Q24 | À produire | CTO + expert-comptable *(non attribué)* | P1 |
| DR-064 | Contrat de raccordement à une plateforme agréée de facturation électronique, avec sa date | Q24. L'obligation de réception est en vigueur depuis le 1er septembre 2026 si l'entité est française, depuis le 1er janvier 2026 si elle est belge | À produire | CEO + expert-comptable *(non attribué)* | P0 |
| DR-065 | Position écrite sur l'e-reporting des ventes B2C des créateurs au 1er septembre 2027 | Q24. Risque de réclamation client de masse, ou argument de différenciation | À produire | CEO | P1 |
| DR-066 | Note de position motivée sur l'applicabilité de DAC7, traitant la commission et l'encaissement | Q4. La non-applicabilité est défendable pour les produits numériques, ouverte pour le coaching | À produire | Avocat fiscaliste *(à mandater)* | P1 |
| DR-067 | Politique d'usage acceptable et procédure de modération des pages de vente | Q23. Le cœur de cible longe la liste des activités restreintes du prestataire de paiement | Absente | CEO | P0 |

---

## 5. Architecture et infrastructure

| # | Pièce | Ce qu'elle ferme | État | Responsable | Priorité |
|---|---|---|---|---|---|
| DR-068 | Schéma d'architecture daté : hébergeur, régions, CDN, pare-feu applicatif, protection anti-déni de service | Q13. Huit lignes sur dix-huit du tableau de stack sont vides | À produire | CTO | P0 |
| DR-069 | Inventaire des environnements exposés publiquement, et statut de l'alias tiers relevé | Q13, Q40. Un environnement de prévisualisation public contourne le CDN et le pare-feu du domaine principal | À produire | CTO | P1 |
| DR-070 | Inventaire des fournisseurs critiques, avec rôle, localisation et substituabilité | Q13, Q41 | À produire | CTO | P0 |
| DR-071 | Relevé des en-têtes de sécurité et de la configuration TLS sur les quatre hôtes principaux | Q6. Une demi-journée de travail une fois l'accès rétabli | À produire | CTO | P0 |
| DR-072 | Politique d'émission des certificats des sites clients : par hôte ou générique, condition d'émission | Q14, Q38 | À produire | CTO | P1 |
| DR-073 | Fournisseur vidéo et preuve que les URL de lecture sont signées et expirantes | Q15. Si elles sont publiques, le contenu payant de tous les créateurs est repartageable par lien | À produire | CTO | P1 |
| DR-074 | Politique de sauvegarde : fréquence, périmètre, chiffrement, rétention | Q17 | À produire | CTO | P1 |
| DR-075 | Journal du dernier test de restauration réussi, avec sa date | Q17. Pièce la plus importante de ce domaine | À produire | CTO | P1 |
| DR-076 | Objectifs de point et de délai de reprise, et plan de reprise écrit | Q17 | À produire | CTO | P1 |
| DR-077 | Page de statut public et engagement de disponibilité | Q17. Aucune page de statut n'a été trouvée | Absente | CTO | P2 |
| DR-078 | Description des environnements et règle écrite sur les données réelles hors production | Q40. Le compte fourni à l'audit comme compte de test contenait les données réelles du dirigeant | À produire | CTO | P0 |
| DR-079 | Processus de mise en production : revue par un pair, retour arrière, infrastructure décrite en code | Point sensible avec une équipe très réduite | À produire | CTO | P1 |
| DR-080 | Scores de performance, d'accessibilité et de référencement des pages principales | Q13. Croise l'obligation d'accessibilité applicable aux clients | À produire | CTO | P2 |

---

## 6. Sécurité

| # | Pièce | Ce qu'elle ferme | État | Responsable | Priorité |
|---|---|---|---|---|---|
| DR-081 | Attributs réels du cookie de session, relevés après authentification | Q6. Avec DR-082, la vérification la plus rentable du dossier | À produire | CTO | P0 |
| DR-082 | Attribut `sandbox` complet du bloc de code personnalisé, domaine enregistrable de son `src`, et CSP de cette origine | Q6, Q21, Q33. Décide si trois risques majeurs sont théoriques ou réels | À produire | CTO | P0 |
| DR-083 | Recherche de `tinypages` dans la Public Suffix List, et demande d'inscription si absente | Q6. L'inscription ne produit son effet qu'après diffusion dans les navigateurs : ce n'est pas un correctif d'urgence | À produire | CTO | P1 |
| DR-084 | Configuration CORS de l'API : liste blanche explicite ou correspondance par suffixe | Q6 | À produire | CTO | P0 |
| DR-085 | Mécanisme anti-CSRF employé | Q6. Une protection fondée sur `SameSite` seul serait inopérante dans cette topologie | À produire | CTO | P0 |
| DR-086 | Décision documentée sur la séparation des sites clients et de l'application sur deux domaines enregistrables | Q6, Q39. Chantier qui ne peut pas aboutir avant l'ouverture : à exposer chiffré et daté | À produire | CTO | P1 |
| DR-087 | Rapport du dernier test d'intrusion : date, prestataire, périmètre, constats, remédiation | Q36. Sans lui, 85 des 97 lignes du questionnaire de sécurité restent « inconnue » | Absente (sauf pièce contraire) | CTO | P0 |
| DR-088 | Test d'intrusion couvrant explicitement l'isolement entre locataires et la chaîne d'injection indirecte | Q33, Q36. Un test qui ne couvre pas le multi-tenant ne couvre pas le risque principal | Absente | CTO + prestataire *(à mandater)* | P1 |
| DR-089 | Politique de gestion des vulnérabilités : délais de correction par gravité, analyse des dépendances | Q36 | À produire | CTO | P1 |
| DR-090 | Captures du produit montrant l'authentification à deux facteurs pour les comptes ouverts par email | Q37 | À produire | CTO | P1 |
| DR-091 | Politique d'accès à la production : nombre de personnes, moindre privilège, revue périodique, journalisation | Q37 | À produire | CTO | P1 |
| DR-092 | Règle et journal de la prise d'identité d'un client par un employé | Q37. Question systématique en due diligence | À produire | CTO | P1 |
| DR-093 | Politique de gestion des sessions : durée, révocation, déconnexion de tous les appareils | Q37 | À produire | CTO | P1 |
| DR-094 | Liste de mots réservés pour les identifiants de sous-domaine | Q38 | Absente (sauf pièce contraire) | CTO | P1 |
| DR-095 | Politique de non-réutilisation des identifiants libérés par un créateur parti | Q38 | Absente (sauf pièce contraire) | CTO | P1 |
| DR-096 | Procédure de vérification de propriété d'un domaine personnalisé, à l'ajout et après résiliation | Q38. Dans ce schéma, TinyPages est le service tiers vulnérable, pas la victime | À produire | CTO | P1 |
| DR-097 | Limites de débit sur la création et la publication, par compte et par adresse, plan gratuit inclus | Q39 | Absente (sauf pièce contraire) | CTO | P0 |
| DR-098 | Dispositif de détection d'abus sur le contenu publié | Q39 | Absente (sauf pièce contraire) | CTO | P1 |
| DR-099 | Fichier `security.txt` publié, avec contact et date d'expiration | Q36. Coût quasi nul, marqueur de maturité lu par les auditeurs | À produire | CTO | P0 |
| DR-100 | Page publique de signalement d'abus, adresse dédiée surveillée, délai de traitement affiché | Q23, Q39, Q41. Exigence du règlement sur les services numériques, quelle que soit la taille | Absente (sauf pièce contraire) | CEO | P0 |
| DR-101 | Registre des signalements et des retraits de contenu depuis le lancement | Q23, Q39 | À produire | CEO | P1 |
| DR-102 | Relevé de réputation du domaine auprès des principaux filtres de navigation, et surveillance mise en place | Q39. Un signalement sur le domaine partagé retirerait d'un coup tous les sites clients et l'application | À produire | CTO | P1 |
| DR-103 | Plan de crise pour le scénario « le domaine est signalé comme dangereux par un navigateur majeur » | Q39. Risque de continuité d'activité, pas seulement de sécurité | Absente | CTO | P1 |
| DR-104 | Historique des incidents de sécurité depuis le lancement | Q36. La question sera posée telle quelle : une réponse préparée vaut mieux qu'une découverte | À produire | CEO | P0 |
| DR-105 | Plan de réponse à incident et procédure de notification de violation sous 72 heures | Q41 | À produire | CTO + avocat *(à mandater)* | P1 |
| DR-106 | Preuve de l'isolation des données entre créateurs au niveau de la base, et test automatisé qui échoue si une requête franchit la frontière de locataire | Q6, Q36 | À produire | CTO | P1 |
| DR-107 | Certification ou audit de sécurité externe, s'il en existe un | Aucune mention publique n'a été trouvée | Absente (sauf pièce contraire) | CTO | P2 |
| DR-108 | Attestation d'assurance cyber | Domaine gouvernance du questionnaire de sécurité | À produire | CEO | P1 |

---

## 7. IA et serveur MCP

C'est le domaine où l'audit a produit le plus de preuves, et où les manques sont les plus structurants.

| # | Pièce | Ce qu'elle ferme | État | Responsable | Priorité |
|---|---|---|---|---|---|
| DR-109 | Export du catalogue complet des 104 actions avec leurs schémas JSON et leurs descriptions | Q12. Le périmètre réel n'est pas auditable depuis la liste d'outils que voit le client | À produire | CTO | P0 |
| DR-110 | Preuve d'une confirmation appliquée côté serveur avant publication, dépublication, envoi et programmation | Q2, Q3. Le test du 18 septembre 2026 a établi qu'il n'y en a aucune sur la publication | Absente | CTO | P0 |
| DR-111 | Nouvelle liste d'outils exposant les 15 actions à effet public ou irréversible comme des outils nommés et annotés | Q12, Q35. Conditionne aussi toute candidature à un annuaire de connecteurs | Absente | CTO | P0 |
| DR-112 | Journal des actions effectuées par l'IA : horodatage, acteur humain ou IA, session, compte cible, action, résultat | Q30. Première demande de l'auditeur technique et d'une autorité de protection des données | Absente (sauf pièce contraire) | CTO | P0 |
| DR-113 | Fonction d'annulation et historique des versions des pages | Q30 | Absente (sauf pièce contraire) | CTO | P1 |
| DR-114 | Batterie d'évals versionnée : un cas par garde-fou, par modèle et par client supporté, avec seuils bloquants | Q7. Le risque produit le plus structurant du dossier | Absente (sauf pièce contraire) | CTO | P1 |
| DR-115 | Résultats datés de la dernière exécution de la batterie, et politique de gel de version en cas d'échec | Q7 | Absente | CTO | P1 |
| DR-116 | Métadonnées OAuth du serveur MCP (`oauth-authorization-server` et `oauth-protected-resource`) | Q31. PKCE, enregistrement dynamique de clients, portées, durées | À produire | CTO | P0 |
| DR-117 | Liste des portées disponibles, dont une portée lecture seule | Q31. L'autorisation observée semble être tout ou rien | Absente (sauf pièce contraire) | CTO | P1 |
| DR-118 | Cycle de vie du jeton API : durée, portée, rotation, révocation, dernière utilisation, jetons distincts par client | Q31. Secret à haut privilège donnant accès en écriture, en publication et en envoi | À produire | CTO | P0 |
| DR-119 | Règle de portée du jeton entre comptes et comportement exact de `switch_account` | Q32. Le cloisonnement n'a pas pu être éprouvé, faute d'un second compte | À produire | CTO | P1 |
| DR-120 | Relevé de test multi-comptes sur deux locataires réellement distincts | Q32 | À produire | CTO | P1 |
| DR-121 | Relevé du test d'envoi sur un compte au plan payant dédié | Q3, Q34. Seule question laissée ouverte par le screening : le mur de facturation est-il le seul contrôle ? | À produire | CTO | P0 |
| DR-122 | Contre-mesures d'injection indirecte : marqueur de données non fiables, échappement, troncature dans les réponses d'outil | Q33 | Absente | CTO | P1 |
| DR-123 | Règle de protection du contexte métier persistant et des consignes système du compte | Q33. Un champ libre de 10 000 caractères, écrit par le canal automatisé, oriente toutes les générations futures sans apparaître dans le contenu produit | Absente | CTO | P1 |
| DR-124 | Version corrigée des instructions du serveur : périmètre réduit au strict usage des outils, affichage d'un différentiel avant publication | Q30, Q35. Les instructions actuelles demandent au modèle de ne pas montrer son travail | Absente | CTO | P0 |
| DR-125 | Documentation corrigée sur la capacité d'envoi d'emails par l'IA, avec sa date | Q34. L'affirmation actuelle est fausse : `send_email` et `schedule_email` existent | À produire | CTO | P0 |
| DR-126 | Échange écrit avec l'éditeur de l'annuaire de connecteurs, ou déclaration qu'aucune soumission n'a été faite | Q35. Un refus déjà essuyé serait une information matérielle | À produire | CEO | P1 |
| DR-127 | Échange écrit avec l'éditeur du modèle sur l'usage de sa marque dans les accroches produit | Q8. Conclusion juridique à valider par un avocat | À produire | CEO + avocat *(à mandater)* | P1 |
| DR-128 | Matrice de compatibilité par client et par version, réellement testée | Q8, Q31 | À produire | CTO | P1 |
| DR-129 | Modèle, fournisseur, coût unitaire, plafonds d'usage et données transmises pour l'IA intégrée au produit | Q16, Q44. Marge brute, sous-traitance, transparence | À produire | CTO | P1 |
| DR-130 | Contrat du fournisseur de modèle, avec clause de non-entraînement | Q41, Q44 | À produire | CEO + avocat *(à mandater)* | P1 |
| DR-131 | Spécification OpenAPI de l'API, grille de limites de débit par plan, liste des webhooks | Q31. Une data room de ce niveau sans spécification d'API sur un produit « pilotable par IA » est un signal négatif | À produire | CTO | P1 |
| DR-132 | Locataire de test dédié et anonyme, pour les tests restants et le test d'intrusion | Q40, Q3 | À produire | CTO | P0 |

---

## 8. Emails et délivrabilité

| # | Pièce | Ce qu'elle ferme | État | Responsable | Priorité |
|---|---|---|---|---|---|
| DR-133 | Relevé DNS des domaines d'envoi : SPF, DKIM, DMARC, sur TinyPages et sur des domaines créateurs | Q26, Q27. Une commande suffit une fois l'accès ouvert | À produire | CTO | P0 |
| DR-134 | Description de l'isolation de réputation entre créateurs : flux, pools d'adresses, sélecteurs | Q26. Risque systémique si absente | À produire | CTO | P0 |
| DR-135 | Règle de suspension automatique d'un créateur au-delà du seuil de plaintes | Q26, Q27 | Absente (sauf pièce contraire) | CTO | P1 |
| DR-136 | En-têtes complets d'un email créateur réellement reçu, anonymisés | Q27. Désinscription en un clic, alignement DMARC | À produire | CTO | P1 |
| DR-137 | Taux de plaintes agrégé du parc sur douze mois | Q27. Seuil de 0,3 % imposé par les principaux fournisseurs de messagerie | À produire | CTO | P0 |
| DR-138 | Facture du fournisseur d'envoi sur douze mois et plan réellement souscrit | Q25. Le chiffrage du dossier repose aujourd'hui sur des tarifs publics et une hypothèse de volume | À produire | CEO | P1 |
| DR-139 | Distribution des tailles de liste et fréquence d'envoi moyenne du parc | Q25. Sans elles, aucune projection de coût n'est possible | À produire | CTO | P1 |
| DR-140 | Règle de gestion de la liste de suppression : partagée entre créateurs ou cloisonnée | Q26, Q41. Une mutualisation sans base légale poserait une question de finalité | À produire | CTO | P1 |
| DR-141 | Schéma du modèle de contact montrant date, origine et libellé du consentement, et export les incluant | Q28. Sans eux, la preuve du consentement est inexécutable pour tous les créateurs | À produire | CTO | P0 |
| DR-142 | Procédure d'import en masse et ce qu'elle conserve de la preuve de consentement | Q28. Concerne aussi la migration annoncée d'une liste de plus de 70 000 contacts | À produire | CTO | P1 |
| DR-143 | En-têtes d'un envoi réel de la newsletter du fondateur depuis la plateforme, et volume mensuel | Q29. Preuve de passage à l'échelle la plus simple à produire et la plus scrutée | À produire | CEO | P1 |

---

## 9. Données personnelles et conformité réglementaire

| # | Pièce | Ce qu'elle ferme | État | Responsable | Priorité |
|---|---|---|---|---|---|
| DR-144 | Politique de confidentialité de TinyPages | Q41, Q43 | À produire | CEO + avocat *(à mandater)* | P0 |
| DR-145 | Conditions générales d'utilisation et de vente de TinyPages | Q41, Q46 | À produire | CEO + avocat *(à mandater)* | P0 |
| DR-146 | Contrat de sous-traitance proposé aux créateurs, ou conditions qui en tiennent lieu, vérifiées mention par mention | Q41. Sans lui, chaque créateur est en écart et la plateforme devient un risque pour son propre client | À produire | CEO + avocat *(à mandater)* | P0 |
| DR-147 | Liste publiée et datée des sous-traitants ultérieurs, incluant le fournisseur du modèle d'IA, avec rôle et localisation | Q41. Distinguer sous-traitant, responsable autonome et responsable conjoint | À produire | CEO | P0 |
| DR-148 | Matrice des rôles par traitement | Q41. Trois casquettes cumulées : sous-traitant, responsable, hébergeur | À produire | CEO + avocat *(à mandater)* | P0 |
| DR-149 | Registre des traitements | Q41. Première pièce demandée par une autorité comme par un auditeur | À produire | CEO | P0 |
| DR-150 | Registre des violations de données | Q41 | À produire | CEO | P1 |
| DR-151 | Localisation des données par traitement, et mécanismes d'encadrement des transferts hors Union avec analyses d'impact | Q41 | À produire | CTO + avocat *(à mandater)* | P1 |
| DR-152 | Tableau des durées de conservation, et preuve de leur application dans le produit | Q41 | À produire | CTO | P1 |
| DR-153 | Preuve qu'un contact peut être supprimé définitivement depuis l'interface, y compris dans les sauvegardes | Q42. Établi comme impossible par le canal automatisé ; non testé en interface | À produire | CTO | P0 |
| DR-154 | Gabarits juridiques réellement remplis, non indexés tant qu'ils sont vides, et capture d'un compte neuf corrigé | Q5. Aujourd'hui, deux documents juridiques vides sont publiés et indexés sur chaque compte | Absente | CTO + avocat *(à mandater)* | P0 |
| DR-155 | Double opt-in activé par défaut, et capture du parcours de confirmation | Q5, Q28. Le compte observé porte `doubleOptin: false` | Absente | CTO | P0 |
| DR-156 | Relevé réseau d'un chargement de page avec navigateur vierge, avant et après consentement, sur le site et sur un site client | Q43. Test de cinq minutes, documentable en une capture | À produire | CTO | P0 |
| DR-157 | Mécanisme de consentement aux traceurs fourni aux sites clients, et son périmètre | Q43. S'il n'existe pas, TinyPages place ses clients en écart par conception | À produire | CTO | P0 |
| DR-158 | Paquet « hébergeur » au titre du règlement sur les services numériques : points de contact, conditions décrivant la modération, mécanisme de notification et d'action, exposé des motifs de toute restriction | Q41. L'exemption des petites entreprises ne couvre pas ces articles | Absente (sauf pièce contraire) | CEO + avocat *(à mandater)* | P0 |
| DR-159 | Désignation d'un représentant dans l'Union, si l'entité est établie hors Union | Q9, Q41. Deux désignations distinctes le cas échéant | À produire | CEO + avocat *(à mandater)* | P1 |
| DR-160 | Mention informant l'utilisateur qu'il interagit avec un système d'IA | Q44. Applicable depuis le 2 août 2026 | À produire | CTO | P0 |
| DR-161 | Marquage lisible par machine des contenus générés | Q44. Exigible au 2 décembre 2026 : l'échéance la plus proche du dossier, elle tombe pendant la levée | Absente | CTO | P0 |
| DR-162 | Note de qualification fournisseur ou déployeur au titre du règlement sur l'IA | Q44 | À produire | Avocat *(à mandater)* | P1 |
| DR-163 | Note de sensibilisation interne à l'IA, datée, et son registre | Q44. Sans seuil d'effectif ; coût quasi nul, absence facile à relever | Absente | CEO | P1 |
| DR-164 | Fonction d'export complet des données d'un créateur, et son format | Q47. Absente du catalogue automatisé ; les frais de changement disparaissent le 12 janvier 2027 | Absente (sauf pièce contraire) | CTO | P1 |
| DR-165 | Clauses de sortie dans les conditions générales : préavis, transition assistée, suppression après migration | Q47 | À produire | CEO + avocat *(à mandater)* | P1 |
| DR-166 | Preuve du recueil, dans le tunnel d'achat, de l'accord exprès à l'exécution immédiate et de la renonciation au droit de rétractation | Q46, Q47. Sinon ce sont les créateurs qui sont exposés | À produire | CTO | P1 |
| DR-167 | Déclaration d'accessibilité et mesure du niveau atteint | Q13. TinyPages peut être exempté comme microentreprise ; ses clients ne le sont pas tous | À produire | CTO | P2 |
| DR-168 | Conservation des données d'identification des créateurs, au titre du régime de l'hébergeur | Q41 | À produire | CTO + avocat *(à mandater)* | P1 |

---

## 10. Produit, marché et discours commercial

| # | Pièce | Ce qu'elle ferme | État | Responsable | Priorité |
|---|---|---|---|---|---|
| DR-169 | Grille tarifaire officielle datée, et matrice des limites par plan | Q46. Trois contradictions d'offre restent ouvertes sur ce que le client achète | À produire | CEO | P0 |
| DR-170 | Journal des corrections apportées aux pages contradictoires du site et de la documentation | Q34, Q46. Les contradictions C-001 et C-003 restent ouvertes | À produire | CEO | P0 |
| DR-171 | Comparatif fonctionnel daté et archivé face aux concurrents dotés d'un serveur MCP officiel | Q1. Au moins cinq concurrents en publient un | À produire | CEO | P0 |
| DR-172 | Retrait ou reformulation de l'affirmation « seule plateforme », sur tous les supports | Q1, Q45. Vérifiable en dix minutes par n'importe quel investisseur | À produire | CEO | P0 |
| DR-173 | Dossier de preuve daté pour chaque allégation absolue ou chiffrée | Q45, Q50. La charge de la preuve pèse sur l'annonceur | À produire | CEO | P0 |
| DR-174 | Archives datées des neuf pages comparatives et des pages concurrentes au jour du relevé, avec révision trimestrielle | Q45. Un comparatif périmé devient trompeur sans que personne n'ait rien changé | À produire | CEO | P1 |
| DR-175 | Registre des consentements aux témoignages publiés : accord écrit, date, identité vérifiable | Q45 | À produire | CEO | P1 |
| DR-176 | Méthode d'invitation aux avis en ligne et distribution réelle des notes | Q45. Une trentaine d'avis, avec une note rapportée de façon contradictoire : aucun chiffre n'est citable en l'état | À produire | CEO | P1 |
| DR-177 | Journal des versions du produit, daté, depuis le lancement | Q49. Les dates de la bascule vers l'IA n'ont pas pu être confirmées | À produire | CTO | P1 |
| DR-178 | Feuille de route produit datée, distinguant ce qui existe, ce qui est prévu et ce qui ne sera pas fait | Q48. Une lacune assumée avec une date coûte moins cher qu'une lacune découverte | À produire | CEO | P0 |
| DR-179 | Note de positionnement face à l'hébergement natif de pages par les éditeurs de modèles | Q8. Deux offres datées de juin et juillet 2026 | À produire | CEO | P0 |
| DR-180 | Charte de support : canaux, horaires, délais d'engagement | Q48. Les seules données disponibles sont des avis publics anecdotiques | À produire | CEO | P1 |
| DR-181 | Avertissement produit sur le risque de publication de contenu en volume sans révision | Q50. Risque reporté sur les clients, mais pertinent si le référencement est un argument de vente | À produire | CEO | P2 |
| DR-182 | Statut de `emilio.tinypages.co` : site de démonstration interne ou client tiers | Q9, Q10. Conditionne son usage comme preuve de traction | À produire | CEO | P1 |
| DR-183 | Contrats clients types et éventuels contrats grands comptes | Pièce standard de due diligence commerciale | À produire | CEO | P1 |

---

## 11. Équipe et gouvernance

| # | Pièce | Ce qu'elle ferme | État | Responsable | Priorité |
|---|---|---|---|---|---|
| DR-184 | Organigramme et liste des contrats de travail et de prestation en cours | Pièce standard ; les deux rôles clés sont tenus par la même personne | À produire | CEO | P0 |
| DR-185 | Plan de renfort et de continuité en cas d'indisponibilité du dirigeant | La concentration CEO / CTO sur une seule personne est un risque identifié du dossier | À produire | CEO | P0 |
| DR-186 | Engagements de confidentialité signés, y compris prestataires | Domaine ressources humaines du questionnaire de sécurité | À produire | CEO | P1 |
| DR-187 | Procédure de retrait des accès au départ d'une personne | Q37 | À produire | CTO | P1 |
| DR-188 | Politique de sécurité écrite et approuvée, et désignation d'un responsable | Q36, Q41 | À produire | CTO | P1 |
| DR-189 | Mandat de l'avocat et mandat de l'expert-comptable ou du conseil fiscal | Q4, Q11, Q41, Q44. **Trente pièces de cet index dépendent d'un rôle non pourvu** : 20 d'un avocat à mandater, 10 d'une fonction financière ou comptable qu'aucun paramètre ne désigne | À produire | CEO | P0 |

---

## 12. Précautions de constitution

Ces règles ne sont pas des pièces, mais elles conditionnent la recevabilité du dossier.

- **Aucune donnée personnelle de créateur, de contact, d'acheteur ou d'élève** ne doit entrer dans la data room. Tous les exemples de reçus, factures, exports et en-têtes sont anonymisés avant versement.
- **Aucun secret** : clé, jeton, mot de passe, URL signée. Les captures de la gestion des jetons sont masquées.
- **Les sites clients restent anonymisés** (Site A, Site B…) sauf accord écrit du créateur concerné.
- **Le compte utilisé pour les relevés d'exécution ne doit pas être versé ni cité nominativement** : il porte l'identité réelle du dirigeant. Les relevés qui en sont issus sont déjà anonymisés dans les annexes.
- **Chaque pièce est datée au jour de son extraction.** Une capture non datée ne prouve rien dans six semaines.
- **Les références réglementaires des livrables doivent être reconfirmées sur source officielle** avant diffusion : elles proviennent de sources secondaires, l'accès aux bases officielles ayant été fermé pendant tout l'audit.

---

## 13. Décompte

| État | Nombre de pièces | Part |
|---|---|---|
| Prête | 18 | 10 % |
| À produire | 143 | 76 % |
| Absente | 28 | 15 % |
| **Total** | **189** | **100 %** |

Répartition des états « absente » : 14 portent la mention *(sauf pièce contraire)* — l'audit présume l'absence sans l'avoir établie sur tous les canaux — et 14 sont des absences établies ou des chantiers dont l'objet n'existe pas aujourd'hui.

**Comment lire ce décompte.** Les 18 pièces prêtes ont toutes été produites par l'audit lui-même : ce sont des analyses, pas des pièces de société. **Aucune pièce d'entreprise n'a été vue par cet audit.** Les 143 pièces « à produire » existent probablement en partie déjà, chez le CTO, dans une console fournisseur ou chez l'expert-comptable : les extraire est une affaire d'heures, pas de semaines. Les 28 pièces « absente » sont d'une autre nature : elles supposent un chantier, et sept d'entre elles sont des chantiers P0 — confirmation serveur avant publication, outils nommés pour les actions irréversibles, journal des actions de l'IA, gabarits juridiques remplis, double opt-in par défaut, marquage des contenus générés, politique d'usage acceptable et canal de signalement.

**Ce que ce décompte ne dit pas.** Il ne mesure pas la qualité du produit. Il mesure l'écart entre ce que TinyPages sait de lui-même et ce qu'il peut montrer. Un auditeur mandaté arrivera dans la même position que cet audit : sans accès interne, il ne verra que ce qui lui est remis. Les 76 % de pièces à produire sont donc la charge de travail réelle avant l'ouverture, et elle se concentre sur une seule personne.

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
10. **DR-087, DR-088 — le test d'intrusion.** À commander dès que le locataire dédié DR-132 existe, avec un périmètre couvrant explicitement le multi-tenant et l'injection indirecte.

---

*Index établi sans aucun accès interne, avec l'egress réseau fermé et sans test actif. L'état « à produire » ne préjuge pas de l'existence d'une pièce : il constate qu'aucun tiers ne l'a vue. À relire par le CTO, le CEO et l'avocat à mandater avant diffusion.*
