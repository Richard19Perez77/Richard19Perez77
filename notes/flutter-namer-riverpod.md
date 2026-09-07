# Flutter Namer (Riverpod, single file) — rebuild outline

Snapshot of **flutter_application_2** before the GitHub repo was deleted. Rebuild this when learning Flutter; do not restore the old default-named repo.

## Goal

A smaller **Namer App** than app 1: same word-pair idea, **no image tab**. Almost everything lived in `lib/main.dart` on purpose (learning Riverpod in one place).

Material 3, deep-orange seed, title **Namer App**.

Two destinations:

1. **Home** — random `english_words` pair on a big card, Like / Next, history list with a top fade (`ShaderMask` + `AnimatedList`)
2. **Favorites** — grid of liked pairs, delete icon to unlike, empty: "No favorites yet."

Narrow (`< 450`): `BottomNavigationBar`. Wider: `NavigationRail` (extended at `>= 600`). `AnimatedSwitcher` (200 ms) when swapping pages.

## Packages

- `english_words`
- `flutter_riverpod`

Riverpod codegen packages were in `pubspec.yaml` but the app used a hand-written `StateNotifier` + `StateNotifierProvider`. You can stay with that, or use the newer `Notifier` API.

## State that existed

```
MyAppState
  current: WordPair
  history: List<WordPair>
  favorites: List<WordPair>
  historyListKey: GlobalKey?   # so Next can insert into AnimatedList

MyAppStateNotifier
  getNext()
  toggleFavorite([WordPair? pair])  # defaults to current
  removeFavorite(WordPair)
```

The old notifier copied fields with cascade (`MyAppState()..current = ...`). When you rebuild, prefer an immutable `copyWith` or a `Notifier` that mutates then assigns a new state object cleanly.

## Widgets that lived in `main.dart`

- `MyApp` — `MaterialApp`
- `MyHomePage` — `ConsumerStatefulWidget`, `selectedIndex` 0/1
- `GeneratorPage` — history + `BigCard` + Like/Next
- `BigCard` — pair first (w200) + second (bold), `AnimatedSize`, `MergeSemantics`
- `FavoritesPage` — count header + `GridView` (`maxCrossAxisExtent: 400`)
- `HistoryListView` — reverse `AnimatedList`, favorite heart on rows already liked

## Tests

`test/widget_test.dart` was still the **default counter** test (find `'0'`, tap `Icons.add`). It did not match the namer UI and would fail. When you rebuild, write tests for Like/Next and Favorites instead.

## Run

```bash
flutter create flutter_namer_riverpod
cd flutter_namer_riverpod
flutter pub get
flutter run
flutter test
```

Name the new repo after the app (`flutter-namer-riverpod` or similar), not `flutter_application_2`.

## How this differed from app 1

| | App 1 (`flutter-namer-image.md`) | This app |
| --- | --- | --- |
| Files | Split under `lib/` | One `lib/main.dart` |
| Tabs | Home, Favorites, Image Uploader | Home, Favorites |
| Tests | Widget + integration tests that matched the UI | Stale counter test |
| Extra | Stray Kotlin files at repo root | None |
