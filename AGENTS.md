# Repository Guidelines

## Project Structure & Module Organization
- Core sources: `Core/Src/*.c`, headers: `Core/Inc/*.h` (place application code here). 
- HAL, CMSIS, BSP: `Drivers/**` (vendor code; do not edit). 
- CMake integration: `cmake/stm32cubemx/CMakeLists.txt` (generated wiring), top-level `CMakeLists.txt` (add your modules). 
- Toolchains: `cmake/gcc-arm-none-eabi.cmake`, `cmake/starm-clang.cmake`. 
- Linker/startup: `STM32H533xx_FLASH.ld`, `startup_stm32h533xx.s`. 
- CubeMX config: `STMProject2.ioc` (regenerate code via STM32CubeMX when needed).

## Build, Test, and Development Commands
- Prerequisites: CMake ≥ 3.22, Ninja, and an ARM toolchain. Edit `TOOLCHAIN_PREFIX` in `cmake/gcc-arm-none-eabi.cmake` or ensure `arm-none-eabi-*` is on your `PATH`.
- Configure: `cmake --preset Debug`
- Build: `cmake --build --preset Debug` (outputs `build/Debug/STMProject2.elf`)
- Alternative configure (explicit): `cmake -S . -B build/Debug -G Ninja -DCMAKE_TOOLCHAIN_FILE=cmake/gcc-arm-none-eabi.cmake`
- Size report: `arm-none-eabi-size build/Debug/STMProject2.elf`
- Flashing (example): `STM32_Programmer_CLI -c port=SWD -w build/Debug/STMProject2.elf`

## Coding Style & Naming Conventions
- Language: C11, `-Wall`, optimize per preset. 
- Indentation: 2 spaces; no tabs. Lines ≤ 100 chars. 
- Keep STM32CubeMX markers (`USER CODE BEGIN/END`) and only modify inside those regions in generated files. 
- User code: `lower_snake_case` for functions/variables, `UPPER_SNAKE_CASE` for macros. 
- File layout: put headers in `Core/Inc`, sources in `Core/Src`; update top-level `CMakeLists.txt` via `target_sources()`/`target_include_directories()`.

## Testing Guidelines
- No unit test framework is bundled. Validate on hardware (Nucleo H5). Use UART logs (COM1 at 115200) and LEDs for smoke checks. 
- Prefer pure, side‑effect‑free helpers to enable future unit tests; place them in separate modules under `Core`.

## Commit & Pull Request Guidelines
- Commits: concise, imperative subject; recommend Conventional Commits (`feat:`, `fix:`, `build:`). Include rationale and scope. 
- PRs: clear description, linked issues, how you tested (board, steps, logs), and any `.ioc` or toolchain changes. Attach size diffs or `size` output when relevant. 
- Do not reformat vendor code under `Drivers/**`.

## Configuration Tips
- Toolchain path is set in `cmake/gcc-arm-none-eabi.cmake` (`TOOLCHAIN_PREFIX`). Adjust for your host or switch toolchain with `-DCMAKE_TOOLCHAIN_FILE=cmake/starm-clang.cmake` (ensure required env vars are set).

