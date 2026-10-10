# inspiration
# Toolkit: Inspiration — Setup Guide (version 1.0.0)

*To God be the Glory*

This guide gets the app live for Kayla, with a staging copy for testing. Steps are in order. SQL is at the end of this file in one block: copy it whole into the Supabase SQL editor.

---

## What's in version 1

- **Library shelf**: each project is a book spine (its height grows with the word count). Tabs for On the desk, Paused, Finished, Archived. "Pick up where you left off" link. Daily word goal and streak.
- **Workspace**: outline on the left (folders and documents, drag to reorder or move into folders, right-click for more), the page in the middle, and Synopsis / Label / Notes / History on the right.
- **Editor**: chapter title, heading and subheading styles; bold, italic, underline, strikethrough; alignment; lists; block quote; scene break ornament; undo/redo. Pasting from Word keeps the basic formatting and drops the clutter.
- **Folder view**: click a folder to see its documents as index cards with their synopses.
- **Focus mode**: everything but the page fades away. Ctrl+Shift+F to enter, Esc to leave.
- **History**: a version is saved automatically every couple of hours of writing, and she can save named versions any time. Any version can be previewed and restored (what she had is saved first).
- **Looks**: four themes (Parchment, Library, Forest, Plain), four painted backgrounds plus your photos, her own photos, writing font, text size, line spacing, page width, indented or spaced paragraphs.
- **Ideas box**: the lightbulb. Her ideas land in the `suggestions` table for you.
- **Backup & export**: one-click full backup file, restore from backup, export any selection of a project to Word, PDF (print) or plain text, in easy-reading or standard manuscript style, with a title page using her pen name. Import Word, text, Markdown or HTML files, optionally split into chapters.
- **Offline**: works with no internet once she's signed in on a device. Changes sync when the connection returns.
- **Safe updates**: when you publish a new version, she sees "Update ready, apply when you're done" instead of the app reloading on her.

Nothing is ever deleted. Archived projects and documents can be restored, and the database has no delete permission at all.

---

## Which files change when

| File | What it is | Edited when |
|---|---|---|
| `js/config.js` | Version number, Supabase details, live/staging | Every update (version bump), and to tell live and staging apart |
| `js/app.js` | Shelf, settings, ideas box, sign-in | Library or settings changes |
| `js/editor.js` | Outline, editor, notes, history | Writing-screen changes |
| `js/transfer.js` | Backup, export, import | Export or import changes |
| `js/store.js`, `js/sync.js` | Saving on the device and syncing | Rarely |
| `js/scenes.js` | Painted backgrounds | Rarely |
| `css/app.css` | All styling | Look changes |
| `index.html`, `sw.js`, `manifest.webmanifest` | Page shell, offline support, app install info | Rarely |
| `lib/`, `fonts/`, `img/` | Editor, database client, Word tools, fonts, icon | Essentially never |
| `backgrounds/` | Your photos | When you add photos |

Each update I give you will start with a "files changed" list. Only upload those.

---

## Step 1. Live database (main Supabase account)

You may have done some of this already.

1. Create the project **Inspiration** (East US, same as The Ledger). Save the database password.
2. **SQL Editor > New query**: paste the whole SQL block at the end of this guide and press **Run**. You should see "Success. No rows returned."
3. **Authentication > Users > Add user > Create new user**: Kayla's email, a temporary password, check **Auto Confirm User**.
4. **Authentication > Sign In / Providers**: turn off **Allow new users to sign up**.
5. **Authentication > URL Configuration**: Site URL = `https://YOURNAME.github.io/inspiration/` (with the trailing slash). Add the same address under **Redirect URLs**.
6. **Project Settings > API** (may be labeled **API Keys**): copy the **Project URL** and the **anon** / **publishable** key. Never use the **service_role** / **secret** key in the app.

## Step 2. Staging database (second Supabase account)

Same as Step 1 with these differences:
- Project name **Inspiration-Staging**
- Add **yourself** as the user (any password you like)
- Site URL = `https://YOURNAME.github.io/inspiration-staging/`

Free projects pause after a week without use. When you want to test, open the staging project in Supabase and press **Restore** if it's paused.

## Step 3. Fill in config.js (two versions)

Open `js/config.js` in any text editor.

**Live copy:**
```
ENVIRONMENT: 'live',
SUPABASE_URL: 'https://xxxxxxxx.supabase.co',
SUPABASE_ANON_KEY: 'eyJ... (the anon / publishable key)',
```

