# PROMPT PROFESSIONNEL - Création Application Android SOLITAIRE HACK

## 📋 INSTRUCTIONS POUR L'IA DÉVELOPPEUR ANDROID

Tu es un expert en développement Android avec Kotlin. Tu dois créer une application Android complète pour SOLITAIRE HACK qui se connecte à un backend existant avec 40 APIs.

---

## 🎯 OBJECTIF DU PROJET

Créer une application Android qui reproduit **TOUTES les fonctionnalités du site web** fifaxpred.onrender.com. L'application doit avoir accès à 40 APIs REST et offrir une expérience utilisateur moderne et fluide.

---

## 🔌 CONFIGURATION BACKEND

### Base URL
- **Production:** `https://fifaxpred.onrender.com`
- **Development:** Configurable dans `local.properties`

### Format de Réponse Standard
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

## 📱 STRUCTURE DU PROJET ANDROID

Crée la structure suivante:

```
app/
├── src/main/
│   ├── java/com/solitairehack/
│   │   ├── data/
│   │   │   ├── api/
│   │   │   │   ├── ApiService.kt
│   │   │   │   ├── RetrofitClient.kt
│   │   │   │   └── ApiConstants.kt
│   │   │   ├── model/
│   │   │   │   ├── Match.kt
│   │   │   │   ├── Coupon.kt
│   │   │   │   ├── Prediction.kt
│   │   │   │   ├── Team.kt
│   │   │   │   ├── League.kt
│   │   │   │   ├── Favorite.kt
│   │   │   │   ├── ApiResponse.kt
│   │   │   │   └── ErrorResponse.kt
│   │   │   └── repository/
│   │   │       ├── MatchRepository.kt
│   │   │       ├── CouponRepository.kt
│   │   │       ├── PredictionRepository.kt
│   │   │       └── FavoriteRepository.kt
│   │   ├── ui/
│   │   │   ├── main/
│   │   │   │   ├── MainActivity.kt
│   │   │   │   ├── MainViewModel.kt
│   │   │   │   └── MainAdapter.kt
│   │   │   ├── matches/
│   │   │   │   ├── MatchListFragment.kt
│   │   │   │   ├── MatchListViewModel.kt
│   │   │   │   ├── MatchDetailFragment.kt
│   │   │   │   └── MatchDetailViewModel.kt
│   │   │   ├── coupons/
│   │   │   │   ├── CouponFragment.kt
│   │   │   │   ├── CouponViewModel.kt
│   │   │   │   ├── CouponGenerateFragment.kt
│   │   │   │   └── CouponLadderFragment.kt
│   │   │   ├── predictions/
│   │   │   │   ├── PredictionListFragment.kt
│   │   │   │   ├── PredictionListViewModel.kt
│   │   │   │   └── PredictionDetailFragment.kt
│   │   │   └── favorites/
│   │   │       ├── FavoritesFragment.kt
│   │   │       └── FavoritesViewModel.kt
│   │   └── util/
│   │       ├── Extensions.kt
│   │       └── DateUtils.kt
├── res/
│   ├── layout/
│   │   ├── activity_main.xml
│   │   ├── fragment_match_list.xml
│   │   ├── fragment_match_detail.xml
│   │   ├── fragment_coupon.xml
│   │   ├── item_match.xml
│   │   └── item_coupon.xml
│   ├── values/
│   │   ├── colors.xml
│   │   ├── strings.xml
│   │   └── themes.xml
│   └── drawable/
```

---

## 🔌 LES 40 APIs À IMPLÉMENTER

### 1. Matchs (7 APIs)

```kotlin
@GET("/api/matches")
suspend fun getMatches(): ApiResponse<MatchesResponse>

@GET("/api/matches/live")
suspend fun getLiveMatches(): ApiResponse<MatchesResponse>

@GET("/api/matches/upcoming")
suspend fun getUpcomingMatches(): ApiResponse<MatchesResponse>

@GET("/api/matches/finished")
suspend fun getFinishedMatches(): ApiResponse<MatchesResponse>

@GET("/api/matches/search")
suspend fun searchMatches(@Query("q") query: String): ApiResponse<MatchesResponse>

@GET("/api/matches/filter")
suspend fun filterMatches(
    @Query("league") league: String?,
    @Query("team") team: String?,
    @Query("status") status: String?,
    @Query("minOdds") minOdds: Double?,
    @Query("maxOdds") maxOdds: Double?
): ApiResponse<MatchesResponse>

@GET("/api/matches/{id}/details")
suspend fun getMatchDetails(@Path("id") matchId: String): ApiResponse<MatchDetailsResponse>
```

