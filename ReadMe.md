# 🚀 ai-labs-claude-skills  

<div align="center">

[![npm version](https://img.shields.io/npm/v/ai-labs-claude-skills.svg?color=blue)](https://www.npmjs.com/package/ai-labs-claude-skills)
[![npm downloads](https://img.shields.io/npm/dt/ai-labs-claude-skills.svg?color=brightgreen)](https://www.npmjs.com/package/ai-labs-claude-skills)
[![license](https://img.shields.io/npm/l/ai-labs-claude-skills.svg)](https://github.com/ailabs-393/ai-labs-claude-skills/blob/main/LICENSE)
[![Node.js](https://img.shields.io/badge/Node.js-%3E%3D18-green)](https://nodejs.org)

</div>

> 🧠 A collection of reusable **"skills"** for Claude AI and developer tooling.  
> Each skill is a focused, modular package that brings automation to your dev workflows — from SEO analysis to document parsing, CI/CD generation, Docker automation, and more.

---

## ✨ Key Benefits

- ⚙️ **Modular:** Pick and use only the skills you need.  
- 🚀 **Ready-to-run Scripts:** Includes tools for SEO analysis, sitemap generation, document unpacking, resume creation, and more.  
- 🤖 **Automations:** Automatically generates index and package files on build.  
- 🔁 **Reproducible Builds:** Ensures clean and consistent skill packaging.  
- 🧩 **Extensible:** Easily add new skills by following existing folder patterns.  

---

## ⚡️ Installation

```bash
npm i ai-labs-claude-skills
```

## And if want to download the latest version than go for this:
```
npm i ai-labs-claude-skills@latest
```

## Quick start
1. Install (postinstall will attempt to copy skills into the host project):
   npm install
2. Build distribution:
   npm run build
3. Generate missing package or index files:
   npm run gen:pkg
   npm run gen:index

## Notable files & scripts
- Root package manifest: package.json
- Installer that copies skills into projects: install-skills.mjs
- Helpers to create packages/index files: create-packages.js, generate-index-files.js
- Skills directory: packages/skills/ (each skill contains scripts, assets, and a SKILL.md)

## 📚 Available Skills

All skills are organized by category for easy discovery:

### 🚀 Development & DevOps
- **`api-generator`** - REST/GraphQL API generation with OpenAPI/Swagger documentation and best practices ⭐ NEW
- **`security-auditor`** - Comprehensive security analysis, OWASP Top 10 checks, and vulnerability assessment ⭐ NEW
- **`performance-optimizer`** - Performance profiling, bottleneck identification, and optimization strategies ⭐ NEW
- **`database-designer`** - Database schema design, migration generation, and ER diagram creation ⭐ NEW
- **`kubernetes-deployer`** - Kubernetes manifest generation, Helm charts, and container orchestration ⭐ NEW
- **`cicd-pipeline-generator`** - Automated CI/CD pipeline configuration for GitHub Actions, GitLab CI, Jenkins, and more
- **`docker-containerization`** - Docker container setup, Dockerfile generation, and orchestration configurations
- **`codebase-documenter`** - Automatic documentation generation for codebases (API docs, README, architecture diagrams)
- **`test-specialist`** - Test generation, bug analysis, and testing pattern implementation
- **`tech-debt-analyzer`** - Technical debt analysis, ADR generation, and debt register management

### 📊 Data & Analytics
- **`data-analyst`** - Data cleaning, analysis, and statistical modeling with imputation methods
- **`csv-data-visualizer`** - CSV data visualization with charts and interactive dashboards
- **`business-analytics-reporter`** - Business analytics reports with frameworks and visualization guides

### 📝 Business & Documentation
- **`business-document-generator`** - Generate business plans, proposals, and budgets
- **`pitch-deck`** - Create compelling pitch decks with best practices
- **`research-paper-writer`** - Academic paper writing with IEEE/ACM formatting
- **`resume-manager`** - Professional resume creation and optimization
- **`finance-manager`** - Financial analysis, budgeting, and reporting

### 🎨 Design & Marketing
- **`brand-analyzer`** - Brand analysis, guidelines creation, and brand archetype identification
- **`frontend-enhancer`** - UI/UX improvements with design principles and color palettes
- **`seo-optimizer`** - SEO analysis, metadata generation, schema markup, and sitemap creation
- **`social-media-generator`** - Social media content for Facebook, LinkedIn, Instagram, Twitter

### 📄 Document Processing
- **`document-skills`** - Comprehensive document handling:
  - **pdf** - PDF creation, parsing, and form handling
  - **docx** - Word document generation with OOXML
  - **xlsx** - Excel spreadsheet automation
  - **pptx** - PowerPoint presentation creation

### 🎬 Creative & Content
- **`script-writer`** - Screenplay and script writing with proper formatting
- **`storyboard-manager`** - Story structure and character development
- **`travel-planner`** - Travel itinerary planning with cultural etiquette guides
- **`nutritional-specialist`** - Nutrition planning and dietary analysis

### 🤖 Productivity
- **`personal-assistant`** - Personal productivity and task management
- **`startup-validator`** - Startup idea validation with frameworks and research templates

## 🧠 Example Use Cases

- Use these Claude "skills" to automate and extend your workflows:

1. 🔍 SEO report and metadata generation
2. 🧾 Document unpacking and validation
3. 🧱 Docker container creation and setup
4. 🧠 Resume or report generation scripts
5. ⚙️ CI/CD pipeline auto-generation
6. 📊 Developer project automation utilities

## 🤝 Contributing

- We welcome contributions from the community!
Here’s how to add a new skill:

1. Create a new folder inside packages/skills/. (or just add a new skill)

2. Include a SKILL.md file describing your skill.

3. Add your scripts, assets, or templates.
   
5. Run these commands to create index and package inside skills

6. Follow the existing project structure for consistency.

7. Submit a pull request with clear details.

## License
- This project is licensed under the MIT License

## HAPPY CODING :)
