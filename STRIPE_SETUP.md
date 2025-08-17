# Guide d'Installation Stripe pour AERH CONSULTING TAHITI

## 🔧 Configuration Stripe

### 1. URLs d'endpoint à configurer dans Stripe

Voici les URLs que vous devez configurer dans votre dashboard Stripe :

#### Webhooks
- **URL du webhook :** `https://votre-domaine.com/api/stripe/webhook`
- **Événements à écouter :**
  - `checkout.session.completed`
  - `customer.subscription.created`
  - `customer.subscription.updated`
  - `customer.subscription.deleted`
  - `invoice.payment_succeeded`
  - `invoice.payment_failed`

#### URLs de redirection
- **URL de succès :** `https://votre-domaine.com/success?session_id={CHECKOUT_SESSION_ID}`
- **URL d'annulation :** `https://votre-domaine.com/pricing?canceled=true`

### 2. Création des produits dans Stripe

Vous devez créer 2 produits dans votre dashboard Stripe :

#### Produit 1 : Plan Mensuel
- **Nom :** AERH CONSULTING TAHITI - Plan Mensuel
- **Prix :** 42 EUR/mois
- **Type :** Abonnement récurrent
- **Période de facturation :** Mensuelle

#### Produit 2 : Plan Annuel
- **Nom :** AERH CONSULTING TAHITI - Plan Annuel
- **Prix :** 420 EUR/an
- **Type :** Abonnement récurrent
- **Période de facturation :** Annuelle

### 3. Variables d'environnement

Créez un fichier `.env.local` avec vos clés Stripe :

```env
# URL de base
NEXT_PUBLIC_BASE_URL=https://votre-domaine.com

# Clés Stripe
STRIPE_PUBLISHABLE_KEY=pk_live_VOTRE_CLE_PUBLIQUE
STRIPE_SECRET_KEY=sk_live_VOTRE_CLE_SECRETE
STRIPE_WEBHOOK_SECRET=whsec_VOTRE_SECRET_WEBHOOK

# Clé publique pour le frontend
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_live_VOTRE_CLE_PUBLIQUE

# Price IDs (récupérés après création des produits)
STRIPE_MONTHLY_PRICE_ID=price_VOTRE_ID_MENSUEL
STRIPE_YEARLY_PRICE_ID=price_VOTRE_ID_ANNUEL
```

### 4. Mise à jour des Price IDs

Une fois vos produits créés dans Stripe, mettez à jour les Price IDs dans :

**Fichier :** `src/app/pricing-stripe/page.tsx`

```tsx
{/* Plan Mensuel */}
<StripeCheckout
  planType="monthly"
  planName="Plan Mensuel"
  price="42€"
  priceId="price_VOTRE_VRAI_PRICE_ID_MENSUEL" // ← Remplacez ici
  features={monthlyFeatures}
/>

{/* Plan Annuel */}
<StripeCheckout
  planType="yearly"
  planName="Plan Annuel"
  price="420€"
  priceId="price_VOTRE_VRAI_PRICE_ID_ANNUEL" // ← Remplacez ici
  features={yearlyFeatures}
/>
```

## 🧪 Tests

### Test des endpoints API

```bash
# Test création session de paiement
curl -X POST http://localhost:8000/api/stripe/checkout \
  -H "Content-Type: application/json" \
  -d '{
    "priceId": "price_VOTRE_PRICE_ID",
    "planType": "monthly"
  }'

# Test webhook (depuis Stripe CLI)
stripe listen --forward-to localhost:8000/api/stripe/webhook
```

### Test des pages

- ✅ Page tarification : `http://localhost:8000/pricing-stripe`
- ✅ Page succès : `http://localhost:8000/success`
- ✅ Page annulation : `http://localhost:8000/cancel`

## 🚀 Déploiement

### 1. Variables d'environnement en production

Configurez ces variables dans votre plateforme de déploiement (Vercel, Netlify, etc.) :

```env
NEXT_PUBLIC_BASE_URL=https://votre-domaine-production.com
STRIPE_PUBLISHABLE_KEY=pk_live_...
STRIPE_SECRET_KEY=sk_live_...
STRIPE_WEBHOOK_SECRET=whsec_...
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_live_...
```

### 2. Configuration du webhook en production

Dans votre dashboard Stripe :
- **URL :** `https://votre-domaine-production.com/api/stripe/webhook`
- **Mode :** Production
- **Événements :** Même liste que ci-dessus

## 📋 Checklist de mise en production

- [ ] Produits créés dans Stripe
- [ ] Price IDs mis à jour dans le code
- [ ] Variables d'environnement configurées
- [ ] Webhook configuré et testé
- [ ] URLs de redirection testées
- [ ] Tests de paiement effectués
- [ ] Page de succès fonctionnelle
- [ ] Page d'annulation fonctionnelle

## 🔒 Sécurité

- ✅ Clés secrètes jamais exposées côté client
- ✅ Validation des webhooks avec signature
- ✅ Gestion des erreurs appropriée
- ✅ HTTPS obligatoire en production
- ✅ Variables d'environnement sécurisées

## 📞 Support

En cas de problème :
1. Vérifiez les logs Stripe dans votre dashboard
2. Consultez les logs de votre application
3. Testez avec Stripe CLI en local
4. Contactez le support Stripe si nécessaire

---

**L'intégration Stripe est maintenant prête pour la production !** 🎉
