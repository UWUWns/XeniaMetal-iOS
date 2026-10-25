# XeniaMetal-iOS

An experimental iOS app and Metal-renderer scaffold for research into an iPhone/iPad port of Xenia.

> **Status: scaffold only — not a working Xbox 360 emulator.** This repository contains a native SwiftUI/MetalKit shell and a small Metal rendering test. It does not contain Xenia's CPU emulator, Xbox 360 kernel/API emulation, GPU command processor, shader translator, audio system, or game compatibility layer. Those must be ported and integrated separately.

## Goals

- Native iOS/iPadOS app shell.
- MetalKit rendering surface and a minimal Metal clear/draw test.
- A file-import UI for selecting a game dump you own, for future integration.
- A documented integration boundary for a future C++ Xenia core.
- GitHub-friendly structure.

## Current capabilities

- Launches a SwiftUI app.
- Creates a Metal device and `MTKView`.
- Clears the view using a Metal render pass.
- Lets the user select a file through Apple's document picker; currently it only displays the selected filename and does **not** load or execute it.

## Important technical reality

Upstream Xenia currently uses Vulkan and, on Windows, D3D12 backends. A native Metal backend is a substantial graphics-backend project, not a simple API rename. Xenia's existing GPU command processing, shader translation, render-target behavior, synchronization, and Xbox 360 eDRAM behavior all need to be connected to the new backend. The iOS port also needs platform-specific work for memory mapping, JIT/code generation, audio, controller input, filesystem access, and app lifecycle.

This scaffold intentionally does not pretend that a Metal clear-screen test is a functioning emulator.

## Requirements

- A Mac with Xcode installed to build and sign an iOS app.
- XcodeGen (`brew install xcodegen`) to generate the `.xcodeproj` from `project.yml`.
- An iPhone/iPad running a supported iOS version for device testing.

## Build

```bash
brew install xcodegen
xcodegen generate
open XeniaMetal.xcodeproj
```

In Xcode:
1. Select the `XeniaMetal` app target.
2. Set your own unique **Bundle Identifier** under Signing & Capabilities.
3. Select your Apple development team if deploying to a physical device.
4. Choose an iPhone simulator or connected device and press Run.

Alternatively, after generating the project:

```bash
xcodebuild -project XeniaMetal.xcodeproj \
  -scheme XeniaMetal \
  -destination 'generic/platform=iOS Simulator' \
  build
```

## Repository layout

```text
XeniaMetal-iOS/
├── .github/workflows/ios-build.yml
├── XeniaMetal/
│   ├── App/
│   │   └── XeniaMetalApp.swift
│   ├── UI/
│   │   └── ContentView.swift
│   ├── Renderer/
│   │   ├── MetalRenderer.swift
│   │   └── Shaders.metal
│   └── Resources/
├── docs/
│   └── XENIA_PORTING_PLAN.md
├── .gitignore
├── project.yml
└── README.md
```

## Xenia integration plan

Read [`docs/XENIA_PORTING_PLAN.md`](docs/XENIA_PORTING_PLAN.md) before copying Xenia source into this repository. Keep upstream source and licensing notices intact, and follow the exact license terms of the upstream revision and every dependency. Do not bundle copyrighted game files, console firmware, keys, or other proprietary assets.

Upstream project: https://github.com/xenia-project/xenia  
Apple Metal documentation: https://developer.apple.com/documentation/metal  
Apple Metal C++: https://developer.apple.com/metal/cpp/

## Legal / compatibility note

This project is an independent experimental scaffold and is not affiliated with or endorsed by the Xenia project or Apple. Only use game data and other files you are legally entitled to use. Game compatibility and performance are not implemented or guaranteed.
