# Wellness Tracker iOS App - Architecture Proposal

## Overview
Transform your current iOS Shortcuts + Claude API + Google Sheets workflow into a native iOS app with deep Apple Health integration.

## Core Features

### 1. **Nutrition Tracking**
- **Manual Food Logging**: Photo capture, voice input, or text entry
- **Claude API Integration**: Automatic macro calculation from food descriptions
- **Barcode Scanner**: Quick logging of packaged foods
- **Apple Health Sync**: Write nutrition data to HealthKit
- **Meal Templates**: Save and reuse common meals
- **Daily Macro Goals**: Customizable targets with visual progress

### 2. **Sleep Monitoring**
- **Apple Health Integration**: Read sleep data from Apple Watch/iPhone
- **Sleep Quality Rating**: Manual 1-10 rating each morning
- **Sleep Notes**: Track factors affecting sleep (caffeine, stress, etc.)
- **Trends Analysis**: Weekly/monthly sleep pattern insights
- **Sleep Goals**: Target hours with consistency tracking

### 3. **Exercise Tracking**
- **Apple Health Integration**: Pull workouts from Health app
- **Manual Entry**: For activities not tracked automatically
- **Workout Categories**: Strength, cardio, flexibility, sports
- **Weekly Activity Goals**: Minutes, calories, or custom metrics
- **Rest Day Tracking**: Intentional recovery monitoring

### 4. **Purposefulness Logging**
- **Daily Reflection**: End-of-day rating (1-10)
- **Goal Tracking**: Link daily activities to larger goals
- **Win of the Day**: Capture one meaningful accomplishment
- **Time Allocation**: Track time spent on purposeful activities
- **Weekly Review**: Reflection prompts and insights

### 5. **Relationship Tracking**
- **Quality Time Log**: Track meaningful interactions
- **Relationship Categories**: Partner, family, friends, professional
- **Interaction Quality**: Rating system for connection depth
- **Gratitude Journal**: What you appreciated in relationships
- **Balance View**: Visual representation of relationship investment

---

## Technical Architecture

### **Architecture Pattern: MVVM + Clean Architecture**

```
┌─────────────────────────────────────────────┐
│              Presentation Layer              │
│   ├── Views (SwiftUI)                       │
│   ├── ViewModels                            │
│   └── Coordinators (Navigation)             │
└─────────────────────────────────────────────┘
                     ↓
┌─────────────────────────────────────────────┐
│              Domain Layer                    │
│   ├── Use Cases / Interactors               │
│   ├── Entities (Models)                     │
│   └── Repository Protocols                  │
└─────────────────────────────────────────────┘
                     ↓
┌─────────────────────────────────────────────┐
│              Data Layer                      │
│   ├── Repositories (Implementations)        │
│   ├── Local Storage (Core Data)             │
│   ├── Remote APIs (Claude, Google Sheets)   │
│   └── HealthKit Manager                     │
└─────────────────────────────────────────────┘
```

---

## Project Structure

