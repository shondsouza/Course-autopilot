# Course Pilot 

automates VTU course lecture completion with a local web UI, CLI, and queue-backed server.

Course Pilot logs in to VTU, fetches your course lectures, and submits lecture progress in batches with retry handling.

---

## Features

- ✅ Web UI for easy local access
- ✅ CLI mode for direct automation
- ✅ Live progress via Server-Sent Events (SSE)
- ✅ Queueing with configurable concurrency
- ✅ Automatic session refresh for VTU auth failures
- ✅ Optional Redis-backed stats and persistence
- ✅ Runtime admin config support

---

## Quickstart

### 1. Install

```bash
git clone https://github.com/vikas-bhat-d/vtu-course-automation.git
cd vtu-course-automation
npm install
```

### 2. Run the server

```bash
npm run serve
```

### 3. Open the web UI

Open `http://localhost:3000` in your browser.

Enter your VTU email, password, and course slug, then submit.

---

## Web UI Usage (Recommended)

The web UI posts jobs to `/api/submit` and tracks progress via `/api/status/:jobId`.

### Course slug example

For this VTU course URL:

`https://online.vtu.ac.in/courses/1-social-networks`

The course slug is:

`1-social-networks`

---

Then run:

```bash
npm start
```

---

## Development Mode

Use auto-reload while developing:

```bash
npm run dev
```

---
---

## API Reference

### Submit a job

`POST /api/submit`

Request body:

```json
{
  "email": "you@example.com",
  "password": "your-password",
  "courseSlug": "1-social-networks"
}
```

