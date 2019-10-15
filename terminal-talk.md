# Terminal Velocity
### Speaker: Chris Waldon, IBM SRE
### Sponsored by Red Hat

## philosphy
what makes a good tool good? long list of items
feedback is the one he likes the most, the core of agility

## tools
### getting around
you end up doing cd tab, tab, tab

#### Z - jump around
* {ba,z}sh: github.com/rupa/z
* fish:github.com/jethrokuan/z
* 100% shell script
* frequency
* use instead of cd
  * need to use it once so it can learn
  * has tab completion
  * stores files in .local/share/z/data
  db format: dir | number of times | ...

 finding stuff

 new shiny tools instead of find, grep and locate.

#### fzf -fuzzy find
 * github.com/junegunn/fzf
 * golang
 * fuzzy, not regex
 * smart case
 * can be used like:
    * find to find files
    * grep to search within files
    * decision to make a decision like what branch to checkout
* example
for example, dmesg dumps everything; dmesg | fzf. Then type
error and it finds all the error in different formats.
It also find "allocatE ResouRce fOR EISA"

fzf emits to stdout

gc script he wrote for git checkout using fzf to figure out
what branch to checkout

fzf can be used to search history

#### fd - find
* github.com/sharkdp/fd
* rust
* use instead of find
* heuristics
  * skip hidden files by default
  * skip ignored files by default
  * smartcase
* even without heuristics, 9 times faster than find
  * it can read .gitignore and other source control types and ignores those as
    well

#### ripgrep = grep, but better
* github.com/BurntSushi/ripgrep
* rust
* use instead of grep
* heuristics:
  * recursive file search by default
  * skip hidden files by default
  * skip ignored files by default
* you can output json if you want as well

example: rg uinput vs grep -r uinput (SLOW)

rged const (rged is a script that uses rg and fzf)


#### gron - make json greppable
* github.com/tomnomnom/gron

gron converts the json into equivalent javascript

combines really well with yaml2json and json2yaml

### Looking at things

#### exa = ls for humans
* github.com/ogham/exa
* rust
* alias to ls

color coded permissions, understands git, also replaces tree

-h instead of human readable gives you headers

#### bat - cat with wings
* github.com/sharkdp/bat
* rust

#### hexyl = read the matrix
* github.com/sharkdp/hexyl
* similar to xxd but color coded

### fixing ux
#### detox
* detox.sourceforge.net
* c
* inline form of it too (inline-detox) uses stdin and renames on the fly


#### fish
* github.com/fish-shel/fish-shell
* c++
* fish_update_completions
* funced and funcsave
* location-sensitive completion

can parse your  man pages and create completions for them
can tell you if no executable exists

#### entr - run commands when files change
* eradman.com/entrproject

### Multitasking
#### tmux
* https://github.com/tmux/tmux

### Editing

#### kakoune
* github.com/mawww/kakoune
* C++
* vi-like, but constant feedback

### Contact
christopher.waldon.dev@gmail.com
keybase.io/whereswaldon
waldon.blog/contact
github.com/whereswaldon

git.sr.ht/~whereswaldon/presentations
