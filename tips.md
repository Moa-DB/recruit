## Master for submodules

Run this command in the git master repo to pull all latest master branches in the submodules:
git submodule update --remote --merge

Then add and commit these changes for an up-to-date master repo:
git add .
git commit -m "message"
