# ⚡ CodeStash — Modern Neubrutalism Code Snippet Manager

<p align="center">
  <img src="https://img.shields.io/badge/Design-Neubrutalism-FFE600?style=for-the-badge&logoColor=000&labelColor=000" alt="Neubrutalism Design" />
  <img src="https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white" alt="Tailwind CSS" />
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript" />
  <img src="https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white" alt="Supabase" />
  <img src="https://img.shields.io/badge/Prism.js-2A2B2E?style=for-the-badge&logo=codeigniter&logoColor=white" alt="Prism.js" />
  <img src="https://img.shields.io/badge/License-MIT-black?style=for-the-badge" alt="MIT License" />
</p>

---

## 📌 Overview

**CodeStash** is a lightweight, responsive Single Page Application (SPA) designed for developers to store, manage, and share code snippets. It embraces a bold, modern **Neubrutalism** aesthetic — featuring prominent black borders, hard offset drop shadows, vibrant high-contrast color accents, and tactile interactive elements.

Built without complex compilation pipelines or heavy frameworks, CodeStash combines the simplicity of **Vanilla HTML & Modern JavaScript** with the security and real-time capabilities of **Supabase** (PostgreSQL Database & Authentication).

---

## ✨ Features

- 🎨 **Distinct Neubrutalism Design System**
  - Crisp solid 2px/3px black borders with deep hard offset shadows (`shadow-neubrutal`).
  - High-energy palette: *Electric Yellow, Neo Cyan, Hot Pink, and Crisp Surface White*.
  - Tactile micro-interactions on hover and active states across all buttons and snippet cards.

- ⚡ **Quick Language Navigation (Sidebar & Mobile Drawer)**
  - Direct 1-click filtering by programming language:
    - 🌐 **HTML & CSS** (seamlessly matches snippets saved in HTML or CSS)
    - 🟨 **JavaScript** (JS & TypeScript)
    - 🎯 **Dart / Flutter**
    - 🐘 **PHP / Laravel**
    - 🐍 **Python**
    - 🗄️ **SQL Database**

- 🔍 **Real-Time Multi-Field Search**
  - Instant live filtering by snippet title, code content, or tags without full page reloads.
  - Combines seamlessly with active language category filters.

- 🔐 **Privacy Control (Public vs. Private)**
  - **Public Snippets**: Accessible to the developer community.
  - **Private Snippets**: Securely restricted to your personal authenticated account.

- 🌈 **Syntax Highlighting**
  - Powered by **Prism.js** with an eye-friendly dark theme tailored for high readability across multiple syntax styles.

- 📋 **One-Click Code Copying**
  - Quick copy-to-clipboard functionality with instant visual status feedback.

- 📱 **Fully Responsive Layout**
  - Seamlessly adapted for mobile devices with an interactive sliding drawer, as well as desktop viewports with a sticky sidebar.

---

## 🛠️ Tech Stack

| Category | Technology |
| :--- | :--- |
| **Core Architecture** | HTML5, Modern ES6+ JavaScript (Client-side Hash Router SPA) |
| **Styling & CSS** | Tailwind CSS (CDN) + Custom Neubrutalism Utilities & Tokens |
| **Typography** | Google Fonts (*Plus Jakarta Sans* & *Space Mono*) |
| **Icons** | Font Awesome 6 Free CDN |
| **Syntax Highlighter** | Prism.js (Core + Autoloader & Language Components) |
| **Backend & Auth** | Supabase (User Auth & PostgreSQL Realtime Database) |
| **Database** | PostgreSQL |

---

## 📁 Project Structure

```text
CodeStash/
├── .gitignore          # Git ignore specifications
├── README.md           # Project documentation
└── index.html          # Entire SPA application (views, layout, router, styling & logic)
```

---

## 🚀 Getting Started

No build tools, bundlers, or package installations are required. You can run the project locally right away:

### 1. Clone the Repository
```bash
git clone https://github.com/Qaidy/codestash.git
cd codestash
```

### 2. Run Locally

Open `index.html` directly in your favorite web browser, or serve it using any local HTTP server:

- **Using Python 3:**
  ```bash
  python3 -m http.server 8000
  ```
- **Using Node.js (`npx serve`):**
  ```bash
  npx serve .
  ```
- **Using VS Code Live Server:**
  Right-click `index.html` and select **"Open with Live Server"**.

Visit the application at: `http://localhost:8000`

---

## 🗄️ Supabase Database Setup (Optional)

If you wish to connect your own Supabase project:

1. Create a new project on [Supabase](https://supabase.com).
2. Run the following script in the **SQL Editor**:

```sql
-- Create snippets table
CREATE TABLE public.snippets (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
    title TEXT NOT NULL,
    language TEXT NOT NULL,
    code_content TEXT NOT NULL,
    tags TEXT[] DEFAULT '{}',
    is_public BOOLEAN DEFAULT true,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Enable Row Level Security
ALTER TABLE public.snippets ENABLE ROW LEVEL SECURITY;

-- Policy: Everyone can view public snippets
CREATE POLICY "Public snippets are viewable by everyone" 
ON public.snippets FOR SELECT 
USING (is_public = true);

-- Policy: Authenticated users can view their own snippets (including private)
CREATE POLICY "Users can view own snippets" 
ON public.snippets FOR SELECT 
USING (auth.uid() = user_id);

-- Policy: Authenticated users can insert new snippets
CREATE POLICY "Authenticated users can insert snippets" 
ON public.snippets FOR INSERT 
WITH CHECK (auth.uid() = user_id);

-- Policy: Users can delete only their own snippets
CREATE POLICY "Users can delete own snippets" 
ON public.snippets FOR DELETE 
USING (auth.uid() = user_id);
```

3. Update `SUPABASE_URL` and `SUPABASE_ANON_KEY` inside the `<script>` tag in `index.html`:
```javascript
const SUPABASE_URL = "https://YOUR_PROJECT_ID.supabase.co";
const SUPABASE_ANON_KEY = "YOUR_ANON_PUBLIC_KEY";
```

---

## 📄 License

Distributed under the **MIT** License. Feel free to use, modify, and distribute for personal or commercial projects.
