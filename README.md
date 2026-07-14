# UTNGRunner
Este proyecto implementa el juego UTNG Runner directamente en un reloj Wear OS usando Compose for Wear OS, arquitectura MVVM limpia, principio de responsabilidad única y separación de capas. Al terminar tendrás una app publicable en Google Play.
1.1 Stack tecnológico

Componente
Tecnología
Lenguaje
Kotlin 1.9+
UI Framework
Compose for Wear OS (androidx.wear.compose)
Arquitectura
MVVM + Clean Architecture (3 capas)
Motor de juego
Canvas + coroutines (loop a 60 fps)
Estado
StateFlow + rememberStateOf
Persistencia
DataStore Preferences (best score)
Sensor de FC
Health Services API (PassiveMonitoringClient)
Haptics
WearableHapticFeedback
Testing
JUnit 4 + coroutinesTest
Build
Gradle KTS · minSdk 30 (Wear OS 3.0)

Arquitectura de Capas
Regla de oro de arquitectura limpia: las capas internas NO conocen a las capas externas. Domain no importa nada de Data ni de UI. Data no importa nada de UI.


Capa
Paquete
Responsabilidad
Presentación
utngrunner.presentation
Composables, ViewModel, Estado de UI. No tiene lógica de negocio.
Dominio
utngrunner.domain
Entidades del juego, casos de uso, interfaces de repositorio. Kotlin puro, sin Android.
Datos
utngrunner.data
Implementaciones de repositorio, DataStore, Health Services. Conoce Android SDK.


2. Estructura del Proyecto
UTNGRunner/
├── wear/
│   ├── build.gradle.kts
│   └── src/main/
│       ├── AndroidManifest.xml
│       └── java/mx/utng/utngrunner/
│           ├── domain/
│           │   ├── model/
│           │   │   ├── GameState.kt   ← Estado del juego
│           │   │   ├── Player.kt      ← Entidad jugador
│           │   │   ├── Obstacle.kt    ← Entidad obstáculo
│           │   │   └── Coin.kt        ← Entidad moneda
│           │   ├── usecase/
│           │   │   ├── GetHighScoreUseCase.kt
│           │   │   └── SaveHighScoreUseCase.kt
│           │   └── repository/
│           │       └── ScoreRepository.kt  ← interfaz
│           ├── data/
│           │   ├── repository/
│           │   │   └── ScoreRepositoryImpl.kt
│           │   └── datasource/
│           │       └── PreferencesDataSource.kt
│           └── presentation/
│               ├── game/
│               │   ├── GameViewModel.kt    ← MVVM
│               │   ├── GameScreen.kt       ← Composable
│               │   ├── GameEngine.kt       ← Motor 60fps
│               │   └── GameRenderer.kt     ← Canvas
│               ├── menu/
│               │   └── MenuScreen.kt
│               └── GameActivity.kt

