# Idler Project

## Кратко о проекте

- Студия разрабатывает idle-игру в Unity; репозиторий хранит исходники и ассеты.
- Стек: Unity 6.3 (6000.3.2f1) + Git; крупные ассеты ведутся через Git LFS.

## Требования окружения и запуск

- Установите Unity Hub и редактор 6000.3.2f1; другие версии дают шум сериализации.
- Установите Git LFS (`git lfs install`) перед клонированием или сразу после `git clone` выполните `git lfs pull`, чтобы подтянуть бинарные ассеты (арт, аудио, FBX и т.д.).
- Откройте репозиторий через Unity Hub (`Add project from disk`), загрузите сцену `Assets/!_ProjectMain/Scenes/Prototype.unity` и нажмите Play. В CLI можно запускать без UI:

  ```bash
  /path/to/Unity-6000.3.2f1 -projectPath . -logFile Logs/editor.log
  ```

## Как создать локальную копию

- Проверьте Git и Git LFS: `git --version`, затем `git lfs install`.
- Клонируйте: `git clone <repo-url> idler-project && cd idler-project`.
- Подтяните ассеты: `git lfs pull`.
- Откройте папку в Unity Hub или через CLI. При первом запуске дождитесь импорта и сохраните сцену, чтобы проверить ссылки.
- `git switch dev` – для переключение на работу с веткой разработки.

## Структура проекта

- `Assets/!_ProjectMain` — основное содержимое игры: `Scenes` (входная сцена Prototype), `Prefabs`, `Art`, `Audio`, `Scripts` (добавлять код сюда).
- `Assets/!_ThirdParty` — внешние ассеты; фиксируйте лицензию и источник в README внутри папки.
- `ProjectSettings/` и `Packages/` — конфигурация проекта; не правьте вручную `Library/` и `Logs/`.
- `Assets/InputSystem_Actions.inputactions` — карта ввода; изменяйте через окно Input System, чтобы автоматически регенерировать C#-обертки.

## Конвенции кода (C# / Unity)

- Отступы 4 пробела, UTF-8.
- Именование: классы/структуры/enum — PascalCase; методы/свойства — PascalCase; приватные поля — `_camelCase`; сериализуемые поля — `[SerializeField] private`; константы/readonly — PascalCase или `ALL_CAPS` только для глобальных констант.
- Файл `SomeFeatureController.cs` должен содержать одноименный публичный класс `SomeFeatureController`.
- Избегайте `FindObjectOfType`; используйте ссылки через инспектор или внедрение зависимостей. Не хардкодьте пути; применяйте ссылки на ScriptableObject/Prefab.
- Корутины применяйте осознанно; экономический тик держите детерминированным и тестируемым.

## Конвенции ассетов и файлов

- Сцены: понятные имена с префиксами `Main`, `Prototype`, `Menu`, либо модульными (`Feature_`).
- Префабы/ScriptableObjects: PascalCase с префиксами по назначению (`UI_ButtonPrimary`, `FX_Sparkle`, `SO_CurrencyConfig`).
- Материалы/текстуры/аудио: сгруппированы в подпапки фич, имена без пробелов (`Mat_UI_Background`, `SFX_CoinPickup`).
- Папки создавайте под фичи; переименовывайте ассеты через Unity, чтобы обновлялись ссылки и `.meta`.

## Git и workflow

- Перед коммитом: сохраните сцены/префабы, убедитесь в чистоте Console, проверьте, что в диффе нет `Library/` или случайных reimport шумов.
- Для больших бинарников используйте LFS (паттерны уже в `.gitattributes`). При добавлении новых типов файлов добавьте маску в `.gitattributes`.
- Рекомендован формат сообщения коммита — императив: `Add idle income ticker`, `Fix input rebinding`.
- Для совместной работы включайте в Unity: Project Settings → Editor → Asset Serialization: `Force Text`; Version Control: `Visible Meta Files`.
- Настройте Smart Merge (UnityYAMLMerge) локально, чтобы авторазрешать простые конфликты сцен/префабов:

  ```bash
  git config merge.unityyamlmerge.name "Unity SmartMerge"
  git config merge.unityyamlmerge.driver "unityyamlmerge merge -p \"\$BASE\" \"\$REMOTE\" \"\$LOCAL\" \"\$MERGED\""
  ```

  В `.gitattributes` уже проставлены правила для `*.unity`, `*.prefab`, `*.asset`, `*.meta`, `*.mat`.