### 2. Paris & Cotes (1 API)

```kotlin
@GET("/api/odds/{matchId}")
suspend fun getOdds(@Path("matchId") matchId: String): ApiResponse<OddsResponse>
```

### 3. Coupons (10 APIs)

```kotlin
@POST("/api/coupon/generate")
suspend fun generateCoupon(@Body request: CouponGenerateRequest): ApiResponse<CouponResponse>

@POST("/api/coupon/validate")
suspend fun validateCoupon(@Body request: CouponValidateRequest): ApiResponse<CouponValidationResponse>

@POST("/api/coupon/ladder")
suspend fun generateLadder(@Body request: LadderRequest): ApiResponse<LadderResponse>

@POST("/api/coupon/multi")
suspend fun generateMulti(@Body request: MultiRequest): ApiResponse<MultiResponse>

@GET("/api/coupon/history")
suspend fun getCouponHistory(@Query("limit") limit: Int): ApiResponse<CouponHistoryResponse>

@GET("/api/coupon/{id}")
suspend fun getCouponById(@Path("id") couponId: String): ApiResponse<CouponResponse>

@GET("/api/coupon/stats")
suspend fun getCouponStats(): ApiResponse<CouponStatsResponse>

@POST("/api/coupon/favorite")
suspend fun addToFavorites(@Body request: FavoriteRequest): ApiResponse<FavoriteResponse>

@GET("/api/coupon/favorites")
suspend fun getFavorites(@Query("userId") userId: String): ApiResponse<FavoritesResponse>

@GET("/api/coupon/journal")
suspend fun getCouponJournal(): ApiResponse<CouponJournalResponse>
```

### 4. Équipes & Ligues (5 APIs)

```kotlin
@GET("/api/teams")
suspend fun getTeams(): ApiResponse<TeamsResponse>

@GET("/api/teams/{id}/matches")
suspend fun getTeamMatches(@Path("id") teamId: String): ApiResponse<MatchesResponse>

@GET("/api/leagues")
suspend fun getLeagues(): ApiResponse<LeaguesResponse>

@GET("/api/leagues/{id}/matches")
suspend fun getLeagueMatches(@Path("id") leagueId: String): ApiResponse<MatchesResponse>

@GET("/api/leagues/{id}/standings")
suspend fun getLeagueStandings(@Path("id") leagueId: String): ApiResponse<StandingsResponse>
```

### 5. Prédictions IA (3 APIs)

```kotlin
@GET("/api/predictions")
suspend fun getPredictions(): ApiResponse<PredictionsResponse>

@GET("/api/predictions/top")
suspend fun getTopPredictions(): ApiResponse<PredictionsResponse>

@GET("/api/predictions/{matchId}")
suspend fun getMatchPrediction(@Path("matchId") matchId: String): ApiResponse<PredictionResponse>
```

### 6. Page d'Accueil (5 APIs)

```kotlin
@GET("/api/stats/global")
suspend fun getGlobalStats(): ApiResponse<GlobalStatsResponse>

@GET("/api/watchlist")
suspend fun getWatchlist(): ApiResponse<WatchlistResponse>

@GET("/api/heatmap")
suspend fun getHeatmap(): ApiResponse<HeatmapResponse>

@GET("/api/denicheur")
suspend fun getDenicheur(@Query("full") full: Boolean?): ApiResponse<DenicheurResponse>

@GET("/api/odds/alerts")
suspend fun getOddsAlerts(): ApiResponse<OddsAlertsResponse>
```

### 7. Page Match - Détails (4 APIs)

```kotlin
@GET("/api/match/{id}/coach")
suspend fun getCoachAnalysis(@Path("id") matchId: String): ApiResponse<CoachResponse>

@GET("/api/match/{id}/kpi")
suspend fun getKPI(@Path("id") matchId: String): ApiResponse<KPIResponse>

@GET("/api/match/{id}/insight")
suspend fun getInsight(@Path("id") matchId: String): ApiResponse<InsightResponse>

@GET("/api/match/{id}/exact-score")
suspend fun getExactScore(@Path("id") matchId: String): ApiResponse<ExactScoreResponse>
```

### 8. Supplémentaires (5 APIs)

```kotlin
@GET("/api/structure")
suspend fun getStructure(): ApiResponse<StructureResponse>

@GET("/api/team-badge")
suspend fun getTeamBadge(@Query("name") teamName: String): ResponseBody

@GET("/api/logo/{fileName}")
suspend fun getLogo(@Path("fileName") fileName: String): ResponseBody

@POST("/api/chat")
suspend fun chat(@Body request: ChatRequest): ApiResponse<ChatResponse>

@GET("/api/db/status")
suspend fun getDbStatus(): ApiResponse<DbStatusResponse>
```