**Staging copy:** the same, with `ENVIRONMENT: 'staging'` and the staging project's URL and key. Staging shows a small orange "Staging" tag so you always know which one you're in.

Keep both versions of `config.js` saved on your computer (for example `config.live.js` and `config.staging.js`) so you never have to look the keys up again. Upload them to GitHub named `config.js`.

The anon key is designed to be public. The row-level security in the SQL is what keeps her writing private.

## Step 4. Upload to GitHub

For **each** repo (`inspiration` and `inspiration-staging`):
1. On the repo page: **Add file > Upload files**. Drag in everything from the app folder (the folders `css`, `js`, `lib`, `fonts`, `img`, `backgrounds`, plus `index.html`, `sw.js`, `manifest.webmanifest`). Dragging the folders keeps their structure.
2. Commit. Then replace `js/config.js` with that repo's version.
3. **Settings > Pages**: Deploy from a branch, `main`, `/ (root)`. The site appears in a minute or two.

`SETUP-GUIDE.md` can go in the repo too; it does no harm.

## Step 5. Test on staging first

On staging, signed in as yourself:
1. Create a project, write a few paragraphs, add a second document, drag it into a folder.
2. Open the same site on your phone. The project should be there after a moment.
3. Turn on airplane mode on your phone, write a sentence, turn it off. It should sync ("Synced" in the top bar).
4. Export to Word and open the file. Download a backup.
5. Send yourself an idea from the lightbulb and look for it in Supabase **Table Editor > suggestions**.

If anything misbehaves, tell me what you saw (a screenshot of the top bar and any message helps) and I'll fix it before it reaches the live site.

## Step 6. Setting it up on Kayla's Surface

1. Open the live site in **Microsoft Edge** and sign in with her email and temporary password.
2. Edge menu (**…**) > **Apps** > **Install this site as an app**. It gets its own icon and window, like a regular program, and opens offline.
3. Settings (gear) > **Account**: she can set her own password.
4. Pin it to the taskbar.

## Step 7. Background photos (optional)

The app has four painted backgrounds built in. To add photos, see `backgrounds/README.txt`: save up to five photos named `library-1.jpg`, `library-2.jpg`, `nature-1.jpg`, `nature-2.jpg`, `nature-3.jpg` into the `backgrounds` folder on GitHub. Free sources: unsplash.com and pexels.com. Kayla can also add her own photos in Settings; those stay on the device where she added them.

---

## Updating the app later

1. I give you the changed files and a new version number in `config.js`.
2. Upload them to **staging** first and check.
3. Upload the same files to **live**, with the live `config.js` (same new version number, live keys).
4. Her app shows "Update ready" within about ten minutes, or the next time she opens it.

If I ever change the database, the SQL comes in a downloadable guide like this one. Run it on staging first, then live.

## Reading and answering her ideas

Supabase > **Table Editor > suggestions**. You can change `status` to `planned`, `building`, `done` or `declined`, and type a note in `reply`. She sees the status and your reply in her ideas box.

## Keyboard shortcuts

| Keys | Does |
|---|---|
| Ctrl+B / Ctrl+I / Ctrl+U | Bold / italic / underline |
| Ctrl+Z / Ctrl+Y | Undo / redo |
| Ctrl+S | Save now (it also saves on its own) |
| Ctrl+Alt+N | New document after this one |
| Ctrl+Shift+F | Focus mode (Esc to leave) |
| F2 in the outline | Rename |

---

## Things to know (flagged, not guessed)

1. **Tested against a simulated database, not real Supabase yet.** I ran the app in a browser and tested saving, two devices editing the same chapter, offline launch, export, backup and import. The first real run should be on staging.
2. **When two devices change the same document before syncing**, the device that syncs last wins, and the other version is saved in that document's History with the name "Version from another device." She gets a message when this happens.
3. **Spelling** uses the computer's own dictionary (Edge on Windows). Right-click a word to add it. A per-project dictionary for character and place names comes with grammar checking in phase 2.
4. **Automatic versions are never cleaned up.** Text is small, so this should be fine for years on the free plan's 500 MB. I'll add a tidy-up option if it ever matters.
5. **Pages and Scrivener files** need to be exported to Word first (in Pages: File > Export To > Word; in Scrivener: File > Export > Files as .docx), then imported.
6. **Free-tier pausing**: the live project only pauses after a full week without anyone opening the app. If she takes a long break, the app keeps working on her device and catches up once you press Restore in Supabase.
7. **Name**: I used "Toolkit: Inspiration" without the trailing colon. It's one line in `config.js` if you want it different.
8. **Her name and pen name** default to Kayla and K.E. Nielsen (greeting and title pages). She can change both in Settings.

