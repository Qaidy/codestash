# 💾 CodeStash — Windows 95 Nostalgia Code Snippet Manager

<p align="center">
  <img src="https://img.shields.io/badge/OS_Style-Windows_95-000080?style=for-the-badge&logoColor=white&labelColor=008080" alt="Windows 95 Style" />
  <img src="https://img.shields.io/badge/Theme-Retro_90s-C0C0C0?style=for-the-badge&logoColor=000&labelColor=808080" alt="Retro 90s Theme" />
  <img src="https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript" />
  <img src="https://img.shields.io/badge/Tailwind_CSS-CDN-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white" alt="Tailwind CSS" />
  <img src="https://img.shields.io/badge/Supabase-Database_%26_Auth-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white" alt="Supabase" />
  <img src="https://img.shields.io/badge/Prism.js-Syntax_Highlight-2A2B2E?style=for-the-badge&logo=codeigniter&logoColor=white" alt="Prism.js" />
  <img src="https://img.shields.io/badge/License-MIT-black?style=for-the-badge" alt="MIT License" />
</p>

<p align="center">
  <b>🌐 Welcome to the World Wide Web! Best viewed in 800×600 with Netscape Navigator 3.0+ 🌐</b>
</p>

---

## 📌 Overview

**CodeStash.EXE** is a fast, lightweight, and fully functional Single Page Application (SPA) designed for developers to save, organize, and share code snippets — wrapped in an authentic, nostalgic **Windows 95 & Retro 90s Web** design system.

Forget modern flat design. CodeStash brings back:
- 🪟 **Authentic 3D Beveled Windows**: Pure `#FFFFFF` highlight and `#000000` / `#808080` shadow borders.
- 🎨 **Classic Color Palette**: Windows 95 Teal desktop (`#008080`), Silver dialogs (`#C0C0C0`), and Deep Navy active title bars (`#000080`).
- 📟 **90s Nostalgia Badges & Micro-Interactions**: Scrolling marquee announcement ticker, animated rainbow text, blinking caret, 7-segment LED hit counter, and yellow/black "Under Construction" hazard stripes.
- 💾 **Custom 3.5" Floppy Disk Favicon**: Handcrafted SVG favicon styled like a real vintage 1.44MB diskette with sliding metallic shutter.

Built completely in a **single portable file (`index.html`)** with **zero build tools or compilation steps**, powered by modern **ES6+ JavaScript** and backed by **Supabase** (PostgreSQL Database & Authentication).

---

## ✨ Features

### 🎨 Retro Windows 95 UI/UX Experience
- **Classic Titlebars & Dialog Boxes**: Complete with minimize (`_`), maximize (`□`), and close (`×`) decorative buttons.
- **Outset & Sunken 3D Bevels**: Realistic Windows 95 push buttons that physically depress when clicked (`:active` inset bevel shift).
- **Custom Win95 Scrollbars**: Classic gray thumbs with up/down arrow buttons.
- **Marquee & Rainbow Accents**: Dynamic vintage web elements with full `prefers-reduced-motion` accessibility support.
- **Floppy Disk Brand Identity**: 3.5" diskette iconography throughout the interface.

### ⚡ Developer & Snippet Management
- **Instant Category Filtering**: Quick 1-click sidebar navigation supporting 14+ languages:
  - 🌐 HTML & CSS
  - 🟨 JavaScript & TypeScript
  - 🎯 Dart / Flutter
  - 🐘 PHP / Laravel
  - 🐍 Python
  - 🗄️ SQL Database
  - ☕ Java & C#
  - 🐹 Go & 🦀 Rust
  - 💻 Bash / Shell
  - 📄 JSON
- **Real-Time Live Search**: Multi-field instant search across snippet titles, code bodies, and tags without page reload.
- **Privacy Controls**:
  - **Public Snippets**: Shared with the community (features optional Admin moderation).
  - **Private Snippets**: Restricted strictly to your personal authenticated account.
