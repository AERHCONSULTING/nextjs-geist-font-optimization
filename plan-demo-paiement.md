# Plan d'Implémentation - Mode Démo et Système de Paiement

## 🎯 Objectifs
1. Implémenter un mode démo avec données fictives (Option 1)
2. Créer un système d'abonnement avec Stripe et PayPal
3. Gérer les tarifs : 5 000 F CFP/mois ou 50 000 F CFP/an

---

## 📋 Phase 1 : Mode Démo

### 1.1 Interface et Navigation
- **Fichier :** `src/app/page.tsx`
  - Ajouter bouton "Essayer la démo" sur la page d'accueil
  - Design attractif avec call-to-action

- **Fichier :** `src/components/navigation.tsx`
  - Ajouter indicateur "Mode Démo" dans le header
  - Timer de session visible

### 1.2 Gestion des Sessions Démo
- **Fichier :** `src/components/auth/DemoProvider.tsx`
  - Contexte pour gérer l'état démo
  - Timer de 30 minutes
  - Données fictives pré-chargées

- **Fichier :** `src/middleware.ts`
  - Ajouter route `/demo` comme publique
  - Gestion des sessions démo temporaires

### 1.3 Données Fictives
- **Fichier :** `src/lib/demo-data.ts`
  - Données réalistes pour TPE polynésienne
  - KPIs, graphiques, tableaux de bord
  - Données pour chaque module

### 1.4 Pages Démo
- **Fichier :** `src/app/demo/page.tsx`
  - Dashboard démo avec données fictives
  - Liens vers tous les modules en mode démo

---

## 📋 Phase 2 : Système de Paiement

### 2.1 Configuration des Paiements
- **Fichier :** `src/lib/stripe.ts`
  - Configuration Stripe avec clés API
  - Création des produits et prix

- **Fichier :** `src/lib/paypal.ts`
  - Configuration PayPal
  - Gestion des abonnements

### 2.2 Pages de Tarification
- **Fichier :** `src/app/pricing/page.tsx`
  - Affichage des plans tarifaires
  - Comparaison mensuel vs annuel
  - Boutons de souscription

### 2.3 APIs de Paiement
- **Fichier :** `src/app/api/stripe/create-checkout/route.ts`
  - Création des sessions Stripe Checkout
  - Gestion des webhooks

- **Fichier :** `src/app/api/paypal/create-order/route.ts`
  - Création des commandes PayPal
  - Gestion des notifications

### 2.4 Gestion des Abonnements
- **Fichier :** `src/components/subscription/SubscriptionManager.tsx`
  - Interface de gestion d'abonnement
  - Changement de plan, annulation

- **Fichier :** `src/app/api/subscription/route.ts`
  - CRUD des abonnements
  - Vérification du statut

---

## 📋 Phase 3 : Intégration et Sécurité

### 3.1 Middleware de Vérification
- **Fichier :** `src/middleware.ts` (mise à jour)
  - Vérification du statut d'abonnement
  - Redirection vers page de paiement si nécessaire

### 3.2 Base de Données (Simulation)
- **Fichier :** `src/lib/subscription-db.ts`
  - Simulation BDD pour les abonnements
  - Stockage local des statuts

### 3.3 Composants UI
- **Fichier :** `src/components/ui/pricing-card.tsx`
  - Cartes de tarification
  - Badges "Populaire", "Économique"

---

## 💰 Tarification Définie

### Plans d'Abonnement
- **Mensuel :** 5 000 F CFP/mois
- **Annuel :** 50 000 F CFP/an (économie de 10 000 F CFP)

### Avantages Mode Démo
- Accès limité à 30 minutes
- Toutes les fonctionnalités visibles
- Données fictives réalistes
- Call-to-action pour s'abonner

---

## 🔄 Flux Utilisateur

1. **Visiteur** → Page d'accueil → "Essayer la démo"
2. **Mode Démo** → 30 min d'exploration → Invitation à s'abonner
3. **Choix du Plan** → Paiement Stripe/PayPal → Accès complet
4. **Utilisateur Payant** → Accès illimité à tous les modules

---

## 📝 Variables d'Environnement Nécessaires

```env
# Stripe
STRIPE_PUBLIC_KEY=pk_...
STRIPE_SECRET_KEY=sk_...
STRIPE_WEBHOOK_SECRET=whsec_...

# PayPal
PAYPAL_CLIENT_ID=...
PAYPAL_CLIENT_SECRET=...
PAYPAL_WEBHOOK_ID=...
```

---

## ✅ Étapes d'Implémentation

1. **Mode Démo** (Phase 1)
2. **Système de Paiement** (Phase 2)  
3. **Tests et Intégration** (Phase 3)
4. **Déploiement Production**
