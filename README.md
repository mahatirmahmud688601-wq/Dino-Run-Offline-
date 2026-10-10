# Dino Run Clone

A 2D endless runner built with the [Flame](https://flame-engine.org/) engine —
same core mechanics as the original Dino Run repo (tap to jump, dodge
obstacles, score climbs over time), rebuilt from scratch since no source was
provided, only project metadata.

## Notes on this build

- No sprite/audio assets were provided in the original upload, so the
  player, obstacles and background are drawn procedurally with Flutter's
  `Canvas` API instead of `SpriteComponent`/`ParallaxComponent`. Swap
  `Player.render()`, `Obstacle.render()` and `Background.render()` for
  sprite-based rendering once you add art under `assets/images/`.
- High score persists locally via Hive (`PlayerData`).
- UI state (score, menu/playing/game-over) is exposed via `Provider`
  (`GameState`) so Flutter overlay widgets can react without polling the
  game loop.

## Architecture

```
lib/
  main.dart                 # Hive init, Provider + GameWidget wiring
  models/
    game_state.dart         # ChangeNotifier: score, status
    player_data.dart         # Hive-persisted high score
    player_data.g.dart      # Hand-written TypeAdapter (no codegen needed)
  game/
    dino_run.dart            # FlameGame: input, collision, run lifecycle
    player.dart               # Jump physics + rendering
    obstacle.dart             # Scrolling obstacle
    obstacle_manager.dart    # Spawns obstacles, scales difficulty
    background.dart          # Procedural parallax-style background
  widgets/
    main_menu.dart
    hud.dart
    game_over_menu.dart
```

## Run

```bash
flutter pub get
flutter run
```

Tap anywhere to jump. Difficulty (spawn rate + obstacle speed) increases
with score.

## Extending

- Add sprites: drop PNGs under `assets/images/`, declare them in
  `pubspec.yaml`, and load with `Sprite.load()` inside each component's
  `onLoad`.
- Add sound: `flame_audio` was in the original pubspec; re-add it and call
  `FlameAudio.play(...)` on jump/hit/score events.
- Add ducking/double-jump: extend `PlayerState` and branch in
  `Player.update`.
