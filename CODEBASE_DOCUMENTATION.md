# VOXANI - Complete Codebase Documentation

> **A full guide to how the Voxani Anime Streaming Website works — explained so simply that even a kid can understand!**

---

## Table of Contents

1. [What is Voxani?](#1-what-is-voxani)
2. [The Big Picture — How the Website is Built](#2-the-big-picture--how-the-website-is-built)
3. [Project Structure — Where Everything Lives](#3-project-structure--where-everything-lives)
4. [Configuration Files — The Rule Books](#4-configuration-files--the-rule-books)
5. [The Starting Point — How the App Boots Up](#5-the-starting-point--how-the-app-boots-up)
6. [Routing — The Road Map of Pages](#6-routing--the-road-map-of-pages)
7. [Pages — Each Screen You Can Visit](#7-pages--each-screen-you-can-visit)
8. [Components — Reusable Building Blocks](#8-components--reusable-building-blocks)
9. [Services — Talking to the Outside World](#9-services--talking-to-the-outside-world)
10. [State Management — The Brain's Memory](#10-state-management--the-brains-memory)
11. [Supabase — The Database & Authentication](#11-supabase--the-database--authentication)
12. [Styling — Making Everything Look Pretty](#12-styling--making-everything-look-pretty)
13. [How Key Features Work End-to-End](#13-how-key-features-work-end-to-end)
14. [Mobile App (Capacitor)](#14-mobile-app-capacitor)
15. [Deployment](#15-deployment)
16. [Glossary of Terms](#16-glossary-of-terms)

---

## 1. What is Voxani?

**Voxani** is an anime streaming website. Think of it like Netflix, but specifically for anime!

Here's what you can do on Voxani:
- **Browse** thousands of anime shows and movies
- **Watch** episodes directly in your web browser
- **Search** for any anime you want
- **Track** which episodes you've watched (it remembers where you stopped!)
- **Create a list** of anime you want to watch later
- **See schedules** of when new episodes come out
- **Discover** anime by genre, mood, or popularity
- **Sign up / Log in** to save your progress across devices
- **Get random anime** suggestions when you can't decide what to watch!

---

## 2. The Big Picture — How the Website is Built

### What is a "Tech Stack"?

A **tech stack** is like a recipe — it lists all the tools and ingredients used to build the website. Here's what Voxani uses:

| Tool | What It Does | Simple Explanation |
|------|-------------|-------------------|
| **React** | Frontend Framework | The main tool that builds what you see on screen. Like LEGO blocks for websites. |
| **Vite** | Build Tool | A super-fast helper that packages all the code and lets developers test the website quickly. Like a chef's oven that cooks the code. |
| **Tailwind CSS** | Styling | Makes the website look pretty using shortcut class names instead of writing long style rules. |
| **Supabase** | Database + Auth | The "brain" that stores user data (accounts, watch progress, watchlists) in the cloud. |
| **Zustand** | State Management | A tiny library that lets different parts of the website share information (like "is the user logged in?"). |
| **React Router** | Navigation | Lets you click links and move between different pages without reloading the whole website. |
| **Framer Motion** | Animations | Makes things slide, fade, and bounce smoothly — all those cool effects you see. |
| **Axios** | HTTP Client | Sends messages to external servers (APIs) to fetch anime data. Like a messenger pigeon. |
| **HLS.js** | Video Streaming | Plays high-quality video streams (the actual anime episodes). |
| **Swiper** | Carousel/Slider | Creates those sliding image galleries (the hero section on the home page). |
| **React Toastify** | Notifications | Shows little popup messages like "Added to list!" at the bottom of the screen. |
| **Vercel** | Hosting | The service that puts the website on the internet so everyone can visit it. |

### How It All Fits Together

```
┌─────────────────────────────────────────────────────────────┐
│                     YOUR WEB BROWSER                         │
│                                                              │
│  ┌───────────────────────────────────────────────────────┐   │
│  │                    REACT APP (Voxani)                  │   │
│  │                                                        │   │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────────────────┐ │   │
│  │  │  Pages   │  │Components│  │  State (Zustand)     │ │   │
│  │  │(screens) │  │(reusable │  │  - auth info         │ │   │
│  │  │          │  │ pieces)  │  │  - watchlist          │ │   │
│  │  └────┬─────┘  └────┬─────┘  │  - settings          │ │   │
│  │       │              │        └──────────────────────┘ │   │
│  │       └──────┬───────┘                                 │   │
│  │              │                                         │   │
│  │      ┌───────▼───────┐                                 │   │
│  │      │   Services    │                                 │   │
│  │      │  (API calls)  │                                 │   │
│  │      └───────┬───────┘                                 │   │
│  └──────────────┼────────────────────────────────────────┘   │
│                 │                                            │
└─────────────────┼────────────────────────────────────────────┘
                  │ (Internet)
    ┌─────────────▼──────────────┐    ┌────────────────────┐
    │   Anime API Server         │    │   Supabase Cloud   │
    │  (itzzmeapi.vercel.app)    │    │  (Database + Auth) │
    │  - anime info              │    │  - user accounts   │
    │  - episodes                │    │  - watch progress  │
    │  - streaming links         │    │  - watchlists      │
    └────────────────────────────┘    └────────────────────┘
```

**In plain English:** When you open Voxani, your browser loads the React app. The app asks two external services for data:
1. **The Anime API** — for all anime information (titles, images, episodes, streams)
2. **Supabase** — for your personal data (account, what you've watched, your list)

---

## 3. Project Structure — Where Everything Lives

Think of the project folder like a house. Each room has a specific purpose:

```
voxani-anime-streaming-main/
│
├── index.html              ← The front door. The very first file that loads in your browser.
├── package.json            ← The shopping list. Lists all tools/libraries the project needs.
├── vite.config.js          ← Instructions for the build tool (Vite).
├── tailwind.config.js      ← Rules for how the design system (Tailwind) works.
├── postcss.config.js       ← Helper for processing CSS styles.
├── vercel.json             ← Instructions for the hosting service (Vercel).
├── supabase-schema.sql     ← Blueprint for the database tables.
│
├── public/                 ← Static files (images, icons) served directly.
│
├── src/                    ← THE MAIN CODE — where all the magic happens!
│   ├── main.jsx            ← The ignition key. Starts the whole React app.
│   ├── App.jsx             ← The main switchboard. Decides which page to show.
│   ├── index.css           ← Global styles (colors, fonts, animations).
│   │
│   ├── components/         ← Reusable building blocks (like LEGO pieces)
│   │   ├── ImmersiveLayout.jsx    ← The main page wrapper (sidebar + header + content)
│   │   ├── SpotifySidebar.jsx     ← The side menu (desktop)
│   │   ├── MobileNav.jsx          ← Bottom navigation bar (mobile phones)
│   │   ├── Navbar.jsx             ← Top navigation bar
│   │   ├── HeroCarousel.jsx       ← The big sliding banner on the home page
│   │   ├── AnimeCard.jsx          ← A single anime poster card
│   │   ├── AnimeCardSmall.jsx     ← A smaller version of the anime card
│   │   ├── AnimeRow.jsx           ← A horizontal scrolling row of anime cards
│   │   ├── SearchOverlay.jsx      ← The search popup
│   │   ├── VideoPlayer.jsx        ← Custom video player with controls
│   │   ├── IframePlayer.jsx       ← Embedded video player (from external source)
│   │   ├── ContinueWatching.jsx   ← "Continue Watching" section
│   │   ├── BentoGrid.jsx          ← Grid layout for featured content
│   │   ├── MoodSelector.jsx       ← "What mood are you in?" selector
│   │   ├── ListTabs.jsx           ← Tab buttons (Watching, Planning, Completed, etc.)
│   │   ├── PageTransition.jsx     ← Smooth fade effects between pages
│   │   ├── PageLoader.jsx         ← Loading spinner with the "V" logo
│   │   ├── Layout.jsx             ← Alternative page layout wrapper
│   │   └── Sidebar.jsx            ← Alternative sidebar component
│   │
│   ├── pages/              ← Full screens/pages (each URL shows a different page)
│   │   ├── Landing.jsx           ← The welcome/intro page (first thing you see)
│   │   ├── Home.jsx              ← Main home page (trending, popular anime)
│   │   ├── Discover.jsx          ← Browse anime by category
│   │   ├── Browse.jsx            ← Advanced search with filters
│   │   ├── AnimeDetails.jsx      ← Detailed info about one anime
│   │   ├── Watch.jsx             ← The video watching page
│   │   ├── Search.jsx            ← Search results page
│   │   ├── Login.jsx             ← Sign in / Sign up page
│   │   ├── Profile.jsx           ← User profile page
│   │   ├── MyList.jsx            ← Your saved anime list
│   │   ├── Schedule.jsx          ← Weekly airing schedule
│   │   ├── Genre.jsx             ← Browse anime by genre
│   │   ├── RandomAnime.jsx       ← Random anime picker
│   │   ├── AuthCallback.jsx      ← Handles email verification redirect
│   │   └── NotFound.jsx          ← 404 page (when a page doesn't exist)
│   │
│   ├── services/           ← Functions that talk to external APIs
│   │   ├── api.js                ← All anime data API calls
│   │   └── anilist.js            ← AniList integration (external anime database)
│   │
│   ├── store/              ← App-wide state (shared memory)
│   │   └── index.js              ← Zustand stores (auth, watchlist, settings, UI)
│   │
│   └── lib/                ← Utility/helper libraries
│       └── supabase.js           ← All Supabase functions (auth, database queries)
│
└── my-app/                 ← Mobile app version (Capacitor wrapper)
    ├── capacitor.config.json
    └── src/
```

---

## 4. Configuration Files — The Rule Books

### `package.json` — The Shopping List

This file is like a shopping list for the project. It tells the computer:
- **What libraries to download** (dependencies) — things like React, Axios, Framer Motion
- **What commands to run** — like `npm run dev` to start the development server
- **The project name** — "voxani"

**Key dependencies explained:**
```
"react"                    → The core framework (builds the user interface)
"react-dom"                → Connects React to the web browser
"react-router-dom"         → Enables page navigation (clicking between pages)
"@supabase/supabase-js"    → Talks to the Supabase database
"axios"                    → Sends HTTP requests to fetch anime data
"zustand"                  → Manages shared state (like a brain for the app)
"framer-motion"            → Makes smooth animations
"hls.js"                   → Plays HLS video streams (the actual anime video)
"swiper"                   → Creates sliding carousels
"react-toastify"           → Shows popup notification messages
"react-icons"              → Provides thousands of icons (play buttons, hearts, etc.)
"artplayer"                → Another video player library
"lodash.debounce"          → Delays rapid function calls (used in search)
```

### `vite.config.js` — The Oven Settings

Vite is the tool that **builds** the website. This file tells it:
- Use the **React plugin** (so it understands React code)
- Use `@` as a shortcut for the `src/` folder (so you can write `@/components` instead of `../../components`)
- Run the development server on **port 3000**
- **Open the browser** automatically when you start the dev server
- **Proxy** API requests to `https://itzzmeapi.vercel.app` (this means when the code asks for `/api/something`, Vite forwards it to the anime API server)

```javascript
export default defineConfig({
  plugins: [react()],           // Enable React support
  resolve: {
    alias: { '@': '/src' },     // Shortcut: @/ means src/
  },
  server: {
    port: 3000,                 // Website runs on localhost:3000
    open: true,                 // Auto-opens browser
    proxy: {
      '/api': {
        target: 'https://itzzmeapi.vercel.app',  // Forward API calls here
        changeOrigin: true,
      },
    },
  },
})
```

### `tailwind.config.js` — The Design System

Tailwind CSS lets you style things by adding class names directly in your HTML/JSX. This config file customizes the design system for Voxani:

**Custom Colors (the green theme):**
```
bg-bg         → #0a0f0a  (very dark green-black background)
bg-surface    → #0c1210  (slightly lighter background)
bg-card       → #121a16  (card backgrounds)
accent        → #359772  (the main green accent color)
accent-light  → #72c490  (lighter green)
accent-mint   → #99dabf  (mint/teal green)
```

**Custom Fonts:**
```
Inter         → The main text font (clean and readable)
Space Grotesk → Used for headings and the logo (more stylish)
```

**Custom Animations:**
```
fade-in       → Elements smoothly appear
fade-up       → Elements slide up while appearing
slide-in-left → Elements slide in from the left
scale-in      → Elements grow from small to normal size
shimmer       → The loading skeleton shimmer effect
pulse-glow    → A gentle glowing pulse
```

### `vercel.json` — Hosting Instructions

This tiny file tells Vercel (the hosting service):
- For **any URL** that isn't `/api/*`, serve `index.html`
- This is needed because Voxani is a **Single Page Application (SPA)** — all pages are actually one HTML file, and React handles showing the right content

```json
{
  "rewrites": [
    { "source": "/((?!api).*)", "destination": "/index.html" }
  ]
}
```

### `postcss.config.js` — CSS Processing

PostCSS processes CSS files. This config says:
- Use **Tailwind CSS** (to convert Tailwind class names into actual CSS)
- Use **Autoprefixer** (automatically adds browser-specific CSS prefixes like `-webkit-` so styles work everywhere)

---

## 5. The Starting Point — How the App Boots Up

When someone visits the Voxani website, here's exactly what happens, step by step:

### Step 1: `index.html` loads

This is the **front door**. The browser loads this file first. It contains:
- Meta tags (title, description, theme color)
- Font imports (Inter and Space Grotesk from Google Fonts)
- A `<div id="root"></div>` — this empty box is where React will put the entire website
- A service worker registration (for offline/PWA support)
- A `<script>` tag that loads `main.jsx`

### Step 2: `main.jsx` runs

This is the **ignition key**. It:
1. Finds the `<div id="root">` in the HTML
2. Creates a React "root" inside it
3. Wraps the entire app in several providers:
   - `React.StrictMode` — helps catch bugs during development
   - `BrowserRouter` — enables page navigation
   - `App` — the actual application
   - `Analytics` — tracks page visits (Vercel Analytics)
   - `ToastContainer` — the notification popup system

```jsx
ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
      <Analytics />
      <ToastContainer position="bottom-right" theme="dark" />
    </BrowserRouter>
  </React.StrictMode>,
)
```

### Step 3: `App.jsx` takes over

This is the **main switchboard**. It does two important things:

1. **Sets up authentication monitoring** — Checks if the user is logged in when the app starts, and keeps listening for login/logout events
2. **Defines all the routes** — Maps every URL to a page component

---

## 6. Routing — The Road Map of Pages

React Router is like a GPS for the website. When you type a URL or click a link, it figures out which page to show.

Here's every route in Voxani:

| URL Pattern | Page Component | Layout | Description |
|-------------|---------------|--------|-------------|
| `/` | `Landing` | None (standalone) | The welcome/intro page |
| `/home` | `Home` | ImmersiveLayout | Main home page with trending anime |
| `/discover` | `Discover` | ImmersiveLayout | Browse by category |
| `/browse` | `Browse` | ImmersiveLayout | Advanced filter/search |
| `/my-list` | `MyList` | ImmersiveLayout | Your saved anime list |
| `/anime/:id` | `AnimeDetails` | ImmersiveLayout | Details about a specific anime |
| `/watch/:id` | `Watch` | None | Video player page |
| `/watch/:id/:episode` | `Watch` | None | Video player for a specific episode |
| `/search` | `Search` | ImmersiveLayout | Search results |
| `/login` | `Login` | PageTransition | Sign in / Sign up |
| `/auth/callback` | `AuthCallback` | None | Email verification handler |
| `/profile` | `Profile` | ImmersiveLayout | User profile |
| `/schedule` | `Schedule` | ImmersiveLayout | Weekly airing schedule |
| `/genre/:genre` | `Genre` | ImmersiveLayout | Anime by genre |
| `/random` | `RandomAnime` | ImmersiveLayout | Random anime picker |
| `/watchlist` | Redirects to `/my-list` | — | Old URL redirect |
| `*` (anything else) | `NotFound` | ImmersiveLayout | 404 error page |

**What does `:id` mean?** The colon means it's a **variable**. So `/anime/one-piece` and `/anime/naruto` both go to the same `AnimeDetails` page, but show different anime. The `:id` part changes.

**What is `ImmersiveLayout`?** Most pages are wrapped in this layout, which gives them:
- A sidebar on the left (desktop)
- A top header bar with search
- A bottom navigation bar (mobile)
- Smooth page transition animations

The **Watch page** has NO layout — it's a fullscreen video experience.

### How `AnimatePresence` works:

The `App.jsx` wraps all routes in Framer Motion's `AnimatePresence`, which enables **exit animations**. When you navigate from one page to another:
1. The current page fades out
2. The new page fades in

This creates a smooth, polished feel.

---

## 7. Pages — Each Screen You Can Visit

### 7.1 `Landing.jsx` — The Welcome Page

**What it is:** The very first page you see when you visit Voxani for the first time.

**What it does:**
1. **Shows an intro animation** — A 3.5-second splash screen with the Voxani logo, a "Made with love by the VOXURA team" message, and a green loading bar. This only plays once per browser session (it saves a flag in `sessionStorage`).
2. **Displays a hero section** — A big background image with the Voxani branding, a tagline, and buttons to enter the app.
3. **Shows feature highlights** — Cards explaining features like "Mood-Based Discovery," "Smart Progress Tracking," and "Personal Lists."
4. **Has a "Get Started" button** — Takes you to `/home`.

**Key code concepts:**
- Uses `sessionStorage` to track if the intro was already shown (so it doesn't replay every time)
- Green glowing background effects using CSS blur and gradients
- Framer Motion animations for smooth entrance effects

---

### 7.2 `Home.jsx` — The Main Home Page

**What it is:** The Netflix-style home page you see after landing. The heart of the app.

**What it does:**
1. **Fetches data** from the anime API using `getHomePage()` and `getTopTen()`
2. **Shows a hero section** — A big carousel (slideshow) of spotlighted anime with:
   - Background image with parallax scrolling effect
   - Title, description, genre tags
   - "Watch Now" and "Add to List" buttons
   - Auto-advances every 7 seconds
3. **Shows "Continue Watching"** — If you've been watching something, it shows those anime first
4. **Shows rows of anime** — Horizontal scrolling rows like:
   - 🔥 Trending Now
   - ⭐ Top Airing
   - 📈 Most Popular
   - ❤️ Most Favorite
   - 🆕 Latest Episodes
   - ✅ Latest Completed

**How the Hero Section works:**
- It receives `spotlights` (featured anime) from the API
- Uses `useState` to track which spotlight is currently showing
- A `setInterval` changes the active spotlight every 7 seconds
- Uses Framer Motion's `useScroll` for parallax (the background moves slower than you scroll, creating a 3D effect)
- Shows status dropdown to add anime with different statuses (Watching, Planning, Completed, etc.)

**How each Anime Row works (`AnimeRow` component inside Home):**
- Has a `useRef` pointing to the scroll container
- Two arrow buttons scroll the container left/right by 400px
- `checkScroll()` determines whether to show/hide the arrows
- Each card shows: poster image, title, episode count, type (TV/Movie/etc.)
- Hovering reveals a "Play" button

---

### 7.3 `AnimeDetails.jsx` — The Anime Info Page

**What it is:** When you click on any anime, you see this page with all its details.

**What it does:**
1. **Reads the anime ID** from the URL using `useParams()` (e.g., `/anime/one-piece` → id = "one-piece")
2. **Fetches anime info and episodes** simultaneously using `Promise.all()`
3. **Displays:**
   - Banner image with gradient overlay
   - Title (English and Japanese)
   - Rating, type, episode count, duration, release date
   - Full description (expandable)
   - Genre tags (clickable — takes you to that genre page)
   - Season selector (if the anime has multiple seasons)
4. **Episode list** with:
   - Search/filter for episodes
   - Sort order (ascending/descending)
   - Green progress indicators showing which episodes you've watched
5. **Cast tab** — Shows character images and names (fetched separately)
6. **Related anime tab** — Finds similar anime based on shared genres
7. **Watchlist management** — Add/remove from your list with status selection

**Watch Progress Integration:**
- If logged in, fetches your watch progress from Supabase
- Shows a green progress bar under episodes you've partially watched
- Highlights the "Continue from EP X" button

---

### 7.4 `Watch.jsx` — The Video Player Page

**What it is:** The page where you actually watch anime episodes.

**This is the most important page!** Here's how it works:

1. **Reads URL parameters:**
   - `animeId` from the URL path (`/watch/one-piece`)
   - `episodeId` from query params (`?ep=12345`)

2. **Fetches data:**
   - Anime info (title, poster)
   - Episode list
   - Available servers for the current episode

3. **The Video Player:**
   - Uses **Megaplay iframe embeds** (`megaplay.buzz/stream/s-2/{episodeId}/{sub|dub}`)
   - The iframe is loaded with specific permissions (sandbox) to block popups/ads
   - Supports multiple servers: HD-1, HD-2, HD-3, HD-4
   - Supports **sub** (subtitled) and **dub** (dubbed) versions
   - Server preferences are saved in `localStorage`

4. **Episode Navigation:**
   - Shows all episodes in a side panel
   - Episode search/filter
   - Previous/Next episode buttons
   - Click any episode to switch to it (forces iframe remount with a `playerKey`)

5. **Watch Progress Tracking:**
   - When an episode starts, progress is saved locally AND to Supabase
   - A timer updates progress every 30 seconds
   - This data powers the "Continue Watching" section on the Home page

6. **Watchlist integration:**
   - Add/remove the current anime from your watchlist directly from the player

---

### 7.5 `Login.jsx` — Sign In / Sign Up Page

**What it is:** Where users create accounts or log in.

**Features:**
- **Two modes:** Sign In and Sign Up (toggle between them)
- **Sign Up** requires: email, password, username
- **Sign In** requires: email and password
- **Google Sign-In** — One-click login with Google (OAuth)
- **Guest Access** — Enter the app without an account (limited features)
- **Password Reset** — "Forgot password?" sends a reset email
- **Beautiful UI:**
  - Split layout: left side shows branding/image, right side has the form
  - On mobile, it's a single column
  - Animated with Framer Motion
- **Validation:** Checks if email is confirmed before allowing login

**How authentication works:**
1. User fills in the form and clicks Submit
2. `signIn()` or `signUp()` from `supabase.js` is called
3. If successful, the user data and token are saved to the Zustand auth store
4. For sign up: A confirmation email is sent. The user must click the link before logging in.
5. For Google: The user is redirected to Google's login page, then back to `/home`

---

### 7.6 `Browse.jsx` — Advanced Filter Page

**What it is:** A powerful search and filter page where you can find exactly the anime you want.

**Filters available:**
- **Keyword search** — Type any anime name
- **Type** — TV, Movie, OVA, ONA, Special, Music
- **Status** — Finished Airing, Currently Airing, Not Yet Aired
- **Rating** — G, PG, PG-13, R, R+, Rx
- **Score** — 1 through 10
- **Season** — Winter, Spring, Summer, Fall
- **Language** — Sub, Dub, Sub & Dub
- **Year Range** — From 1940 to current year+1
- **Genres** — 40+ genres like Action, Romance, Fantasy, etc.
- **Sort** — By score, name, release date, most watched, etc.

**How it works:**
1. Each filter updates a `filters` state object
2. URL search params are synced with filters (so you can share filtered URLs)
3. On filter change, `filterAnime()` or `searchAnime()` is called
4. Results display in a grid (can toggle between grid and list view)
5. Pagination with "Load More" or page navigation

---

### 7.7 `Discover.jsx` — Category Browser

**What it is:** A simpler browsing page where you pick a category and see anime in that category.

**Categories:** Top Airing, Most Popular, Most Favorite, Completed, Recently Updated, Recently Added, Movies, TV Series, OVA, ONA, Specials

**How it works:**
1. Click a category button at the top
2. The API is called with `getCategory(categoryName)`
3. Results are displayed in a responsive grid
4. "Load More" button for pagination
5. Includes a search bar for quick filtering
6. Toggle between grid and list view

---

### 7.8 `Profile.jsx` — User Profile

**What it is:** Shows your account info, watch history, and saved list.

**Features:**
- **Profile card** with avatar, name, email
- **Stats:** Number of anime in progress, in your list, total watch time
- **Two tabs:**
  - **Continue Watching** — Shows all in-progress anime, grouped by show. Each group is expandable to see individual episodes.
  - **My List** — All saved anime with their status
- **Remove from history** — Delete an anime from your watch history
- **Logout button**

**Data flow:**
- If logged in, fetches from Supabase (cloud)
- Falls back to local Zustand store data
- Groups episodes by anime for a clean view

---

### 7.9 `MyList.jsx` — Your Anime Collection

**What it is:** A dedicated page for managing your saved anime.

**Features:**
- **Filter tabs:** All, Watching, Planning, Completed, On Hold, Dropped
- **Continue Watching section** at the top (quick access)
- **Full watchlist** with status badges
- **Change status** — Click to update an anime's status
- **Remove** — Delete an anime from your list
- **Requires login** — Shows a "Sign in" prompt if not authenticated

---

### 7.10 `Schedule.jsx` — Airing Schedule

**What it is:** Shows which anime episodes air on each day of the week.

**How it works:**
1. Calculates dates for the current week (Sunday to Saturday)
2. Fetches schedule data for each day using `getSchedule(date)`
3. Shows a day selector:
   - Desktop: All 7 days visible as buttons
   - Mobile: Swipeable with left/right arrows
4. Displays anime airing on the selected day with:
   - Title
   - Air time
   - Episode number
   - Quick link to the anime page

---

### 7.11 `Genre.jsx` — Browse by Genre

**What it is:** Shows all anime in a specific genre.

**Features:**
- **Sidebar** (desktop) or **Scrollable pills** (mobile) with 40+ genres
- **Sort options:** Most Popular, Highest Rated, Recently Updated, Newest, Title A-Z
- **Responsive grid** of anime cards
- **Load More** pagination
- URL-based genre selection (`/genre/action`, `/genre/romance`)

---

### 7.12 `RandomAnime.jsx` — Random Picker

**What it is:** Can't decide what to watch? This page picks a random anime for you!

**How it works:**
1. Calls `getRandomAnime()` from the API
2. If successful, immediately redirects to `/anime/{randomId}`
3. If it fails, redirects to `/home`
4. Shows a loading spinner while fetching

---

### 7.13 `AuthCallback.jsx` — Email Verification Handler

**What it is:** A hidden page that handles the redirect after a user clicks the email confirmation link.

**How it works:**
1. User signs up → gets a confirmation email
2. User clicks the link → browser opens `/auth/callback`
3. This page checks for a valid session using Supabase
4. If session found → logs the user in and redirects to `/home`
5. If no session → redirects to `/login` with a success message

---

### 7.14 `NotFound.jsx` — 404 Page

**What it is:** Shown when someone visits a URL that doesn't exist.

**Fun features:**
- Big animated "404" text with gradient colors
- A bouncing 😵 emoji
- Message: *"Looks like this page went on a filler episode adventure"*
- Buttons: Go Home, Search Anime, Browse Genres

---

## 8. Components — Reusable Building Blocks

Components are like LEGO pieces that can be used on multiple pages.

### 8.1 `ImmersiveLayout.jsx` — The Main Wrapper

**What it does:** Wraps most pages in a consistent layout with:
- **SpotifySidebar** (desktop only, on the left)
- **Top header bar** with search button and user avatar
- **MobileNav** (mobile only, at the bottom)
- **SearchOverlay** (popup when you click search)
- **Page loading transitions**

**Responsive behavior:**
- Detects screen width with `window.innerWidth < 1024`
- If desktop: Shows sidebar, adjusts main content margin based on sidebar state (collapsed = 72px, expanded = 240px)
- If mobile: Shows bottom navigation, no sidebar, adds bottom padding

---

### 8.2 `SpotifySidebar.jsx` — Side Navigation (Desktop)

**What it does:** A sidebar inspired by Spotify's design. Sits on the left side of the screen.

**Features:**
- **Collapsible:** Click the hamburger menu to collapse to icon-only mode (72px) or expand (240px)
- **Navigation links:** Home, Discover, Browse, Random
- **Library section:** My List, Schedule
- **User profile** at the bottom (if logged in)
- **Active indicator:** The current page is highlighted with a green accent

**How collapsing works:**
- `sidebarCollapsed` state is stored in `useSettingsStore` (persisted in localStorage)
- When collapsed, only icons are shown; when expanded, icons + labels are shown
- The main content area adjusts its left padding to match

---

### 8.3 `MobileNav.jsx` — Bottom Navigation (Mobile)

**What it does:** A fixed bottom bar for phone screens with 5 buttons:
- Home, Discover, Browse, My List, Random

**How active state works:** Compares the current URL path with each button's path using `useLocation()`.

---

### 8.4 `HeroCarousel.jsx` — The Big Sliding Banner

**What it does:** Shows a slideshow of featured/spotlight anime on the home page.

**Features:**
- **Auto-advance:** Changes every 7 seconds
- **Manual controls:** Left/right arrows, dot indicators
- **Touch support:** Swipe left/right on mobile
- **Pause on hover:** Stops auto-advancing when you hover
- **Animated transitions:** Smooth fade between slides using AnimatePresence
- **Content overlay:** Shows genre tags, title, description, Watch Now / Add to List buttons
- **Progress bar:** Shows how long until the next auto-advance

---

### 8.5 `AnimeCard.jsx` — Anime Poster Card

**What it does:** Displays a single anime as a clickable card with its poster image.

**Features:**
- **Lazy loading:** Image loads only when the card is visible
- **Skeleton loading:** Shows a shimmering placeholder while the image loads
- **Hover effects:** Image scales up, a play button appears, info becomes more visible
- **Adaptive:** Handles different API response structures (normalizes data fields)
- **Click:** Navigates to `/anime/{id}`

**Data normalization:**
The API returns data in slightly different formats depending on the endpoint. This component handles all of them:
```javascript
const id = anime?.id || anime?.data_id
const title = anime?.title || anime?.name
const image = anime?.poster || anime?.image
```

---

### 8.6 `AnimeRow.jsx` — Horizontal Scroll Row

**What it does:** A Netflix-style horizontal row of anime cards with a title and scroll arrows.

**Features:**
- Title with optional "View All" link
- Left/right scroll arrows (only shown when there's content to scroll to)
- Loading skeleton placeholders
- Fade gradient on edges

---

### 8.7 `SearchOverlay.jsx` — The Search Popup

**What it does:** A fullscreen overlay for searching anime.

**How it works:**
1. Opens when you click the search button or press `⌘K` / `Ctrl+K`
2. Shows a text input with auto-focus
3. As you type, it **debounces** (waits 300ms after you stop typing) then calls `searchAnime()`
4. Results appear as a list with poster thumbnails
5. Clicking a result navigates to that anime's page
6. Without typing, shows:
   - **Recent searches** (stored in localStorage)
   - **Trending searches** (hardcoded popular titles)
7. Press `ESC` to close

---

### 8.8 `VideoPlayer.jsx` — Custom Video Player

**What it does:** A full-featured HTML5 video player for HLS streams.

**Features:**
- **HLS streaming support** using hls.js
- **Custom controls:** Play/pause, volume, seek, fullscreen, pip, settings
- **Quality selection:** Auto or specific resolutions (720p, 1080p, etc.)
- **Playback speed:** 0.5x to 2x
- **Subtitle support**
- **Keyboard shortcuts:**
  - Space / K = Play/Pause
  - F = Fullscreen
  - M = Mute
  - Arrow Keys = Skip forward/backward
- **Time tracking:** Reports current time to parent component for progress saving
- **Auto-hide controls:** Controls fade away after 3 seconds of inactivity

---

### 8.9 `IframePlayer.jsx` — Embedded Video Player

**What it does:** Embeds an external video player in an iframe.

**Features:**
- Loading spinner while the iframe loads
- Error state with retry button
- Fullscreen button
- Refresh button
- **Sandbox** attribute to limit what the iframe can do (blocks popups, limits scripts)

---

### 8.10 `ContinueWatching.jsx` — Continue Watching Section

**What it does:** Shows anime you've been watching, so you can quickly resume.

**Data flow:**
1. If authenticated → Fetches from Supabase (cloud storage)
2. Falls back to local Zustand store
3. Shows a grid of cards with:
   - Anime thumbnail
   - Title
   - Episode number
   - Progress percentage bar
   - Play button on hover
   - Remove button (X) on hover

**Smart deduplication:** Shows only the latest episode per anime (not every single episode you've watched).

---

### 8.11 `MoodSelector.jsx` — Mood-Based Discovery

**What it does:** Lets you pick a mood and find anime that match it.

**Moods:**
| Mood | Emoji | Genres |
|------|-------|--------|
| Exciting | 🔥 | Action, Adventure |
| Emotional | 😢 | Drama, Romance |
| Fun | 😂 | Comedy, Slice of Life |
| Intense | 🎭 | Thriller, Psychological |
| Fantasy | ✨ | Fantasy, Isekai |
| Sci-Fi | 🤖 | Mecha, Sci-Fi |

Each mood maps to specific genres, which are then used to fetch anime from those genres.

---

### 8.12 `PageTransition.jsx` — Smooth Page Transitions

**Contains three things:**

1. **`VLogo`** — The Voxani logo component with a green glow effect
2. **`PageTransition`** — A wrapper that fades pages in/out
3. **`LoadingScreen`** — A fullscreen loading overlay with the animated V logo
4. **`usePageTransition`** — A custom hook that:
   - Detects when the URL changes
   - Shows a loading screen for 400ms
   - Then reveals the new page

---

### 8.13 `BentoGrid.jsx` — Grid Layout Components

**What it does:** Provides different card sizes for a "bento box" style grid layout.

**Card types:**
- **FeaturedHero** — Large 2x2 card for featured anime
- **MediumCard** — Wide card showing 3 posters
- **PosterCard** — Single anime poster with hover effects
- **TallCard** — Vertical card showing a ranked list of 5 anime

---

### 8.14 `ListTabs.jsx` — Status Filter Tabs

**What it does:** A row of tab buttons for filtering anime by status.

**Tabs:** All, Watching, Plan to Watch, Completed, On Hold, Dropped

Also includes a **StatusDropdown** component — a dropdown menu to change an anime's status.

---

## 9. Services — Talking to the Outside World

### 9.1 `api.js` — The Anime Data Service

This file is like a **phone book** for the anime API. It contains every function that fetches data from the anime server at `https://itzzmeapi.vercel.app/api`.

**Base URL:** `https://itzzmeapi.vercel.app/api`

**Here's every function, what it calls, and what it returns:**

#### Home & Spotlight
| Function | API Endpoint | Returns |
|----------|-------------|---------|
| `getHomePage()` | `GET /api/` | Spotlights, trending, schedule, top airing, popular, favorites, latest |
| `getTopTen()` | `GET /api/top-ten` | Top 10 anime lists (today, week, month) |
| `getTopSearch()` | `GET /api/top-search` | Popular search queries |

#### Search
| Function | API Endpoint | Returns |
|----------|-------------|---------|
| `searchAnime(keyword, page)` | `GET /api/search?keyword=...` | Search results array |
| `getSearchSuggestions(keyword)` | `GET /api/search/suggest?keyword=...` | Autocomplete suggestions |
| `filterAnime(filters)` | `GET /api/filter?...` | Filtered results |

#### Anime Info
| Function | API Endpoint | Returns |
|----------|-------------|---------|
| `getAnimeInfo(id)` | `GET /api/info?id=...` | Full anime details |
| `getRandomAnime()` | `GET /api/random` | Random anime info |
| `getQtipInfo(dataId)` | `GET /api/qtip/{dataId}` | Quick popup card data |

#### Episodes & Streaming
| Function | API Endpoint | Returns |
|----------|-------------|---------|
| `getAnimeEpisodes(animeId)` | `GET /api/episodes/{animeId}` | List of episodes |
| `getMegaplayEmbed(episodeId, type)` | (builds URL) | Iframe URL for video player |
| `getStreamingSources(episodeId, server, type)` | `GET /api/stream?...` | HLS stream links |
| `getFallbackSources(episodeId, server, type)` | `GET /api/stream/fallback?...` | Backup stream links |
| `getServers(animeId, episodeId)` | `GET /api/servers/{animeId}?ep=...` | Available servers |

#### Categories
| Function | API Endpoint | Returns |
|----------|-------------|---------|
| `getCategory(category, page)` | `GET /api/{category}?page=...` | Anime list for that category |
| `getTopAiring(page)` | `GET /api/top-airing` | Shortcut for top airing |
| `getMostPopular(page)` | `GET /api/most-popular` | Shortcut for most popular |
| (and 13 more convenience functions) | | |

#### Genres
| Function | API Endpoint | Returns |
|----------|-------------|---------|
| `getGenreAnime(genre, page)` | `GET /api/genre/{genre}?page=...` | Anime in that genre |
| `getGenreList()` | (returns constant) | List of 40+ genre names |

#### Schedule
| Function | API Endpoint | Returns |
|----------|-------------|---------|
| `getSchedule(date)` | `GET /api/schedule?date=YYYY-MM-DD` | Anime airing on that date |
| `getNextEpisodeSchedule(animeId)` | `GET /api/schedule/{animeId}` | Next episode air date |

#### Characters
| Function | API Endpoint | Returns |
|----------|-------------|---------|
| `getAnimeCharacters(animeId, page)` | `GET /api/character/list/{animeId}` | Character list |
| `getCharacterDetails(characterId)` | `GET /api/character/{characterId}` | Single character info |
| `getVoiceActorDetails(actorId)` | `GET /api/actors/{actorId}` | Voice actor info |

#### A-Z & Producers
| Function | API Endpoint | Returns |
|----------|-------------|---------|
| `getAZList(letter, page)` | `GET /api/az-list/{letter}` | Anime starting with that letter |
| `getProducerAnime(producer, page)` | `GET /api/producer/{producer}` | Anime by a specific studio |

**How the Megaplay embed URL works:**
```
Base URL: https://megaplay.buzz/stream/s-2
Format:   {base}/{episodeId}/{sub_or_dub}
Example:  https://megaplay.buzz/stream/s-2/141674/sub

For HD-4 server: https://vidwish.live/stream/s-2/{episodeId}/{type}
```

---

### 9.2 `anilist.js` — AniList Integration

AniList is a popular anime tracking website. Voxani integrates with it so users can sync their watchlists.

**How AniList integration works:**
1. User clicks "Login with AniList" → redirected to AniList's OAuth page
2. User authorizes → AniList sends a `code` back to Voxani
3. Voxani exchanges the code for an access token
4. Using the token, Voxani can:
   - Get the user's AniList profile
   - Fetch their AniList watchlist
   - Update their episode progress
   - Add anime to their AniList watchlist

**Key functions:**
| Function | What it does |
|----------|-------------|
| `getAnilistAuthUrl()` | Builds the login URL for AniList OAuth |
| `exchangeCodeForToken(code)` | Trades the auth code for an access token |
| `getAnilistUser(token)` | Gets the user's AniList profile (name, avatar, stats) |
| `getAnilistWatchlist(token, userId)` | Fetches all anime lists from AniList |
| `updateAnilistProgress(token, mediaId, progress)` | Updates episode count on AniList |
| `addToAnilistWatchlist(token, mediaId, status)` | Adds anime to AniList list |
| `searchAnilist(query, page, perPage)` | Searches AniList's database |

**AniList uses GraphQL:**
Unlike the main anime API which uses REST (simple GET/POST to URLs), AniList uses **GraphQL** — a query language where you specify exactly what data you want. The queries are written as template strings in the code.

---

## 10. State Management — The Brain's Memory

### What is State?

**State** is information that the app needs to remember. For example:
- Is the user logged in?
- What's in their watchlist?
- Is the sidebar open or closed?

Voxani uses **Zustand** — a tiny state management library. Think of it as a shared notebook that any component can read from or write to.

### 10.1 `useAuthStore` — Authentication State

**What it remembers:**
```
user              → The logged-in user object (name, email, avatar)
isAuthenticated   → true/false — is someone logged in?
isLoading         → true/false — are we checking auth status?
token             → The authentication access token
anilistToken      → The AniList access token (separate system)
```

**Actions (things you can do):**
```
setUser(user)            → Save the user info
setToken(token)          → Save the auth token
setIsAuthenticated(bool) → Mark as logged in/out
setAnilistToken(token)   → Save AniList token
logout()                 → Clear everything (log out)
```

**Persistence:** Saved in `localStorage` under the key `voxani-auth`. This means if you close the browser and come back, you're still logged in!

---

### 10.2 `useWatchlistStore` — Watchlist & Progress

**What it remembers:**
```
watchlist         → Array of saved anime (with status like "watching", "planning")
continueWatching  → Array of anime you've been watching (with episode/timestamp data)
```

**Actions:**
```
addToWatchlist(anime)              → Add an anime to your list
removeFromWatchlist(animeId)       → Remove an anime from your list
isInWatchlist(animeId)             → Check if an anime is in your list (returns true/false)
getWatchlistStatus(animeId)        → Get the status of an anime in your list
updateContinueWatching(...)        → Save/update watch progress for an episode
removeContinueWatching(animeId)    → Remove an anime from continue watching
syncFromAnilist(anilistWatchlist)   → Replace watchlist with AniList data
```

**How `updateContinueWatching` works:**
1. Receives: anime info, episode number, timestamp, duration
2. Calculates progress as a percentage: `(timestamp / duration) * 100`
3. If the anime already exists in the list → updates it
4. If it's new → adds it to the front of the list (max 20 items)

**Persistence:** Saved in `localStorage` under `voxani-watchlist`.

---

### 10.3 `useSettingsStore` — User Preferences

**What it remembers:**
```
theme             → "dark" (currently only dark mode)
autoPlay          → true/false — auto-play next episode?
autoNext          → true/false — auto-advance to next episode?
preferredQuality  → "auto", "720p", "1080p", etc.
preferredServer   → "default", "hd-1", etc.
subtitleLanguage  → "en" (English), "jp", etc.
sidebarCollapsed  → true/false — is the sidebar in mini mode?
```

**Persistence:** Saved in `localStorage` under `voxani-settings`.

---

### 10.4 `useUIStore` — UI Toggle States

**What it remembers (NOT persisted — resets on page refresh):**
```
sidebarOpen     → Is the sidebar open? (mobile)
searchOpen      → Is the search overlay showing?
mobileMenuOpen  → Is the mobile menu open?
```

---

## 11. Supabase — The Database & Authentication

### What is Supabase?

Supabase is like a **cloud storage locker** for your website. It provides:
- **A database** (PostgreSQL) to store data in tables
- **Authentication** (login/signup/OAuth)
- **Real-time subscriptions** (live updates without refreshing)
- **Row Level Security (RLS)** — rules that prevent users from seeing each other's data

### 11.1 Database Tables

The database has 7 tables. Here's each one:

#### 📋 `profiles` — User Info
| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Links to the auth user (primary key) |
| username | TEXT | Unique username |
| display_name | TEXT | Display name |
| avatar_url | TEXT | Profile picture URL |
| created_at | TIMESTAMP | When the profile was created |

**Auto-creation:** When a new user signs up, a database trigger automatically creates their profile row.

#### 📺 `watch_progress` — Episode Watch Progress
| Column | Type | Description |
|--------|------|-------------|
| user_id | UUID | Who watched it |
| anime_id | TEXT | Which anime |
| anime_title | TEXT | Anime name (cached for fast display) |
| anime_image | TEXT | Anime poster URL |
| episode_id | TEXT | Which episode |
| episode_number | INTEGER | Episode number |
| timestamp | REAL | How many seconds watched |
| duration | REAL | Total episode length in seconds |
| progress | REAL | Percentage watched (0-100) |
| last_updated | TIMESTAMP | When it was last updated |

**Unique constraint:** One row per user + anime + episode combo (uses upsert to update existing rows).

#### 📚 `watchlist` — Saved Anime List
| Column | Type | Description |
|--------|------|-------------|
| user_id | UUID | Whose list |
| anime_id | TEXT | Which anime |
| anime_title | TEXT | Anime name |
| anime_image | TEXT | Poster URL |
| status | TEXT | "watching", "planning", "completed", "dropped" |
| added_at | TIMESTAMP | When it was added |

#### 🎭 `watch_rooms` — Watch Together Rooms
| Column | Type | Description |
|--------|------|-------------|
| host_id | UUID | Who created the room |
| anime_id | TEXT | What anime is being watched |
| episode_id | TEXT | Current episode |
| playback_time | REAL | Current playback position |
| is_playing | BOOLEAN | Is the video playing or paused? |
| expires_at | TIMESTAMP | Rooms auto-expire after 24 hours |

#### 👥 `watch_room_participants` — Who's in a Room
| Column | Type | Description |
|--------|------|-------------|
| room_id | UUID | Which room |
| user_id | UUID | Which user |
| joined_at | TIMESTAMP | When they joined |

#### 💬 `watch_room_messages` — Room Chat
| Column | Type | Description |
|--------|------|-------------|
| room_id | UUID | Which room |
| user_id | UUID | Who sent it |
| message | TEXT | The message text |
| created_at | TIMESTAMP | When it was sent |

#### ⚙️ `user_preferences` — User Settings
| Column | Type | Description |
|--------|------|-------------|
| color_theme | TEXT | "green" (default) |
| auto_play | BOOLEAN | Auto-play setting |
| auto_next | BOOLEAN | Auto-next setting |
| preferred_quality | TEXT | Video quality preference |
| subtitle_language | TEXT | Subtitle language |

### 11.2 Row Level Security (RLS)

**RLS is like a lock on each row.** Every table has rules that say:
- "A user can only **see** their own data"
- "A user can only **add** data with their own user ID"
- "A user can only **change** their own data"
- "A user can only **delete** their own data"

This means even if someone tries to hack the database, they can't see other people's watchlists or progress.

**Exception:** Watch rooms are partially public — anyone who is authenticated can **see** rooms (to join them), but only the host can **modify** or **delete** them.

### 11.3 Realtime

Three tables have **realtime** enabled:
- `watch_rooms` — So all participants see when the host plays/pauses
- `watch_room_messages` — So new chat messages appear instantly
- `watch_room_participants` — So everyone sees when someone joins/leaves

### 11.4 `supabase.js` — All the Database Functions

This 740-line file contains every function that talks to Supabase.

**Authentication Functions:**
| Function | What it does |
|----------|-------------|
| `signUp(email, password, username)` | Creates a new account |
| `signIn(email, password)` | Logs in with email/password |
| `signOut()` | Logs out |
| `signInWithGoogle()` | One-click Google login |
| `getSession()` | Gets the current login session |
| `getUser()` | Gets the current user object |
| `onAuthStateChange(callback)` | Listens for login/logout events |
| `resetPassword(email)` | Sends a password reset email |
| `updatePassword(newPassword)` | Changes the password |

**Profile Functions:**
| Function | What it does |
|----------|-------------|
| `getProfile(userId)` | Gets a user's profile |
| `updateProfile(userId, updates)` | Updates profile info |

**Watch Progress Functions:**
| Function | What it does |
|----------|-------------|
| `saveWatchProgress(...)` | Saves how far you've watched (upsert — creates or updates) |
| `getEpisodeProgress(userId, animeId, episodeId)` | Gets progress for one episode |
| `getAnimeProgress(userId, animeId)` | Gets all episode progress for one anime |
| `getContinueWatching(userId, limit)` | Gets your "continue watching" list (deduped by anime) |
| `getAllWatchProgress(userId, limit)` | Gets all episodes (no dedup — for profile page) |
| `getRecentlyWatched(userId, limit)` | Gets all history including completed episodes |
| `deleteEpisodeProgress(userId, animeId, episodeId)` | Removes one episode's progress |
| `deleteAnimeProgress(userId, animeId)` | Removes all progress for one anime |

**How `getContinueWatching` deduplication works:**
1. Fetches many rows, ordered by last updated
2. Filters out episodes with >90% progress (considered "completed")
3. Uses a `Set` to track seen anime IDs
4. Keeps only the first (most recent) entry per anime

**Watchlist Functions:**
| Function | What it does |
|----------|-------------|
| `addToWatchlistDB(userId, animeId, ...)` | Adds anime to watchlist (upsert) |
| `removeFromWatchlistDB(userId, animeId)` | Removes anime from watchlist |
| `updateWatchlistStatus(userId, animeId, status)` | Changes anime status |
| `getWatchlistDB(userId, status)` | Gets watchlist (optionally filtered by status) |
| `isInWatchlistDB(userId, animeId)` | Checks if anime is in watchlist |

**Watch Room Functions (Watch Together):**
| Function | What it does |
|----------|-------------|
| `createWatchRoom(hostId, animeId, ...)` | Creates a new watch room |
| `getWatchRoom(roomId)` | Gets room info with host and participants |
| `joinWatchRoom(roomId, userId)` | Adds user to a room |
| `leaveWatchRoom(roomId, userId)` | Removes user from a room |
| `updateRoomState(roomId, playbackTime, isPlaying)` | Host syncs playback (all participants see this) |
| `subscribeToRoom(roomId, callback)` | Listens for real-time room changes |
| `unsubscribeFromRoom(channel)` | Stops listening |
| `sendWatchRoomMessage(roomId, userId, message)` | Sends a chat message |
| `getRoomMessages(roomId, limit)` | Gets chat history |
| `deleteWatchRoom(roomId)` | Deletes a room |

---

## 12. Styling — Making Everything Look Pretty

### 12.1 Tailwind CSS

Voxani uses Tailwind CSS, which means instead of writing CSS in separate files, you add class names directly to HTML elements.

**Example:**
```jsx
// Traditional CSS would need a separate .css file:
// .my-button { background: green; padding: 12px 24px; border-radius: 12px; }

// Tailwind does it inline:
<button className="bg-green-500 px-6 py-3 rounded-xl">Click Me</button>
```

### 12.2 The Color Scheme

Voxani uses a **green-themed dark mode** design. All colors are defined as CSS variables and Tailwind utilities:

```
Background Colors:
  - Primary:   #0a0f0a  (almost black with a hint of green)
  - Surface:   #0c1210  (slightly lighter)
  - Card:      #121a16  (card backgrounds)
  - Elevated:  #1a2820  (things that need to stand out)

Accent Colors:
  - Main:      #359772  (teal green - buttons, links, active states)
  - Light:     #72c490  (lighter green - secondary elements)
  - Pale:      #97ca9d  (very light green)
  - Mint:      #99dabf  (mint green - subtle highlights)
  - Dark:      #105f3e  (deep green - shadows/depth)

Text Colors:
  - Primary:   #ffffff  (white - main text)
  - Secondary: #99dabf  (mint - less important text)
  - Muted:     #72c490  (green - subtle text)
  - Dim:       #4a8a6a  (dark green - least important text)

Borders:
  - Default:   rgba(53, 151, 114, 0.2)  (very subtle green border)
  - Light:     rgba(114, 196, 144, 0.25)
  - Hover:     rgba(151, 202, 157, 0.4)
```

### 12.3 `index.css` — Global Styles

This file contains:

1. **CSS Variables** — All the color values defined as `--variable-name` for reuse
2. **Base styles** — Reset margins/padding, set default font, background color
3. **Custom scrollbar** — Styled scrollbar with green thumb on dark track
4. **Selection color** — When you highlight text, it shows a green background
5. **Utility classes:**
   - `.text-gradient` — Makes text have a green gradient
   - `.bg-glass` — Frosted glass effect (semi-transparent with blur)
   - `.border-glow` — Glowing green border
   - `.glow-green` — Green outer glow shadow
   - `.card-hover` — Lifts up and scales on hover
   - `.line-clamp-1/2/3` — Limits text to 1, 2, or 3 lines
6. **Animation keyframes:**
   - `float` — Gently bobs up and down
   - `shimmer` — Loading skeleton shine effect
7. **Component styles:**
   - `.btn-primary` — Green gradient button with glow
   - `.btn-secondary` — Subtle green border button
   - `.input-field` — Text input with green focus ring
   - `.card` — Dark card with subtle border
   - `.anime-card` — Poster card with image zoom and gradient overlay
   - `.tag` — Small label badge (genre tags, etc.)
   - `.skeleton` — Loading placeholder with shimmer
   - `.progress-bar` / `.progress-fill` — Green progress bar
8. **Swiper overrides** — Custom styles for the carousel dots

---

## 13. How Key Features Work End-to-End

### 13.1 Watching an Anime Episode (Full Flow)

1. **User clicks "Watch Now"** on an anime card → navigates to `/anime/{id}`
2. **AnimeDetails page loads:**
   - Fetches anime info (`getAnimeInfo(id)`)
   - Fetches episodes (`getAnimeEpisodes(id)`)
   - Shows episode list with any existing progress
3. **User clicks on Episode 1** → navigates to `/watch/{id}?ep={episodeId}`
4. **Watch page loads:**
   - Fetches anime info and episodes again
   - Extracts `episodeId` from the URL query params
   - Fetches available servers (`getServers(animeId, episodeId)`)
   - Builds the Megaplay iframe URL: `https://megaplay.buzz/stream/s-2/{episodeId}/sub`
   - Loads the iframe
5. **While watching:**
   - Every 30 seconds, progress is saved:
     - To local Zustand store (`updateContinueWatching`)
     - To localStorage (for persistence)
     - To Supabase (for cloud sync, if logged in)
6. **User finishes or navigates away:**
   - Progress is saved one final time
   - Next time they visit Home, the "Continue Watching" section shows this anime

### 13.2 Search (Full Flow)

1. **User presses `⌘K`** or clicks the search icon → `SearchOverlay` opens
2. **User types "Naruto"** → after 300ms of no typing (debounce), `searchAnime("Naruto")` is called
3. **API returns results** → displayed as a list of anime with thumbnails
4. **User clicks a result** → navigates to `/anime/{id}`, search overlay closes
5. The search query is saved to `localStorage` under `voxani-recent-searches`

### 13.3 Authentication (Full Flow)

**Sign Up:**
1. User fills in email, password, username on `/login`
2. `signUp(email, password, username)` is called
3. Supabase creates the auth user and triggers `handle_new_user()` which creates a `profiles` row
4. A confirmation email is sent to the user
5. User clicks the email link → directed to `/auth/callback`
6. `AuthCallback` checks the session, logs the user in, redirects to `/home`

**Sign In:**
1. User enters email and password
2. `signIn(email, password)` is called
3. Supabase validates credentials and returns a session with access token
4. Token and user are saved to `useAuthStore`
5. The app redirects to `/home`
6. All pages that check `isAuthenticated` now know the user is logged in

**Google Sign In:**
1. User clicks "Sign in with Google"
2. `signInWithGoogle()` redirects to Google's OAuth page
3. User authorizes → Google redirects back to `/home`
4. Supabase automatically creates/links the account
5. `onAuthStateChange` listener detects the new session and updates the store

### 13.4 Watchlist Management (Full Flow)

1. **User clicks "Add to List"** on an anime
2. A dropdown appears with status options: Watching, Plan to Watch, Completed, On Hold, Dropped
3. User selects a status
4. Three things happen simultaneously:
   - Local Zustand `watchlist` array is updated
   - If authenticated, `addToWatchlistDB()` saves to Supabase
   - A toast notification appears: "Added to Watching"
5. The anime now appears in:
   - `/my-list` page
   - `/profile` page under "My List" tab
   - BookmarkAdded icon shows on the anime's card/detail page

---

## 14. Mobile App (Capacitor)

The `my-app/` folder contains a **Capacitor** configuration for wrapping the web app as a native mobile app.

**What is Capacitor?** It takes a website and wraps it in a native app shell so it can be installed on phones (Android/iOS) from the app store.

```json
// capacitor.config.json
{
  "appId": "com.example.app",
  "appName": "my-app",
  "webDir": "dist",
  "server": {
    "androidScheme": "https"
  }
}
```

This means the production build (in `dist/`) would be loaded inside a native webview on mobile devices.

---

## 15. Deployment

### How Voxani gets on the Internet

1. **Build:** Running `npm run build` (or `vite build`) compiles all the React code into optimized static files (HTML, CSS, JS)
2. **Host:** These files are uploaded to **Vercel** (a cloud hosting platform)
3. **Serve:** When someone visits the Voxani URL, Vercel serves the files
4. **Routing:** `vercel.json` ensures all routes return `index.html` (since React handles routing client-side)
5. **API Proxy:** In development, Vite proxies `/api` calls to the anime API server. In production, the app calls the API directly.

### Environment Variables

The app needs these secret values (stored as environment variables, NOT in the code):
```
VITE_SUPABASE_URL         → Your Supabase project URL
VITE_SUPABASE_ANON_KEY    → Your Supabase public API key
VITE_ANILIST_CLIENT_ID    → AniList OAuth client ID
VITE_ANILIST_CLIENT_SECRET → AniList OAuth client secret
```

These are prefixed with `VITE_` so Vite makes them available in the browser code.

---

## 16. Glossary of Terms

| Term | Simple Explanation |
|------|-------------------|
| **API** | Application Programming Interface — a way for two computers to talk to each other. Like a waiter taking your order to the kitchen and bringing back food. |
| **Component** | A reusable piece of the website (like a LEGO block). Can be used in many places. |
| **State** | Data that the app remembers while you're using it (like "is the user logged in?"). |
| **Props** | Data passed from a parent component to a child component (like handing a note to someone). |
| **Hook** | A special function in React that lets components use features like state and side effects. Names always start with `use` (like `useState`, `useEffect`). |
| **useEffect** | A hook that runs code when something changes (like "fetch data when the page loads"). |
| **useState** | A hook that creates a piece of data that the component remembers. |
| **Route** | A URL pattern mapped to a page (like `/home` → Home page). |
| **OAuth** | A way to log in using another service (like "Sign in with Google"). |
| **JWT / Token** | A digital "ticket" that proves you're logged in. Sent with every request to the server. |
| **REST API** | A style of API that uses standard HTTP methods (GET, POST, PUT, DELETE). |
| **GraphQL** | A query language for APIs where you specify exactly what data you want (used by AniList). |
| **HLS** | HTTP Live Streaming — a video streaming format that breaks video into small chunks for smooth playback. |
| **Iframe** | An embedded mini-webpage inside your webpage (used for the video player). |
| **Proxy** | A middleman that forwards requests (Vite forwards `/api` calls to the anime server). |
| **Upsert** | Insert a new row OR update an existing one if it already exists. |
| **RLS** | Row Level Security — database rules that control who can see/change each row. |
| **Debounce** | Waiting a short time after the user stops an action before reacting (prevents too many API calls while typing). |
| **Skeleton** | A loading placeholder that shows the shape of content before it loads (the shimmering rectangles). |
| **SSR** | Server-Side Rendering — rendering pages on the server (Voxani does NOT use this; it's fully client-side). |
| **SPA** | Single Page Application — the entire website is one HTML file; React shows different content based on the URL. |
| **Zustand** | A tiny state management library (German for "state"). Simpler alternative to Redux. |
| **Framer Motion** | Animation library for React. Makes things move smoothly. |
| **Tailwind CSS** | A CSS framework that uses utility class names instead of writing custom CSS. |
| **Supabase** | An open-source Firebase alternative that provides a database, auth, and real-time features. |
| **Vercel** | A cloud platform for hosting websites, especially ones built with React/Next.js. |
| **Capacitor** | A tool that wraps web apps into native mobile apps (Android/iOS). |

---

## Summary

**Voxani** is a React-based anime streaming website with:

- **20+ pages and 15+ reusable components** that create a polished user experience
- **A green-themed dark mode design** using Tailwind CSS with custom colors, animations, and glass effects
- **Full authentication** with email/password, Google OAuth, and AniList integration
- **Watch progress tracking** that syncs between local storage and the Supabase cloud database
- **Smart video playback** using embedded iframe players with multiple server options
- **Real-time features** like Watch Together rooms with synchronized playback and chat
- **Responsive design** that works on desktop (sidebar navigation) and mobile (bottom tab navigation)
- **State management** with Zustand stores for auth, watchlist, settings, and UI state
- **Smooth animations** throughout, powered by Framer Motion

The code is organized cleanly into pages, components, services, stores, and libraries — each with a clear responsibility, making it maintainable and easy to understand.

---

*This documentation was generated to explain every aspect of the Voxani codebase in simple, accessible language.*
