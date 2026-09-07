# Flutter Namer + image tab — rebuild outline

Snapshot of **flutter_application_1** before the GitHub repo was deleted. Rebuild this when learning Flutter; do not restore the old default-named repo.

## Goal

A Material 3 app titled **Namer App** (deep-orange seed color) with three destinations:

1. **Home** — random English word pairs (`english_words`), Like / Next, history list
2. **Favorites** — liked pairs in a grid, delete to unlike
3. **Image Uploader** — pick from gallery, show bytes, optional remote image transform

Narrow screens (`< 450`): `BottomNavigationBar`. Wider: `NavigationRail` (extended at `>= 600`).

## Packages to start with

- `english_words`
- `flutter_riverpod` (this is what the UI actually used)
- `image_picker` + `http` for the third tab
- `flutter_test` + `integration_test`

Skip leftover `provider` unless you choose Provider instead of Riverpod. Do not add unused web-only picker packages unless you need web.

## Suggested `lib/` layout

```
lib/
  main.dart                 # ProviderScope + MaterialApp
  home_page.dart            # selectedIndex + adaptive scaffold
  app_state.dart            # current pair, history, favorites
  generator_page.dart       # Like / Next
  big_card.dart             # WordPair card (test keys: word_first, word_second)
  history_list_view.dart    # fading AnimatedList of past pairs
  favorites_page.dart       # grid; empty: "No favorites yet."
  image_picker_app.dart     # third tab UI
  image_state.dart          # pick image, then POST bytes to an API if you want
```

Wrap the app in `ProviderScope`. Keep word-pair state in a Riverpod `Notifier` / `NotifierProvider`. Keep tab index in a `StateProvider<int>`.

## Behavior to match

- **Next** — push current pair to the front of history, then a new random `WordPair`
- **Like** — toggle current pair in favorites
- History rows can toggle favorite
- Favorites empty state uses a key (`no_favorites_yet`) if you keep the old integration tests
- Image tab: `ImagePicker.pickImage(source: gallery)`, show `Image.memory`, then a network result if you call an API

## Tests that existed

- Widget: app launches with `ProviderScope` and `MyHomePage`
- Widget: `BigCard` shows both words; first weight `w200`, second `bold`
- Integration: Home shows Like/Next; Next changes `word_first`; Favorites shows empty copy; Home icon returns to Like

## Do not copy from the old repo

- Hardcoded API keys (the old image tab posted to DeepAI Deep Dream with a key in source). Use `--dart-define` or a local ignored file.
- `skyline.kt` and `intervalmerging.kt` at the repo root — those were Kotlin algorithm notes, not Flutter. If you want them, put them in a Kotlin practice folder instead.

## Run

```bash
flutter create flutter_namer
cd flutter_namer
flutter pub get
flutter run
flutter test
```

Name the new repo after the app (`flutter-namer` or similar), not `flutter_application_1`.
