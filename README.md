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

### Method C: The "Astronaut" on an Invisible Mountain (The Ultimate Janky Workaround)
If the engine strictly enforces gravity and forces the creature to land on the floor, we won't fight it. Instead, we will weaponize the gravity mechanics, turning the ocean into a zone of permanent, slow-motion falling.

Using Spore ModAPI and minor landscape modifications, we chain the following exploits:
1. **The Ocean is Mt. Everest:** The planet's heightmap is inverted. Technically, the entire "ocean floor" is the flat peak of a massive mountain raised above the clouds. The physical water plane is pushed deep into the planet's core.
2. **Lunar Gravity Override:** Once the creature crosses the "shoreline," ModAPI dynamically overrides the Creature Stage gravity constant with the low-gravity parameters used for moons/asteroids. The creature now falls at the speed of a feather.
3. **Infinite Jump as Swimming:** The vanilla jump mechanic is modified: whenever the player presses "Spacebar," the engine acts as if the creature is pushing off an invisible surface, propelling it upward. Combined with the ultra-low gravity, the creature smoothly "floats" up and gently drifts back down. Visually, this creates flawless underwater physics.
4. **Tethered Camera:** To prevent the camera from glitching out at such extreme altitudes, its collision grid is locked to an invisible floating satellite prop that follows the creature's Z-coordinate, smoothing out any sudden movements.

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

### Метод C: Эффект «Космонавта» на невидимой горе (Максимальный Костыль)
Если игра намертво блокирует физику полета и заставляет существо падать на дно, мы не будем с этим бороться. Мы заставим гравитацию работать на нас, превратив океан в зону бесконечного замедленного падения.

Через Spore ModAPI и редактирование файлов ландшафта реализуется следующая цепочка костылей:
1. **Океан — это Эверест:** Ландшафт планеты выворачивается наизнанку. Технически всё «океанское дно» — это вершина гигантской плоской горы, поднятой выше облаков. Вода физически опущена в самое ядро планеты.
2. **Космическая гравитация на Земле:** При пересечении «линии берега» ModAPI подменяет для существа глобальную константу гравитации этапа Существа на константу гравитации Луны/Космоса (из ресурсов игры). Существо начинает падать со скоростью перышка.
3. **Бесконечный прыжок как плавание:** Механика обычного прыжка модифицируется: каждый раз, когда игрок нажимает «Пробел», игра считает, что существо отталкивается от невидимой поверхности, подбрасывая его вверх. Из-за сверхнизкой гравитации существо плавно «взлетает» и медленно опускается. Визуально это выглядит как идеальное плавание в толще воды.
4. **Камера на привязи:** Чтобы камера не сходила с ума на огромной высоте невидимой горы, её коллизия привязывается к невидимому летающему объекту-сателлиту, который следует за игроком по оси Z, сглаживая любые резкие рывки.

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
