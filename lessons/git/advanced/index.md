# Pokročilé příkazy v Gitu

## Konfigurace
* `git config [--global] -e`
 
* lokální, globální, jiná identita, GPG klíč, ...

* aliasy
  ```
  [alias]
  br = branch
  co = checkout
  s = status
  c = commit
  l = log --graph --decorate --pretty=oneline --abbrev-commit --all -n5
  ```


## Cvičení: Změna existujícího commitu, rozdílné local+remote větve
* naklonovat/forknout `roboprojekt` repo (https://github.com/PyLadiesCZ/roboprojekt)
* Nová větev (`git co -b vetvicka` / `git br`) do vlastního remote (`git remote -v`)
* přidáme nový soubor `stromecek.txt` -> commit
* `git push origin -u origin vetvicka` (`--set-upstream`)

Větev `vetvicka` teď "trčí" o jeden commit nad `master` lokálně i na githubu (`origin/master`).

* `git c --amend [--no-edit]` změnit commit message - AHOJ
* `git diff origin/master` nic neukáže, ale přesto má jiný hash. Proč? (hlavička je taky součástí hashe)

Chceme ho přesto pushnout.

* `git push` (origin netřeba specifikovat díky minulému `-u`)
* selže, protože lokální ukazatel na `vetvicka` (=commit) nenavazuje na `origin/vetvicka`

Existuje několik možnosti, co se s tím dá dělat (pozn.: Udělat si zálohu jako `vetvicka.orig`).
1. `git rebase origin/vetvicka` - způsobí jen posun ukazatele, změna v commit msg se zahodí
1. `git pull -r` (rebase, výsledek stejný jako výše)
1. teprve `git pull` zakomponuje změnu i do remote větve, ale už jako `merge` commit (udělá to samé, co `git merge origin/vetvicka`)
1. `git push -f` (force) přepíše historii i na githubu


## cherry-pick, rebase


## Commit message good practises
* Separate subject from body with a blank line
* Limit the subject line to 50 characters
* Use the imperative mood in the subject line
* Wrap the body at 72 characters
* Use the body to explain what and why vs. how
