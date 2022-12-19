set -o errexit

readonly ROOT_UID=0
readonly Project_Name="GRUB2::THEMES"
readonly MAX_DELAY=20                               # max delay for user to enter root password
tui_root_login=

THEME_DIR="/usr/share/grub/themes"
REO_DIR="$(cd $(dirname $0) && pwd)"

THEME_VARIANTS=('tela' 'vimix' 'stylish' 'whitesur')
ICON_VARIANTS=('color' 'white' 'whitesur')
SCREEN_VARIANTS=('1080p' '2k' '4k' 'ultrawide' 'ultrawide2k')

#################################
#   :::::: C O L O R S ::::::   #
#################################

CDEF=" \033[0m"                                     # default color
CCIN=" \033[0;36m"                                  # info color
CGSC=" \033[0;32m"                                  # success color
CRER=" \033[0;31m"                                  # error color
CWAR=" \033[0;33m"                                  # waring color
b_CDEF=" \033[1;37m"                                # bold default color
b_CCIN=" \033[1;36m"                                # bold info color
b_CGSC=" \033[1;32m"                                # bold success color
b_CRER=" \033[1;31m"                                # bold error color
b_CWAR=" \033[1;33m"                                # bold warning color

#######################################
#   :::::: F U N C T I O N S ::::::   #
#######################################

# echo like ... with flag type and display message colors
prompt () {
  case ${1} in
    "-s"|"--success")
      echo -e "${b_CGSC}${@/-s/}${CDEF}";;    # print success message
    "-e"|"--error")
      echo -e "${b_CRER}${@/-e/}${CDEF}";;    # print error message
    "-w"|"--warning")
      echo -e "${b_CWAR}${@/-w/}${CDEF}";;    # print warning message
    "-i"|"--info")
      echo -e "${b_CCIN}${@/-i/}${CDEF}";;    # print info message
    *)
    echo -e "$@"
    ;;
  esac
}

# Check command availability
function has_command() {
  command -v $1 &> /d
