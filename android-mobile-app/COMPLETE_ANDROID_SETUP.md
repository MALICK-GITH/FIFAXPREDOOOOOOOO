# Configuration Complète - Application Android SOLITAIRE HACK

## 📋 Vue d'ensemble

Ce document récapitule tous les éléments nécessaires pour que l'application Android dispose de **TOUTES les fonctionnalités du site**.

---

## 🔌 APIs Disponibles (40 APIs)

### Base URL
- **Production:** `https://fifaxpred.onrender.com`
- **Development:** Configurable dans `local.properties`

### Catégories d'APIs

#### 1. Matchs (7 APIs)
- GET /api/matches - Tous les matchs
- GET /api/matches/live - Matchs en cours
- GET /api/matches/upcoming - Matchs à venir
- GET /api/matches/finished - Matchs terminés
- GET /api/matches/search - Recherche
- GET /api/matches/filter - Filtrage avancé
- GET /api/matches/:id/details - Détails complets

#### 2. Paris & Cotes (1 API)
- GET /api/odds/:matchId - Cotes d'un match

#### 3. Coupons (10 APIs)
- POST /api/coupon/generate - Génération (avec DB)
- POST /api/coupon/validate - Validation (avec DB)
- POST /api/coupon/ladder - Ladder IA (avec DB)
- POST /api/coupon/multi - Multi-stratégie (avec DB)
- GET /api/coupon/history - Historique (avec DB)
- GET /api/coupon/:id - Détails coupon
- GET /api/coupon/stats - Statistiques
- POST /api/coupon/favorite - Favoris (avec DB)
- GET /api/coupon/favorites - Liste favoris (avec DB)
- GET /api/coupon/journal - Journal des cotes (avec DB)

#### 4. Équipes & Ligues (5 APIs)
- GET /api/teams - Liste équipes
- GET /api/teams/:id/matches - Matchs équipe
- GET /api/leagues - Liste ligues
- GET /api/leagues/:id/matches - Matchs ligue
- GET /api/leagues/:id/standings - Classement

#### 5. Prédictions IA (3 APIs)
- GET /api/predictions - Prédictions récentes
- GET /api/predictions/top - Meilleures prédictions
- GET /api/predictions/:matchId - Prédiction match

#### 6. Page d'Accueil (5 APIs)
- GET /api/stats/global - Stats globales
- GET /api/watchlist - Watchlist
- GET /api/heatmap - Heatmap ligues
- GET /api/denicheur - Dénicheur de matchs
- GET /api/odds/alerts - Alerts cotes

#### 7. Page Match - Détails (4 APIs)
- GET /api/match/:id/coach - Coach IA
- GET /api/match/:id/kpi - KPI Engine
- GET /api/match/:id/insight - Brief intelligent
- GET /api/match/:id/exact-score - Score exact

#### 8. Supplémentaires (5 APIs)
- GET /api/structure - Structure système
- GET /api/team-badge - Badge SVG
- GET /api/logo/:fileName - Logo équipe
- POST /api/chat - Chat IA
- GET /api/db/status - Statut DB

---

## 📁 Fichiers de Configuration

### 1. API Reference
**Fichier:** `API_REFERENCE.md`
- Documentation complète de toutes les 40 APIs
- Format de réponse standardisé
- Exemples d'utilisation

