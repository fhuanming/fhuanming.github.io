# Blog

## Website

[fhuanming.github.io](https://fhuanming.github.io/)

## Theme

Powered by [jekyll-theme-next](https://github.com/simpleyyt/jekyll-theme-next)

## Development

### Running the Jekyll Server Locally

This repository has a local Ruby version (3.2.9) configured via `.ruby-version`. To run the blog locally:

1. **Initialize rbenv** (required each session):
   ```bash
   eval "$(rbenv init - zsh)"
   ```

2. **Install dependencies** (first time only or when Gemfile changes):
   ```bash
   bundle install
   ```

3. **Start the Jekyll development server**:
   ```bash
   bundle exec jekyll serve --livereload --host 0.0.0.0 --port 4000
   ```

4. **Access the site**:
   - Open your browser and go to: http://localhost:4000/
   - The site will automatically reload when you make changes to files

5. **Stop the server**:
   - Press `Ctrl+C` in the terminal

### One-Command Workflow

You can combine steps 1 and 3 for convenience:
```bash
eval "$(rbenv init - zsh)" && bundle exec jekyll serve --livereload --host 0.0.0.0 --port 4000
```

### Requirements

- Ruby 3.2.9 (managed via rbenv, automatically selected by `.ruby-version`)
- Bundler 2.7.1+
- Jekyll and dependencies (installed via `bundle install`)

### Troubleshooting

If you see bundler version errors:

1. **Verify rbenv is working after initialization**:
   ```bash
   eval "$(rbenv init - zsh)"
   rbenv version  # Should show: 3.2.9 (set by .ruby-version)
   ruby --version  # Should show Ruby 3.2.9
   ```

2. **If you get a different Ruby version, for example the system defualt version 2.6.10**, you forgot to run the rbenv init command