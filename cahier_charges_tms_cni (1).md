# Cahier des charges fonctionnel

## Training Management System (TMS)
### Centre National de l'Informatique (CNI) – Tunisie

Version : 1.0
Date : 16/03/2026

---

# 1. Contexte et enjeux

Le CNI souhaite digitaliser le cycle de vie des formations pour améliorer la planification, la traçabilité et le suivi des compétences. Le TMS centralise les données et réduit les traitements manuels, avec une visibilité claire pour la direction.

---

# 2. Objectifs du projet

## 2.1 Objectif général

Mettre en place une plateforme centralisée pour gérer l'ensemble des formations internes.

## 2.2 Objectifs spécifiques

- Centraliser toutes les formations et leurs sessions
- Automatiser les tâches administratives (inscriptions, validations, présences)
- Assurer un suivi des participants et des résultats
- Produire des indicateurs simples pour la prise de décision
- Améliorer la traçabilité et l'archivage

---

# 3. Périmètre du projet

## 3.1 Inclus

- Gestion des utilisateurs et des rôles
- Catalogue des formations
- Planification des sessions
- Inscriptions et validations
- Suivi des présences
- Évaluations et résultats
- Génération de certificats
- Reporting et tableaux de bord

## 3.2 Hors périmètre (pour cette version)

- Gestion financière et facturation
- Gestion RH complète (carrières, paie)
- Formation e-learning avancée (LMS complet)
- Gestion des fournisseurs externes complexe

---

# 4. Acteurs et rôles

## 4.1 Super administrateur

- Gère le système, les comptes et les rôles
- Configure les paramètres globaux

## 4.2 Gestionnaire formation

- Crée les formations et planifie les sessions
- Valide les inscriptions
- Suit les résultats et la certification

## 4.3 Formateur

- Anime les sessions
- Marque les présences
- Saisit les évaluations

## 4.4 Participant

- S'inscrit aux formations
- Participe aux sessions
- Consulte ses résultats et certificats

---

# 5. Processus principaux

## 5.1 Création d'une formation

- Le gestionnaire crée une formation (titre, objectifs, durée, niveau)
- La formation est enregistrée au statut Brouillon

## 5.2 Planification d'une session

- Le gestionnaire crée une session liée à une formation
- Il définit dates, lieu, formateur, capacité
- La session passe au statut Planifiée

## 5.3 Inscriptions et validation

- Le participant demande l'inscription
- Le gestionnaire approuve ou rejette
- Si la capacité est atteinte, le participant est mis en liste d'attente

## 5.4 Déroulement et présence

- Le formateur marque les présences par session
- Le taux de présence est calculé automatiquement

## 5.5 Évaluation et certification

- Le formateur saisit la note
- Le certificat est généré si les critères sont respectés

---

# 6. Exigences fonctionnelles détaillées

## 6.1 Utilisateurs et rôles

- Créer, modifier, désactiver un utilisateur
- Attribuer un ou plusieurs rôles
- Réinitialiser un mot de passe

Champs utilisateur : Nom, Prénom, Email, Département (optionnel), Rôle, Statut

## 6.2 Formations (catalogue)

- Créer / modifier / supprimer une formation
- Définir objectifs pédagogiques et niveau
- Classer par domaine ou catégorie

Champs formation : Titre, Description, Objectifs, Durée, Niveau, Catégorie, Statut

## 6.3 Sessions

- Créer / modifier / annuler une session
- Définir capacité maximale et lieu
- Affecter un formateur

Champs session : Date début, Date fin, Lieu, Formateur, Capacité, Statut

## 6.4 Inscriptions

- Soumettre une demande d'inscription
- Valider, rejeter ou mettre en liste d'attente
- Notifier l'utilisateur (email interne)

Statuts inscription : Demandée, Approuvée, Rejetée, Liste d'attente

## 6.5 Présences

- Marquer présence ou absence par session
- Calcul automatique du taux de présence

## 6.6 Évaluations

- Saisir une note par participant
- Types d'évaluation : quiz simple
- Notes sur 20 avec seuil de réussite

## 6.7 Certificats

- Génération automatique si critères respectés
- Téléchargement du certificat PDF

Contenu : Nom participant, Formation, Date, Signature, Référence

## 6.8 Reporting

- Nombre total de formations
- Nombre de sessions par période
- Taux de participation et de réussite
- Performance des formateurs

## 6.9 Paramétrage

- Seuil de réussite
- Seuil de présence
- Modèle de certificat
- Annuaire des départements

---

# 7. Règles de gestion

1. Un participant ne peut pas suivre deux sessions qui se chevauchent.
2. Un formateur ne peut pas animer deux sessions au même moment.
3. Le nombre de participants ne dépasse pas la capacité maximale.
4. Un certificat est délivré seulement si les critères sont respectés.
5. Les sessions annulées ne génèrent pas de certificat.
6. Les inscriptions en liste d'attente passent à Approuvée dès qu'une place se libère.

---

# 8. Modèle de données simplifié

## Entités principales

### Utilisateur

- id
- nom
- prénom
- email
- rôle
- département (optionnel)
- statut

### Formation

- id
- titre
- description
- objectifs
- durée
- niveau
- catégorie
- statut

### Session

- id
- formation_id
- formateur_id
- date_début
- date_fin
- lieu
- capacité
- statut

### Inscription

- id
- participant_id
- session_id
- statut
- date_demande

### Présence

- id
- session_id
- participant_id
- présent

### Évaluation

- id
- session_id
- participant_id
- note

### Certificat

- id
- participant_id
- formation_id
- date_émission
- référence

---

# 9. Exigences non fonctionnelles

## Sécurité

- Authentification sécurisée
- Gestion des accès par rôles
- Journalisation des actions sensibles

## Performance

- Temps de réponse < 3 secondes pour les écrans principaux
- Support multi-utilisateurs

## Disponibilité

- Système disponible 24/7
- Sauvegarde quotidienne des données

## Traçabilité

- Historique des actions (création, modification, validation)

## Ergonomie

- Interface simple et responsive
- Temps de formation utilisateur réduit

---

# 10. Livrables

- Plateforme TMS opérationnelle
- Documentation utilisateur
- Documentation technique
- Tableaux de bord simples
- Modèle de certificat

---

# 11. Critères d'acceptation

- 100% des modules listés dans le périmètre sont fonctionnels
- Les règles de gestion sont respectées
- Les certificats sont générés automatiquement
- Les tableaux de bord affichent les indicateurs clés
- Les utilisateurs peuvent se connecter et gérer leurs profils

---

# 12. Propositions d'amélioration (optionnelles)

- Notifications email pour inscriptions, rappels et validations
- Intégration calendrier (invitation de session)
- Import / export Excel des participants et sessions
- Enquête de satisfaction après formation
- Connexion SSO (Active Directory) si disponible
- API simple pour intégration avec d'autres outils internes