```
WellnessTracker/
├── App/
│   ├── WellnessTrackerApp.swift
│   ├── AppDelegate.swift
│   └── Info.plist
│
├── Core/
│   ├── Domain/
│   │   ├── Entities/
│   │   │   ├── NutritionEntry.swift
│   │   │   ├── SleepEntry.swift
│   │   │   ├── ExerciseEntry.swift
│   │   │   ├── PurposeEntry.swift
│   │   │   └── RelationshipEntry.swift
│   │   ├── UseCases/
│   │   │   ├── Nutrition/
│   │   │   │   ├── LogFoodUseCase.swift
│   │   │   │   ├── CalculateMacrosUseCase.swift
│   │   │   │   └── GetDailyNutritionUseCase.swift
│   │   │   ├── Sleep/
│   │   │   │   ├── SyncSleepDataUseCase.swift
│   │   │   │   └── LogSleepQualityUseCase.swift
│   │   │   ├── Exercise/
│   │   │   │   ├── SyncWorkoutsUseCase.swift
│   │   │   │   └── LogWorkoutUseCase.swift
│   │   │   ├── Purpose/
│   │   │   │   └── LogPurposeEntryUseCase.swift
│   │   │   └── Relationship/
│   │   │       └── LogRelationshipEntryUseCase.swift
│   │   └── Repositories/
│   │       ├── NutritionRepositoryProtocol.swift
│   │       ├── SleepRepositoryProtocol.swift
│   │       ├── ExerciseRepositoryProtocol.swift
│   │       ├── PurposeRepositoryProtocol.swift
│   │       └── RelationshipRepositoryProtocol.swift
│   │
│   ├── Data/
│   │   ├── Repositories/
│   │   │   ├── NutritionRepository.swift
│   │   │   ├── SleepRepository.swift
│   │   │   ├── ExerciseRepository.swift
│   │   │   ├── PurposeRepository.swift
│   │   │   └── RelationshipRepository.swift
│   │   ├── Local/
│   │   │   ├── CoreData/
│   │   │   │   ├── WellnessTracker.xcdatamodeld
│   │   │   │   ├── CoreDataStack.swift
│   │   │   │   └── Entities/
│   │   │   │       ├── NutritionEntryEntity+CoreDataClass.swift
│   │   │   │       ├── SleepEntryEntity+CoreDataClass.swift
│   │   │   │       └── ...
│   │   │   └── UserDefaults/
│   │   │       └── UserPreferences.swift
│   │   ├── Remote/
│   │   │   ├── Claude/
│   │   │   │   ├── ClaudeAPIClient.swift
│   │   │   │   ├── MacroCalculationService.swift
│   │   │   │   └── Models/
│   │   │   │       ├── ClaudeRequest.swift
│   │   │   │       └── ClaudeResponse.swift
│   │   │   └── GoogleSheets/
│   │   │       ├── GoogleSheetsClient.swift
│   │   │       └── SheetsExportService.swift
│   │   └── HealthKit/
│   │       ├── HealthKitManager.swift
│   │       ├── HealthKitReader.swift
│   │       ├── HealthKitWriter.swift
│   │       └── Models/
│   │           ├── HealthKitSleepData.swift
│   │           └── HealthKitWorkoutData.swift
│   │
│   └── Utilities/
│       ├── Extensions/
│       │   ├── Date+Extensions.swift
│       │   ├── Double+Extensions.swift
│       │   └── View+Extensions.swift
│       ├── Helpers/
│       │   ├── DateHelper.swift
│       │   └── MacroCalculator.swift
│       └── Constants/
│           └── AppConstants.swift
│
├── Features/
│   ├── Dashboard/
│   │   ├── Views/
│   │   │   ├── DashboardView.swift
│   │   │   ├── DailyOverviewCard.swift
│   │   │   └── WeeklySummaryCard.swift
│   │   └── ViewModels/
│   │       └── DashboardViewModel.swift
│   │
│   ├── Nutrition/
│   │   ├── Views/
│   │   │   ├── NutritionHomeView.swift
│   │   │   ├── AddFoodView.swift
│   │   │   ├── FoodDetailView.swift
│   │   │   ├── MacroProgressView.swift
│   │   │   └── Components/
│   │   │       ├── MacroRingView.swift
│   │   │       └── FoodItemRow.swift
│   │   └── ViewModels/
│   │       ├── NutritionViewModel.swift
│   │       └── AddFoodViewModel.swift
│   │
│   ├── Sleep/
│   │   ├── Views/
│   │   │   ├── SleepHomeView.swift
│   │   │   ├── SleepQualityInputView.swift
│   │   │   ├── SleepTrendsView.swift
│   │   │   └── Components/
│   │   │       └── SleepChartView.swift
│   │   └── ViewModels/
│   │       └── SleepViewModel.swift
│   │
│   ├── Exercise/
│   │   ├── Views/
│   │   │   ├── ExerciseHomeView.swift
│   │   │   ├── WorkoutDetailView.swift
│   │   │   ├── AddWorkoutView.swift
│   │   │   └── Components/
│   │   │       └── WorkoutListItem.swift
│   │   └── ViewModels/
│   │       └── ExerciseViewModel.swift
│   │
│   ├── Purpose/
│   │   ├── Views/
│   │   │   ├── PurposeHomeView.swift
│   │   │   ├── DailyReflectionView.swift
│   │   │   ├── GoalTrackingView.swift
│   │   │   └── Components/
│   │   │       └── PurposeRatingView.swift
│   │   └── ViewModels/
│   │       └── PurposeViewModel.swift
│   │
│   ├── Relationship/
│   │   ├── Views/
│   │   │   ├── RelationshipHomeView.swift
│   │   │   ├── LogInteractionView.swift
│   │   │   ├── RelationshipBalanceView.swift
│   │   │   └── Components/
│   │   │       └── InteractionCard.swift
│   │   └── ViewModels/
│   │       └── RelationshipViewModel.swift
│   │
│   ├── Insights/
│   │   ├── Views/
│   │   │   ├── InsightsView.swift
│   │   │   ├── TrendsView.swift
│   │   │   └── CorrelationsView.swift
│   │   └── ViewModels/
│   │       └── InsightsViewModel.swift
│   │
│   └── Settings/
│       ├── Views/
│       │   ├── SettingsView.swift
│       │   ├── GoalsConfigView.swift
│       │   ├── APIConfigView.swift
│       │   └── HealthKitPermissionsView.swift
│       └── ViewModels/
│           └── SettingsViewModel.swift
│
├── Resources/
│   ├── Assets.xcassets/
│   ├── Localizable.strings
│   └── Fonts/
│
└── Tests/
    ├── UnitTests/
    │   ├── UseCases/
    │   ├── ViewModels/
    │   └── Repositories/
    └── UITests/
```

