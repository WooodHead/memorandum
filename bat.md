# bat

## ��s�ŕ����R�}���h
```cmd
command1 & command2
```
�V���[�g�J�b�g��cmd�v�����v�g��ݒ肷��Ƃ��A���L�̂悤�ɃR�}���h�����������s�����V���[�g�J�b�g��ݒ�ł���B
```
%comspec% /k "cmd1 & cmd2"
```

## �t�@�C���ړ�
`move from to`

## �Ȃ�����
* �z��
* �n�b�V��

## ���΃p�X
bat���̑��΃p�X�́A���s�ꏊ����̑��΃p�X�ŁAbat�t�@�C���̏ꏊ����ł͂Ȃ��B
## �R�}���h�v�����v�g�̃z�[���f�B���N�g���̐ݒ�
�V���[�g�J�b�g��cmd���N������ꍇ�ɁA��ƃt�H���_�Ɏw�肵���f�B���N�g�����z�[���f�B���N�g���ɂȂ�B

## �Ǘ��҂Ƃ��Ď��s
* cmd��bat�����s����ꍇ�ɁA�v���p�e�B->�V���[�g�J�b�g�^�u->�ڍאݒ�->�Ǘ��҂Ƃ��Ď��s�Ƀ`�F�b�N�ŊǗ��҂Ƃ��Ď��s�\�B
* �R�}���h��I�����Ď��s���鎞�ɁACtrl+Shift+Enter

## %�̈���
bat�t�@�C������`%`�𕶎���Ƃ��ēn�������ꍇ�́A`%%`�Ƃ���B
�ϐ���`%%`�Ƃ���̂͂��̂��߁B

## comment
```bat
rem comment here
```

## �ϐ��̐錾
`=`�̑O��ɋ󔒂͓���Ȃ��B�ϐ��ɏ����͑���ł��Ȃ��B
```bat
SET �ϐ���=[������]
SET /A �ϐ���=[����]
```

## ������
������̒��������߂�R�}���h�͂Ȃ��B
```bat
SET str1=abc
SET str2=de f
SET str2= g hi
rem ����
SET str1=aaa
SET str2=bbb
SET str3=%str1%%str2%              �c aaabbb
```
������̐؂�o��
```bat
:: �؂�o��
SET str1=abcd
SET str2=%str1:~0,2%               �c ab�i1���ځi�I�t�Z�b�g0�j����2�����j
```

## redirect
`>>`�ŒǋL�B
```bat
input.bat > output.txt
input.bat >> output.txt
```

## alias
```bat
doskey �}�N����=�e�L�X�g


## for���Aif���𕡐��s�ɏ����Ƃ��̒���
http://tounderlinedk.blogspot.jp/2011/01/if-windowsbatcmd.html

## �����R�[�h�̕ϊ�
```bat
chcp charcter_code
```
* character_code
    * 437      IBM437        OEM United States
    * 932      shift_jis     ANSI/OEM Japanese; Japanese (Shift-JIS)
    * 1200     utf-16        Unicode UTF-16, little endian byte order (BMP of ISO 10646); available only to managed applications
    * 20127    us-ascii      US-ASCII (7-bit)
    * 20932    EUC-JP        Japanese (JIS 0208-1990 and 0121-1990)
    * 50220    iso-2022-jp   ISO 2022 Japanese with no halfwidth Katakana; Japanese (JIS)
    * 50222    iso-2022-jp   ISO 2022 Japanese JIS X 0201-1989; Japanese (JIS-Allow 1 byte Kana - SO/SI)
    * 51932    euc-jp        EUC Japanese
    * 65001    utf-8         Unicode (UTF-8)

## �V���{���b�N�����N�̍쐬
cmd�ňȉ��̃R�}���h�����s�B
```cmd
mklink link target
mklink path\to\link path\to\target
```
* link:�V���{���b�N�����N��
* �����N���Q�Ƃ���p�X
* `/D`�Ńf�B���N�g���ւ̃����N
* `/H`�Ńn�[�h�����N
* `/J`�f�B���N�g���W�����N�V����

## �t�@�C���ꗗ
```bat
@echo off
echo -----------------------�t���p�X
for %%A in (*.txt) do echo %%~fA
echo -----------------------�h���C�u��
for %%A in (*.txt) do echo %%~dA
echo -----------------------�e�p�X
for %%A in (*.txt) do echo %%~pA
echo -----------------------�t�@�C����
for /F %%A in ('dir /b *.txt') do echo %%A
echo -----------------------�t�@�C�����i�g���q�����j
for %%A in (*.txt) do echo %%~nA
echo -----------------------�g���q
for %%A in (*.txt) do echo %%~xA
```

## �t�@�C���폜
�f�B���N�g���̍폜�͂ł��Ȃ��B
```bat
del (option) [file / directory] 
```

## �f�B���N�g���폜
```bat
rmdir directory
```
* `/s`
    * �t�@�C����T�u�f�B���N�g�����܂߂č폜����

## for��
```bat
for %variable in (<pattern>) do <command-line>
for /D %variable in (<pattern>) do <command-line>
for /R [[<drive-letter>:]<path>] %variable in (<pattern>) do <command-line>
for /L %variable in (<start>,<step>,<end>) do <command-line>
for /F ["<options>"] %variable in (<pattern>) do <command-line>
```
* %variable
    * �uvariable�v�ɂ�1�����̉p�������w�肵�܂��B���̕ϐ��́A���L�� <command-line> �ŗp���邱�Ƃ��ł��A�g�p����Ɨ񋓒��̃t�@�C�����ɒu���������܂��B�啶���E����������ʂ���܂��B

�����s�ɏ����Ƃ���`()`�ň͂ށB
```bat
foir %variable in (<pattern>) do (

)
```
����Ƃ���`)`�͍s���ɒu���B

## if��
```bat
IF ���� (
    ����
) ELSE (
    ����
)
```
����
```bat
IF [NOT] ERRORLEVEL �ԍ� �R�}���h
IF [NOT] ������1==������2 �R�}���h
IF [NOT] EXIST �t�@�C���� �R�}���h
IF ������1 ��r���Z�q ������2 �R�}���h
```

��r���Z�q
* EQU - ������
* NEQ - �������Ȃ�
* LSS - ��菬����
* LEQ - �ȉ�
* GTR - ���傫��
* GEQ - �ȏ�
