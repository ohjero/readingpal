# Reading PAL v96.0 - GitHub Pages

This package is prepared for **GitHub Pages + Supabase**. The Reading PAL interface remains a static `index.html`; Supabase provides authentication, synchronized learner data, feedback, activity status, and administrator access.

## Files to upload to GitHub

Upload these files to the root of your repository:

- `index.html`
- `.nojekyll`

`SUPABASE_SETUP.sql` and this README are setup/reference files. They can remain in the repository, but they are not required by the running webpage.

## Recommended GitHub repository

For the simplest URL, create a repository named:

`YOUR-GITHUB-USERNAME.github.io`

Then enable **Settings -> Pages -> Deploy from a branch -> main -> /(root)**.

Your site will be:

`https://YOUR-GITHUB-USERNAME.github.io/`

A normal repository such as `reading-pal` also works; its URL will be:

`https://YOUR-GITHUB-USERNAME.github.io/reading-pal/`

The current Reading PAL file is self-contained, so the project-subfolder URL is supported.

## Supabase setup

1. Open Supabase -> SQL Editor -> New query.
2. Paste the entire contents of `SUPABASE_SETUP.sql` and run it.
3. In **Authentication settings**, disable **Confirm email** for this username-only Reading PAL account design. Reading PAL internally maps usernames to synthetic authentication email addresses; users do not need to receive email.
4. Create your administrator through Reading PAL's normal **Create account** screen first.
5. Promote that account in Supabase SQL Editor:

```sql
update public.reading_pal_profiles
set role = 'admin'
where lower(username) = lower('YOUR_ADMIN_USERNAME');
```

6. Triple-click/tap the Reading PAL logo and log in with that administrator username and password.

## Security

- The HTML contains only the Supabase **publishable** browser key. This key is intentionally public in browser applications.
- **Never** put a Supabase secret key or service-role key in GitHub or `index.html`.
- Teacher isolation is enforced with Supabase Row Level Security (RLS).
- Passwords are handled by Supabase Auth; Reading PAL does not store readable passwords.
- Downloaded Reading PAL backups remain encrypted with AES-256-GCM using the backup passphrase.
- GitHub Pages and Supabase both use HTTPS.

## Updating Reading PAL later

Before replacing `index.html`, download an encrypted master backup from the Reading PAL administrator panel. Replacing the GitHub HTML does not erase Supabase teacher accounts or learner data because those records live in Supabase, not GitHub.
