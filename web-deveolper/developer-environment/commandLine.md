# command line

1. clear：清畫面
2. cd /：移到root
3. cd ~：移到home
4. open：
   1. folder：打開資料夾的視窗
   2. file：預設的程式打開該檔案，和點兩下一樣的意思
   3. -a：
      1. 例如： -a "Visual Studio Code" commandLine.md
      2. -a "Visual Studio Code" . 
5. rm：刪除檔案
   1. -r：刪除資料夾
6. mv：
   1. 搬移資料夾
   2. 改名稱（包括檔案和資料夾）
7. 開啟檔案：
   1. open -a "Visual Studio Code" 資料夾名稱
8. 編輯文字檔：
   1. nano 檔名：直接開始編輯文字檔。
   2. touch 檔名：直接建立一個新的空的檔案。
9. shell：
   1. bash：mac內建，設定檔：~/.bash_profile
   2. fish：
      1. 安裝fish的指令：brew install fish
      2. 設定檔：~/.config/fish/config.fish

        ```fish
            set normal (set_color normal)
            set magenta (set_color magenta)
            set yellow (set_color yellow)
            set green (set_color green)
            set red (set_color red)
            set gray (set_color -o black)

            # Fish git prompt
            set __fish_git_prompt_showdirtystate 'yes'
            set __fish_git_prompt_showstashstate 'yes'
            set __fish_git_prompt_showuntrackedfiles 'yes'
            set __fish_git_prompt_showupstream 'yes'
            set __fish_git_prompt_color_branch yellow
            set __fish_git_prompt_color_upstream_ahead green
            set __fish_git_prompt_color_upstream_behind red

            # Status Chars
            set __fish_git_prompt_char_dirtystate '⚡'
            set __fish_git_prompt_char_stagedstate '→'
            set __fish_git_prompt_char_untrackedfiles '☡'
            set __fish_git_prompt_char_stashstate '↩'
            set __fish_git_prompt_char_upstream_ahead '+'
            set __fish_git_prompt_char_upstream_behind '-'


            function fish_prompt
            set last_status $status

            set_color $fish_color_cwd
            printf '%s' (prompt_pwd)
            set_color normal

            printf '%s ' (__fish_git_prompt)

            set_color normal
            end
        ```

   3. oh my zsh
   4. tree：
      1. 用途，顯示檔案資料結構，以方便寫markdown。
      2. 安裝：brew install tree
      3. 基本指令：
         1. `tree -d 資料夾名稱`：只顯示資料夾
         2. `tree -a 資料夾名稱`：顯示檔案(不包含隱藏檔)
         3. `tree -L 1`：只顯示第一層
         4. `tree -Ld 2`：只顯示2層，只顯示資料夾
10. homebrew：
    1. 類似程式管理員，由於mac app store更新太慢，很多軟體用homebrew更新較快
    2. Homebrew Cask：灌完的app有圖示，可以放在launch bar上，不然只能在command line裡用。
       1. 安裝Homebrew Cask：
           1. brew tap caskroom/cask
           2. brew install brew-cask
       2. 安裝app時的指令：例如，brew cask install sketch
    3. 每次要用homebrew之前，可以先更新homebrew：
       1. brew update：更新
       2. brew doctor：檢查有無錯誤
    4. hombrew有些錯誤和新版的macos安全設計有關，它把/usr/local鎖定了：
       1. 新的安全設計是SIP(System Integrity Protection)
       2. 兩個作法:
           1. 關掉SIP：
               1. 重開機按cmd+R
               2. csrutil disable
           2. 或重灌homebrew：
               1. `ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"`
               2. sudo rm -rf /usr/local/.git
               3. sudo chown -R $(whoami):admin /usr/local
               4. `ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
       3. 灌完PostgreSQL記得再關掉。
11. 可以批次更新，批次灌軟體。
12. <https://onejar99.com/mac-homebrew-homebrew-cask-mac/>
13. 一些特殊設定：<https://www.maketecheasier.com/8-useful-and-interesting-bash-prompts/>
14. 名詞：
    1. line feed (LF)：linux/unix裡面的enter
    2. windows裡的enter：carriage return (CR)
15. vim：
    1. terminal內建的editor，有時候command line的軟體，會自動連結vim editor，而不得不用，例如git的editor就是vim。
    2. 檔案的命令都是`esc`然後加上`:`
        1. w是存檔
        2. q是跳出
    3. undo：`esc`然後再按`u`
    4. option+滑鼠：可以直接改變游標位置
16. ruby升級：`brew install ruby`
