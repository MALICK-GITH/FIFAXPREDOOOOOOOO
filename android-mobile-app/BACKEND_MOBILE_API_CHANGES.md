# CHANGEMENTS BACKEND - Rendre le site compatible Mobile

## 📋 APIs NOUVELLEMENT AJOUTÉES

### 1. Matchs

#### GET /api/matches/live
- **Description:** Récupère uniquement les matchs en direct
- **Response:**
```json
{
  "success": true,
  "data": {
    "matches": [...],
    "total": 10
  },
  "meta": {
    "source": "https://api.1xbet.com",
    "fetchedAt": "2024-04-16T12:00:00.000Z",
    "timestamp": "2024-04-16T12:00:00.000Z"
  }
}
```

#### GET /api/matches/upcoming
- **Description:** Récupère uniquement les matchs à venir
- **Response:** Même format que /api/matches/live

#### GET /api/matches/finished
- **Description:** Récupère uniquement les matchs terminés
- **Response:** Même format que /api/matches/live

#### GET /api/matches/filter
- **Description:** Filtre avancé de matchs
- **Query params:** `league`, `team`, `status`, `minOdds`, `maxOdds`
- **Response:**
```json
{
  "success": true,
  "data": {
    "matches": [...],
    "total": 5,
    "filters": { "league": "...", "team": "...", "status": "...", "minOdds": 1.5, "maxOdds": 3.0 }
  },
  "meta": { "timestamp": "..." }
}
```

#### GET /api/matches/search
- **Description:** Recherche de matchs par texte
- **Query params:** `q` (query)
- **Response:**
```json
{
  "success": true,
  "data": {
    "matches": [...],
    "total": 3,
    "query": "real madrid"
  },
  "meta": { "timestamp": "..." }
}
```

#### GET /api/matches/:id/statistics
- **Description:** Statistiques détaillées d'un match
- **Response:**
```json
{
  "success": true,
  "data": {
    "statistics": {
      "matchId": "...",
      "homeTeam": "...",
      "awayTeam": "...",
      "score": { "home": 2, "away": 1 },
      "possession": { "home": 50, "away": 50 },
      "shots": { "home": 12, "away": 8 },
      "corners": { "home": 5, "away": 3 },
      "fouls": { "home": 8, "away": 10 },
      "yellowCards": { "home": 2, "away": 1 },
      "redCards": { "home": 0, "away": 0 }
    }
  },
  "meta": { "timestamp": "..." }
}
```

### 2. Paris & Cotes

#### GET /api/odds/:matchId
- **Description:** Cotes pour un match spécifique
- **Response:**
```json
{
  "success": true,
  "data": {
    "odds": {
      "matchId": "...",
      "homeTeam": "...",
      "awayTeam": "...",
      "markets": {
        "1X2": { "1": 1.85, "X": 3.50, "2": 4.20 },
        "totalGoals": { "over2.5": 1.75, "under2.5": 2.10 }
      }
    }
  },
  "meta": { "timestamp": "..." }
}
```

### 3. Coupons

#### GET /api/coupon/:id
- **Description:** Récupère les détails d'un coupon spécifique
- **Response:**
```json
{
  "success": true,
  "data": {
    "coupon": { ... }
  },
  "meta": {
    "timestamp": "2024-04-16T12:00:00.000Z"
  }
}
```

#### GET /api/coupon/stats
- **Description:** Statistiques des coupons (win rate, profit, etc.)

#### POST /api/coupon/favorite
- **Description:** Ajouter un coupon aux favoris
- **Body:** `{ couponId, userId }`
- **Response:**
```json
{
  "success": true,
  "data": {
    "message": "Coupon ajoute aux favoris",
    "couponId": "...",
    "userId": "..."
  },
  "meta": { "timestamp": "..." }
}
```

#### GET /api/coupon/favorites
- **Description:** Liste des coupons favoris
- **Query params:** `userId`
- **Response:**
```json
{
  "success": true,
  "data": {
    "favorites": [...],
    "total": 0,
    "userId": "..."
  },
  "meta": { "timestamp": "..." }
}
```

### 4. Équipes & Ligues

#### GET /api/teams/:id/matches
- **Description:** Matchs d'une équipe spécifique
- **Response:**
```json
{
  "success": true,
  "data": {
    "matches": [...],
    "total": 10,
    "teamId": "real-madrid"
  },
  "meta": {
    "source": "https://api.1xbet.com",
    "fetchedAt": "...",
    "timestamp": "..."
  }
}
```

