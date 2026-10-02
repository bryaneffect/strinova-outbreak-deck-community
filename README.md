# Strinova Outbreak Decks

Strinova Outbreak Decks is a community site for sharing and discovering deck builds for Strinova Outbreak. It is an independent community project, not a recreation of the official Strinova website. Its visual theme is inspired by the official site.

## Features

- Browse public decks without signing in.
- Search by deck name, description, category, author, or deck code.
- Filter decks by Superstring or Crystalline energy type and by category.
- Open a deck details dialog to view its tags, description, author, and selected cards.
- Copy deck codes.
- Sign in with Supabase email authentication to upload, edit, or delete your own decks.
- Select multiple Superstring or Crystalline card images when creating or editing a deck.
- Search the card picker by name and preview saved card selections in deck details.
- Switch between Indonesian and English; the language preference is saved in local storage.
- View website updates and a responsive layout for desktop and mobile screens.

## Requirements

- A Supabase project with email authentication enabled
- A modern web browser
- A static web server or static hosting provider

The application does not require Node.js. A local static server is recommended; opening `index.html` directly may also work, depending on browser restrictions and network access.

## Setup

1. Create a Supabase project and enable email authentication.
2. Create a `public.decks` table with these columns:

   | Column | Type | Notes |
   | --- | --- | --- |
   | `id` | integer or identity primary key | Unique deck identifier |
   | `name` | text | Deck name |
   | `author` | text | Author display name |
   | `type` | text | `Superstring` or `Crystalline` |
   | `category` | text | Categories stored as comma-separated text |
   | `description` | text | Deck description |
   | `code` | text | Exported deck code |
   | `created_at` | timestamptz | Set a default of `now()` |

3. Set `SUPABASE_URL` and `SUPABASE_KEY` in `script.js` to your project URL and publishable key. Do not use a service role key in client-side code.
4. Run `supabase-auth-migration.sql` in the Supabase SQL Editor. The migration adds `user_id` and `card_names`, enables row-level security, and creates policies for public reads and owner-only inserts, updates, and deletes.
5. If the new `card_names` column is not immediately recognized by the API, reload the PostgREST schema cache from the Supabase SQL Editor:

   ```sql
   notify pgrst, 'reload schema';
   ```

6. Serve the project directory with a static server or deploy it to a static host, then open `index.html`.

The migration is designed for the existing `decks` table and does not create the base table. Apply it to the same Supabase project configured in `script.js`.

## Deck Data

The `category` column stores one or more category names separated by commas, such as `Damage, Armor`. The `card_names` column stores the selected card filenames as a PostgreSQL text array. The application uses the deck's `type` to locate each card image in the corresponding card folder.

Decks created before card selection was added may have an empty `card_names` array. Edit those decks, select their cards, and save to add the card preview.

## Assets and Credits

- `cards/superstring/` and `cards/crystalline/` contain the card PNG files.
- `cards.js` lists the card names used by the upload picker.
- `assets/news/` contains the website update banner and preview images.
- `yvette summer.png`, `strinova-logo.png`, and `yvette.png` are used in the page design.
- The footer identifies Strinova as the source of artwork and the inspiration for the site's visual theme. This project is an unofficial community resource and is not affiliated with the official Strinova website.

## Project Files

- `index.html` contains the page structure, update section, deck template, and dialogs.
- `script.js` contains the Supabase client, authentication, deck operations, filtering, localization, and card previews.
- `cards.js` contains the Superstring and Crystalline card catalog.
- `style.css` contains the responsive layout and visual theme.
- `supabase-auth-migration.sql` adds ownership and card selection columns and configures row-level security policies.
