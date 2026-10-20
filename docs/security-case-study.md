# Étude de cas sécurité — données de marketplace et statistiques professionnelles

## Contexte

Une marketplace combine plusieurs catégories de données qui ne doivent pas être traitées de la même façon : données publiques d'annonce, données de compte, relations entre utilisateurs, statistiques commerciales et opérations de maintenance.

Le risque principal n'est pas seulement l'affichage d'un écran interdit. Un client mobile peut appeler l'API directement : les règles de sécurité doivent donc être appliquées côté base de données.

## Décision d'architecture

Pour mes fonctionnalités professionnelles, le contrôle est placé dans PostgreSQL/Supabase :

```text
Application mobile
      |
      | session Supabase (identité authentifiée)
      v
RLS + contraintes PostgreSQL + fonctions RPC limitées
      |
      v
Données de marketplace et métriques
```

L'interface améliore l'expérience ; elle n'est pas la frontière de sécurité.

## Cas 1 — Abonnements aux boutiques

### Menace

Un utilisateur tente de créer ou supprimer un abonnement au nom d'un autre compte, ou de multiplier les abonnements pour fausser un compteur.

### Contrôles mis en œuvre

- RLS activée sur la table des abonnements ;
- politiques d'écriture liées à `auth.uid()` ;
- contrainte `UNIQUE` sur le couple abonné / boutique ;
- contrainte interdisant l'auto-abonnement.

### Résultat

L'autorisation est évaluée par la base de données. Modifier l'application, rejouer une requête ou cacher un bouton ne suffit pas à contourner cette règle.

## Cas 2 — Statistiques de visite d'une boutique

### Menace

Les chiffres de fréquentation peuvent exposer une information commerciale et être gonflés par répétition. Une table de visites lisible directement par tout client divulguerait inutilement des données.

### Contrôles mis en œuvre

- RLS activée ; aucun droit d'accès direct donné aux rôles client ;
- fonction d'écriture précise au lieu d'un droit général `INSERT` ;
- fonction de consultation calculée à partir de l'identité de session ;
- une visite maximum par boutique, visiteur et jour via la clé primaire ;
- auto-visite écartée et entrée visiteur contrôlée.

### Résultat

Le vendeur connecté obtient ses propres agrégats, pas les journaux détaillés ou statistiques des autres boutiques. La déduplication protège l'intégrité fonctionnelle des mesures.

## Cas 3 — Expiration automatique des boosts

### Menace

Une application cliente ne doit jamais obtenir un droit large de modifier l'état commercial des annonces des autres utilisateurs.

### Contrôle mis en œuvre

Une fonction `SECURITY DEFINER` dédiée limite l'opération à l'expiration de boosts dont la date est déjà dépassée. Son comportement est fixe : pas de paramètre permettant de changer une autre colonne ou de choisir librement l'annonce à modifier.

### Résultat

Le privilège existe uniquement pour une tâche très étroite, conformément au principe du moindre privilège.

## Pratiques à renforcer ensuite

Cette présentation ne prétend pas qu'un contrôle remplace un programme de sécurité complet. Mes priorités de durcissement seraient :

1. Tests automatisés des politiques RLS avec des rôles `anon`, `authenticated` et administrateur.
2. Revue systématique de chaque fonction `SECURITY DEFINER` : propriétaire, `search_path`, privilèges `PUBLIC`, paramètres et tests d'autorisation.
3. Analyse des dépendances, scan de secrets et vérification des migrations dans une CI avant production.
4. Revue du stockage local des sessions mobiles et validation du besoin d'utiliser le stockage sécurisé de la plateforme.
5. Journalisation d'audit et alertes sur les actions d'administration ou de paiement sensibles.

## Compétences démontrées

`RLS` · `auth.uid()` · `RBAC applicatif` · `RPC PostgreSQL` · `SECURITY DEFINER` · `contrainte d'unicité` · `intégrité des données` · `moindre privilège` · `modélisation de menace`

## Limites de publication

Pour ne pas accroître la surface d'attaque, cette étude ne publie ni les politiques exactes complètes, ni les URL de services, ni les secrets, ni le schéma intégral, ni les procédures opérationnelles. Le code source reste privé.
