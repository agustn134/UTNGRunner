# 🎮 UTNG Runner

**Guía Completa de Desarrollo para Wear OS**  
*Arquitectura Limpia · Compose for Wear OS · Buenas Prácticas*  
*Desarrollo para Dispositivos Inteligentes · UTNG 2025*

---

### 👨‍💻 Datos del Alumno
* **Nombre:** Agustin Lopez Parra
* **Grupo:** GIDS6093
* **Repositorio:** [github.com/[usuario]/UTNGRunner](https://github.com/[usuario]/UTNGRunner) *(Recuerda actualizar este enlace con tu usuario real)*

---

Este proyecto implementa el juego **UTNG Runner** directamente en un reloj Wear OS usando *Compose for Wear OS*, arquitectura MVVM limpia, principio de responsabilidad única y separación de capas. Al terminar, tendrás una app publicable en Google Play.

## 1. Visión General del Proyecto

**UTNG Runner** es un juego *endless runner* que corre nativamente en la pantalla circular del reloj inteligente. El ingeniero (personaje) corre por el campus de la UTNG esquivando obstáculos (tareas, exámenes, bugs) y recogiendo créditos académicos.

### 1.1 Stack Tecnológico

| Componente | Tecnología |
| :--- | :--- |
| **Lenguaje** | Kotlin 1.9+ |
| **UI Framework** | Compose for Wear OS (`androidx.wear.compose`) |
| **Arquitectura** | MVVM + Clean Architecture (3 capas) |
| **Motor de juego** | Canvas + coroutines (loop a 60 fps) |
| **Estado** | StateFlow + `rememberStateOf` |
| **Persistencia** | DataStore Preferences (best score) |
| **Sensor de FC** | Health Services API (`PassiveMonitoringClient`) |
| **Haptics** | WearableHapticFeedback |
| **Testing** | JUnit 4 + coroutinesTest |
| **Build** | Gradle KTS · minSdk 30 (Wear OS 3.0) |

### 1.2 Arquitectura de Capas

> **Regla de oro de arquitectura limpia:** Las capas internas NO conocen a las capas externas. `Domain` no importa nada de `Data` ni de `UI`. `Data` no importa nada de `UI`.

| Capa | Paquete | Responsabilidad |
| :--- | :--- | :--- |
| **Presentación** | `utngrunner.presentation` | Composables, ViewModel, Estado de UI. No tiene lógica de negocio. |
| **Dominio** | `utngrunner.domain` | Entidades del juego, casos de uso, interfaces de repositorio. Kotlin puro, sin Android SDK. |
| **Datos** | `utngrunner.data` | Implementaciones de repositorio, DataStore, Health Services. Conoce Android SDK. |

---

## 2. Estructura del Proyecto

A continuación, se detalla la organización de los directorios y archivos siguiendo la arquitectura limpia:

```text
UTNGRunner/
├── wear/
│   ├── build.gradle.kts
│   └── src/main/
│       ├── AndroidManifest.xml
│       └── java/mx/utng/utngrunner/
│           ├── domain/
│           │   ├── model/
│           │   │   ├── GameState.kt     ← Estado del juego
│           │   │   ├── Player.kt        ← Entidad jugador
│           │   │   ├── Obstacle.kt      ← Entidad obstáculo
│           │   │   └── Coin.kt          ← Entidad moneda
│           │   ├── usecase/
│           │   │   ├── GetHighScoreUseCase.kt
│           │   │   └── SaveHighScoreUseCase.kt
│           │   └── repository/
│           │       └── ScoreRepository.kt  ← Interfaz
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
