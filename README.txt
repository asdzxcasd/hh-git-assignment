# The following commands clone frontik and make two directories in the current directory: frontik_without_tests and frontik_only_tests

# frontik_without_tests
git clone https://github.com/hhru/frontik ./frontik_without_tests
cd ./frontik_without_tests
# track all remote branches
for branch in `git branch -a | grep remotes | grep -v HEAD | grep -v master `; do
   git branch --track ${branch#remotes/origin/} $branch
done
# origin is not needed anymore as ass commits are going to be rewritten
git remote rm origin
# perform splitting
git filter-branch --force --prune-empty --tree-filter 'rm -rf ./tests' --tag-name-filter cat -- --all
cd ../


# frontik_only_tests
git clone https://github.com/hhru/frontik ./frontik_only_tests
cd ./frontik_only_tests
# track all remote branches
for branch in `git branch -a | grep remotes | grep -v HEAD | grep -v master `; do
   git branch --track ${branch#remotes/origin/} $branch
done
# origin is not needed anymore as ass commits are going to be rewritten
git remote rm origin
# perform splitting
git filter-branch --force --prune-empty --subdirectory-filter ./tests --tag-name-filter cat -- --all
cd ../
