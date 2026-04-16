# API Reference - SOLITAIRE HACK Android Application

## Base URL
- **Production:** `https://fifaxpred.onrender.com`
- **Development:** `http://localhost:3029` (configurable)

## Format de Réponse Standard

Toutes les APIs utilisent ce format:

```json
{
  "success": true,
  "data": { ... },
  "meta": {
    "timestamp": "ISO-8601"
  }
}
```

Format d'erreur:

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Description de l'erreur",
    "details": "Détails techniques"
  }
}
```

---

## 1. Matchs

### GET /api/matches
Liste de tous les matchs FIFA penalty

**Response:**
```json
{
  "success": true,
  "data": {
    "matches": [
      {
        "id": "12345",
        "homeTeam": "PSG",
        "awayTeam": "Real Madrid",
        "league": "Champions League",
        "status": "LIVE",
        "score": { "home": 2, "away": 1 },
        "startTime": "2024-01-15T20:00:00Z"
      }
    ],
    "total": 150
  },
  "meta": { "timestamp": "..." }
}
```

### GET /api/matches/live
Matchs en cours

### GET /api/matches/upcoming
Matchs à venir

### GET /api/matches/finished
Matchs terminés

### GET /api/matches/search
Recherche de matchs
- **Query params:** `q` (texte de recherche)

### GET /api/matches/filter
Filtrage avancé
- **Query params:** `league`, `status`, `minOdds`, `maxOdds`

### GET /api/matches/:id/details
Détails complets d'un match avec prédictions

### GET /api/matches/:id/statistics
Statistiques détaillées d'un match

---

## 2. Paris & Cotes

### GET /api/odds/:matchId
Cotes d'un match spécifique

---

## 3. Coupons

### POST /api/coupon/generate
Générer un coupon optimisé
- **Body:** `{ size, league, risk, stake }`
- **Sauvegarde en base de données:** ✅

**Response:**
```json
{
  "success": true,
  "data": {
    "coupon": {
      "id": "coupon_12345",
      "matches": [...],
      "config": { "size": 3, "league": "all", "risk": "balanced", "stake": 1000 },
      "calculated": { "totalOdds": 5.5, "potentialWin": 5500 }
    }
  },
  "meta": { "timestamp": "..." }
}
```

### POST /api/coupon/validate
Valider un ticket (analyse santé)
- **Body:** `{ coupon }`
- **Sauvegarde en base de données:** ✅

### POST /api/coupon/ladder
Coupon Ladder IA (répartition 60/30/10)
- **Body:** `{ size, league, stake }`
- **Sauvegarde en base de données:** ✅

### POST /api/coupon/multi
Multi-stratégie (Ultra-Safe, Safe, Balanced, Aggressive)
- **Body:** `{ size, league, stake }`
- **Sauvegarde en base de données:** ✅

### GET /api/coupon/history
Historique des coupons générés
- **Sauvegarde en base de données:** ✅

### GET /api/coupon/:id
Détails d'un coupon spécifique

### GET /api/coupon/stats
Statistiques des coupons

### POST /api/coupon/favorite
Ajouter un coupon aux favoris
- **Body:** `{ couponId, userId, coupon }`
- **Sauvegarde en base de données:** ✅

### GET /api/coupon/favorites
Liste des favoris d'un utilisateur
- **Query params:** `userId`
- **Sauvegarde en base de données:** ✅

### GET /api/coupon/journal
Journal des cotes et historique
- **Sauvegarde en base de données:** ✅

---

## 4. Équipes & Ligues

### GET /api/teams
Liste de toutes les équipes

### GET /api/teams/:id/matches
Matchs d'une équipe spécifique

### GET /api/leagues
Liste de toutes les ligues

### GET /api/leagues/:id/matches
Matchs d'une ligue spécifique

### GET /api/leagues/:id/standings
Classement d'une ligue

---

## 5. Prédictions IA

### GET /api/predictions
Liste des prédictions récentes

### GET /api/predictions/top
Meilleures prédictions du jour (classées par score combiné)

### GET /api/predictions/:matchId
Prédiction détaillée pour un match spécifique

---

## 6. Page d'Accueil

### GET /api/stats/global
Statistiques globales (matchs, coupons, ligues, profit)

### GET /api/watchlist
Watchlist de matchs surveillés

### GET /api/heatmap
Heatmap des ligues avec activité

### GET /api/denicheur
Dénicheur de matchs (~10 min)
- **Query params:** `full` (option complète)

### GET /api/odds/alerts
Alertes de cotes anormales

---

## 7. Page Match - Détails Avancés

### GET /api/match/:id/coach
Analyse Coach IA pour un match

### GET /api/match/:id/kpi
KPI Match Engine (radar tactique)

### GET /api/match/:id/insight
Brief intelligent (forme, historique, blessures)

### GET /api/match/:id/exact-score
Projection score exact

---

## 8. APIs Supplémentaires

### GET /api/structure
Structure des données du système

### GET /api/team-badge
Générer un badge SVG pour une équipe
- **Query params:** `name`
- **Response:** SVG image (image/svg+xml)

### GET /api/logo/:fileName
Récupérer un logo d'équipe
- **Response:** Image

### POST /api/chat
Chat IA intégré
- **Body:** `{ message, history, context }`

### GET /api/db/status
Statut de la base de données

---

## 9. Exports & Telegram

### POST /api/coupon/pdf
Générer PDF d'un coupon

### POST /api/coupon/image
Générer image d'un coupon

### POST /api/coupon/send-telegram
Envoyer coupon sur Telegram

### POST /api/coupon/send-telegram-pack
Envoyer pack de coupons sur Telegram

---

## Base de Données

Toutes les APIs de génération de coupon sauvegardent les données:
- `saveCouponGeneration` - Sauvegarde les coupons générés
- `saveCouponValidation` - Sauvegarde les validations
- `saveFavorite` - Sauvegarde les favoris
- `getCouponHistory` - Récupère l'historique
- `getFavorites` - Récupère les favoris

Support SQLite et MySQL selon configuration.

---

## Total APIs: 40

Toutes les fonctionnalités du site sont accessibles via API pour l'application Android.

---

**Signé:** SOLITAIRE HACK
