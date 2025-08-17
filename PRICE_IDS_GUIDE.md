# Guide pour Obtenir les Price IDs Stripe

## 🎯 Problème Identifié

Vous avez fourni des **Product IDs** (`prod_`) au lieu des **Price IDs** (`price_`).

Dans Stripe :
- **Product ID** = Le produit général (ex: "Abonnement AERH CONSULTING")
- **Price ID** = Le prix spécifique de ce produit (ex: "42€/mois" ou "420€/an")

## 📋 Comment Obtenir les Price IDs

### 1. Connectez-vous à votre Dashboard Stripe
- Allez sur https://dashboard.stripe.com
- Connectez-vous avec vos identifiants

### 2. Naviguez vers les Produits
- Dans le menu de gauche, cliquez sur **"Produits"**
- Vous devriez voir vos 2 produits :
  - `prod_SslFobo64JoRcF` (Abonnement annuel)
  - `prod_SslEUyqpS7Wkvn` (Abonnement mensuel)

### 3. Récupérez les Price IDs
Pour chaque produit :
1. **Cliquez sur le nom du produit**
2. Dans la section **"Tarification"**, vous verrez les prix
3. **Copiez l'ID qui commence par `price_`** (pas `prod_`)

Exemple de ce que vous devriez voir :
```
Produit: Abonnement Mensuel (prod_SslEUyqpS7Wkvn)
├── Prix: 42€/mois
│   └── ID: price_1234567890abcdef (← COPIEZ CELUI-CI)
```

### 4. Mettez à Jour l'Application

Une fois que vous avez les vrais Price IDs, remplacez dans `.env.local` :

```env
# Remplacez ces valeurs par vos vrais Price IDs
STRIPE_MONTHLY_PRICE_ID=price_VOTRE_VRAI_PRICE_ID_MENSUEL
STRIPE_YEARLY_PRICE_ID=price_VOTRE_VRAI_PRICE_ID_ANNUEL
```

## 🔧 Alternative : Créer les Prix via l'API

Si vous préférez, vous pouvez aussi créer les prix directement :

```bash
# Prix mensuel
curl https://api.stripe.com/v1/prices \
  -u sk_live_VOTRE_CLE_SECRETE: \
  -d product=prod_SslEUyqpS7Wkvn \
  -d unit_amount=4200 \
  -d currency=eur \
  -d "recurring[interval]=month"

# Prix annuel  
curl https://api.stripe.com/v1/prices \
  -u sk_live_VOTRE_CLE_SECRETE: \
  -d product=prod_SslFobo64JoRcF \
  -d unit_amount=42000 \
  -d currency=eur \
  -d "recurring[interval]=year"
```

## ✅ Test Final

Une fois les Price IDs corrects configurés, testez :

```bash
curl -X POST http://localhost:8000/api/stripe/checkout \
  -H "Content-Type: application/json" \
  -d '{"priceId": "price_VOTRE_VRAI_PRICE_ID", "planType": "monthly"}'
```

Vous devriez recevoir une URL de checkout Stripe au lieu d'une erreur.

## 🎯 Résultat Attendu

Avec les bons Price IDs, l'API retournera :
```json
{
  "url": "https://checkout.stripe.com/c/pay/cs_live_..."
}
```

Cette URL redirige vers le checkout sécurisé Stripe pour finaliser le paiement.
