Rewrite git history to remove secret files
Short summary of the process I followed to remove secret.txt / secret.md from the entire git history and prevent them from coming back.

Create a safety backup of the current work

Create/rename the working branch to something like backup-my-code so nothing is lost.
Clone a fresh copy of the remote (all branches)

git clone --no-single-branch https://github.com/yourname/your-repo.git my-repo-clean
cd my-repo-clean
Rewrite history to drop the secret files
git filter-repo --invert-paths --path secret.txt --path secret.md --force
Remove any remaining tracked copies and update .gitignore
git rm -- secret.txt secret.md 2>/dev/null || true
echo "secret.txt" >> .gitignore
echo "secret.md"  >> .gitignore
echo "*.secret"   >> .gitignore   # just in case
git add .gitignore
git commit -m "Remove secret files + update .gitignore"
add origin
Force-push the cleaned history back to the remote
git push --force --all
git push --force --tags
