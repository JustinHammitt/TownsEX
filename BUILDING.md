# Building TownsEX

This repository keeps the original released Towns layout: Java source, XML data, and INI files all live under `src/`.

## Requirements

- JDK 17 or newer is fine for compiling the current source as Java 8 bytecode.
- Gradle 8.x.

The build pulls the legacy game libraries from public Maven repositories:

- LWJGL 2.9.3
- JNA 4.1.0
- TWL PNGDecoder 1.0
- Slick Util 1.0.0

## Compile

```powershell
gradle classes
```

The original Java files use Windows-1252-era source encoding, so the Gradle build compiles them with `windows-1252`.

## Run

```powershell
gradle run
```

The `run` task uses `src/` as the working directory because the original code loads files such as `towns.ini`, `graphics.ini`, and `data/actions.xml` using relative paths.

The build also extracts LWJGL 2 native libraries into `build/natives/lwjgl` and passes that directory to the JVM.

At the moment, `gradle run` reaches startup but exits when the game tries to load missing original graphics assets, beginning with:

```text
data/graphics/icon.png
```

## Check Runtime Assets

```powershell
gradle checkRuntimeAssets
```

This prints which expected runtime files/folders are present.

## Current Asset Status

The source tree currently includes code and data, but not the full runtime asset folders referenced by `towns.ini`:

- `src/data/graphics/`
- `src/data/audio/`

That means compilation can work before the game itself is fully runnable.

## Mod Loading Notes

The game creates a user folder at:

```text
<user home>/.towns/
```

Mods live under:

```text
<user home>/.towns/mods/<mod name>/
```

Many managers load the base XML first, then load files from active mods. A mod can usually provide matching files under paths such as:

```text
<user home>/.towns/mods/<mod name>/data/items.xml
<user home>/.towns/mods/<mod name>/data/actions.xml
<user home>/.towns/mods/<mod name>/data/menu.xml
```

The active mod list is stored in the `MODS` property in `towns.ini`, and the main menu also has a Mods screen for toggling folders it finds there.
