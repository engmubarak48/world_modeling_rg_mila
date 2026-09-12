# World Modelling Reading Group

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

Put square images in `static/images/organizers/` and point at them from
`data/organizers.yaml` (`image: "/images/organizers/name.jpg"`). Anyone without a photo gets a
neutral grey circle, so it looks fine either way.

## Contact

The "Contact" link in the nav has two modes, set in `hugo.toml`.

**Point it somewhere else.** If the form should be run by another organizer — a Google Form, a
Slack invite, a personal page — just set:

```toml
contactURL = "https://forms.gle/..."
```

Nothing else needs to change; the link goes straight there. Delete `content/contact.md` if you
want the built-in page gone entirely.

**Or use the built-in page** at `/contact/`. Leave `contactURL = ""`, create a free form at
[formspree.io](https://formspree.io/), and paste its endpoint:

```toml
formEndpoint = "https://formspree.io/f/abcdwxyz"
```

Submissions are emailed to whoever owns the Formspree form, so it can sit with any organizer.
The form has a hidden `_gotcha` spam trap that Formspree honours automatically. With
`formEndpoint` blank, the page still renders but Send is disabled and a mailto fallback shows —
that is the current state.

## Running locally

```bash
hugo server -D     # http://localhost:1313
```

## Deploying

Pushing to `main` builds and publishes via `.github/workflows/hugo.yml`. One-time setup: in the
repo, **Settings → Pages → Build and deployment → Source: GitHub Actions**.

The workflow passes the correct `baseURL` at build time, so the value in `hugo.toml` only matters
for local previews — but update it anyway once the final URL is known.