---

## 📦 DÉPENDANCES GRADLE

### build.gradle (niveau projet)

```gradle
buildscript {
    ext.kotlin_version = "1.9.0"
    repositories {
        google()
        mavenCentral()
    }
    dependencies {
        classpath "com.android.tools.build:gradle:8.1.0"
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
    }
}

allprojects {
    repositories {
        google()
        mavenCentral()
    }
}
```

### app/build.gradle

```gradle
plugins {
    id 'com.android.application'
    id 'org.jetbrains.kotlin.android'
    id 'kotlin-kapt'
    id 'kotlin-parcelize'
}

android {
    namespace 'com.solitairehack'
    compileSdk 34

    defaultConfig {
        applicationId "com.solitairehack"
        minSdk 24
        targetSdk 34
        versionCode 1
        versionName "1.0.0"

        buildConfigField "String", "API_BASE_URL", "\"${project.findProperty('API_BASE_URL') ?: 'https://fifaxpred.onrender.com'}\""
    }

    buildTypes {
        release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
        debug {
            debuggable true
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_17
        targetCompatibility JavaVersion.VERSION_17
    }

    kotlinOptions {
        jvmTarget = '17'
    }

    buildFeatures {
        viewBinding true
        buildConfig true
    }
}

dependencies {
    // Kotlin
    implementation "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
    implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3"
    implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:1.7.3"

    // AndroidX
    implementation 'androidx.core:core-ktx:1.12.0'
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.11.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
    implementation 'androidx.recyclerview:recyclerview:1.3.2'
    implementation 'androidx.cardview:cardview:1.0.0'
    implementation 'androidx.swiperefreshlayout:swiperefreshlayout:1.1.0'
    implementation 'androidx.lifecycle:lifecycle-viewmodel-ktx:2.7.0'
    implementation 'androidx.lifecycle:lifecycle-livedata-ktx:2.7.0'
    implementation 'androidx.fragment:fragment-ktx:1.6.2'
    implementation 'androidx.navigation:navigation-fragment-ktx:2.7.6'
    implementation 'androidx.navigation:navigation-ui-ktx:2.7.6'

    // Retrofit
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
    implementation 'com.squareup.okhttp3:okhttp:4.12.0'
    implementation 'com.squareup.okhttp3:logging-interceptor:4.12.0'

    // Gson
    implementation 'com.google.code.gson:gson:2.10.1'

    // Glide pour les images
    implementation 'com.github.bumptech.glide:glide:4.16.0'
    kapt 'com.github.bumptech.glide:compiler:4.16.0'

    // Coil pour les images (alternative moderne)
    implementation 'io.coil-kt:coil:2.5.0'

    // Shimmer pour le chargement
    implementation 'com.facebook.shimmer:shimmer:0.5.0'
}
```

---

## 🎨 UI/UX RECOMMANDATIONS

### Design System
- Couleurs principales: #1a1a2e (dark), #16213e (primary), #0f3460 (accent)
- Couleurs secondaires: #e94560 (error), #00d4ff (success)
- Police: Roboto ou Google Sans
- Card Material Design avec elevation
- Bottom Navigation pour navigation principale

### Écrans Principaux
1. **Home** - Stats globales, watchlist, heatmap, denicheur, alerts
2. **Matches** - Liste des matchs (live, upcoming, finished)
3. **Match Detail** - Détails, coach IA, KPI, insights, score exact
4. **Coupons** - Génération, validation, ladder, multi, historique, favoris
5. **Predictions** - Liste des prédictions, top predictions
6. **Settings** - Configuration de l'application

### Animations
- Transition entre écrans avec shared elements
- Shimmer effect pendant le chargement
- Swipe-to-refresh pour les listes
- Ripple effect sur les boutons

---

## 💻 EXEMPLES DE CODE KOTLIN

### ApiService.kt

```kotlin
package com.solitairehack.data.api

import retrofit2.Response
import retrofit2.http.*

interface ApiService {
    // Matchs
    @GET("/api/matches")
    suspend fun getMatches(): Response<ApiResponse<MatchesData>>

    @GET("/api/matches/live")
    suspend fun getLiveMatches(): Response<ApiResponse<MatchesData>>

    @GET("/api/matches/{id}/details")
    suspend fun getMatchDetails(@Path("id") matchId: String): Response<ApiResponse<MatchDetails>>

    // Coupons
    @POST("/api/coupon/generate")
    suspend fun generateCoupon(@Body request: CouponGenerateRequest): Response<ApiResponse<CouponData>>

    @POST("/api/coupon/favorite")
    suspend fun addToFavorites(@Body request: FavoriteRequest): Response<ApiResponse<FavoriteData>>

    @GET("/api/coupon/favorites")
    suspend fun getFavorites(@Query("userId") userId: String): Response<ApiResponse<FavoritesData>>

    // ... autres 40 APIs
}
```

