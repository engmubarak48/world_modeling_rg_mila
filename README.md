# Mila World Modelling Reading Group

Source for the reading group website. Built with [Hugo](https://gohugo.io/); one page, no theme.

## Editing

Almost everything lives in two YAML files — no HTML needed.

| What | File |
|---|---|
| Intro paragraphs | `content/_index.md` |
| Sessions (presenter, paper, links) | `data/sessions.yaml` |
| Organizers | `data/organizers.yaml` |
| Intro text on the contact page | `content/contact.md` |
| Time, place, section heading, contact link | `hugo.toml` |
| Styling (colors, spacing) | `assets/css/site.css` |

### Adding a session

The list starts empty — nothing renders until you add entries.

Add a block at the **top** of `data/sessions.yaml`:

```yaml
- date: "Oct 10, 2026"
  presenter: "Name Surname"
  paper: "Title of the paper"
  links:
    paper: "https://arxiv.org/abs/..."
    video: "https://youtu.be/..."   # optional
```

`presenter`, `note`, `upcoming` and every link are optional; anything you leave out simply
doesn't render. Put `upcoming: true` on the next session to give it an "Upcoming" badge, and
remove it afterwards.

### Organizer photos

Drop the file in `assets/images/organizers/`, named after the person's **lowercased first name**
— `arian.jpg`, `artem.png`, `roger.webp`. Nothing else to do: it is matched by name, cropped to
a square and resized at build time, so any size or aspect ratio works and there is no need to
crop it first. Anyone without a photo gets a neutral grey circle.

If a filename cannot be the first name, add `photo:` to that entry in `data/organizers.yaml`:

```yaml
- name: "Le Thuy Duong Nguyen"
  photo: "thuy"      # looks for assets/images/organizers/thuy.*
```

## Contact form

The form on `/contact/` posts to [FormSubmit](https://formsubmit.co/), which emails submissions
straight to the address in `formEndpoint` (`hugo.toml`). There is no account and no server — a
static site cannot send email on its own, so some third-party handler is required.

**One-time activation:** the first time anyone submits the form, FormSubmit emails that address
a confirmation with an "Activate Form" button. Click it once and the form is live for good.
Until then submissions are not delivered, so send one test message yourself after the site goes
up.

**Worth doing later:** the address currently sits in the page's HTML, where scrapers can read it.
After activation FormSubmit emails you a random-string endpoint that does the same job without
exposing it — swap it in:

```toml
formEndpoint = "https://formsubmit.co/your-random-string"
```

On submission people land on `/thanks/` (`content/thanks.md`); FormSubmit shows its own page if
that redirect is unavailable. The hidden `_honey` field is a spam trap FormSubmit honours.

### Sending people somewhere else instead

To hand contact to another organizer — a Google Form, a Slack invite, their own page — set one
line in `hugo.toml`:

```toml
contactURL = "https://forms.gle/..."
```

Both the nav link and the button then point there, and `/contact/` is no longer linked. Delete
`content/contact.md` if you want that page gone entirely.

### Switching to Formspree

If FormSubmit is ever a problem, [Formspree](https://formspree.io/) is the usual alternative
(free tier, needs an account). Paste its endpoint into `formEndpoint` and rename the `_honey`
field to `_gotcha` in `layouts/partials/contact-form.html`.

## Running locally

```bash
hugo server -D     # http://localhost:1313
```

## Deploying

Remote: `git@github.com:engmubarak48/world_modeling_rg_mila.git`

Pushing to `main` builds and publishes via `.github/workflows/hugo.yml`. One-time setup: in the
repo, **Settings → Pages → Build and deployment → Source: GitHub Actions**. The site then lives
at `https://engmubarak48.github.io/world_modeling_rg_mila/`.

The workflow passes the correct `baseURL` at build time, so the value in `hugo.toml` only matters
for local previews — but update it anyway once the final URL is known.
