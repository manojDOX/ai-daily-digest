# Automated AI Project Digest

An automated n8n workflow that discovers, analyzes, and publishes the top 5 trending AI/ML projects from GitHub every week.

**[➡️ View the Live Demo](https://manojdox.github.io/ai-daily-digest/)** *(Note: Replace `manojDOX` with your username if different)*

---

### ## About The Project

This project is a fully automated content pipeline built with n8n. Every Saturday, it scans GitHub for the most popular Python projects of the week, uses the Google Gemini AI to understand what each project does, and then builds and publishes a custom-styled HTML page to GitHub Pages.

The goal is to create a zero-maintenance, automatically updating digest that helps developers, students, and AI enthusiasts discover new and exciting projects in the AI/ML space.

#### **The n8n Workflow**
![n8n Workflow Diagram](image_c521ad.png)
*(Note: To make this image work, upload your workflow screenshot to your repository and name it `image_c521ad.png`)*

#### **Sample Output**
![Sample Output Page](<URL_to_your_sample_output_image.png>)
*(Note: Take a screenshot of your final, working page, upload it to the repository, and replace the link above.)*

---

### ## ✨ Features

* **Weekly Automation**: The workflow is triggered automatically every Saturday.
* **Dynamic Discovery**: Uses the GitHub API to find the top 5 most-starred Python projects from the past 7 days.
* **Intelligent Analysis**: Leverages the Google Gemini API to analyze each project's README file, generating:
    * A simple, easy-to-understand summary.
    * A list of key technologies used.
    * An assessment of its learning value for students.
* **Custom Page Generation**: Builds a complete, styled HTML page from scratch that matches a professional portfolio design.
* **Automated Publishing**: Pushes the generated `index.html` file to the GitHub repository, updating the live GitHub Pages site automatically.
* **Resilient by Design**: Includes error handling for API rate limits and gracefully manages cases where README files might be missing.

---

### ## Workflow Breakdown

The automation is powered by a sequence of n8n nodes, each with a specific job:

1.  **Schedule Trigger**: Kicks off the entire workflow once a week.
2.  **HTTP Request (GitHub Search)**: Connects to the GitHub API to search for repositories.
3.  **Code (Slice)**: Takes the list of results and keeps only the top 5.
4.  **HTTP Request (Get README Info)**: For each of the 5 projects, this node asks the GitHub API for the exact location and download URL of its README file. This robustly handles different branch names (`main`, `master`, etc.) and file casings (`README.md`, `readme.md`).
5.  **Extract from File**: Takes the downloaded binary README file and cleanly extracts its text content, preparing it for the AI.
6.  **Message a model (Google Gemini)**: Sends the clean README text to the Gemini API with a detailed prompt, asking it to return a structured JSON object with the analysis.
7.  **Edit Fields (Set)**: Combines the original project data (name, URL, author) with the new AI-generated data (`ai_summary`, `ai_technologies`, etc.) into a complete package for each project.
8.  **Code1 (Aggregate & Build HTML)**: The powerhouse of the workflow. This node gathers the 5 fully-analyzed project packages and uses a JavaScript template to build the entire, final HTML page in a single step.
9.  **GitHub (Delete & Create File)**: The final publishing sequence. It first deletes the old `index.html` and then creates the new one to ensure a clean and reliable weekly update.

---

### ## Technology Stack

* **Automation**: [n8n.io](https://n8n.io/)
* **AI Analysis**: [Google Gemini API](https://ai.google.dev/)
* **Data Source**: [GitHub API](https://docs.github.com/en/rest)
* **Hosting**: [GitHub Pages](https://pages.github.com/)
* **Logic**: JavaScript (Node.js)
* **Styling**: [TailwindCSS](https://tailwindcss.com/)

---

### ## Setup & Installation

To run this project yourself:

1.  Clone this repository.
2.  Set up your own instance of n8n (cloud or self-hosted).
3.  In n8n, create credentials for the **GitHub API** and **Google Gemini API**.
4.  Import the `workflow.json` file from this repository into your n8n canvas. *(Note: Export your workflow from n8n and add it to the repo as `workflow.json`)*.
5.  Update the **GitHub nodes** with your own username and repository name.
6.  Activate the workflow!
