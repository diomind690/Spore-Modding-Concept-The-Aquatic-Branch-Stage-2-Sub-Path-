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
*   **The Illusion:** However, the engine flag for the sea level is set as fixed and unchanged. The game technically still thinks that everything below a certain height is underwater.
*   **Environment and Flora** Nests and terrain are generated normally on this dry beach. To add variety to the world, the properties of certain plants are modified in the game files—their type is changed from 'gaprop' to 'underwater'. This causes the engine to automatically place specialized plants in the arid ecosystem. Custom underwater visual effects (bubbles, filters, lighting) have been added to create atmosphere. This also applies to creatures; the game should automatically prefix creatures with 'underwater' if they have gills and fins and no legs (though amphibians may be implemented later).

### Primary Implementation Strategy: Physics Bypass (Method B)
To ensure complete compatibility with Spore's vanilla landscape rendering and automatic shore coloring, the mod will NOT move or delete the physical water plane (`cOcean` / `cWaterModel`). 

Instead, we will use Spore ModAPI (C++) to:
1. **Hook the Physics Loop:** Intercept the `HasFloated` / `IsSwimming` checks for the player creature and the camera.
2. **Force Ground Physics Below Sea Level:** Force the engine to return `false` for swimming checks, allowing the creature to ignore the water surface and walk directly onto the dry ocean bed.
3. **Trigger Visual Filters:** Activate a custom full-screen screen shader (tint, fog, particles) and spawn underwater props (by changing flags to `underwater`) dynamically based on the creature's Z-coordinate (when $Z \leq Z_{water}$).
4. **Activate 3D Swimming:** Utilize modified wing levitation physics to handle vertical movement (floating/diving) seamlessly within the visual water volume.


#### 🧬 2. The Anatomy-Based Choice
The transition from the Cell Stage reads the player's choices in the editor:
*   **The 3D Realization:** The cell looks around, realizes the world is 3D, and enters the protozoan editor.
*   **Staying Underwater:** If the player adds gills and fins, the creature decides to stay below the fixed water line because it finds this world interesting. It utilizes modified wing levitation to swim and hover smoothly without falling (but within the sea level limit).
*   **Going to Land:** If the player gives the creature legs, a standard nose, and a mouth, it attempts to stay but realizes it is suffocating under the fixed engine water line. It panics and runs/swims up to the surface, triggering the standard vanilla cutscene of crawling onto dry land.

---

## Русская версия

Концепция реализации полноценного подводного подэтапа для этапа «Существо» в *Spore*. Реализация строится на фиксации уровня моря в движке, кастомных эффектах и выборе пути развития на основе анатомии существа.

### 🌊 Ключевая логика и механика

#### 1. Костыль с фиксированным уровнем моря
*   **Суть идеи:** Физическая вода убирается, то есть визуально перед нами обычный сухой пляж и суша без воды.
*   **Обман движка:** При этом уровень моря в коде помечается как неизменный и фиксированный. Игра технически продолжает думать, что ниже этой линии находится вода.
*   **Окружение и флора:** Гнезда и ландшафт генерируются обычным образом на этом сухом пляже. Для разнообразия мира свойства определенных растений изменяются в игровых файлах — их тип меняется с `gaprop` на `underwater`. Это приводит к тому, что движок автоматически размещает специализированные растения в засушливой экосистеме. Для создания атмосферы добавлены пользовательские подводные визуальные эффекты (пузырьки, фильтры, освещение), также это работает с существами, игра должна автоматически ставить существам префикс `underwater`, если у них есть жабры и плавники, и нет ног (однако можно позже реализовать земноводных)

### Основная стратегия реализации: Обход физики (Метод Б)
Чтобы обеспечить полную совместимость с ванильным рендерингом ландшафта Spore и автоматическим окрашиванием берегов, мод НЕ будет перемещать или удалять физическую плоскость воды (`cOcean` / `cWaterModel`).

Вместо этого мы используем Spore ModAPI (C++) для следующих задач:
1. **Перехват физического цикла:** Перехват проверок функций `HasFloated` / `IsSwimming` для существа игрока и камеры.
2. **Принудительная физика суши ниже уровня моря:** Заставить движок принудительно возвращать `false` для проверок плавания. Это позволит существу игнорировать поверхность воды и беспрепятственно проходить на сухое океанское дно.
3. **Запуск визуальных фильтров:** Динамическое включение кастомного полноэкранного шейдера (туман, синий фильтр, частицы) и спавн подводных объектов (путем изменения их тегов на `underwater`) на основе координаты Z существа (когда $Z \leq Z_{water}$).
4. **Активация 3D-плавания:** Использование модифицированной физики левитации крыльев для плавной реализации вертикального перемещения (всплытие/погружение) внутри визуального объема воды.

#### 🧬 2. Выбор пути на основе редактора
Переход из этапа «Клетка» завязан на логику и строение тела существа:
*   **Осознание 3D:** Клетка плавает, оглядывается, понимает, что мир не ограничен двумя измерениями, и уходит в 3D, затем полсе катсцены в редактор простейших.
*   **Морской путь:** Если игрок добавляет существу жабры и плавники — оно решает остаться под водой, так как этот мир ему интересен. Плавание реализуется через переделанную левитацию крыльев: существо плавно парит в воздухе над сухим пляжем и не падает (но в пределе уровня моря).
*   **Сухопутный путь:** Если игрок даёт существу ноги, нормальный нос и рот — запускается сцена, где оно понимает, что задыхается под этой невидимой технической толщей воды. Существо паникует, выплывает на поверхность, где срабатывает стандартная бесшовная катсцена выхода на сушу.

---

## 🛠️ Project Status / Статус проекта

> ⚠️ **Project Status:** **Conceptual Framework / Technical Design Document (Design Doc)**
> I am not a programmer or a 3D modeler. This is a logical concept and a technical specification sheet. If you are a *Spore* modder (experienced with SporeModder FX, Spore API, or C++ injection) and want to help realize this idea, feel free to reach out!

> ⚠️ **Статус проекта:** **Концепт и техническое задание (дизайн-документ)**
> Я не являюсь программистом или 3D-моделлером. Это логический концепт и ТЗ. Если вы моддер *Spore* (работаете со SporeModder FX, Spore API или инъекциями C++) и хотите помочь реализовать эту идею — пишите!

## 📬 Contacts / Контакты

*   **Discord:** `heian.shrine`
