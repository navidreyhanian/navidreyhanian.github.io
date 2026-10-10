# navidreyhanian.github.io

Personal website of Navid Reyhanian: https://navidreyhanian.github.io

Built with [Jekyll](https://jekyllrb.com/) and the [al-folio](https://github.com/alshedivat/al-folio) v1.2 template (MIT license).

## Where things live

| Page         | Edit                                                       |
| ------------ | ---------------------------------------------------------- |
| About (home) | `_pages/about.md`, photo at `assets/img/prof_pic.jpg`      |
| Projects     | one Markdown file per project in `_projects/`              |
| Publications | `_bibliography/papers.bib`                                 |
| CV           | `_data/cv.yml`; PDF resume at `files/Resume_Reyhanian_wr.pdf` |
| Social links | `_data/socials.yml`                                        |
| Site settings | `_config.yml`                                             |

## Preview locally

Requires Ruby 3.3+ and ImageMagick (`brew install ruby imagemagick`). Without ImageMagick the build still finishes, but the resized `.webp` images are missing and photos may not show in the preview.

```bash
export PATH=/opt/homebrew/opt/ruby/bin:$PATH
bundle install
bundle exec jekyll serve
```

Then open http://localhost:4000.

## Deploy

Pushing to `master` runs `.github/workflows/deploy.yml`, which builds the site and publishes it to the `gh-pages` branch. GitHub Pages serves from `gh-pages`.
