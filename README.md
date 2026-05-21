# Career Path Static Site

This is a simple static webpage.

## How to edit the career data

Open `career.json` and edit the `projects` array.

Each project supports:

```json
{
  "name": "Project or company name",
  "period": "2022–2025",
  "companyLogo": "assets/company-logo.png",
  "projectImages": [
    { "src": "assets/project-shot-1.png", "alt": "Optional alt text" },
    { "src": "assets/project-shot-2.png", "alt": "Optional alt text" }
  ],
  "heroImage": "assets/fallback-image.png",
  "url": "https://optional-link.example",
  "description": "Brief paragraph describing what you did."
}
```

Notes:
- `companyLogo` is optional.
- `projectImages` is optional. If present, the first image is used as the large hero image.
- `heroImage` is still supported as a fallback when `projectImages` is not present.
- `url` is optional. If present, the images, logo, title, and description link to it.

## How to add real images

Put your files in the `assets` folder, then update the paths in `career.json`.

## How to run locally

Because the page loads `career.json`, open it through a local web server instead of double-clicking the file.

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000
```