#### GET /api/leagues/:id/matches
- **Description:** Matchs d'une ligue spécifique
- **Response:**
```json
{
  "success": true,
  "data": {
    "matches": [...],
    "total": 15,
    "leagueId": "fifa-virtual-penalty"
  },
  "meta": {
    "source": "https://api.1xbet.com",
    "fetchedAt": "...",
    "timestamp": "..."
  }
}
```

#### GET /api/leagues/:id/standings
- **Description:** Classement d'une ligue
- **Response:**
```json
{
  "success": true,
  "data": {
    "standings": [
      { "position": 1, "name": "Real Madrid", "played": 10, "won": 8, "drawn": 1, "lost": 1, "goalsFor": 25, "goalsAgainst": 10, "points": 25 },
      ...
    ],
    "total": 20,
    "leagueId": "fifa-virtual-penalty"
  },
  "meta": { "timestamp": "..." }
}
```

### 5. Prédictions IA

#### GET /api/predictions
- **Description:** Liste des prédictions récentes pour tous les matchs
- **Response:**
```json
{
  "success": true,
  "data": {
    "predictions": [
      {
        "matchId": "...",
        "homeTeam": "...",
        "awayTeam": "...",
        "league": "...",
        "status": "LIVE",
        "score": { "home": 2, "away": 1 },
        "prediction": {
          "recommendation": "1",
          "confidence": 0.85,
          "odds": 1.85
        },
        "extraPowerFilter": { "score": 0.92, "recommendation": "STRONG_BET" }
      }
    ],
    "total": 20
  },
  "meta": { "timestamp": "..." }
}
```

#### GET /api/predictions/top
- **Description:** Meilleures prédictions du jour (classées par score combiné)
- **Response:**
```json
{
  "success": true,
  "data": {
    "predictions": [
      {
        "matchId": "...",
        "homeTeam": "...",
        "awayTeam": "...",
        "prediction": { "recommendation": "X", "confidence": 0.90, "odds": 3.50 },
        "extraPowerFilter": { "score": 0.95 },
        "combinedScore": 0.92
      }
    ],
    "total": 10
  },
  "meta": { "timestamp": "..." }
}
```

#### GET /api/predictions/:matchId
- **Description:** Prédiction détaillée pour un match spécifique
- **Response:**
```json
{
  "success": true,
  "data": {
    "prediction": {
      "match": { ... },
      "bettingMarkets": [ ... ],
      "prediction": {
        "maitre": {
          "decision_finale": {
            "pari_choisi": "1",
            "confidence": 0.85,
            "cote": 1.85
          }
        }
      },
      "extraPowerFilter": { "score": 0.92, "recommendation": "STRONG_BET" }
    }
  },
  "meta": { "timestamp": "..." }
}
```

### 6. Page d'Accueil - Fonctionnalités

#### GET /api/stats/global
- **Description:** Statistiques globales de la page d'accueil
- **Response:**
```json
{
  "success": true,
  "data": {
    "global": {
      "matches": { "total": 150, "live": 12, "upcoming": 30, "finished": 108 },
      "coupons": { "total": 500, "won": 350, "lost": 120, "pending": 30, "winRate": 70.0, "profit": 1250.50 },
      "leagues": [{ "name": "...", "total": 50, "live": 10, "upcoming": 20, "finished": 20 }]
    }
  },
  "meta": { "timestamp": "..." }
}
```

#### GET /api/watchlist
- **Description:** Watchlist de matchs surveillés
- **Response:**
```json
{
  "success": true,
  "data": {
    "watchlist": [...],
    "total": 0,
    "userId": "..."
  },
  "meta": { "timestamp": "..." }
}
```

#### GET /api/heatmap
- **Description:** Heatmap des ligues avec activité
- **Response:**
```json
{
  "success": true,
  "data": {
    "heatmap": [{ "name": "...", "total": 50, "live": 10, "upcoming": 20, "avgOdds": 1.85 }],
    "total": 5
  },
  "meta": { "timestamp": "..." }
}
```

#### GET /api/denicheur
- **Description:** Dénicheur de matchs (matchs ~10 min)
- **Query params:** `full` (option complète)
- **Response:**
```json
{
  "success": true,
  "data": {
    "matches": [{ "matchId": "...", "homeTeam": "...", "awayTeam": "...", "league": "...", "startTime": "...", "odds": 1.85 }],
    "total": 10,
    "fullOption": false
  },
  "meta": { "timestamp": "..." }
}
```

#### GET /api/odds/alerts
- **Description:** Alerts de cotes anormales
- **Response:**
```json
{
  "success": true,
  "data": {
    "alerts": [{ "matchId": "...", "homeTeam": "...", "awayTeam": "...", "odds": 2.8, "type": "HIGH_ODD" }],
    "total": 5
  },
  "meta": { "timestamp": "..." }
}
```

### 7. Page Coupon - Génération & Validation

#### POST /api/coupon/generate
- **Description:** Générer un coupon optimisé
- **Body:** `{ size, league, risk, stake }`
- **Response:**
```json
{
  "success": true,
  "data": {
    "coupon": {
      "id": "coupon_...",
      "matches": [...],
      "config": { "size": 3, "league": "all", "risk": "balanced", "stake": 1000 },
      "calculated": { "totalOdds": 5.5, "potentialWin": 5500 }
    }
  },
  "meta": { "timestamp": "..." }
}
```

#### POST /api/coupon/validate
- **Description:** Valider un ticket (analyse santé)
- **Body:** `{ coupon }`
- **Response:**
```json
{
  "success": true,
  "data": {
    "validation": {
      "isValid": true,
      "health": 85,
      "warnings": [],
      "risk": "medium"
    }
  },
  "meta": { "timestamp": "..." }
}
```

#### POST /api/coupon/ladder
- **Description:** Coupon Ladder IA (répartition 60/30/10)
- **Body:** `{ size, league, stake }`
- **Response:**
```json
{
  "success": true,
  "data": {
    "ladder": {
      "id": "ladder_...",
      "coupons": [
        { "name": "Safe (60%)", "matches": [...], "stake": 600 },
        { "name": "Balanced (30%)", "matches": [...], "stake": 300 },
        { "name": "Aggressive (10%)", "matches": [...], "stake": 100 }
      ]
    }
  },
  "meta": { "timestamp": "..." }
}
```

#### POST /api/coupon/multi
- **Description:** Multi-stratégie (Ultra-Safe, Safe, Balanced, Aggressive)
- **Body:** `{ size, league, stake }`
- **Response:**
```json
{
  "success": true,
  "data": {
    "strategies": [
      { "name": "Ultra-Safe", "risk": "ultra_safe", "matches": [...] },
      { "name": "Safe", "risk": "safe", "matches": [...] },
      { "name": "Balanced", "risk": "balanced", "matches": [...] },
      { "name": "Aggressive", "risk": "aggressive", "matches": [...] }
    ]
  },
  "meta": { "timestamp": "..." }
}
```

#### GET /api/coupon/journal
- **Description:** Journal des cotes et historique
- **Response:**
```json
{
  "success": true,
  "data": {
    "journal": [{ "id": "...", "timestamp": "...", "matches": [], "totalOdds": 5.5, "profit": 1200, "status": "won" }],
    "total": 100
  },
  "meta": { "timestamp": "..." }
}
```

### 8. Page Match - Détails Avancés

#### GET /api/match/:id/coach
- **Description:** Analyse Coach IA pour un match
- **Response:**
```json
{
  "success": true,
  "data": {
    "coach": {
      "matchId": "...",
      "analysis": "1",
      "confidence": 0.85,
      "recommendation": 1.85,
      "reasoning": "Analyse basée sur les indicateurs techniques et historiques"
    }
  },
  "meta": { "timestamp": "..." }
}
```

#### GET /api/match/:id/kpi
- **Description:** KPI Match Engine (radar tactique)
- **Response:**
```json
{
  "success": true,
  "data": {
    "kpi": {
      "matchId": "...",
      "homeTeam": "...",
      "awayTeam": "...",
      "metrics": { "momentum": 75, "form": 80, "h2h": 65, "goals": 70, "defense": 60, "attack": 85 },
      "radar": [75, 80, 65, 70, 60, 85]
    }
  },
  "meta": { "timestamp": "..." }
}
```

#### GET /api/match/:id/insight
- **Description:** Brief intelligent (forme, historique, blessures)
- **Response:**
```json
{
  "success": true,
  "data": {
    "insights": [
      { "type": "form", "title": "Forme récente", "value": "..." },
      { "type": "h2h", "title": "Historique", "value": "..." },
      { "type": "injury", "title": "Blessures", "value": "..." },
      { "type": "weather", "title": "Conditions", "value": "..." }
    ],
    "matchId": "..."
  },
  "meta": { "timestamp": "..." }
}
```

