# Setting up the CoachBase onboarding repo

1. Edit `.github/workflows/nda-signature.yml` and replace `YOUR-GITHUB-USERNAME`
   with your GitHub username (no @).
2. Fill in the [bracketed] fields in `NDA.md` (your name, country/state, city).
3. On GitHub, create a new **public** repository named `coachbase-onboarding`
   with nothing in it (no README, no licence).
4. Upload the files (pick one):
   - **Web:** on the empty repo page, click "uploading an existing file", then
     drag in the *contents* of this folder, including the `.github` folder. On a
     Mac, press Cmd+Shift+. in Finder to show hidden folders like `.github`.
     Commit.
   - **Command line**, from inside this folder:
     ```
     git init -b main
     git add .
     git commit -m "Add NDA signing form"
     git remote add origin https://github.com/YOUR-GITHUB-USERNAME/coachbase-onboarding.git
     git push -u origin main
     ```
5. Check that `.github/ISSUE_TEMPLATE/sign-nda.yml` and
   `.github/workflows/nda-signature.yml` are on GitHub at those exact paths.
6. In the repo, go to Issues → Labels and create `nda-request`.
7. Test it: Issues → New issue → "Sign the CoachBase NDA", sign it yourself,
   and check that the bot labels it `nda-signed` and pings you.
8. You can delete this SETUP.md from the repo afterwards.