---

## Key Technical Components

### **1. HealthKit Integration**

```swift
// HealthKitManager.swift
class HealthKitManager {
    static let shared = HealthKitManager()
    private let healthStore = HKHealthStore()

    // Types to read
    private let readTypes: Set<HKObjectType> = [
        HKObjectType.categoryType(forIdentifier: .sleepAnalysis)!,
        HKObjectType.workoutType(),
        HKObjectType.quantityType(forIdentifier: .activeEnergyBurned)!,
        HKObjectType.quantityType(forIdentifier: .dietaryEnergyConsumed)!,
        // ... more types
    ]

    // Types to write
    private let writeTypes: Set<HKSampleType> = [
        HKObjectType.quantityType(forIdentifier: .dietaryProtein)!,
        HKObjectType.quantityType(forIdentifier: .dietaryCarbohydrates)!,
        HKObjectType.quantityType(forIdentifier: .dietaryFatTotal)!,
        // ... more types
    ]

    func requestAuthorization() async throws -> Bool
    func fetchSleepData(from: Date, to: Date) async throws -> [HKCategorySample]
    func fetchWorkouts(from: Date, to: Date) async throws -> [HKWorkout]
    func writeMacros(protein: Double, carbs: Double, fat: Double, date: Date) async throws
}
```

### **2. Claude API Integration**

```swift
// ClaudeAPIClient.swift
class ClaudeAPIClient {
    func calculateMacros(foodDescription: String) async throws -> MacroNutrients {
        let prompt = """
        Analyze this food and provide nutritional information:
        \(foodDescription)

        Return JSON: {"protein": g, "carbs": g, "fat": g, "calories": kcal}
        """

        // Call Claude API
        // Parse response
        // Return MacroNutrients
    }
}

struct MacroNutrients: Codable {
    let protein: Double
    let carbs: Double
    let fat: Double
    let calories: Double
}
```

### **3. Core Data Schema**

**Entities:**
- `NutritionEntryEntity`: timestamp, foodName, protein, carbs, fat, calories, notes
- `SleepEntryEntity`: date, duration, qualityRating, notes, healthKitId
- `ExerciseEntryEntity`: date, type, duration, calories, notes, healthKitId
- `PurposeEntryEntity`: date, rating, winOfTheDay, goals, timeAllocation, notes
- `RelationshipEntryEntity`: date, personCategory, quality, duration, gratitude, notes

### **4. Navigation Structure**

```
TabView
├── Dashboard (Home)
├── Nutrition
├── Sleep
├── Exercise
├── Purpose
└── Settings

Bottom Sheet Modals:
├── Quick Add Food
├── Daily Reflection
└── Log Interaction
```

