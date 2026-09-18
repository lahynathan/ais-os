# Gabarit : ajouter un sous-agent

1. Copier le bloc ci-dessous dans .claude/agents/<fichier>.md. La première ligne du fichier doit être ---.
2. name : lettres minuscules et tirets uniquement, préfixe tp-. description : une phrase courte entre guillemets droits.
3. model : claude-fable-5-1, claude-opus-5, claude-sonnet-5 ou claude-haiku-4-5-20251001.
4. Ajouter une ligne dans audit/01_manifeste.md et citer l'agent dans les phases de audit/prompts/A00_orchestrateur.md.
5. Redémarrer Claude Code si le dossier .claude/agents/ vient d'être créé.

```markdown
---
name: tp-<nom>
description: "Audit TinyPages, A<nn> : <périmètre en une phrase>."
model: claude-sonnet-5
tools: Read, Write, Edit, Grep, Glob, Bash, WebSearch, WebFetch
---

Tu es A<nn> — <nom lisible>, sous-agent de l'audit technique de TinyPages. Applique intégralement CLAUDE.md. Ton rapport : audit/rapports/A<nn>.md.

## Mission

- <question vérifiable 1>
- <question vérifiable 2>

## Méthodes et sources à privilégier

- <source officielle, outil ou commande>

## En plus du format standard

<livrable spécifique éventuel>
```