#### GET /api/match/:id/exact-score
- **Description:** Projection score exact
- **Response:**
```json
{
  "success": true,
  "data": {
    "exactScore": {
      "matchId": "...",
      "homeTeam": "...",
      "awayTeam": "...",
      "predictions": [
        { "score": "1-0", "probability": 0.35 },
        { "score": "2-1", "probability": 0.25 },
        { "score": "1-1", "probability": 0.20 }
      ],
      "mostLikely": "1-0",
      "confidence": 0.35
    }
  },
  "meta": { "timestamp": "..." }
}
```

### 9. APIs Supplémentaires

#### GET /api/structure
- **Description:** Structure des données du système
- **Response:**
```json
{
  "success": true,
  "data": { ... },
  "meta": {
    "source": "...",
    "timestamp": "..."
  }
}
```

#### GET /api/team-badge
- **Description:** Générer un badge SVG pour une équipe
- **Query params:** `name`
- **Response:** SVG image (image/svg+xml)

#### GET /api/logo/:fileName
- **Description:** Récupérer un logo d'équipe
- **Response:** Image

#### POST /api/chat
- **Description:** Chat IA intégré
- **Body:** `{ message, history, context }`
- **Response:**
```json
{
  "success": true,
  "answer": "...",
  ...
}
```

#### GET /api/db/status
- **Description:** Statut de la base de données
- **Response:**
```json
{
  "success": true,
  "status": "connected"
}
```

### 10. Stats & Analytics

#### GET /api/stats/overview
- **Description:** Vue d'ensemble des statistiques
- **Response:**
```json
{
  "success": true,
  "data": {
    "overview": {
      "matches": {
        "total": 150,
        "live": 12,
        "upcoming": 30,
        "finished": 108
      },
      "coupons": {
        "total": 500,
        "won": 350,
        "winRate": 70.0
      }
    }
  },
  "meta": { "timestamp": "..." }
}
```

---

## 📊 RÉSUMÉ DES CHANGEMENTS

| API | Méthode | Statut | Priorité |
|-----|---------|--------|----------|
| /api/matches/live | GET | ✅ Implémenté | HAUTE |
| /api/matches/upcoming | GET | ✅ Implémenté | HAUTE |
| /api/matches/finished | GET | ✅ Implémenté | HAUTE |
| /api/matches/filter | GET | ✅ Implémenté | HAUTE |
| /api/matches/search | GET | ✅ Implémenté | HAUTE |
| /api/matches/:id/statistics | GET | ✅ Implémenté | HAUTE |
| /api/odds/:matchId | GET | ✅ Implémenté | HAUTE |
| /api/coupon/:id | GET | ✅ Implémenté | HAUTE |
| /api/coupon/stats | GET | ✅ Implémenté | HAUTE |
| /api/teams | GET | ✅ Implémenté | HAUTE |
| /api/leagues | GET | ✅ Implémenté | HAUTE |
| /api/teams/:id/matches | GET | ✅ Implémenté | HAUTE |
| /api/leagues/:id/matches | GET | ✅ Implémenté | HAUTE |
| /api/leagues/:id/standings | GET | ✅ Implémenté | MOYENNE |
| /api/coupon/favorite | POST | ✅ Implémenté | MOYENNE |
| /api/coupon/favorites | GET | ✅ Implémenté | MOYENNE |
| /api/stats/overview | GET | ✅ Implémenté | MOYENNE |
| /api/predictions | GET | ✅ Implémenté | HAUTE |
| /api/predictions/top | GET | ✅ Implémenté | HAUTE |
| /api/predictions/:matchId | GET | ✅ Implémenté | HAUTE |
| /api/stats/global | GET | ✅ Implémenté | HAUTE |
| /api/watchlist | GET | ✅ Implémenté | HAUTE |
| /api/heatmap | GET | ✅ Implémenté | HAUTE |
| /api/denicheur | GET | ✅ Implémenté | HAUTE |
| /api/odds/alerts | GET | ✅ Implémenté | HAUTE |
| /api/coupon/generate | POST | ✅ Implémenté | HAUTE |
| /api/coupon/validate | POST | ✅ Implémenté | HAUTE |
| /api/coupon/ladder | POST | ✅ Implémenté | HAUTE |
| /api/coupon/multi | POST | ✅ Implémenté | HAUTE |
| /api/coupon/journal | GET | ✅ Implémenté | MOYENNE |
| /api/match/:id/coach | GET | ✅ Implémenté | HAUTE |
| /api/match/:id/kpi | GET | ✅ Implémenté | HAUTE |
| /api/match/:id/insight | GET | ✅ Implémenté | HAUTE |
| /api/match/:id/exact-score | GET | ✅ Implémenté | HAUTE |
| /api/structure | GET | ✅ Normalisé | MOYENNE |
| /api/team-badge | GET | ✅ Normalisé | MOYENNE |
| /api/logo/:fileName | GET | ✅ Existant | MOYENNE |
| /api/chat | POST | ✅ Existant | MOYENNE |
| /api/db/status | GET | ✅ Existant | MOYENNE |

