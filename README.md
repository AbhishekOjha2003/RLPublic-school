# 🏫 Shri R.L. Chaudhary School (RL Public School)

> Official web portal and school management platform for **Shri R.L. Chaudhary School (R.L. Public School)**, Prayagraj, UP. Built with Next.js 15 (App Router), React 19, TypeScript, Tailwind CSS, and powered by serverless Google Sheets and Google Drive integrations.

[![Next.js](https://img.shields.io/badge/Next.js-15.3.8-black?style=flat&logo=next.js)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-19.0.0-61dafb?style=flat&logo=react)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue?style=flat&logo=typescript)](https://www.typescriptlang.org/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-v4-38bdf8?style=flat&logo=tailwind-css)](https://tailwindcss.com/)
[![Google Sheets](https://img.shields.io/badge/Google_Sheets-Integrated-34a853?style=flat&logo=googlesheets)](https://developers.google.com/sheets/api)

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [Pages & Modules](#-pages--modules)
- [System Architecture & Integrations](#-system-architecture--integrations)
  - [1. Academic Result Lookup System](#1-academic-result-lookup-system)
  - [2. Online Admission Application Flow](#2-online-admission-application-flow)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Environment Configuration](#environment-configuration)
  - [Running the Development Server](#running-the-development-server)
  - [Building for Production](#building-for-production)
- [Google Services Setup Guide](#-google-services-setup-guide)
  - [Result Portal Google Sheet Format](#result-portal-google-sheet-format)
  - [Admission Form Google Apps Script Webhook](#admission-form-google-apps-script-webhook)
- [Deployment](#-deployment)
- [Contact & School Information](#-contact--school-information)
- [License](#-license)

---

## 🌟 Overview

**Shri R.L. Chaudhary School (R.L. Public School)** is an educational institution located in Chilbila Bazaar, Manda, Prayagraj, Uttar Pradesh. Guided by the leadership of Director **T.L. Akela**, the school is committed to academic excellence, physical fitness, psychological and spiritual well-being, and nurturing Indian values and social responsibility.

This repository contains the full modern digital web portal of the school. It provides parents, students, faculty, and prospective applicants with a responsive, fast, and feature-complete online hub: from viewing campus galleries and checking fee structures, to registering online admissions and retrieving official academic exam results securely.

---

## ✨ Key Features

- 📱 **Fully Responsive Modern UI**: Optimized for mobile phones, tablets, and desktops using Tailwind CSS v4 and modern micro-animations.
- 🎓 **Online Academic Result Portal**: Secure, multi-parameter verification (Student Name, Class, and Date of Birth) connected to Google Sheets, dynamically serving official PDF report cards previewed from Google Drive.
- 📝 **Digital Admission Portal**:
  - Interactive admission application form capturing student details, parent Aadhar information, and past academic records.
  - Serverless admission processing submitting directly to Google Sheets via Google Apps Script webhooks.
  - Transparent fee structure breakdown and required document guidelines.
- 👥 **Faculty Directory**: Showcase of educators, their designations, qualifications, and department roles.
- 🖼️ **Interactive Campus Gallery**: Modal lightbox viewer showcasing school activities, Republic Day parades, student toppers, awards, newspaper coverage, and annual trips.
- 📢 **Announcements & Hero Slider**: Auto-rotating hero carousel with breaking announcements and director's address.
- 📍 **Contact & Campus Information**: Interactive contact channels, office hours, and administrative telephone lines.

---

## 🧭 Pages & Modules

| Route | Page | Description |
|---|---|---|
| `/` | **Home** | Hero slider, Director's Welcome Note, Vision & Mission, Faculty carousel, Image gallery with lightbox, and School entrance highlights. |
| `/about` | **About Us** | Institutional history (founding journey, milestones), educational philosophy, and mission statements. |
| `/admission` | **Admissions** | Online student admission form, comprehensive fee schedule (Admission, Prep to UKG, I to VIII, exam fees), and list of required admission documents. |
| `/result` | **Result Checker** | Student result lookup requiring Full Name, Class, and DOB. Displays roll number and embeds the official PDF mark sheet. |
| `/faculty` | **Faculty** | Grid directory of teachers, staff, and leadership with photos, designations, and academic profiles. |
| `/contact` | **Contact** | Campus address, administrative telephone lines (Principal, Director, Office), and contact inquiry form. |
| `/api/result` | **Result API** | Serverless Next.js Route Handler querying Google Sheets with JWT Service Account authentication. |

---

## 🏗️ System Architecture & Integrations

```
                          ┌───────────────────────────┐
                          │       Next.js 15 App      │
                          │   (React 19 / TypeScript) │
                          └─────────────┬─────────────┘
                                        │
             ┌──────────────────────────┴──────────────────────────┐
             ▼                                                     ▼
┌───────────────────────────┐                         ┌───────────────────────────┐
│       /api/result         │                         │   Admission Form (Client) │
│  (Next.js Route Handler)  │                         │ (src/components/admission)│
└────────────┬──────────────┘                         └─────────────┬─────────────┘
             │ JWT Service Account Auth                             │ POST (no-cors)
             ▼                                                     ▼
┌───────────────────────────┐                         ┌───────────────────────────┐
│     Google Sheets API     │                         │  Google Apps Script URL   │
│ (Academic Results Sheet)  │                         │       (/exec Webhook)     │
└────────────┬──────────────┘                         └─────────────┬─────────────┘
             │                                                     │
             ▼                                                     ▼
┌───────────────────────────┐                         ┌───────────────────────────┐
│     Google Drive API      │                         │  Admissions Google Sheet  │
│  (Report Card PDF Preview)│                         │     (Auto-Appended Row)   │
└───────────────────────────┘                         └───────────────────────────┘
```

### 1. Academic Result Lookup System
- Client enters: `Student Name`, `Class`, and `Date of Birth (DOB)`.
- Request hits `/api/result?Name=...&Class=...&DOB=...`.
- Server authenticates using a Google Service Account (`JWT`) via `google-spreadsheet`.
- The server loads the specified Google Sheet (`GOOGLE_SHEET_ID`), normalizes the DOB formats (`DD/MM/YYYY` or `YYYY-MM-DD`), and looks up the student.
- On match, the endpoint returns:
  ```json
  {
    "name": "Priya Sharma",
    "class": "10th",
    "rollno": "1024",
    "pdfUrl": "https://drive.google.com/file/d/<PDFFileID>/preview"
  }
  ```
- The frontend renders an inline Google Drive PDF viewer allowing parents to view or print the marksheet.

### 2. Online Admission Application Flow
- Parents submit the detailed admission form.
- The form dispatches an asynchronous `POST` request directly to a Google Apps Script Web App (`/exec` endpoint).
- The script appends the student's personal details, parental IDs, address, and contact numbers directly to the admissions ledger sheet.

---

## 🛠️ Tech Stack

- **Framework**: [Next.js 15.3.8](https://nextjs.org/) (App Router, Turbopack)
- **Library**: [React 19.0.0](https://react.dev/) / [React DOM 19.0.0](https://react.dev/)
- **Styling**: [Tailwind CSS v4](https://tailwindcss.com/)
- **Icons & UI**: [Lucide React](https://lucide.dev/), [Heroicons](https://heroicons.com/)
- **Notifications**: [React Toastify](https://fkhadra.github.io/react-toastify/)
- **Backend & Cloud**:
  - `google-spreadsheet` (`v4.1.5`)
  - `google-auth-library` (`v10.1.0`)
  - Next.js Serverless Route Handlers
- **Language**: [TypeScript 5](https://www.typescriptlang.org/)
- **Deployment**: [Vercel](https://vercel.com/)

---

## 📁 Project Structure

```text
RLPublic-school/
├── public/
│   └── images/
│       ├── about/           # School history, assembly & campus images
│       ├── faculty/         # Photos of faculty and administrative staff
│       ├── gallary/         # Republic day, awards, tours, toppers, newspapers
│       └── home/            # Hero banners, logo, vision & entrance graphics
├── src/
│   ├── app/
│   │   ├── about/           # About Us page route
│   │   ├── admission/       # Admission form & fee structure route
│   │   ├── api/
│   │   │   └── result/      # API route handler for Google Sheets result query
│   │   ├── contact/         # Contact us page route
│   │   ├── faculty/         # Faculty roster page route
│   │   ├── result/          # Student result lookup page route
│   │   ├── globals.css      # Global styles and Tailwind configuration
│   │   ├── layout.tsx       # Root layout with Navbar and Footer wrappers
│   │   └── page.tsx         # Home landing page
│   └── components/
│       ├── about_us/        # SchoolHistory, MissionSection, AboutHero
│       ├── admission/       # AdmissionForm, FeeStructureSection, AdmissionHero
│       ├── faculty/         # FacultyGrid, FacultyHero
│       ├── home/            # Hero, WelcomeNote, VisionMission, OurFaculty, Gallery, SchoolEntrance
│       ├── result/          # ResultChecker, ResultHero
│       └── shared/          # Navbar, Footer
├── .env.example             # Template for required environment variables
├── eslint.config.mjs        # ESLint configuration
├── next.config.ts           # Next.js configuration
├── package.json             # Dependencies and scripts
├── postcss.config.mjs       # PostCSS configuration
├── tsconfig.json            # TypeScript configuration
└── README.md                # Project documentation
```

---

## 🚀 Getting Started

### Prerequisites

Ensure you have the following installed on your local development machine:
- **Node.js**: `v18.18.0` or higher (Node 20+ recommended)
- **npm**, **yarn**, **pnpm**, or **bun**

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/AbhishekOjha2003/RLPublic-school.git
   cd RLPublic-school
   ```

2. **Install dependencies:**
   ```bash
   npm install
   # or
   yarn install
   # or
   pnpm install
   ```

### Environment Configuration

Create a `.env.local` file in the root directory by copying `.env.example`:

```bash
cp .env.example .env.local
```

Populate the required credentials in `.env.local`:

```env
# Google Cloud Service Account Credentials
GOOGLE_CLIENT_EMAIL=your-service-account@your-project.iam.gserviceaccount.com
GOOGLE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\nMIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQC...\n-----END PRIVATE KEY-----\n"
GOOGLE_SHEET_ID=your_google_spreadsheet_id_here
```

### Running the Development Server

Start the Next.js development server with Turbopack:

```bash
npm run dev
```

Visit [http://localhost:3000](http://localhost:3000) in your browser.

### Building for Production

To create an optimized production build:

```bash
npm run build
npm run start
```

---

## 📊 Google Services Setup Guide

### Result Portal Google Sheet Format

For the `/result` page and `/api/result` route to function correctly, create a Google Spreadsheet and share it with your Service Account email (`GOOGLE_CLIENT_EMAIL`) with **Viewer** permissions.

The first sheet (`sheetsByIndex[0]`) must contain the following column headers in Row 1:

| StudentName | Class | DOB | RollNo | PDFFileID |
|---|---|---|---|---|
| Priya Sharma | 10th | 2008-05-15 | 1024 | 1a2B3c4D5e6F... |
| Amit Kumar | 8th | 12/08/2010 | 815 | 9z8Y7x6W5v4U... |

> **Notes**:
> - `DOB` can be formatted as either `YYYY-MM-DD` or `DD/MM/YYYY`. The API automatically standardizes it during lookup.
> - `PDFFileID` is the Google Drive File ID of the uploaded report card PDF (e.g. from `https://drive.google.com/file/d/<FILE_ID>/view`). Ensure the PDF's share settings are set to **"Anyone with the link can view"**.

---

### Admission Form Google Apps Script Webhook

The Admission form in `src/components/admission/AdmissionForm.tsx` sends data to Google Apps Script. To set up your own destination sheet:

1. Create a new Google Sheet for admissions.
2. Go to **Extensions** → **Apps Script**.
3. Paste the following script:
   ```javascript
   function doPost(e) {
     var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
     var data = JSON.parse(e.postData.contents);
     
     sheet.appendRow([
       new Date(),
       data.scholarName,
       data.studentAadhar,
       data.fatherName,
       data.fatherAadharNumber,
       data.motherName,
       data.motherAadharNumber,
       data.parentsAddress,
       data.dateOfBirth,
       data.religion,
       data.classAdmission,
       data.stayDuration,
       data.lastInstitution1,
       data.motherTongue,
       data.mobileNumber,
       data.whatsappNumber,
       data.dateOfApplication
     ]);
     
     return ContentService.createTextOutput(JSON.stringify({ status: "success" }))
       .setMimeType(ContentService.MimeType.JSON);
   }
   ```
4. Click **Deploy** → **New deployment** → Select type **Web app**.
5. Set **Execute as**: `Me`, and **Who has access**: `Anyone`.
6. Copy the resulting Web App URL (ends with `/exec`) and configure it in `src/components/admission/AdmissionForm.tsx`.

---

## 🚢 Deployment

The easiest way to deploy this Next.js project is via the [Vercel Platform](https://vercel.com/):

1. Push your repository to GitHub.
2. Import the project into [Vercel](https://vercel.com/new).
3. Under **Environment Variables**, add:
   - `GOOGLE_CLIENT_EMAIL`
   - `GOOGLE_PRIVATE_KEY`
   - `GOOGLE_SHEET_ID`
4. Click **Deploy**.

---

## 📞 Contact & School Information

- **School**: Shri R.L. Chaudhary School (R.L. Public School)
- **Director**: T.L. Akela
- **Address**: Near Main Market, Chilbila Bazaar, Manda, Prayagraj, Uttar Pradesh - 212104, India
- **Office Contact**: +91 77964 17508
- **Principal Contact**: +91 94505 89446
- **Director Contact**: +91 88741 71606

---

## 📄 License

This project is licensed under the terms of the private institutional license or specified by the school administration. All rights reserved © Shri R.L. Chaudhary School.
