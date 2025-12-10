# Copilot Instructions - מרדכי סיונוב גידור אתרי בניה

## Project Overview
Hebrew-language (RTL) marketing website for a construction site fencing contractor in Israel. Static site generated from JSON data + HTML templates via Python build script.

## Architecture Pattern: Template-Based Static Generation

### Data-Driven Page Generation
- **`build.py`** - Main build script that generates all service/city pages
- **`data.json`** - City/location pages data (21 cities with SEO content)
- **`data_services.json`** - Service pages data (11 services with full content)
- **`template_city.html`** - Template for city landing pages
- **`template_service.html`** - Template for service detail pages

### Build Workflow
```bash
python build.py
```
This generates:
- `service-areas/*.html` from `data.json` + `template_city.html`
- `services/*.html` from `data_services.json` + `template_service.html`

### Template Placeholder Convention
Placeholders use `{placeholder_name}` syntax (not `{{ }}`):
```html
<h1>קבלן גידור אתרי בניה ב{city_name}</h1>
<p>{seo_desc_start}</p>
```

## Key Conventions

### Hebrew RTL Layout
- All HTML uses `lang="he" dir="rtl"`
- CSS uses `direction: rtl` on body
- Font: Heebo (Google Fonts)
- Icons: Font Awesome 6

### CSS Variables (style.css)
```css
--primary-color: #0c2d48;   /* Dark blue */
--secondary-color: #2e8bc0; /* Light blue */
--accent-color: #f39c12;    /* Orange CTA */
```

### File Naming
- City pages: `service-areas/{city-slug}.html` (e.g., `tel-aviv.html`)
- Service pages: `services/{service-slug}.html` (e.g., `iskurit.html`)
- Use kebab-case for all filenames

## Adding Content

### New City Page
1. Add entry to `data.json`:
```json
{
    "filename": "city-name.html",
    "city_name": "שם העיר",
    "region": "אזור",
    "seo_desc_start": "תיאור SEO",
    "unique_paragraph_1": "פסקה ייחודית 1",
    "unique_paragraph_2": "פסקה ייחודית 2"
}
```
2. Run `python build.py`

### New Service Page
1. Add entry to `data_services.json` with all required fields:
   - `filename`, `service_name`, `menu_group` ("primary"/"secondary")
   - SEO fields: `meta_title`, `meta_description`, `meta_keywords`, `canonical`
   - Content: `hero_title`, `intro_paragraph`, `benefits[]`, `uses[]`, `specs[]`
   - `related[]` - Array of related service cards
2. Run `python build.py`

## Important Notes

### Generated vs Source Files
- **DO NOT edit** files in `services/` or `service-areas/` directly - they are generated
- **Edit only**: `data.json`, `data_services.json`, templates, `index.html`, `style.css`

### Menu Groups
Services with `"menu_group": "primary"` appear in first menu column, others in second.

### Navigation Sync
Menu links in templates and `index.html` must be manually kept in sync - not auto-generated.