## Open questions

1. Should the daily goal start at 500 words, or something else (or hidden until she turns it on)?
2. Do you want the Notes & research folder in every new project, or just a single Manuscript folder?
3. For phase 2, the order I'd suggest: grammar check with a per-project dictionary, then Story Atlas, then the Bible tools, then templates. Does that match what she'd want first?

---

## SQL: run once in each project (live and staging)

```sql
-- Toolkit: Inspiration, database setup v1.0.0
-- Safe to run on a new, empty project. Creates tables, security rules, and automatic timestamps.
-- No delete permission is granted: the app archives instead of deleting.

create or replace function public.inspiration_row_meta()
returns trigger language plpgsql as $$
begin
  new.updated_at := clock_timestamp();
  if tg_op = 'UPDATE' and new.rev is not distinct from old.rev then
    new.rev := coalesce(old.rev, 0) + 1;   -- edits made in the dashboard still reach the app
  end if;
  return new;
end $$;

create table if not exists public.projects (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null default auth.uid() references auth.users(id),
  title text not null default 'Untitled',
  subtitle text,
  kind text,
  status text not null default 'active',
  word_goal integer,
  color text,
  notes text,
  sort_order double precision default 0,
  archived_at timestamptz,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now(),
  rev integer not null default 0
);

create table if not exists public.nodes (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null default auth.uid() references auth.users(id),
  project_id uuid not null,
  parent_id uuid,
  kind text not null default 'doc',          -- 'doc' or 'folder'
  title text,
  synopsis text,
  notes text,
  label text,
  content jsonb,                             -- the formatted text
  plain text,                                -- plain copy for search and word counts
  word_count integer not null default 0,
  sort_order double precision default 0,
  archived_at timestamptz,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now(),
  rev integer not null default 0
);

create table if not exists public.snapshots (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null default auth.uid() references auth.users(id),
  node_id uuid not null,
  project_id uuid,
  name text,
  reason text,                               -- auto, manual, restore, conflict
  content jsonb,
  word_count integer not null default 0,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now(),
  rev integer not null default 0
);

create table if not exists public.writing_log (
  id text primary key,                       -- day + device
  user_id uuid not null default auth.uid() references auth.users(id),
  day date not null,
  device text,
  words integer not null default 0,
  minutes integer not null default 0,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now(),
  rev integer not null default 0
);

create table if not exists public.suggestions (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null default auth.uid() references auth.users(id),
  body text not null,
  status text not null default 'new',        -- new, planned, building, done, declined
  reply text,
  archived_at timestamptz,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now(),
  rev integer not null default 0
);

create table if not exists public.settings (
  id text primary key,                       -- the user's id
  user_id uuid not null default auth.uid() references auth.users(id),
  prefs jsonb not null default '{}'::jsonb,
  last_backup_at timestamptz,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now(),
  rev integer not null default 0
);

do $$
declare t text;
begin
  foreach t in array array['projects','nodes','snapshots','writing_log','suggestions','settings'] loop
    execute format('drop trigger if exists %1$s_meta on public.%1$s', t);
    execute format('create trigger %1$s_meta before insert or update on public.%1$s for each row execute function public.inspiration_row_meta()', t);
    execute format('alter table public.%1$s enable row level security', t);
    execute format('drop policy if exists "%1$s: read own" on public.%1$s', t);
    execute format('drop policy if exists "%1$s: add own" on public.%1$s', t);
    execute format('drop policy if exists "%1$s: change own" on public.%1$s', t);
    execute format('create policy "%1$s: read own" on public.%1$s for select to authenticated using (user_id = auth.uid())', t);
    execute format('create policy "%1$s: add own" on public.%1$s for insert to authenticated with check (user_id = auth.uid())', t);
    execute format('create policy "%1$s: change own" on public.%1$s for update to authenticated using (user_id = auth.uid()) with check (user_id = auth.uid())', t);
    execute format('create index if not exists %1$s_sync_idx on public.%1$s (user_id, updated_at)', t);
  end loop;
end $$;

create index if not exists nodes_project_idx on public.nodes (project_id);
create index if not exists snapshots_node_idx on public.snapshots (node_id);
```
