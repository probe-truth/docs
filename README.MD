
✅ 1. Commit Your Changes in VS Code
Make sure you've saved all changes, then commit them:

git add .
git commit -m "Update API docs and site configuration"
git push origin main
Replace main with your actual branch name if it's different.

✅ 2. Build the MkDocs Site
In your terminal (in the root folder of your project where mkdocs.yml is), run:

mkdocs build
This generates the static site files into a site/ folder.

✅ 3. Deploy to GitHub Pages
Now deploy it using:

mkdocs gh-deploy
This command:

Builds the site (if not already)

Pushes the contents of the site/ folder to the gh-pages branch

Updates your GitHub Pages site

✅ 4. Verify Deployment
Go to:
📎 https://probe-truth.github.io/docs/
and confirm your changes are visible.