# Pokročilé příkazy v Gitu

![asdf](https://imgs.xkcd.com/comics/git.png)

## Konfigurace
* `git config [--global] -e`
* používat `[Tab]`!
* používat `[Arrow Up/Down]` pro historii
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

## Štábní kultura
* styl commit messages (např. prefix komponenty?)
* nejdůležitější je první řádek commit message - krátký, do 50 zn.
* jeden commit = jedna logická změna
* pojmenování větví (`feature/abc`, `bugfix/abc`, `master`, ...)
* lineární historie vs. PR/MR
* čištění historie pomocí `--amend`, `rebase -i, squash`
* číslo issue v komentáři jako reference
* ...



## stash - dočasné odložení rozdělané práce
Přesuneme se na `master` větev:

* zeditovat `README.md`
* `git co <nekam>`
* `git stash push [-m my-message]`, příp. `--include-untracked` 
* `git stash list`, `pop`, `drop`, `clear`


----------

`watch -n 0.1 --color git l --color=always`

----------


## Cvičení: Změna existujícího commitu, rozdílné local+remote větve
* naklonovat/forknout `roboprojekt` repo (https://github.com/PyLadiesCZ/roboprojekt)
* Nová větev (`git co -b vetvicka` / `git br`) do vlastního remote (`git remote -v`)
* přidáme nový soubor `stromecek.txt` -> commit (na obsahu nezáleží)
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

## `revert`
1. "vyrušit" commit `4d206cef72249632`

## Cvičení: remote, fetch, reset, rebase
**Cíl**: umět přidat remote a synchronizovat si od něj změny
1. přidat si nový remote `https://github.com/mpavlase/roboprojekt.git` (`git@github.com:mpavlase/roboprojekt.git`) jako `martin`
1. (ukázat log)
1. ukázat `git remote show origin`
1. `git fetch` (lokální větve ještě neexistují),
1. přepnout se na `zmena-readme`

### `cherry-pick`, `reset`
1. zkusit si cherry-pick z větve `zmena-readme` na `master`
1. zkusit si to s jiným commitem

Vrátit se do původního stavu (`git reset --hard`):
* `zmena-readme -> martin/zmena-readme`
* `master -> origin/master` - ukázat rozdíl s `git reset --soft` (jen posune ukazatel, neaktualizuje working dir.)

### `rebase`, +interaktivní
Pomocí `rebase` doslova přeroubovat celou větev na `master` (doplní pouze chybějící commity).
* způsobí změnu SHA (rodič v hlavičce)
* `reword`
* `reorder`
* `squash` / `fixup`
* `remove`
* `edit` (možno např. přidat další soubor)

## Samostatné cvičení: "Commitnula jsem do špatné větve!"
* cherry-pick
* reset

## Commit message good practises
https://chris.beams.io/posts/git-commit/

* Separate subject from body with a blank line
* Limit the subject line (= 1st line) to 50 characters
* Use the imperative mood in the subject line
  * *If applied, this commit will **your subject line here***
* Wrap the body at 72 characters
* Use the body to explain what and *why* vs. *how*

## Cvičení: Jak obnovit smazanou větev
* `git reflog`

## Malé drobky
* *na délce nezáleží* (hash)
* meld ![meld](http://meldmerge.org/images/meld-merge-full.png)
* `git log --oneline`
* `git remote prune origin` - smaže lokálních reference na neexistující remote větve
* `git shortlog` - seskupení commitů podle autorů
* `git grep 'Tile'` vs. `git grep 'Tile('`
* `git blame backend.py`
* `git add -p`
* `git rm`, `git mv`
* `git checkout <commit> -- file.py`