### RetrofitClient.kt

```kotlin
package com.solitairehack.data.api

import okhttp3.OkHttpClient
import okhttp3.logging.HttpLoggingInterceptor
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory
import java.util.concurrent.TimeUnit

object RetrofitClient {
    private const val BASE_URL = BuildConfig.API_BASE_URL

    private val loggingInterceptor = HttpLoggingInterceptor().apply {
        level = HttpLoggingInterceptor.Level.BODY
    }

    private val okHttpClient = OkHttpClient.Builder()
        .addInterceptor(loggingInterceptor)
        .connectTimeout(30, TimeUnit.SECONDS)
        .readTimeout(30, TimeUnit.SECONDS)
        .writeTimeout(30, TimeUnit.SECONDS)
        .build()

    private val retrofit = Retrofit.Builder()
        .baseUrl(BASE_URL)
        .client(okHttpClient)
        .addConverterFactory(GsonConverterFactory.create())
        .build()

    val apiService: ApiService = retrofit.create(ApiService::class.java)
}
```

### ApiResponse.kt

```kotlin
package com.solitairehack.data.model

data class ApiResponse<T>(
    val success: Boolean,
    val data: T? = null,
    val meta: Meta? = null,
    val error: ErrorResponse? = null
)

data class Meta(
    val timestamp: String?,
    val source: String? = null
)

data class ErrorResponse(
    val code: String,
    val message: String,
    val details: String? = null
)
```

### Match.kt

```kotlin
package com.solitairehack.data.model

import android.os.Parcelable
import kotlinx.parcelize.Parcelize

@Parcelize
data class Match(
    val id: String,
    val homeTeam: String,
    val awayTeam: String,
    val league: String,
    val status: String,
    val score: Score?,
    val startTime: String,
    val odds: Double?
) : Parcelable

@Parcelize
data class Score(
    val home: Int?,
    val away: Int?
) : Parcelable
```

### MatchRepository.kt

```kotlin
package com.solitairehack.data.repository

import com.solitairehack.data.api.RetrofitClient
import com.solitairehack.data.model.Match

class MatchRepository {
    private val apiService = RetrofitClient.apiService

    suspend fun getMatches(): Result<List<Match>> {
        return try {
            val response = apiService.getMatches()
            if (response.isSuccessful && response.body()?.success == true) {
                Result.success(response.body()?.data?.matches ?: emptyList())
            } else {
                Result.failure(Exception(response.body()?.error?.message ?: "Unknown error"))
            }
        } catch (e: Exception) {
            Result.failure(e)
        }
    }

    suspend fun getLiveMatches(): Result<List<Match>> {
        return try {
            val response = apiService.getLiveMatches()
            if (response.isSuccessful && response.body()?.success == true) {
                Result.success(response.body()?.data?.matches ?: emptyList())
            } else {
                Result.failure(Exception(response.body()?.error?.message ?: "Unknown error"))
            }
        } catch (e: Exception) {
            Result.failure(e)
        }
    }

    // ... autres méthodes
}
```

### MatchListViewModel.kt

```kotlin
package com.solitairehack.ui.matches

import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import com.solitairehack.data.model.Match
import com.solitairehack.data.repository.MatchRepository
import kotlinx.coroutines.launch

class MatchListViewModel : ViewModel() {
    private val repository = MatchRepository()

    private val _matches = MutableLiveData<List<Match>>()
    val matches: LiveData<List<Match>> = _matches

    private val _isLoading = MutableLiveData<Boolean>()
    val isLoading: LiveData<Boolean> = _isLoading

    private val _error = MutableLiveData<String?>()
    val error: LiveData<String?> = _error

    fun loadMatches() {
        viewModelScope.launch {
            _isLoading.value = true
            repository.getMatches()
                .onSuccess { _matches.value = it }
                .onFailure { _error.value = it.message }
            _isLoading.value = false
        }
    }

    fun loadLiveMatches() {
        viewModelScope.launch {
            _isLoading.value = true
            repository.getLiveMatches()
                .onSuccess { _matches.value = it }
                .onFailure { _error.value = it.message }
            _isLoading.value = false
        }
    }
}
```

### MainActivity.kt

