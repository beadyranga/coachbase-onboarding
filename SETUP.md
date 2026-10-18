# Setting up the CoachBase onboarding repo

This repo (`beadyranga/coachbase-onboarding`) is public and holds only the NDA
and the signing form. The code lives in the private `beadyranga/coachbase`
repo. When someone signs, the workflow in
`.github/workflows/nda-signature.yml` checks the signature and, if they are on
the allow list, invites them to `beadyranga/coachbase` with write ("push")
access.

## 1. Create the invite token

1. On GitHub, go to your profile picture → **Settings → Developer settings →
   Personal access tokens → Fine-grained tokens → Generate new token**.
2. Name it `coachbase-invite`, and pick an expiry (put a reminder in your
   calendar to renew it).
3. **Resource owner:** `beadyranga`.
4. **Repository access:** *Only select repositories* → `beadyranga/coachbase`
   only. Don't add `coachbase-onboarding` or any other repo.
5. **Permissions → Repository permissions → Administration:** *Read and
   write*. Leave everything else at *No access* (Metadata: Read-only is added
   automatically).
6. Generate it and copy the token. You won't see it again.

## 2. Add the INVITE_TOKEN secret

1. In `beadyranga/coachbase-onboarding`, go to **Settings → Secrets and
   variables → Actions → New repository secret**.
2. Name: `INVITE_TOKEN`. Value: the token from step 1. Save.
3. When the token expires, generate a new one the same way and update this
   secret. Never paste the token anywhere else.

## 3. Approving signers

Nobody is invited unless you approve them, in one of two ways:

- **After they sign (approve by label):** their issue is labelled
  `signed-pending-approval` and you are @mentioned. To approve, open the issue
  and add the `approved` label (Labels, on the right). The invitation is sent
  within a minute. To refuse, close the issue. Only a label added by
  @beadyranga counts.
- **Before they sign (allowed-users.txt):** add their GitHub username to
  `allowed-users.txt` at the repo root and commit it to `main` (through a pull
  request, see step 4). They'll be invited as soon as they sign. One username
  per line, no `@` needed, case doesn't matter, and lines starting with `#`
  are ignored. This repo is public, so the list is visible to anyone.

To remove someone's access later, remove them under `beadyranga/coachbase` →
Settings → Collaborators (and from `allowed-users.txt` if they're on it).
Neither the label nor the list revokes anything by itself.

Re-running the workflow (Actions tab → *NDA signature* → **Re-run all jobs**)
is always safe. It skips people who are already collaborators or already
invited, and doesn't post duplicate comments.

## 4. Protect main

In `beadyranga/coachbase-onboarding`, go to **Settings → Rules → Rulesets →
New ruleset → New branch ruleset**:

1. Name: `protect-main`. Enforcement status: **Active**.
2. **Target branches → Add target → Include default branch.**
3. Turn on:
   - **Restrict deletions**
   - **Block force pushes**
   - **Require a pull request before merging.** Set *Required approvals* to
     `0`, since you're the only maintainer and can't approve your own pull
     requests.
4. Save.

`.github/CODEOWNERS` assigns every file to @beadyranga, so you are requested
as reviewer on every pull request.

Never add collaborators to this repo. People only ever get access to
`beadyranga/coachbase`, through the workflow.

## 5. Require approval for fork pull request workflows

In `beadyranga/coachbase-onboarding`, go to **Settings → Actions → General →
Approval for running fork pull request workflows from contributors**, choose
**Require approval for all external contributors**, and save.

## 6. Test it

1. Add your own username to `allowed-users.txt`.
2. Go to **Issues → New issue → Sign the CoachBase NDA**, sign it, and check
   that the issue gets the `signed` and `invited` labels, an "already has
   access" comment, and is locked.
3. Remove your username from the list again if you like.

## Onboarding someone

1. Send them the link: https://github.com/beadyranga/coachbase-onboarding
2. They read the NDA and submit the **Sign the CoachBase NDA** form.
3. The workflow checks the signature and @mentions you. Add the `approved`
   label to their issue (or add them to `allowed-users.txt` beforehand to
   skip this step).
4. The workflow invites them to `beadyranga/coachbase` with write access,
   comments on the issue and locks it.
5. They accept the invitation from their email or
   https://github.com/beadyranga/coachbase/invitations.
