#### Content-creation-with-N8N-AI-Agent🤖

# 🎬 Animation Automation Workflow for Social Media
---

Welcome to the **Animation Automation** workflow! This is a powerful n8n workflow designed to automatically create and publish unique, viral-style animated videos to YouTube Shorts and Facebook, all without human intervention.

![▶️ Animation Automation - n8n DEV _page-0001](https://github.com/user-attachments/assets/91cd2588-6155-40ae-be09-01ca2604d66a)

## ✨ What Does It Do?

This workflow handles the entire creative process from start to finish:

1.  ⏰ **Triggers on a Schedule:** The workflow runs automatically on a set schedule (e.g., every Tuesday, Thursday, and Saturday at 10 AM).
2.  💡 **Generates a Concept:** It uses a Gemini AI model to generate a random, high-potential viral video prompt about cute animals and an orange cat.
3.  ✍️ **Enhances the Prompt:** The initial prompt is enhanced with a second Gemini model to add specific details for an engaging 8-second animated loop with a dynamic background.
4.  🎥 **Generates the Video:** The enhanced prompt is sent to Google's Veo AI on Vertex AI to generate a high-quality 3D animated video in the requested style.
5.  🎶 **Generates and Mixes Audio:** Simultaneously, it generates a cheerful kids' rhyme using Gemini and a royalty-free background music track from a Google Sheet. It then uses FFmpeg to merge the video, voiceover, and music with professional audio mixing.
6.  🚀 **Publishes to Social Media:** The final, polished video is automatically uploaded to your connected YouTube and Facebook accounts, ready for sharing!

### Example

Here is an example of the video concepts it can generate:



https://github.com/user-attachments/assets/1abda1a2-64c8-499f-a884-e4958144420c


---

## ⚙️ Prerequisites to Run this Workflow (Detailed Guide)

This section provides a step-by-step guide for setting up all the necessary services and software to run the workflow smoothly.

### Step 1: Create a Local `assets` Folder

First, you need to create a folder on your local machine or server where your n8n instance is running. This folder will be used to temporarily store the video, voiceover, and background music files.

* Create a folder named `assets` in the same directory where your n8n workflow files are located. The workflow will read from and write to this folder.

### Step 2: Set up the n8n Server and Dependencies

* **Node.js & npm:** If you are setting up n8n manually, ensure you have a recent version of Node.js installed on your server (version 18 or later is recommended). You can download it from the official Node.js website.
* **Docker (Recommended):** The easiest and most reliable way to run n8n is using Docker. It handles all dependencies for you, including Node.js. Follow the official Docker installation guide for your operating system and then use the n8n Docker image to get your instance running.

### Step 3: Install FFmpeg

FFmpeg is a critical command-line tool used by the `Execute Command` node to merge the video, voiceover, and background music files. It must be installed on the server where your n8n instance is running and be accessible from the command line.

* **On Windows:** Download a recent FFmpeg build from a trusted source (e.g., <https://ffmpeg.org/download.html>). Unzip the files and add the `bin` directory to your system's PATH environment variable.
* **On macOS:** The simplest way is to use Homebrew. Open your terminal and run:
    ```bash
    brew install ffmpeg
    ```
* **On Linux (Debian/Ubuntu):** Use the `apt` package manager:
    ```bash
    sudo apt update
    sudo apt install ffmpeg
    ```

### Step 4: Google Cloud Platform (GCP) Setup

This workflow uses Google's Vertex AI for video generation and Google Cloud Storage (GCS) to manage the output files. You will need a GCP account to set this up.

1.  **Create a New GCP Project:** Go to the Google Cloud Console and create a new project. This will be where your services and billing are managed.
    > The standard Google Cloud free trial offers new customers $300 in free credits for 90 days, which can be used for any Google Cloud product. After the 90-day period or when the $300 credit runs out, whichever comes first, you are no longer eligible for the free trial and can upgrade to a paid account. There is no obligation to do so, and you can remain in the Free Tier for many products without charge as long as you stay within specific usage limits.
2.  **Enable Required APIs:** In the GCP console, navigate to "APIs & Services" > "Library" and enable the following APIs:
    * **Vertex AI API**
    * **Cloud Storage API**
3.  **Create a GCS Bucket:** Go to the "Cloud Storage" section and create a new bucket. This bucket will be used by the Veo AI model to store the generated video. Remember the exact name of this bucket.
4.  **Set up Service Account & Permissions:**
    * Create a service account with the necessary permissions for your n8n instance to access your GCP resources.
    * Grant this service account the **`Vertex AI User`** and **`Storage Object Admin`** roles to allow it to run the AI model and write to the GCS bucket.

### Step 5: API and Credential Setup in n8n

For the workflow to connect to the external services, you must create and configure credentials in your n8n instance.

1.  **Google Cloud (OAuth2):** This credential is used to authenticate with GCP. Create a new "Google Cloud (OAuth2)" credential and link it to the service account you created in Step 4.
2.  **Eleven Labs API Key:** Create a new "HTTP Header Auth" credential and enter your Eleven Labs API key.
3.  **Google Sheets (OAuth2):** I will provide you with a pre-formatted Google Sheet that contains a list of background music links in a column titled `Bg music link`. You just need to copy this sheet to your own Google Drive account. Then, create a "Google Sheets (OAuth2)" credential to grant the workflow access to your sheet.
4.  **Google Drive (OAuth2):** I will also provide you with a Google Drive folder link containing all the necessary background audios. Create a "Google Drive (OAuth2)" credential to allow the workflow to download these files.
5.  **YouTube (OAuth2):** Create a "YouTube (OAuth2)" credential to enable the workflow to upload videos to your channel.
6.  **Facebook Graph API:** Create a "Facebook Graph API" credential with access to your Facebook Page for video uploading.

---

## 🛠️ Required Changes to the Workflow

This workflow is an export and does **not** contain any of my personal API keys, account IDs, or private data. You **must** replace all placeholders with your own information.

1.  **Import the Workflow:** Import the provided JSON file into your n8n instance.
2.  **Connect Credentials:** Go to each node that has a "Credentials" section and connect your own credentials. The workflow will show a warning if credentials are not set.
3.  **Update Hardcoded Values:** Manually update the following nodes by replacing the placeholder values:

    * **Generate Cartoon Video (Veo on Vertex AI):**
        * In the URL, replace `YOUR_GOOGLE_CLOUD_PROJECT_ID` with your actual GCP Project ID.
        * In the `storageUri`, replace `YOUR_GCS_BUCKET_NAME` with your GCS bucket name.
    * **Get All BG Music Links:**
        * Replace `YOUR_GOOGLE_SHEET_ID_HERE` with your Google Sheet's document ID.
    * **Download BG Music from Drive:**
        * Replace `YOUR_GOOGLE_DRIVE_FILE_ID_OR_URL_HERE` with the URL of your background music file on Google Drive.
    * **My Facebook Page Upload:**
        * Replace `YOUR_FACEBOOK_PAGE_ID` with your Facebook Page ID.

Once you have made these changes, the workflow is ready to run!

---

## ❤️ Support My Work

If you found this workflow useful and want to support my work, you can buy me a coffee!

[Buy Me a Coffee Link]

Thank you for your support! 🙏
