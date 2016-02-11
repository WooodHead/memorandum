#gvim

##�C���X�g�[��(Win7)
VimShell��Vimfiler��unix�R�}���h���g�p����̂ŁAcygwin��MSYS���C���X�g�[������K�v������B
cygwin�͏d���̂ŁAMSYS�̃C���X�g�[��������B

### MSYS�̃C���X�g�[��(32bit)
1. `http://sourceforge.net/projects/mingw/files/`���A`mingw-get-setup.exe`���_�E�����[�h�B
2. ���s����ƁA�C���X�g�[������p�b�P�[�W�̑I����ʂɂȂ�B
3. `MSYS Basic System`�ƓK����gcc�R���p�C��(64bit�̏ꍇ�́A64bit��gcc�R���p�C��������̂ŕs�v�H�j��I�����A���͂��D�ނŃC���X�g�[������B
4. MSYS�̃C���X�g�[�����bin�t�H���_��PATH��ʂ��B
    * �C���X�g�[���悪`C:\MinGW`�Ȃ�A`C:\MinGW\msys\1.0\bin`

### MSYS�̃C���X�g�[��(64bit)
MSYS32bit�̃C���X�g�[���ɉ�����64bit��MinGW64���C���X�g�[������B
`vimproc`��`make-mingw64.mak`�ł́AMinGW64��gcc���ĂԂ悤�ɂȂ��Ă���̂ŁA���L��ݒ���L�q���ANeoBundleInstall��OK
```vim
		NeoBundle 'Shougo/vimproc', {
		  \ 'build' : {
			\ 'windows' : 'make -f make_mingw64.mak',
			\ 'cygwin' : 'make -f make_cygwin.mak',
			\ 'mac' : 'make -f make_mac.mak',
			\ 'unix' : 'make -f make_unix.mak',
		  \ },
		\ }
```
1. ���L���64bit�C���X�g�[��`mingw-w64-install.exe`��DL����B
    `http://sourceforge.net/projects/mingw-w64/`
2. �N�������architecture�̑I�����ł�̂ŁA�K���ɑI�����A`C:\MinGW64`�ɃC���X�g�[������B�i�ǂ��ł��ǂ����j
    * version�͍ŐV
    * cpu architecture��`x86_64`��I��
    * thread��posix
    * exception��`SEF`��I���Bwin64���Ƒ����炵���B
    * build version�͓K����1��I���B
3. �C���X�g�[��������������A`C:\MinGW64\mingw64\bin`�Ƀp�X��ʂ��B���̂Ƃ��AMSYS�̂��̂���Ƀp�X��ʂ��B

## VimShell�̃C���X�g�[��

##NeoBundle�̃C���X�g�[��
Git�̓C���X�g�[���ς݂̑O��B
    mkdir %userprofile%\.vim\bundle
    cd %userprofile%\.vim\bundle
    git clone git://github.com/Shougo/neobundle.vim


## option

##�J���[�X�L�[�}�ɂ���
�Ƃ肠�����f�t�H���g�̃J���[�X�L�[�}����悳���Ȃ��̂�I�ԁB
http://nanasi.jp/colorscheme/default_install.html

�������߂�`colorscheme desert`

### wombat
�ǉ��ł����Ȃ�ȉ����ǂ��B
http://www.vim.org/scripts/script.php?script_id=1778

1. ��L�T�C�g����`wombat.vim`��DL
2. `vim73-kaoriya-win64\vim73\colors`�ɔz�u�B
3. vimrc��`colorscheme wombat`

##gvimrc��vimrc�̏ꏊ
�ȉ��̃R�}���h�ł����ꂩ�Ŋm�F�ł���B
windows�̏ꍇ�́A`%userprofile%`�Ɍ��т��Ă�̂����B
windows�̏ꍇ�A�V���{���b�N�����N�����Γǂݍ���ł����B�i�V���[�g�J�b�g�͂��߁j
```vimrc
:echo $HOME
:echo $VIM
```


## �t�H���g
vimrc�Ɉȉ����L�ڂ���ΐݒ�\�B
Ricty�t�H���g���������߁B
```vim
"���p�����̐ݒ�
set guifont=Ricty\ Discord:h12
"�S�p�����̐ݒ�
set guifontwide=Ricty\ Discord:h12
```

