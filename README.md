# Blogstacle

[![Jekyll](https://img.shields.io/badge/Jekyll-Theme%20Chirpy-blue)](https://github.com/cotes2020/jekyll-theme-chirpy)
[![License](https://img.shields.io/github/license/afrintisha/blogstacle)](LICENSE)

**Blogstacle** is a personal blog built using the [Chirpy Jekyll Theme](https://github.com/cotes2020/jekyll-theme-chirpy). It serves as a platform to document projects, share knowledge, and explore the world of self-hosting, homelabs, and technology.

## Features

- **Homelab Enthusiast's Journal**: Posts about building and managing a homelab, including virtualization, containerization, and networking.
- **Knowledge Sharing**: Tutorials, guides, and insights into various technologies.
- **Personal Touch**: A mix of professional and personal content, including anecdotes and lessons learned.
- **Chirpy Theme**: Powered by the feature-rich Chirpy Jekyll theme, offering a clean and modern design.

## Repository Structure

```plaintext
.
├── _config.yml          # Site configuration
├── _data/               # Data files (e.g., authors, contact info)
├── _plugins/            # Custom Jekyll plugins
├── _posts/              # Blog posts
├── _tabs/               # Navigation tabs (e.g., About, Tags, Archives)
├── assets/              # Static assets (images, CSS, JS)
├── tools/               # Utility scripts (e.g., testing)
├── Gemfile              # Ruby dependencies
├── index.html           # Homepage
└── README.md            # This file
```

## Getting Started

### Prerequisites

- [Ruby](https://www.ruby-lang.org/en/) (>= 2.5)
- [Bundler](https://bundler.io/)
- [Jekyll](https://jekyllrb.com/)

### Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/afrintisha/blogstacle.git
   cd blogstacle
   ```

2. Install dependencies:

   ```bash
   bundle install
   ```

3. Serve the site locally:

   ```bash
   bundle exec jekyll serve
   ```

4. Open your browser and navigate to `http://localhost:4000`.

## Usage

- **Writing Posts**: Add new Markdown files to the `_posts/` directory. Follow the naming convention `YYYY-MM-DD-title.md`.
- **Customizing Tabs**: Modify files in the `_tabs/` directory to update navigation tabs like "About" and "Tags."
- **Testing**: Use the `tools/test.sh` script to build and test the site content.

## Deployment

This site can be deployed to GitHub Pages or any static hosting platform. For GitHub Pages, ensure the `gh-pages` branch is configured in your repository settings.

## Contributing

Contributions are welcome! Feel free to open issues or submit pull requests to improve the blog.

## License

This repository is licensed under the [MIT License](LICENSE).

---

Happy blogging! 🚀