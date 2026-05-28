# Deploying EduMaster (Free Tier)

This guide will walk you through deploying the Backend to **Render** and the Frontend to **Vercel**. Both platforms offer generous free tiers.

## Part 1: Push your code to GitHub
Before you can deploy, your code must be in a GitHub repository.
1. Go to GitHub and create a new repository (e.g. `edumaster`).
2. Push this codebase to that repository.

---

## Part 2: Backend (Render)

Render is perfect for hosting Node.js backends for free.

1. Go to [Render.com](https://render.com) and sign in with GitHub.
2. Click **New** -> **Web Service**.
3. Select **"Build and deploy from a Git repository"**.
4. Connect your GitHub account and select your `edumaster` repository.
5. **Configuration:**
   - **Name:** `edumaster-backend`
   - **Root Directory:** `server` (Important! Tell Render where the backend is)
   - **Environment:** `Node`
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`
   - **Instance Type:** Free
6. **Environment Variables (Advanced -> Add Environment Variable):**
   Copy everything from your `server/.env` file. You must add:
   - `MONGO_URI` (You can use the original `mongodb+srv://...` one here!)
   - `JWT_SECRET`
   - `GEMINI_API_KEY`
   - `NODE_ENV` (Set to `production`)
   - `CLIENT_URL` (Leave blank for now, we will update it after Vercel is deployed)
7. Click **Create Web Service**. Wait 3-5 minutes for it to build and deploy.
8. Once live, copy the Render URL (e.g., `https://edumaster-backend-xyz.onrender.com`).

---

## Part 3: Frontend (Vercel)

Vercel is the best platform for hosting React/Vite frontends.

1. Go to [Vercel.com](https://vercel.com) and sign in with GitHub.
2. Click **Add New** -> **Project**.
3. Import your `edumaster` repository.
4. **Configuration:**
   - **Framework Preset:** Vite
   - **Root Directory:** `client` (Click Edit and select the client folder)
5. **Environment Variables:**
   - Name: `VITE_API_URL`
   - Value: `https://edumaster-backend-xyz.onrender.com/api` *(Paste the Render URL you copied in Part 2, and make sure to add `/api` at the end!)*
6. Click **Deploy**.
7. Once finished, copy your new Vercel frontend URL (e.g., `https://edumaster-client.vercel.app`).

---

## Part 4: Final Connection

Now that your frontend is live, you must tell the backend to allow requests from it (CORS).

1. Go back to your **Render Web Service** dashboard.
2. Go to the **Environment** tab.
3. Find `CLIENT_URL` and set its value to your Vercel URL (e.g., `https://edumaster-client.vercel.app`).
4. **Save Changes** (Render will automatically restart the server).

## Part 5: MongoDB Network Access
Don't forget! Your new Render server has a different IP address than your home computer.
1. Go to MongoDB Atlas -> Network Access.
2. Ensure you have `0.0.0.0/0` (Allow Access from Anywhere) in your IP whitelist so Render can connect to the database.

**You're done! Your full-stack application is live.**
