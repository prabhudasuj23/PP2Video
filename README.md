# PPT2Video Pro: AI-Powered Video Creation Suite

PPT2Video Pro is a comprehensive web-based application that transforms your presentations (PowerPoint or PDF) into engaging videos. It leverages AI for script generation, offers a rich media library, and provides a professional-grade video editor to add dynamic overlays and annotations.

This project is built with a modern tech stack, featuring a React/Vite frontend for a fast, interactive user experience, and a robust Node.js/Express backend for powerful media processing with FFmpeg.

## Key Features

### Core Functionality
- **PPT/PDF to Video:** Seamlessly convert your PowerPoint (`.pptx`) or PDF documents into a sequence of video scenes.
- **AI Script Generation:** Utilize Google Gemini to automatically generate compelling video scripts from your presentation content.
- **Text-to-Speech (TTS):** Generate high-quality voiceovers for your scenes using web speech APIs.
- **Stock Asset Integration:** (Future) Search and import stock images and videos to enrich your content.

### Pro Video Editor
- **Canvas-Based Editing:** A powerful, intuitive editor built with Fabric.js for precise control over video overlays.
- **Rich Overlay Tools:**
    - **Text:** Add and customize text with various fonts, colors, and styles.
    - **Images & Stickers:** Upload your own images or add emoji stickers.
    - **Shapes & Arrows:** Draw attention with customizable rectangles, circles, and arrows.
    - **Highlights:** Emphasize key areas with a semi-transparent highlighter tool.
- **Interactive Canvas:**
    - **Pan & Zoom:** Navigate large canvases with ease using mouse/trackpad gestures (Alt/Ctrl + Drag, Scroll Wheel).
    - **Object Manipulation:** Resize, rotate, and reposition all overlay elements.
    - **Undo/Redo:** Full support for undoing and redoing actions (`Ctrl+Z` / `Ctrl+Shift+Z`).
- **Timeline View:** A simple, intuitive timeline to navigate between different scenes (slides) of your project.
- **Persistent Overlays:** All your edits are automatically saved per-slide using IndexedDB, ensuring no work is lost.

### Backend & Rendering
- **High-Performance Rendering:** The Node.js backend uses `fluent-ffmpeg` to apply overlays and render the final video.
- **Configurable FFmpeg:** Fine-tune video output quality, speed, and file size via environment variables (preset, CRF, FPS, etc.).
- **Concurrent Processing:** Renders multiple scenes in parallel to significantly speed up the final video creation process.

## Tech Stack

- **Frontend:**
    - **Framework:** React with TypeScript
    - **Build Tool:** Vite
    - **Canvas Library:** Fabric.js
    - **Routing:** React Router
    - **State Management:** React Hooks & Context API
- **Backend:**
    - **Framework:** Node.js with Express.js
    - **Video Processing:** FFmpeg, `fluent-ffmpeg`
    - **File Handling:** Multer for uploads
- **Storage:**
    - **Client-Side:** IndexedDB for scene and overlay data.
    - **Server-Side:** Local file system for media assets, uploads, and final renders.
- **APIs & Services:**
    - **AI:** Google Gemini API for script generation.

## Project Structure

```
/
├── client/         # Original client-side application
├── server/         # Node.js backend (Express, FFmpeg rendering, API)
├── webapp/         # NEW React/Vite frontend with the Pro Editor
├── data/           # Server-side storage for projects, assets, and uploads
├── ppt2video_example_model/ # Boilerplate/example code
└── ...             # Root configuration files (package.json, etc.)
```

## Prerequisites

Before you begin, ensure you have the following installed:

1.  **Node.js:** Version 18.x or later.
2.  **npm:** Should be included with Node.js.
3.  **FFmpeg:** This is **critical** for video rendering.
    - Download it from the [official FFmpeg website](https://ffmpeg.org/download.html).
    - **Important:** Add the `bin` directory from your FFmpeg installation to your system's `PATH` environment variable so it can be called from the command line.
4.  **LibreOffice:** Required for converting `.pptx` files. Install it and ensure its command-line interface is accessible.

## Installation & Setup

1.  **Clone the Repository:**
    ```bash
    git clone <your-repository-url>
    cd <repository-folder>
    ```

2.  **Install Root Dependencies:**
    ```bash
    npm install
    ```

3.  **Install Server Dependencies:**
    ```bash
    cd server
    npm install
    ```

4.  **Set Up Server Environment (`.env`):**
    - In the `server/` directory, create a file named `.env`.
    - Copy the contents from the provided `server/.env` attachment or use the template below.
    - **Important:** Add your Google Gemini API key if you want to use the AI script generation feature.

    ```dotenv
    # server/.env

    # Server Configuration
    PORT=5000
    NODE_ENV=development

    # Google Gemini API (Optional)
    GEMINI_API_KEY=YOUR_GEMINI_API_KEY_HERE

    # FFmpeg Performance Tuning
    FFMPEG_PRESET=veryfast
    FFMPEG_CRF=22
    FFMPEG_THREADS=0
    RENDER_FPS=30
    AUDIO_BITRATE=160k
    RENDER_CONCURRENCY=6
    ```

5.  **Install Webapp (Pro Editor) Dependencies:**
    ```bash
    cd ../webapp
    npm install
    ```

## Running the Application

You need to run the backend server and the frontend webapp simultaneously in two separate terminals.

1.  **Start the Backend Server:**
    - Open a terminal in the `server/` directory.
    - ```bash
      npm start
      ```
    - The server will start on `http://localhost:5000`.

2.  **Start the Frontend Webapp:**
    - Open a second terminal in the `webapp/` directory.
    - ```bash
      npm run dev
      ```
    - The React application will start, typically on `http://localhost:5173` or the next available port. Open this URL in your browser.

You can now use the application to upload a presentation and create your video.

## FFmpeg Configuration

You can adjust the FFmpeg settings in `server/.env` to balance rendering speed and output quality:

- `FFMPEG_PRESET`: Controls the encoding speed. `veryfast` is a good balance. Use `ultrafast` for quicker tests or `medium` for higher quality.
- `FFMPEG_CRF`: Constant Rate Factor (18-28). Lower values mean better quality and larger files. `22` is a great default. `18` is near-lossless.
- `FFMPEG_THREADS`: Number of CPU threads to use. `0` automatically detects and uses all available cores.
- `RENDER_FPS`: Frames per second for the output video. `30` is standard for smooth motion.
- `RENDER_CONCURRENCY`: How many scenes to render in parallel. Defaults to `6`. Set to the number of your CPU cores for maximum performance, but be mindful of I/O limitations.
