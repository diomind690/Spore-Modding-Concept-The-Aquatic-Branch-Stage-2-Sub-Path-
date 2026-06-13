# Spore-Modding-Concept-The-Aquatic-Branch-Stage-2-Sub-Path-
ENG:A technical concept and framework for implementing a 3D Aquatic Stage in Spore using engine exploits, custom physics overrides, and vertical biome progression.

RUS:Технический концепт подводного этапа «Существо» для Spore. Реализация через костыли движка, переделку физики полета под плавание и разделение биомов по глубине.

---

## 🌍 Language / Язык

*   [English Version](#english-version)
*   [Русская версия](#русская-версия)

---

## English Version

This project outlines a smart method to implement the cut Aquatic Stage by manipulating how the *Spore* engine perceives water levels, allowing players to choose between staying in the ocean or moving to land based on their creature's anatomy.

### 🌊 Core Mechanics & Logic

#### 1. The Fixed Sea Level Trick
*   **The Concept:** The actual, visible water level is turned off or set to a point where there is no physical water. On the screen, it looks like a normal beach and dry land.
*   **The Illusion:** However, the engine flag for the sea level is set as fixed and unchanged. The game technically still thinks that everything below a certain height is "underwater."
*   **Environment & Custom Fauna:** Nests and terrain generate normally on this dry beach. To populate the world, the properties of specific fauna are modified in the game files—changing their type from `gaprop` to `underwater`. This forces the engine to automatically spawn specialized marine life across the dry ecosystem. Custom underwater VFX (bubbles, filters, lighting) are added to complete the atmosphere.

#### 🧬 2. The Anatomy-Based Choice
The transition from the Cell Stage reads the player's choices in the editor:
*   **The 3D Realization:** The cell looks around, realizes the world is 3D, and enters the editor.
*   **Staying Underwater:** If the player adds gills and fins, the creature decides to stay below the fixed water line because it finds this world interesting. It utilizes modified wing levitation to swim and hover smoothly without falling.
*   **Going to Land:** If the player gives the creature legs, a standard nose, and a mouth, it attempts to stay but realizes it is suffocating under the fixed engine water line. It panics and runs/swims up to the surface, triggering the standard vanilla cutscene of crawling onto dry land.

---

## Русская версия

Концепция реализации полноценного подводного подэтапа для этапа «Существо» в *Spore*. Реализация строится на фиксации уровня моря в движке, кастомных эффектах и выборе пути развития на основе анатомии существа.

### 🌊 Ключевая логика и механика

#### 1. Костыль с фиксированным уровнем моря
*   **Суть идеи:** Физическая вода убирается, то есть визуально перед нами обычный сухой пляж и суша без воды.
*   **Обман движка:** При этом уровень моря в коде помечается как неизменный и фиксированный. Игра технически продолжает думать, что ниже этой линии находится вода.
*   **Окружение и кастомная фауна:** На этом сухом пляже стандартно генерируется ландшафт и гнёзда. Чтобы разнообразить мир, в файлах игры изменяются свойства некоторой флоры — их тег меняется с обычного `gaprop` на `underwater`. Это заставляет движок автоматически спавнить на «сухом дне» только специализированную морскую флору. Атмосфера океана дорабатывается кастомными эффектами (пузыри, фильтры света).

#### 🧬 2. Выбор пути на основе редактора
Переход из этапа «Клетка» завязан на логику и строение тела существа:
*   **Осознание 3D:** Клетка плавает, оглядывается, понимает, что мир не ограничен двумя измерениями, и уходит в 3D (в редактор простейших).
*   **Морской путь:** Если игрок добавляет существу жабры и плавники — оно решает остаться под водой, так как этот мир ему интересен. Плавание реализуется через переделанную левитацию крыльев: существо плавно парит в воздухе над сухим пляжем и не падает.
*   **Сухопутный путь:** Если игрок даёт существу ноги, нормальный нос и рот — запускается сцена, где оно понимает, что задыхается под этой невидимой технической толщей воды. Существо паникует, выплывает на поверхность, где срабатывает стандартная бесшовная катсцена выхода на сушу.

---

## 🛠️ Project Status / Статус проекта

> ⚠️ **Status:** **Conceptual Framework / Technical Design Document (Design Doc)**
> Я не являюсь программистом или 3D-моделлером. Это логический концепт и ТЗ. Если вы моддер (SporeModder FX, Spore API) и хотите реализовать эту идею — пишите!

## 📬 Contacts / Контакты

*   **Discord:** `ВАШ_ТЕГ_ДИСКОРДА`
