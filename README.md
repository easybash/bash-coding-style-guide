# Bash Coding Style Guide

This coding style guide is based on [Google Shell Style Guide](https://google.github.io/styleguide/shell.xml#Line_Length_and_Long_Strings) and we made some changes to it. 

- Differences
    - **Indentation** - We suggest 4 spaces, just like common programming languages.
    - **Array** - We add this part to coding style guide for increasing readability.
    - **Heredoc** - Clear explanation of here-document.
    - **Shebang** - `#!/usr/bin/env bash`, not `#!/usr/bin/bash`

Other differences may not motion here, for more details you can check out the Table of Content below.

## Table of Content

- Shell Files
    - [File Extension](#file-extension)
    - [Shebang](#shebang)
    - [Include Files](#include-files)
- Naming Convention
    - [Varible](#variable)
    - [Function](#function)
    - [Source Filename](#source-filename)
- Formating
    - [Indentation](#indentation)
    - [Lines](#lines)
    - [Pipelines](#pipelines)
    - [Variables](#variables)
    - [Arrays](#arrays)
    - [Heredoc](#heredoc)
    - [Quoting](#quoting)

- Control Structures
    - [If Statement](#if-statement)
    - [For Statement](#for-statement)
    - [While Statement](#while-statement)
    - [Case Statement](#case-statement)
- Comments
    - File header
    - Function Comment
    - Section Comment
    - Implementation Comments
    - Todo Comments
- Conclusion

## Shell Files

### File Extension

- [x] Executables should have a .sh extension.
- [x] Libraries (included files) should have a .sh extension.

```
easybash.sh
```

### Shebang

- [x] Shebang lines must always be `#!/usr/bin/env bash`<br />
 `#!/usr/bin/env` searches `PATH` for `bash`.
This allows you to change your `PATH` to get the interpreter without having to edit every file you're working on.

```
#!/usr/bin/env bash
```

### Include Files

- [x] 1. The included files should not have shebang lines.
- [x] 2. Use `source` instead of  `.` to include files. Although `.` is POSIX standard, the `source` word provides more readability.

Yes
```
source "${EASYBASH_DIR}/inc/functions.sh"
```

No `2`
```
# Use source keyword provides more readability.
. "${EASYBASH_DIR}/inc/functions.sh"
```

## Naming Convention

### Variable

- [x] 1. Always use underscore naming convention.
- [x] 2. No camel-case naming convention.
- [x] 3. If a variable can be changed from its parent environment, it should be in uppercase.
- [x] 4. If a variable can be changed from a command line argument, it could be in uppercase, and name it starting with an underscore.
- [x] 5. A constant variable should be in uppercase, and make it `readonly`.
- [X] 6. If the value of a variable is `ANSI color code`, it should be in uppercase, and name it starting with the word `COLOR_`.
- [x] 7. Other varibles should be lowercase.

Yes
```
php_version="7.2"
```

No `2`
```
# No camel-case naming convention.
phpVersion="7.2"
```

Yes
```
readonly PHP_VERSION="7.2"
```

No `5`
```
# A constant variable should be readony.
PHP_VERSION="7.2"
```
Yes
```
COLOR_EOF="\e[0m"
COLOR_BLUE="\e[34m"
COLOR_RED="\e[91m"
COLOR_GREEN="\e[92m"
COLOR_WHITE="\e[97m"
COLOR_DARK="\e[90m"
COLOR_BG_BLUE="\e[44m"
COLOR_BG_GREEN="\e[42m"
COLOR_BG_DARK="\e[100m"

```
No `6`
```
# Should be in uppercase starting with the word `COLOR_`.

eof_sign="\e[0m"
blue="\e[34m"
red="\e[91m"
green="\e[92m"
white="\e[97m"
dark="\e[90m"
bg_blue="\e[44m"
bg_green="\e[42m"
bg_dark="\e[100m"
```

### Function

- [x] 1. Always use underscore naming convention.
- [x] 2. No camel-case naming convention.
- [x] 3. A function name should be lowercase.
- [x] 4. Braces must be on the same line as the function name.
- [X] 5. Adding a namespace with `::` provides more readability. (recommended)

Yes
```
func_test() {
   local this_is_local="Local variable"
   echo ${this_is_local}
}

func_test
```

No `4`
```
# Braces must be on the same line as the function name.

func_test() 
{
   local this_is_local="Local variable"
   echo ${this_is_local}
}

func_test
```

Recommend
```
# Namespace provides more readability.

func::test() {
   local this_is_local="Local variable"
   echo ${this_is_local}
}

func::test
```

### Source Filename

- [x] Lowercase, with underscores to separate words if needed.

## Formating

### Indentation

- [x] 1. Indent 4 spaces for each level of indentation.
- [x] 2. No Tabs
- [x] 3. Vertical indentation in *array* is 4 spaces and up to five values per line. ([example](#arrays))

```
if [ "${abc}" == "yes" ]; then
    echo "ok"
fi
```

### Lines

- [x] 1. Maximum line length is 80 characters.
- [x] 2. No trailing spaces in the end of line.
- [x] 3. No spaces in empty line.

If you need a string that is longer than 80 characters, either use embedded newlines or here documents if possible. Literal strings that have to be longer than 80 characters and can't sensibly be split are ok, but use common sense if in doubt. It's common to use longer lines if it helps, not hurts readability.

```
# DO use 'here document's.
cat << EOF
I am an exceptionally long
string.
EOF

# Embedded newlines are ok too.
long_string="I am an exceptionally
    long string."
```

### Pipelines

If a pipeline don't all fit on one line, it should be split at one pipe segment per line with the pipe on the newline. Indent each of the subsequent lines by four spaces. 

```
# All fits on one line
command_1 | command_2

# Long commands
command_1 \
    | command_2 \
    | command_3 \
    | command_4
```

### Variables

- [x] 1. Using the local keyword inside functions prevents problems with global variables.
- [x] 2. Always quote the value, unless the value is Integer.
- [x] 3. Always brace-quote the variables when using them, except single character shell specials. (ex. `?`, `*`, `#`, etc..)

Yes
```
php_version="7.2"
```

No `3`
```
# Missing quotes.
php_version=7.2
```


Yes
```
func_test() {
   local this_is_local="Local variable"
   echo ${this_is_local}
}

func_test
```

No `8`
```
# Missing local keyword when defining a varible in a function.

func_test() {
   this_is_local="Local variable"
   echo ${this_is_local}
}

func_test
```

### Arrays

- [x] 1. Vertical indentation is 4 space width and up to 5 values per line.
- [x] 2. Always quotes the values in the array, unless index.

Yes
```
php_modules=(
    "bcmath"    "bz2"         "cgi"          "cli"         "common" 
    "curl"      "dba"         "dev"          "enchant"     "gd"       
    "gmp"       "imap"        "interbase"    "intl"        "json"
    "ldap"      "mbstring"    "mysql"        "odbc"        "opcache"  
    "pgsql"     "phpdbg"      "pspell"       "readline"    "recode"   
    "redis"     "snmp"        "soap"         "sqlite3"     "sybase" 
    "tidy"      "xml"         "xmlrpc"       "xsl"         "zip" 
)
```
Yes
```
array=(
    "one"         "two"     "three"    "four"    [6]="five"
    [8]="eight"   "nine"    "ten"
)
```
No `2`
```
# Missing quotes in each value.
php_modules=(
    bcmath    bz2         cgi          cli         common 
    curl      dba         dev          enchant     gd       
    gmp       imap        interbase    intl        json
    ldap      mbstring    mysql        odbc        opcache  
    pgsql     phpdbg      pspell       readline    recode   
    redis     snmp        soap         sqlite3     sybase 
    tidy      xml         xmlrpc       xsl         zip 
)
```
No `1` `2`
```
# Missing vertical indentation.
php_modules=(bcmath bz2 cgi cli common curl dba dev enchant gd gmp imap interbase intl jsonldap mbstring mysql odbc opcache pgsql phpdbg pspell readline recode redis snmp soap sqlite3 sybase tidy xml xmlrpc xsl zip)
```
No `2`
```
# Missing quotes in each value.
array=(
    one         two     three    four    [6]=five
    [8]=eight   nine    ten
)
```

### Heredoc

- [x] 1. Delimiting identifier should always be uppercase.
- [x] 2. No semicolon `;` after delimiting identifier.
- [x] 3. Redirect and the delimiting identifier should be separated by a space.

YES
```
cat << EOF
This is an example.
easybash
EOF
```

No `1`
```
# Delimiting identifier should be uppercase.
cat << eof
This is an example.
easybash
eof
```
No `2`
```
# Semicolon is unnecessary.
cat << EOF;
This is an example.
easybash
EOF
```

No `3`
```
# Missing a sapce between redirect and the delimiting identifier.
cat <<EOF
This is an example.
easybash
EOF
```

### Quoting

- [x] Always quote strings containing variables, command substitutions, spaces or shell meta characters, unless careful unquoted expansion is required.
- [x] Double quotes is strongly preferred.

## Control Structures

### If Statement

- [x] `;` and `then` should be on the same line, separated by a space.
- [x] `else`、`elif`、`fi` should be on its own line vertically aligned with the `if` statement.

```
status="error"

# if..fi statement
if [ "${status}" == "success" ]; then
    echo "success"
else
    echo "other"
fi

# if..elif..fi statement
if [ "${status}" == "success" ]; then
    echo "success"
elif [ "${status}" == "error" ]; then
    echo "error"
else
    echo "other"
fi
```

### For Statement

- [x] `;` and `do` should be on the same line, separated by a space.
- [x] `done` should be on its own line vertically aligned with the `for` statement
```
for module in ${php_modules[@]}; do
    func_easybash_msg info "Proceeding to install PHP module \"${module}\" ..."
    sudo ${_PM} install -y php${package_version}-${module}
done
```

### While Statement

- [x] `;` and `do` should be on the same line, separated by a space.
- [x] `done` should be on its own line vertically aligned with the `while` statement.

```
i=0

while [ ${i} -lt 5 ]; do
    i=$(($i+1))
    echo ${i}
done
```

### Case Statement

- [x] Indent 4 spaces.
- [x] The patterns `"action")` and the corresponding action terminator `;;` are indented at the same level.
- [x] The pattern strings should be around with double quotes to keep readability.
- [x] Do not quote pattern-matching metacharacters. (`*`, `?`, `|`, etc...)

```
while [ "$#" -gt 0 ]; do
    case "$1" in
        "-v") 
            package_version="${2}"
            shift 2
        ;;
        "--version="*) 
            package_version="${1#*=}"; 
            shift 1
        ;;
        # Help
        "-h"|"--help")
            show_script_help
            exit 1
        ;;
        # Info
        "-i"|"--information")
            show_script_information
            exit 1
        ;;
        "-"*)
            echo "Unknown option: ${1}"
            exit 1
        ;;
        *)
            echo "Unknown option: ${1}"
            exit 1
        ;;
    esac
done
```

## Comments

## Conclusion