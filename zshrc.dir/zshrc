export TERM=xterm-256color # setenv TERM 256color
autoload -Uz promptinit
promptinit
#prompt adam1

setopt histignorealldups sharehistory

# Use emacs keybindings even if our EDITOR is set to vi
#bindkey -e

# Keep 1000 lines of history within the shell and save it to ~/.zsh_history:
HISTSIZE=1000
SAVEHIST=1000
HISTFILE=~/.histfile
unsetopt beep

# Use modern completion system
autoload -Uz compinit
compinit

zstyle ':completion:*' auto-description 'specify: %d'
zstyle ':completion:*' completer _expand _complete _correct _approximate
zstyle ':completion:*' format 'Completing %d'
zstyle ':completion:*' group-name ''
zstyle ':completion:*' menu select=2
eval "$(dircolors -b)"
zstyle ':completion:*:default' list-colors ${(s.:.)LS_COLORS}
zstyle ':completion:*' list-colors ''
zstyle ':completion:*' list-prompt %SAt %p: Hit TAB for more, or the character to insert%s
zstyle ':completion:*' matcher-list '' 'm:{a-z}={A-Z}' 'm:{a-zA-Z}={A-Za-z}' 'r:|[._-]=* r:|=* l:|=*'
zstyle ':completion:*' menu select=long
zstyle ':completion:*' select-prompt %SScrolling active: current selection at %p%s
zstyle ':completion:*' use-compctl false
zstyle ':completion:*' verbose true

zstyle ':completion:*:*:kill:*:processes' list-colors '=(#b) #([0-9]#)*=0=01;31'
zstyle ':completion:*:kill:*' command 'ps -u $USER -o pid,%cpu,tty,cputime,cmd'

bindkey -v # vim mode settings

autoload -U colors && colors

bindkey '^P' up-history
bindkey '^N' down-history
bindkey '^?' backward-delete-char
bindkey '^h' backward-delete-char
bindkey '^w' backward-kill-word
bindkey '^r' history-incremental-search-backward

VIM_NORM="%{$fg[blue]%} -- NORMAL -- %{$reset_color%}"
VIM_INST="%{$bg[red]%} -- INSERT -- %{$reset_color%}"

function zle-line-init zle-keymap-select {
    RPS1="${${KEYMAP/vicmd/$VIM_NORM}/(main|viins)/$VIM_INST}"
    RPS2=$RPS1
    zle reset-prompt
}

function zle-line-finish {
}
zle -N zle-line-finish
zle -N zle-line-init
zle -N zle-keymap-select
export KEYTIMEOUT=1

setopt transient_rprompt
PS1="%(1j.[%j]"$'\n'".)%{$fg[magenta]%}%~ %{$fg_bold[magenta]%}>%{$reset_color%} "
RPROMPT="${VIM_INST}"

### convenience
alias    ls='\ls -l -phF'
alias    ll='\ls -halp -F'
alias     l='\ls -p'
alias    l.='\ls -halpd .*'
alias     j='jobs -l'
alias     h='history'
alias     t='tree -C'
alias  scns='scn -ls'

# TODO check if screen session of <fruit> name already exists
# before creating a same-named session.
function scn() {

    fruits=(
        banana      mango       orange
        peach       cherry      pineapple
        watermelon  rasberry    strawberry
        grape       grapefruit  pomegranate)

    numFruits=${#fruits}

    if (( $# > 0 )) {
        screen $@
    } else {
        name=$fruits[$[RANDOM % numFruits + 1]]

        screen -S "$name"
    }
}

### convenience (more specific)
alias   mem='ps -ax -o %mem=--MEM--,user=---USER---,pid=---PID--,cmd | grep -v root | sort -Vr'

### convenience pipes (global)
alias -g      G='| grep --color'
alias -g   grev='| grep --color -v'
alias -g      L='| less -R'
alias -g  quiet='2> /dev/null'
alias -g silent='&> /dev/null'

### convenience functions
function dump() {
    mv -i $1 $HOME/bups/$1
}
function dump-empty() {
    mv $HOME/bups/* $HOME/.bups/
}
function dump-ls() {
    ls -la $HOME/bups/
}
function notes() {
    grep -r $HOME/notes/ --exclude-dir=".git" -e $1 | grep --color -C 10 $2
}
function calc() {
    echo "$@" | bc
}

### info aliases
alias ports='sudo netstat -uplant'        # list all TCP/UDP ports on the server
alias    df='df -H'                       # report file system disk space usage
alias    du='du -ch --summarize'          # print estimated disk usage

### safety aliases
alias    mv='mv -i'                       # prompts for safety
alias    rm='rm -I'                       # "
alias    cp='cp -i'                       # "
alias    ln='ln -i'                       # "

function grade() {
    print "scale=2; $1 / $2 * 100" | bc
}

### scripts 
alias  f='~/scripts/superfind.sh'   # Usage: f  <dir> <filename>
alias fr='~/scripts/repofind.sh'    # Usage: f  <dir> <reponame>
alias  r='~/scripts/superrm.sh'     # Usage: rm <dir> (with tree output + prompt)
alias  s='~/scripts/supergrep.sh'   # Usage: s  <dir> <pattern>
alias  d='~/scripts/difffiles.sh'   # Usage: d  <dir1> <dir2>
alias  c='~/scripts/ipinfo.sh'      # Usage: c  <ip>
alias  e='~/scripts/penv.sh'        # Usage: e
alias gm='~/scripts/gd-sum.sh'      # Usage: gm
alias al='~/scripts/aliases.sh'     # Usage: al

###################
### git aliases ###
###################
alias   g='git'                     #
alias  st='git status -s -b'        #shortform git status + branch info
alias  gs='git status'              #
alias  gd='git diff'                #
alias gdd="git diff --color-words='[^[:space:]]|([[:alnum:]]|UTF_8_GUARD)+'" #show character substitutions on the same line (and avert wordsalad)
alias gds='git diff --shortstat'    # just the stats, ma'am
alias gdc='git diff --cached'       # what did I just add ...?
alias gsl='git stash list --date=local' #
alias  gt='git log --graph --oneline --decorate --all' #
alias  "g?"='git remote -v update'    #fetch updates from all branches no set to skipDefaultUpdate (show ahead or behind)
alias  gf='git log -p --follow --oneline --' #
alias gfp='git log -p --follow --'  #
function gg() { git grep "$*"; }  # simpler git-grep (concatenates params)

alias  gp='git pull'              #
alias gpr='git pull \
    --recurse-submodules'         # update submodules
alias gpu='git push'              #
alias  ga='git add'               #
alias  gc='git checkout'          # shortcuts
alias gcm='git commit'            #
alias  gl='git log --name-status' # show file action summaries
alias glp='git log -p'            #
alias  gb='git blame -c'          ### TODO add more parms for better readout
alias gcf='git commit -F- <<EOF'  # use heredocs for commit message
alias gap='git add --patch'       # commit individual hunks