- **Syntax Highlighting**: Syntax highlighting powered by **Prism.js** inside retro terminal code viewports.
- **1-Click Copy**: Instant clipboard copying with tactile retro visual feedback.
- **Admin Moderation Portal**: Dedicated dashboard for administrators to review, approve, or reject pending public snippets.
- **Responsive Mobile Drawer**: Seamlessly shrinks into a retro Windows mobile drawer on smartphones and tablets.

---

## 🛠️ Tech Stack

| Category | Technology | Description |
| :--- | :--- | :--- |
| **Architecture** | Single-File SPA (HTML5 + ES6+ Vanilla JS) | Hash-based client router (`#/dashboard`, `#/snippet/:id`, etc.) |
| **Design System** | Custom Windows 95 Bevel System + Tailwind CSS CDN | 3D outset/inset bevel utilities, retro colors, zero border-radius |
| **Typography** | `Arial Black`, `Impact`, `Courier New` | Authentic system fonts with zero external font dependencies |
| **Backend & Auth** | Supabase Client JS | User authentication (Sign Up / Sign In) & PostgreSQL Realtime DB |
| **Syntax Highlighting** | Prism.js CDN | Multi-language syntax formatting |
| **Icons** | Font Awesome 6 CDN | Lightweight vector iconography |

---

## 📁 Project Structure

```text
CodeStash/
├── .gitignore          # Git ignore specifications
├── README.md           # Project documentation
└── index.html          # Entire standalone application (views, router, styles & logic)
```

---

## 🚀 Getting Started

No `npm install`, Node.js bundling, or build processes are required. Run it directly:

### 1. Clone the Repository
```bash
git clone https://github.com/Qaidy/codestash.git
cd codestash
```

### 2. Run Locally

Open `index.html` directly in your web browser, or launch a simple local HTTP server:

- **Using Python 3:**
  ```bash
  python3 -m http.server 8000
  ```
- **Using Node.js (`npx serve`):**
  ```bash
  npx serve .
  ```
- **Using VS Code:**
  Right-click `index.html` and select **"Open with Live Server"**.

Visit the application at: **`http://localhost:8000`**

---

## 🗄️ Supabase Database Setup

To connect your own Supabase backend:

1. Create a project at [supabase.com](https://supabase.com).
2. Go to the **SQL Editor** and execute:

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
    status TEXT DEFAULT 'approved', -- 'pending', 'approved', 'rejected'
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Enable Row Level Security
ALTER TABLE public.snippets ENABLE ROW LEVEL SECURITY;

-- Policy: Everyone can view approved public snippets
CREATE POLICY "Public snippets are viewable by everyone" 
ON public.snippets FOR SELECT 
USING (is_public = true AND (status = 'approved' OR status IS NULL));

-- Policy: Authenticated users can view their own snippets
CREATE POLICY "Users can view own snippets" 
ON public.snippets FOR SELECT 
USING (auth.uid() = user_id);

-- Policy: Authenticated users can insert new snippets
CREATE POLICY "Authenticated users can insert snippets" 
ON public.snippets FOR INSERT 
WITH CHECK (auth.uid() = user_id);

-- Policy: Users can update their own snippets
CREATE POLICY "Users can update own snippets" 
ON public.snippets FOR UPDATE 
USING (auth.uid() = user_id);

-- Policy: Users can delete only their own snippets
CREATE POLICY "Users can delete own snippets" 
ON public.snippets FOR DELETE 
USING (auth.uid() = user_id);
```

3. Update the credentials in `index.html` (around line 432):
```javascript
const SUPABASE_URL = "https://YOUR_PROJECT_ID.supabase.co";
const SUPABASE_ANON_KEY = "YOUR_ANON_PUBLIC_KEY";
```

---

## 📄 License

Distributed under the **MIT** License. Feel free to use, modify, and distribute for personal or commercial projects.