**Total APIs:** 40 (34 nouvelles + 6 existantes normalisées/documentées)

---

## 🔧 CONNEXION BASE DE DONNÉES

Toutes les APIs de génération de coupon sont connectées à la base de données:

### APIs avec sauvegarde en base de données:
- **POST /api/coupon/generate** - Sauvegarde avec `saveCouponGeneration`
- **POST /api/coupon/ladder** - Sauvegarde avec `saveCouponGeneration`
- **POST /api/coupon/multi** - Sauvegarde avec `saveCouponGeneration`
- **POST /api/coupon/favorite** - Sauvegarde avec `saveFavorite`

### APIs avec lecture en base de données:
- **GET /api/coupon/history** - Lecture avec `getCouponHistory`
- **GET /api/coupon/favorites** - Lecture avec `getFavorites`
- **GET /api/coupon/journal** - Lecture avec `getCouponHistory`
- **GET /api/db/status** - Statut de la base de données

### Support des bases de données:
- SQLite (par défaut, fichier `data/app.sqlite`)
- MySQL (configurable via variables d'environnement)

### Nouvelle table ajoutée:
- **favorites** - Stockage des favoris par utilisateur

## 🔧 NORMALISATION DES FORMATS DE RÉPONSE

### Format Standardisé

Toutes les nouvelles APIs utilisent ce format:

```json
{
  "success": true,
  "data": { ... },
  "meta": {
    "timestamp": "2024-04-16T12:00:00.000Z"
  }
}
```

### Format d'Erreur Standardisé

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

### APIs Normalisées

- ✅ GET /api/matches
- ✅ GET /api/matches/live
- ✅ GET /api/matches/upcoming
- ✅ GET /api/matches/finished
- ✅ GET /api/matches/filter
- ✅ GET /api/matches/search
- ✅ GET /api/matches/:id/statistics
- ✅ GET /api/odds/:matchId
- ✅ GET /api/coupon/:id
- ✅ GET /api/coupon/stats
- ✅ POST /api/coupon/favorite
- ✅ GET /api/coupon/favorites
- ✅ GET /api/teams
- ✅ GET /api/teams/:id/matches
- ✅ GET /api/leagues
- ✅ GET /api/leagues/:id/matches
- ✅ GET /api/leagues/:id/standings
- ✅ GET /api/stats/overview

---

## 🚀 UTILISATION POUR APPLICATION ANDROID

### Exemple d'appel depuis Android (Kotlin)

```kotlin
// Récupérer les matchs en direct
val response = apiService.getLiveMatches()
val liveMatches = response.data.matches

// Récupérer les statistiques des coupons
val statsResponse = apiService.getCouponStats()
val winRate = statsResponse.data.stats.winRate

// Récupérer la liste des équipes
val teamsResponse = apiService.getTeams()
val teams = teamsResponse.data.teams
```

---

## ⚠️ NOTES IMPORTANTES

1. **Format de réponse:** Les nouvelles APIs utilisent le format standardisé avec `data` et `meta`
2. **Codes d'erreur:** Chaque erreur a un code unique pour un meilleur handling côté mobile
3. **Timestamps:** Toutes les réponses incluent un timestamp UTC
4. **Compatibilité:** Les APIs existantes (/api/matches, /api/coupon/history, etc.) n'ont pas été modifiées pour éviter de casser le frontend existant

---

## 🔄 DÉPLOIEMENT

Pour déployer ces changements sur Render:

1. Committer les changements dans `server.js`
2. Push vers le repository Git
3. Render déploiera automatiquement

```bash
git add server.js
git commit -m "feat: add mobile-compatible APIs"
git push
```

---

**SIGNÉ:** SOLITAIRE HACK
