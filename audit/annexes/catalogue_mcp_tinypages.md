# Inventaire du catalogue MCP TinyPages

Relevé par A00 le 18 septembre 2026. **Méthode : lecture de la description publiée par le serveur `Tinypage` lui-même** (champ `description` de l'outil `search_actions`, plus les schémas des 24 outils directs chargés dans la session d'orchestration). Aucune action n'a été exécutée, aucun objet créé, aucun compte modifié.

Statut : **CONFIRMÉ** pour l'existence et le nom des actions. Le comportement réel de chaque action reste non vérifié.

## Surface d'action totale exposée à l'IA : 104

| Famille | Nombre | Part |
|---|---|---|
| Lecture (`get_`, `list_`, `search_`) | 47 | 45 % |
| Écriture (`create_`, `update_`, `add_`, `remove_`) | 39 | 38 % |
| Publication (`publish_`, `unpublish_`) | 10 | 10 % |
| Suppression (`delete_`) | 3 | 3 % |
| Envoi (`send_email`, `schedule_email`) | 2 | 2 % |
| Utilitaires (`search_actions`, `execute_action`, `switch_account`, `send_feedback`) | 3 | 3 % |

Détail : 24 outils directs (15 écritures, 6 lectures, 3 utilitaires) et 80 actions du catalogue interne atteignables par `search_actions` puis `execute_action`.

## Constats qui en découlent

**M-001 — CONFIRMÉ. Le catalogue contient `send_email` et `schedule_email`.** Il contient aussi `publish_webpage`, `publish_blog_post`, `publish_lesson`, `publish_all_lessons`, `publish_form`, `publish_automation_email` et leurs quatre inverses. **La contradiction C-002 est tranchée : l'IA peut envoyer et programmer des emails, et publier.** La documentation `docs.tinypages.co/getting-started/1-4-mcp-setup`, qui affirme que l'envoi n'est pas accessible via l'IA, est fausse ou périmée. La FAQ du site a raison.

**M-002 — CONFIRMÉ. Trois actions de suppression existent** : `delete_order_bump`, `delete_upsell`, `delete_automation_stop_condition`. Cela **corrige deux affirmations des rapports A07 et A04**, qui concluaient à l'absence totale de `delete_*`. L'affirmation exacte, plus étroite, est la suivante : aucune action ne supprime un contact, un produit, une page, un article, un formulaire, une leçon ou un email. Les objets secondaires, eux, sont supprimables par l'IA.

**M-003 — CONFIRMÉ. Aucune action ne couvre** : la gestion du domaine personnalisé, les paramètres de paiement ou la connexion Stripe, les remboursements, l'export des données du compte, les réglages de sécurité, la gestion des abonnés à un produit au-delà de `add_member_to_product`. Cela étaye le constat A07-004 : la promesse d'un produit « pilotable de bout en bout par Claude » ne couvre pas l'administration du compte ni l'argent.

**M-004 — CONFIRMÉ. Le catalogue donne accès aux données personnelles des contacts** : `list_contacts`, `search_contacts`, `create_contact`, `update_contact`, `list_form_submissions`, `list_email_recipients`, `list_automation_email_recipients`, `list_product_members`. Une IA connectée lit donc les contacts d'un créateur et les soumissions de ses formulaires. À croiser avec A06 sur le rôle de sous-traitant et avec A04 sur l'injection indirecte : ces champs sont alimentés par des tiers.

**M-005 — CONFIRMÉ. `switch_account` et `list_accounts` existent.** Le multi-comptes est donc réel et pilotable par l'IA. Portée de l'autorisation OAuth entre comptes : à déterminer.

**M-006 — étaye A04-004.** Toute action hors des 24 outils directs passe par `execute_action`. Un client MCP autorise par outil : une seule autorisation permanente sur `execute_action` couvre les 80 actions du catalogue, dont les 10 de publication, les 3 de suppression et les 2 d'envoi. Le consentement par outil du client est structurellement contourné.

## Liste complète

### Lecture (41 dans le catalogue interne)
`get_account`, `get_analytics_sales`, `get_analytics_summary`, `get_automation_email_stats`, `get_business_context`, `get_email_stats`, `get_form`, `get_image`, `get_modules`, `get_product_stats`, `get_template`, `get_upsell`, `get_video`, `list_accounts`, `list_automation_email_recipients`, `list_automation_emails`, `list_automation_stop_conditions`, `list_automations`, `list_blog_posts`, `list_contacts`, `list_coupons`, `list_email_recipients`, `list_emails`, `list_evergreen_deadlines`, `list_form_submissions`, `list_forms`, `list_images`, `list_lessons`, `list_links`, `list_product_members`, `list_products`, `list_templates`, `list_videos`, `list_webpages`, `search_contacts`, `search_docs`, `search_emails`, `search_images`, `search_products`, `search_tags`, `search_videos`

### Écriture (24 dans le catalogue interne)
`add_form_destination`, `add_form_page`, `add_member_to_product`, `add_order_bump`, `add_tag_to_contact`, `add_upsell`, `create_automation_stop_condition`, `create_contact`, `create_coupon`, `create_evergreen_deadline`, `create_link`, `create_tag`, `create_template`, `remove_tag_from_contact`, `update_business_context`, `update_contact`, `update_coupon`, `update_evergreen_deadline`, `update_form_destination`, `update_form_page`, `update_link`, `update_modules`, `update_template`, `update_upsell_content`

### Publication (10)
`publish_all_lessons`, `publish_automation_email`, `publish_blog_post`, `publish_form`, `publish_lesson`, `publish_webpage`, `unpublish_automation_email`, `unpublish_blog_post`, `unpublish_lesson`, `unpublish_webpage`

### Suppression (3)
`delete_automation_stop_condition`, `delete_order_bump`, `delete_upsell`

### Envoi (2)
`schedule_email`, `send_email`

### Outils directs (24)
`create_automation_email`, `create_blog_post`, `create_email`, `create_form`, `create_lesson`, `create_product`, `create_sales_page`, `create_webpage`, `execute_action`, `get_automation_email`, `get_blog_post`, `get_email`, `get_lesson`, `get_webpage`, `search_actions`, `send_feedback`, `switch_account`, `update_automation_email`, `update_blog_post`, `update_email`, `update_form`, `update_lesson`, `update_product`, `update_webpage`

## Limite

Les **schémas détaillés** des 80 actions du catalogue interne n'ont pas été extraits : cela demanderait un appel `search_actions` par famille. Les noms et leur classification suffisent aux constats ci-dessus. Les schémas restent nécessaires pour juger les confirmations, les champs obligatoires et les actions réellement destructrices.
