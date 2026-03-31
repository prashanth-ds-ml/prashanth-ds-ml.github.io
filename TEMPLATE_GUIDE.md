# Portfolio Template Guide

This is a clean, modular Jekyll portfolio template designed for easy content updates through YAML data files.

## How to Update Content

### 1. Personal Information
Edit `_config.yml`:
```yaml
title: "Your Name"
description: "Your professional tagline"
timezone: Your/Timezone
```

### 2. Projects
Edit `_data/projects.yml`:
```yaml
- title: "Project Title"
  description: "Project description"
  code: "https://github.com/your-username/repo"
  demo: "https://demo-url.com"
  post: "/project-blog-post/"
  skills: ["Skill1", "Skill2"]
  tools: ["Tool1", "Tool2"]
```

### 3. Skills
Edit `_data/skills.yml`:
```yaml
- name: "Skill Name"
  category: "Category"
  level: "Beginner|Intermediate|Advanced|Expert"
```

### 4. Papers
Edit `_data/papers.yml`:
```yaml
- title: "Paper Title"
  authors: "Author1, Author2"
  venue: "Conference/Journal Name"
  year: "2023"
  summary: "Brief summary"
  pdf: "https://link-to-pdf"
  code: "https://github.com/repo"
```

### 5. Certificates
Edit `_data/certificates.yml`:
```yaml
- name: "Certificate Name"
  provider: "Issuing Organization"
  issued: "Month DD, YYYY"
  url: "https://credential-url"
  summary: "What you learned"
```

### 6. Contact Info
Edit `_data/contact.yml`:
```yaml
email: "your@email.com"
linkedin: "https://linkedin.com/in/your-profile"
github: "https://github.com/your-username"
availability: "Your availability statement"
```

### 7. Blog Posts
Add markdown files to `_posts/` folder:
```
_posts/YYYY-MM-DD-title.md
```

## Structure

- **Modular Components**: Sections are built using includes (`_includes/`)
- **Data-Driven**: All content managed through YAML files (`_data/`)
- **Auto-generated**: Skills section automatically aggregates from projects
- **Responsive**: Built with CSS Grid/Flexbox for mobile-friendly design

## Customization

- **Styling**: Edit `assets/style.css`
- **Layout**: Modify HTML templates in `_includes/`
- **Navigation**: Update `_includes/navigation.html`
- **Sections**: Add/remove sections in `index.html`

## Deployment

Push to GitHub repository with `gh-pages` branch enabled for automatic GitHub Pages deployment.