```kotlin
package com.solitairehack.ui.main

import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import androidx.navigation.fragment.NavHostFragment
import androidx.navigation.ui.setupWithNavController
import com.google.android.material.bottomnavigation.BottomNavigationView
import com.solitairehack.R

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val navHostFragment = supportFragmentManager
            .findFragmentById(R.id.nav_host_fragment) as NavHostFragment
        val navController = navHostFragment.navController

        val bottomNav = findViewById<BottomNavigationView>(R.id.bottom_navigation)
        bottomNav.setupWithNavController(navController)
    }
}
```

---

## 🔧 FICHIERS DE CONFIGURATION

### local.properties.example

```properties
# API Configuration
API_BASE_URL=https://fifaxpred.onrender.com

# Debug Configuration
DEBUG_MODE=true
LOG_LEVEL=FULL
```

### AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.SolitaireHack"
        android:usesCleartextTraffic="false"
        tools:targetApi="31">
        
        <activity
            android:name=".ui.main.MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

---

## ✅ CHECKLIST DE DÉVELOPPEMENT

1. **Configuration du projet**
   - [ ] Créer le projet Android avec Kotlin
   - [ ] Configurer Gradle avec les dépendances
   - [ ] Configurer BuildConfig pour l'URL API
   - [ ] Configurer AndroidManifest avec les permissions

2. **Architecture**
   - [ ] Créer les packages (data/api, data/model, data/repository, ui/*)
   - [ ] Configurer RetrofitClient
   - [ ] Créer ApiService avec les 40 endpoints
   - [ ] Créer les modèles de données (DTOs)

3. **Repositories**
   - [ ] MatchRepository
   - [ ] CouponRepository
   - [ ] PredictionRepository
   - [ ] FavoriteRepository

4. **ViewModels**
   - [ ] MainViewModel
   - [ ] MatchListViewModel
   - [ ] MatchDetailViewModel
   - [ ] CouponViewModel
   - [ ] PredictionViewModel
   - [ ] FavoritesViewModel

5. **UI Fragments**
   - [ ] MatchListFragment avec RecyclerView
   - [ ] MatchDetailFragment
   - [ ] CouponFragment
   - [ ] CouponGenerateFragment
   - [ ] PredictionListFragment
   - [ ] FavoritesFragment

6. **Navigation**
   - [ ] Configurer Navigation Graph
   - [ ] Bottom Navigation
   - [ ] Transitions entre écrans

7. **Tests**
   - [ ] Tests unitaires des repositories
   - [ ] Tests unitaires des ViewModels
   - [ ] Tests d'intégration API

---

## 🎯 PRIORITÉS DE DÉVELOPPEMENT

1. **Phase 1: Infrastructure**
   - Configuration du projet
   - Setup Retrofit
   - Création des modèles de données

2. **Phase 2: APIs Core**
   - Implémentation des APIs matchs
   - Implémentation des APIs coupons
   - Implémentation des APIs prédictions

3. **Phase 3: UI Core**
   - Écran principal avec navigation
   - Liste des matchs
   - Détails d'un match

4. **Phase 4: Features Avancées**
   - Génération de coupons
   - Ladder et multi-stratégie
   - Favoris
   - Coach IA et KPI

5. **Phase 5: Polish**
   - Animations
   - Gestion des erreurs
   - Tests
   - Optimisation

---

## 📝 NOTES IMPORTANTES

1. **Gestion des erreurs:** Utiliser `Result<T>` pour gérer les erreurs de manière élégante
2. **Coroutines:** Utiliser `viewModelScope` pour les coroutines dans les ViewModels
3. **LiveData:** Utiliser LiveData pour observer les changements de données
4. **ViewBinding:** Activer ViewBinding pour accéder aux vues de manière type-safe
5. **Navigation:** Utiliser Navigation Component pour la navigation entre écrans
6. **Images:** Utiliser Coil pour charger les images (logos, badges)
7. **Thème:** Utiliser Material Design 3 avec thème sombre par défaut

---

## 🚀 COMMANDES DE BUILD

```bash
# Debug build
./gradlew assembleDebug

# Release build
./gradlew assembleRelease

# Install sur device
./gradlew installDebug

# Run tests
./gradlew test
```

---

## 📚 DOCUMENTATION SUPPLÉMENTAIRE

- API Reference: `API_REFERENCE.md`
- Complete Setup: `COMPLETE_ANDROID_SETUP.md`
- Backend Changes: `../oracpr-main/BACKEND_MOBILE_API_CHANGES.md`

---

**Crée cette application Android complète avec toutes les fonctionnalités décrites. L'application doit être professionnelle, moderne et offrir une excellente expérience utilisateur.**

---

**Signé:** SOLITAIRE HACK
