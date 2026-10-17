# TNS Review

A separate dashboard for reviewing stories submitted to TNS News.

## 1. Publish with GitHub Pages

1. Open this repository's **Settings → Pages**.
2. Under **Build and deployment**, choose **Deploy from a branch**.
3. Select branch `main` and folder `/ (root)`, then Save.
4. Wait for the site URL to appear on the Pages screen.

## 2. Configure the public Supabase key

1. Open the Supabase project used by TNS News: https://supabase.com/dashboard/project/dueysmzmtpmfqsxjmzhk
2. Find **Project Settings → API Keys** (or the Connect/API section).
3. Copy the **publishable** key or legacy **anon/public** key.
4. In this repository, edit `index.html` and replace `PASTE_YOUR_SUPABASE_PUBLIC_ANON_OR_PUBLISHABLE_KEY_HERE` with that public key.
5. Never use the `service_role` key or any secret key in browser code.

## 3. Set up your editor login and permissions

1. In Supabase, open **Authentication → Users** and create an editor user with an email and password.
2. Copy that user's UUID.
3. Open the Supabase SQL Editor and run the following, replacing the placeholder with the UUID:

```sql
insert into public.tns_admins (user_id)
values ('PASTE-EDITOR-USER-UUID-HERE')
on conflict (user_id) do nothing;
```

4. Ensure the database schema in the original [TNS News repository](https://github.com/jebediahkerman1/TNS-News/blob/main/supabase/schema.sql) has been run in this same Supabase project. It creates the `stories` and `tns_admins` tables and row-level security policies.
5. Open the TNS Review GitHub Pages URL and sign in.

## What it does

- Lists pending stories, newest first.
- Shows author, category, submission time and full story text.
- Lets an authorized editor publish or reject a story.
- Uses Supabase Auth and database row-level security. A secret URL alone is not security.

The dashboard only changes a story's status. Confirm the public TNS News site queries only stories with status `published` for stories to appear publicly.

## Security

The browser contains only a public publishable/anon key. Do not commit passwords, service-role keys, or other secrets.
