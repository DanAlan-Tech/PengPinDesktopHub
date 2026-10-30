# Shucks — Pengpin Desktop Hub

Shucks is a Windows desktop application built around the Pengpin welcome screen. It brings several small tools and learning experiences together behind one graphical interface, including system utilities, study activities, chat interfaces, and the Pengpins game.

This page presents the project as a portfolio piece: it describes the application and the engineering work behind it rather than positioning it as a packaged product or hosted service.

For a visual tour of the main screens, see the [Shucks screenshot gallery](SCREENSHOTS.md).

## What it does

After opening the application, a user can create a local account or sign in, then choose a feature from the workspace:

- **Shucks assistant** — a general-purpose local chat companion in the main workspace.
- **Kernel** — a network-operations and diagnostics workspace with a local assistant for networking questions and tool guidance.
- **Diagnostics** — a graphical entry point to system health checks.
- **Knowledge Base and Professor Patch** — technology study guides, practice activities, a course planner, and an AI study coach.
- **Pengpins** — a small interactive game with a dedicated control panel and live status display.

The interface uses a consistent dark theme, custom artwork, reusable buttons, and separate windows for the different areas.

## The assistants

The project has three assistant experiences, each aimed at a different task:

| Assistant | Purpose |
| --- | --- |
| **Shucks** | A friendly, general-purpose assistant for everyday questions and conversation. |
| **Kernel** | A local assistant embedded in the network operations workspace. It complements networking diagnostics and helps explain network concepts and tool output. |
| **Professor Patch** | An offline-oriented study coach for technology and certification topics. It can explain material, make concise notes, quiz one question at a time, help interpret errors, and suggest safe troubleshooting steps. It receives relevant study context from the Knowledge Base and does not run commands or change the computer. |

These experiences share the chat interface and local model integration, while using different names, instructions, and context for their intended roles.

## Knowledge Base and course planning

The Knowledge Base combines offline study materials and question banks with course-planning and progress features. Its study catalog covers technology topics such as AI services, Linux, networking, and security. Learners can work through lessons and practice assessments, then keep progress on the device.

The planner connects course work to the learning hub by organizing an operating-systems course plan into weekly sections and task records, including labs, homework, assessments, and career-skills milestones. A task can include its category, title, due date, and completion state. Calendar-derived course events are represented as a **local snapshot** in the project; due dates can be edited in the planner, and completion and date changes are saved locally. This is not a live connection to a school portal or calendar account, so updated events need to be reflected in the local course-plan data or edited in the planner.

Professor Patch receives a limited study snapshot when its chat opens: available course names, recorded practice results, excerpts from upcoming lessons, and up to five upcoming planner deadlines. This lets the coach relate study help to the learner's current topics and schedule without presenting it as a live calendar integration. The Knowledge Base also includes a tracker for items such as certifications, digital badges, licenses, and assignments.

## How it works

`start.py` is the application's entry point. It creates the welcome window, handles the account screens, and opens the feature menu. From there, it routes each selection to a focused Python module rather than putting every feature in the launcher.

The launcher and feature modules work together in a few ways:

1. **Navigation:** The welcome screen and workspace open and close feature windows while keeping the user in a clear path through the app.
2. **Local data:** Account and study-related data are handled locally; structured study content, course-plan records, and question banks are loaded from project data files.
3. **Background work:** The game runs separately from the main interface. A worker thread reads its output and passes it to the UI, which updates the game feed and status display without blocking the window.
4. **Study context:** The Knowledge Base gathers selected progress, lesson, and planner information and passes it to Professor Patch as context for a chat session.
5. **Shared presentation:** Common UI helpers and image assets are reused across screens to keep the visual style consistent.
6. **Feature modules:** Chat, diagnostics, networking, study tools, and the game each have their own implementation, making them easier to develop and maintain independently.

## Project structure

| Area | Role |
| --- | --- |
| `start.py` | Welcome screen, account flow, workspace menu, and feature navigation |
| `auth.py` | Local account operations |
| `bot.py`, `Kernelbot.py`, `professor_patch.py` | Shared chat UI and assistant-specific configurations |
| `it_manager.py`, `network_manager.py` | System diagnostics and networking utilities |
| `test.py`, `knowledge_study_tools.py` | Study activities, course planner, progress, and study-coach context |
| `Pengpins_update_1.py` | Pengpins game |
| `ui_helpers.py`, `ctk_compat.py` | Shared UI helpers and widget compatibility layer |
| JSON files and image assets | Study content, question banks, branding, and interface artwork |

## Technology

- **Python** for application behavior and feature modules
- **Tkinter / CustomTkinter** for the desktop interface
- **Pillow** for image loading and resizing
- **SQLite and JSON** for local application data and structured learning content
- **Threads, queues, and subprocesses** for keeping long-running feature work separate from the UI event loop
- **Windows system integration** for selected diagnostic features
- **Local model integration** for the chat experience when its optional model/runtime is available

## Skills demonstrated and developed

- **Desktop UI development:** Building windows, forms, navigation, responsive layouts, and reusable controls.
- **Modular Python design:** Organizing a multi-feature application into modules and connecting them through a central launcher.
- **Local data handling:** Working with SQLite, JSON content, and persistent learning progress.
- **Asynchronous UI patterns:** Using queues, threads, and child processes to keep the interface responsive while receiving live output.
- **Resource management:** Loading artwork and other assets relative to the application, and handling missing resources gracefully.
- **Windows integration:** Connecting a Python interface to operating-system diagnostics and utilities.
- **Problem solving and iteration:** Tracing behavior across modules, refining user flows, and improving the presentation of status and error feedback.

## Running locally

The project is designed for a Windows desktop environment. The main entry point is `start.py`; the interface requires Python's Tk support, CustomTkinter, and Pillow. Some optional features require additional local components, such as a compatible AI model/runtime. Those components are not bundled as part of this portfolio description.

## Portfolio note

Shucks is a personal software project intended to showcase application design, Python development, and integration across several features. It is not presented as a production-ready product or hosted service.
