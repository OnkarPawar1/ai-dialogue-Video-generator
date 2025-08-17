# AI Dialogue Video Generator

A powerful, browser-based tool that creates engaging dialogue videos using Google's generative AI and Text-to-Speech technologies. Simply provide a topic and two character videos, and the application will automatically generate a script, synthesize voices, and render a complete video conversation, ready for download.
<img width="751" height="705" alt="image" src="https://github.com/user-attachments/assets/ca17c046-f664-4712-9b4a-7e364f8101f8" />

## Features ✨

  - **🤖 AI-Powered Script Generation**: Leverages various Google **Gemini models** (including Gemini 2.5 Pro & Flash) via the Vertex AI API to generate dynamic and creative scripts from a simple topic.
  - **🗣️ High-Quality Text-to-Speech**: Integrates with Google Cloud's Text-to-Speech API, offering a wide selection of languages and voices, including premium **Studio** and **Chirp** voices.
  - **🎬 Custom Video Characters**: Upload your own `MP4` or `WebM` videos to serve as the talking characters in the dialogue.
  - **📐 Flexible Aspect Ratios**: Easily switch between **9:16 (Portrait)** for social media shorts and **16:9 (Landscape)** for standard video platforms.
  - **🔧 Rich Customization**: Fine-tune the output by adjusting speaking speed, selecting specific voices for each character, and even editing the full AI prompt.
  - **🚀 Fully Client-Side**: All video processing and rendering happens directly in your browser using the Canvas and MediaRecorder APIs. No server-side video processing is required.
  - **📥 Direct Download**: Once generated, the video can be downloaded directly as an `MP4` or `WebM` file.

-----

## How It Works

The application follows a streamlined, multi-step process entirely within the user's browser:

1.  **Script Generation**: The user provides a topic. The app formats this into a detailed prompt and sends it to the selected **Gemini model** via the Vertex AI API. The model returns a formatted script (e.g., `Man: ...`, `Woman: ...`).
2.  **Voice Synthesis**: The script is parsed line by line. For each line, the app calls the **Google Text-to-Speech API** to generate an MP3 audio clip with the selected voice and speaking rate.
3.  **Video & Audio Sequencing**: The application plays back the dialogue in sequence. For each line:
      * The corresponding character's video is played on a hidden `<video>` element.
      * The generated audio for that line is played through a hidden `<audio>` element.
4.  **Canvas Rendering**: On every frame, the content of the currently active (speaking) character's video is drawn onto a `<canvas>` element. This canvas acts as the final "stage" for the video.
5.  **Recording**: A `MediaRecorder` instance captures the visual stream from the `<canvas>` and the audio stream from the Web Audio API context. It combines them into a single media stream.
6.  **Finalization**: Once the entire dialogue sequence is complete, the `MediaRecorder` stops. The recorded data chunks are compiled into a single video file (a Blob), which is then made available to the user via a download link.

-----

## Setup and Usage 📋

To run this project, you need credentials for the Google Cloud APIs it uses. All processing is done client-side, so you just need to open the `index.html` file in a browser.

### Prerequisites

1.  **Google Cloud Project**: You need a Google Cloud project with billing enabled.
2.  **Enable APIs**: In your project, enable the **Vertex AI API** and the **Text-to-Speech API**.
3.  **API Key for TTS**: Create an API key and restrict it to be used only with the Text-to-Speech API.
      * Go to "APIs & Services" \> "Credentials".
      * Click "Create Credentials" \> "API key".
      * Copy the key and keep it safe.
4.  **Google Cloud CLI**: [Install and initialize the `gcloud` CLI](https://www.google.com/search?q=%5Bhttps://cloud.google.com/sdk/docs/install%5D\(https://cloud.google.com/sdk/docs/install\)) on your local machine to get an access token for Vertex AI.

### Running the Application

1.  **Clone or Download**: Clone this repository or download the `index.html` file.

2.  **Get Your Access Token**: Open your terminal and run the following command. This will authenticate you and print a temporary access token.

    ```bash
    gcloud auth print-access-token
    ```

    > **Note**: This token is short-lived (typically expires in 1 hour) and must be regenerated for new sessions.

3.  **Open the App**: Open the `index.html` file in a modern web browser like Google Chrome or Firefox.

4.  **Enter Credentials**:

      * **Google Cloud Project ID**: Paste your GCP project ID.
      * **Text-to-Speech API Key**: Paste the API key you created.
      * **Vertex AI Access Token**: Paste the token you generated with the `gcloud` command.

5.  **Upload Videos**:

      * Upload a video file for the **Man's Video**. This video will be shown whenever the "Man" character is speaking.
      * Upload a second video file for the **Woman's Video**.

6.  **Provide a Topic**:

      * In the **Script Topic** field, enter a simple idea for the conversation (e.g., "A debate about whether coffee or tea is better").
      * The application will automatically generate a full prompt based on your topic. You can edit this in the "Full AI Prompt" box if needed.

7.  **Generate\!**

      * (Optional) Customize the aspect ratio, speaking speed, language, or voices.
      * Click the **Generate Video** button.
      * The status area will show the current progress (generating script, synthesizing audio, rendering scenes). This may take some time depending on the script length and your computer's performance.

8.  **Download**: Once the process is complete, a **Download Video** button will appear. Click it to save your final video file.

-----

## Technical Details

### Core Technologies

  - **Frontend**: Vanilla JavaScript, HTML5, Tailwind CSS (via CDN)
  - **Google Cloud APIs**:
      - `Vertex AI API`: For state-of-the-art script generation with Gemini models.
      - `Text-to-Speech API`: For generating high-quality, natural-sounding audio.
  - **Browser APIs**:
      - **`Canvas API`**: Used as the primary rendering engine to compose video frames in real-time.
      - **`Web Audio API`**: Manages the audio context, allowing the generated speech to be captured alongside the canvas visuals.
      - **`MediaRecorder API`**: The engine behind the video creation. It records the combined audio and video streams from the canvas and audio context.
      - **`Screen Wake Lock API`**: Prevents the screen from turning off during the potentially long video generation process, ensuring the browser tab remains active.

### Limitations

  - **Client-Side Performance**: Since all rendering is done on the client machine, performance is dependent on the user's CPU/GPU. Longer videos may be resource-intensive.
  - **Token Expiration**: The Vertex AI access token from `gcloud` expires after a short period. You will need to generate a new one if you leave the page open for an extended time.
  - **API Costs**: Use of the Vertex AI and Text-to-Speech APIs will incur costs on your Google Cloud account. Be sure to monitor your usage.
  - **Browser Compatibility**: The application relies on modern Web APIs. It is best used on the latest versions of Chromium-based browsers (Chrome, Edge) or Firefox.

-----

## License

This project is licensed under the **MIT License**. See the `LICENSE` file for details.
