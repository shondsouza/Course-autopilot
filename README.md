# Course Pilot 🚀

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

## CLI Usage

The CLI mode runs `automation.js` directly and requires credentials in a `.env` file.

Create a `.env` file with:

```env
VTU_EMAIL=your-email@example.com
VTU_PASSWORD=your-password
VTU_COURSE_SLUG=1-social-networks
VTU_BATCH_SIZE=10
VTU_MAX_ATTEMPTS=100
```

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

## Configuration

Supported environment variables:

- `PORT` — HTTP port (default: `3000`)
- `VTU_API_BASE_URL` — VTU API base URL (default: `https://online.vtu.ac.in/api/v1`)
- `CORS_ORIGIN` — allowed CORS origin (default: `*`)
- `DEFAULT_BATCH_SIZE` — lectures processed per batch (default: `10`)
- `DEFAULT_MAX_ATTEMPTS` — max retry rounds (default: `50`)
- `RETRY_DELAY_MS` — delay before retrying VTU requests (default: `2000`)
- `REQUEST_DELAY_MS` — delay between lecture requests (default: `500`)
- `MAX_RETRIES` — retry attempts for transient errors (default: `10`)
- `MAX_CONCURRENT` — concurrent jobs processed by the server (default: `2`)
- `GITHUB_URL` — optional GitHub repo URL shown in stats
- `KV_REST_API_URL` — Upstash Redis REST URL for stats persistence
- `KV_REST_API_TOKEN` — Upstash Redis token for stats persistence
- `ADMIN_PASSWORD` — enable admin endpoints

> Note: the web server does not require VTU credentials in `.env`. The web UI sends credentials in the API request body.

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

Response:

```json
{
  "jobId": "uuid-here",
  "position": 1
}
```

### Job progress

`GET /api/status/:jobId`

Returns Server-Sent Events for live progress updates.

### Stats

`GET /api/stats`

Returns usage statistics and optional GitHub URL.

### Queue status

`GET /api/queue`

Returns current queue depth and active job count.

---

## Admin Endpoints

Set `ADMIN_PASSWORD` in `.env` to enable admin routes.

- `GET /api/admin/config?password=<pw>` — view runtime config
- `GET /api/admin/config?password=<pw>&batchSize=20` — update runtime config
- `GET /api/admin/monitor?password=<pw>` — inspect active and queued jobs
- `GET /api/admin/notification?password=<pw>&message=...&disabled=false` — view or update notification state

---

## Notes

- Credentials are held only in memory during a job and are not persisted to Redis or disk.
- Queued jobs will fail after a server restart because credentials are ephemeral.
- Jobs are retained for up to 1 hour for status replay.
- Redis is optional; the app falls back to in-memory stats if `KV_REST_API_URL` / `KV_REST_API_TOKEN` are not set.

---

## Project Structure

- `index.js` — CLI entrypoint
- `server.js` — Express server, API, queue, SSE, and admin support
- `automation.js` — VTU login and lecture progress automation
- `lib/redis.js` — optional Redis stats and persistence helpers
- `frontend/index.html` — browser UI
- `public/` — static assets

---

## License

MIT License