### 2. Configuration Build
**Fichier:** `app/build.gradle`
- URLs injectées via BuildConfig
- Configuration Firebase (commentée pour l'instant)
- Dépendances Android complètes

### 3. Configuration Locale
**Fichier:** `local.properties.example`
- Exemple de configuration pour développement
- URLs de développement et production

### 4. Configuration Application
**Fichier:** `config.properties.example`
- Variables d'environnement pour les URLs
- Configuration des endpoints

---

## 🗄️ Base de Données

### Support
- **SQLite** (par défaut) - Fichier `data/app.sqlite`
- **MySQL** (optionnel) - Configurable via variables d'environnement

### Tables
- `coupon_generations` - Historique des coupons générés
- `coupon_validations` - Validations de tickets
- `telegram_logs` - Logs Telegram
- `audit_reports` - Rapports d'audit
- `favorites` - Favoris utilisateurs (nouvelle)

### APIs connectées à la DB
- POST /api/coupon/generate → `saveCouponGeneration`
- POST /api/coupon/ladder → `saveCouponGeneration`
- POST /api/coupon/multi → `saveCouponGeneration`
- POST /api/coupon/favorite → `saveFavorite`
- GET /api/coupon/history → `getCouponHistory`
- GET /api/coupon/favorites → `getFavorites`
- GET /api/coupon/journal → `getCouponHistory`

---

## 🎨 Fonctionnalités du Site Couvertes

### Page d'Accueil
✅ Stats globales (matchs, coupons, profit)
✅ Watchlist de matchs
✅ Heatmap des ligues
✅ Dénicheur de matchs (~10 min)
✅ Modes de match (upcoming, turbo, finder, live, finished)
✅ Recherche et filtrage avancé
✅ Alerts de cotes anormales

### Page Coupon
✅ Génération de coupon (simple et avancée)
✅ Validation de ticket (analyse santé)
✅ Ladder IA (répartition 60/30/10)
✅ Multi-stratégie (4 profils)
✅ Historique des coupons
✅ Favoris (ajout et liste)
✅ Journal des cotes
✅ Statistiques de coupons
✅ Exports (PDF, image, Telegram)

### Page Match
✅ Détails complets du match
✅ Coach IA (analyse)
✅ KPI Engine (radar tactique)
✅ Brief intelligent (forme, historique, blessures)
✅ Score exact (projection)
✅ Prédictions IA (liste, top, par match)

### Fonctionnalités Supplémentaires
✅ Chat IA intégré
✅ Badges d'équipe (SVG)
✅ Logos d'équipe
✅ Structure du système

---

## 🔧 Configuration Android

### 1. Copier les fichiers de configuration
```bash
cp local.properties.example local.properties
cp config.properties.example config.properties
```

### 2. Configurer l'URL du backend
Dans `local.properties`:
```
API_BASE_URL=https://fifaxpred.onrender.com
```

### 3. Synchroniser Gradle
```bash
./gradlew sync
```

### 4. Builder l'application
```bash
./gradlew assembleDebug
```

---

## 📱 Utilisation des APIs dans Android

### Exemple - Récupérer les matchs
```kotlin
val response = RetrofitClient.apiService.getMatches()
if (response.isSuccessful && response.body()?.success == true) {
    val matches = response.body()?.data?.matches
    // Utiliser les matchs
}
```

### Exemple - Générer un coupon
```kotlin
val request = CouponRequest(
    size = 3,
    league = "all",
    risk = "balanced",
    stake = 1000
)
val response = RetrofitClient.apiService.generateCoupon(request)
if (response.isSuccessful && response.body()?.success == true) {
    val coupon = response.body()?.data?.coupon
    // Sauvegarder le coupon en local
}
```

### Exemple - Favoris
```kotlin
// Ajouter aux favoris
val request = FavoriteRequest(
    couponId = "coupon_12345",
    userId = "user_123",
    coupon = couponObject
)
RetrofitClient.apiService.addToFavorites(request)

// Récupérer les favoris
val response = RetrofitClient.apiService.getFavorites("user_123")
val favorites = response.body()?.data?.favorites
```

---

## ✅ Checklist Déploiement

- [x] 40 APIs implémentées et documentées
- [x] Format de réponse standardisé
- [x] Connexion base de données (SQLite/MySQL)
- [x] Sauvegarde automatique des coupons générés
- [x] Sauvegarde des favoris
- [x] Documentation API complète
- [x] Configuration Android prête
- [x] Exemples d'utilisation

---

## 🚀 État Final

**L'application Android dispose de 100% des fonctionnalités du site.**

Toutes les APIs sont:
- ✅ Implémentées
- ✅ Documentées
- ✅ Connectées à la base de données
- ✅ Normalisées (format data/meta)
- ✅ Prêtes pour la production

**Total APIs:** 40

---

**Signé:** SOLITAIRE HACK