---

## Apple Health Integration Points

### **Read From HealthKit:**
- Sleep data (in bed, asleep, REM, deep sleep)
- Workouts (type, duration, calories, heart rate)
- Active energy burned
- Steps
- Heart rate variability (for stress/recovery insights)
- Mindful minutes (if tracked)

### **Write To HealthKit:**
- Dietary macros (protein, carbs, fat)
- Dietary calories
- Dietary fiber
- Water intake
- Custom nutrition entries

### **Correlations & Insights:**
- Sleep quality vs. exercise
- Nutrition vs. energy levels
- Purpose rating vs. workout consistency
- Relationship quality vs. sleep quality

---

## Data Persistence Strategy

### **Local First:**
- Core Data as primary storage
- Background sync to Google Sheets (optional export)
- iCloud sync for multi-device support (optional)

### **Data Flow:**
1. User input → Core Data (immediate save)
2. HealthKit data → Core Data (periodic sync)
3. Claude API → Enriched data → Core Data
4. Core Data → HealthKit (write macro data)
5. Core Data → Google Sheets (backup/export)

---

## Tech Stack Recommendations

### **UI Framework:**
- **SwiftUI**: Modern, declarative, great for rapid development
- **Charts Framework**: Native iOS 16+ charts for visualizations

### **Storage:**
- **Core Data**: Robust, proven, built-in
- **CloudKit**: Optional sync between devices

### **Networking:**
- **URLSession**: Native, async/await support
- **Codable**: JSON parsing

### **Dependencies (Minimal):**
- **KeychainSwift**: Secure API key storage
- **GoogleSignIn**: For Google Sheets integration (if needed)

---

## MVP Features (Phase 1)

**Week 1-2: Foundation**
- Project setup with Clean Architecture
- Core Data models
- HealthKit integration (read sleep & workouts)
- Basic SwiftUI navigation

**Week 3-4: Core Features**
- Nutrition logging with Claude API integration
- Sleep quality rating with HealthKit data display
- Exercise list from HealthKit
- Purpose daily reflection (simple 1-10 rating)
- Relationship logging (basic form)

**Week 5-6: Dashboard & Polish**
- Dashboard with today's summary
- Basic charts for each category
- Settings for API keys and goals
- Data export to Google Sheets

---

## Future Enhancements (Phase 2+)

- **AI Insights**: Use Claude to analyze patterns and provide personalized recommendations
- **Widgets**: Home screen widgets for quick logging
- **Siri Shortcuts**: Voice logging integration
- **Photo Food Logging**: Use Vision API + Claude to analyze food photos
- **Social Features**: Share wins with accountability partners
- **Habit Streaks**: Gamification for consistency
- **Apple Watch App**: Quick logging from watch
- **Notifications**: Smart reminders based on patterns

---

## Implementation Priority

1. **HealthKit + Core Data Foundation** (Must have)
2. **Nutrition with Claude API** (Must have - already working in Shortcuts)
3. **Dashboard View** (Must have - visibility)
4. **Sleep & Exercise Views** (Should have)
5. **Purpose & Relationship** (Should have)
6. **Insights & Trends** (Nice to have)
7. **Google Sheets Export** (Nice to have - migration from current system)

---

## Security & Privacy

- **API Keys**: Store in Keychain (never in UserDefaults or code)
- **HealthKit**: Request minimal permissions, clear privacy descriptions
- **Local First**: All data stored locally, user controls export
- **No Analytics**: Privacy-focused, no third-party tracking
- **Optional Sync**: User chooses if/when to export to Google Sheets

---

## Development Environment

- **Xcode 15+**
- **iOS 17+ target** (for latest SwiftUI and Charts features)
- **Swift 5.9+**
- **Git** for version control
- **TestFlight** for beta testing

---

## Next Steps

1. **Review & Refine**: Discuss any adjustments to the architecture
2. **Setup Project**: Initialize Xcode project with folder structure
3. **HealthKit Permissions**: Set up Info.plist with privacy descriptions
4. **Core Data Models**: Define schema
5. **Build MVP**: Start with foundation, add features incrementally

---

Would you like me to start implementing any specific component of this architecture?
