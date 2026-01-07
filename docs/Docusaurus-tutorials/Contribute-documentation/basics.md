---
sidebar_position: 1
---

# How to contribute and documentate your progress in DIF3DGELA: 
The documentation in Dif3D gela uses docusaurus framework to documentate the progress of the students in markdown language. 

# Getting Started: Project Documentation

This repository uses the [Docusaurus](https://docusaurus.io/) framework to document student progress and tutorials in Markdown.

## Prerequisites

- A [GitHub account](https://github.com/)
- Git installed: https://git-scm.com/downloads
- Basic Git knowledge (guide: https://docs.github.com/en/get-started/using-git)
- Basic Markdown knowledge (guide: https://www.markdownguide.org/basic-syntax/)

## Getting Access

To add a tutorial or document your work:
1. Request repository write access from the class coordinators.
2. Once access is granted, proceed with the installation steps below.

## Clone the Repository

```bash
git clone git@github.com:dif3dgela/KickOFFTutorials.git
cd KickOFFTutorials/dif3d-documentation
```

## Install Dependencies

Using npm:
```bash
npm install
```

Or using yarn (if you prefer):
```bash
yarn install
```

## Start the Local Development Server

Using npm:
```bash
npm run start
```

Using yarn:
```bash
yarn start
```

Docusaurus will open the site locally (default: http://localhost:3000/).

## Adding Documentation

1. Create a new branch:
   ```bash
   git switch -c docs/your-topic
   ```
2. Add a new Markdown file under:
   ```
   docs/
   ```
   Organize it in an existing folder or create one if needed.
3. (Optional) Add frontmatter to control ordering:
   ```markdown
   ---
   sidebar_position: 3
   ---
   ```
4. Write content using Markdown. Use relative links when referencing other docs.
5. Build locally (optional) to test production build:
   ```bash
   npm run build
   ```
6. Commit and push:
   ```bash
   git add .
   git commit -m "docs: add tutorial on <topic>"
   git push -u origin docs/your-topic
   ```
7. Open a Pull Request on GitHub.

## Style Guidelines (Quick)

- Use sentence case for headings.
- Prefer present tense.
- Add code blocks with language hints: 