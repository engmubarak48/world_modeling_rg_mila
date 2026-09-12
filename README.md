# Mila World Modelling Reading Group

Website for the reading group. Built with [Hugo](https://gohugo.io/) — one page, no theme.

```bash
hugo server     # http://localhost:1313
```

## What to edit

| What | Where |
|---|---|
| Intro text | `content/_index.md` |
| Schedule | `data/sessions.yaml` |
| Organizers | `data/organizers.yaml` |
| Time, place, contact settings | `hugo.toml` |
| Styling | `assets/css/site.css` |

### Adding a session

Newest at the top of `data/sessions.yaml`:

```yaml
- date: "Oct 10, 2026"
  presenter: "Name Surname"
  paper: "Title of the paper"
  links:
    paper: "https://arxiv.org/abs/..."
    video: "https://youtu.be/..."   # optional
```

Only `date` and `paper` are required. `upcoming: true` adds an "Upcoming" badge — remove it
after the session happens.

### Adding an organizer

```yaml
- name: "Name Surname"
  affiliation: "Mila, Université de Montréal"
  url: "https://their-website.com"    # optional
```

For the photo, drop a file in `assets/images/organizers/` named after their lowercased first
name — `arian.jpg`, `artem.png`, any format. It gets cropped square and resized automatically,
so no need to prepare it. No photo means a grey circle.

If the filename can't be the first name, add `photo: "othername"` to their entry.

## Contact form

The form on `/contact/` posts to [FormSubmit](https://formsubmit.co/), which emails submissions
to the address in `formEndpoint`. A static site can't send email itself, so it needs a handler
like this.

**The form must be activated once.** On the first submission FormSubmit emails that address an
"Activate Form" link. Until someone clicks it, nothing is delivered — so send a test message
yourself before announcing the site.

After activating, FormSubmit gives you a random-string endpoint that hides the address from
scrapers. Worth swapping in:

```toml
formEndpoint = "https://formsubmit.co/your-random-string"
```

To point contact somewhere else entirely — a Google Form, a Slack invite — set `contactURL` in
`hugo.toml` and both the nav link and the button follow it.

## Deploying

Push to `main`; GitHub Actions builds and publishes. One-time setup:
**Settings → Pages → Source: GitHub Actions**.
