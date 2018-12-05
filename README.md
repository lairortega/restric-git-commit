# Commit validator
This script validates the message structure of the commit command.

 - The line: **msg="$(cat $1 | grep -v \# | head -n 1)"** takes the
   message passed though the argument -m or by the interactive editor
   (vim, nano, etc) and procces the first not comment line (excludes the
   The lines with the # symbol.

 - In the line: **if ! [[ \$msg =~ ^(feat|fix|docs?|style|refactor|pref|chore|revert?)\(.+\):{1}\ ?.{3,}$ ]]; then** Validates the message structure.

# Instructions
Write the following code into a file named *commit-msg* and save it into: /repo/path/.git/hooks/

    #!/bin/sh

    RED='\033[0;31m'
    YELLOW='\e[33m'
    GREEN='\e[32m'
    CYAN='\e[36m'
    NC='\033[0m'
    BOLD='\e[1m'
    NORMAL='\e[0m'

    msg="$(cat $1 | grep -v \# | head -n 1)"
    if ! [[ $msg =~ ^(feat|fix|docs?|style|refactor|pref|chore|revert?)\(.+\):{1}\ ?.{3,}$ ]]; the
		echo -e "${RED}${BOLD}Invalid commit${NORMAL}${NC}"
		echo -e "\t${YELLOW}Follow the structure: \"type(module): description, at lease 3 words\""
		echo -e "\tAvailable types:"
		echo -e "\t\tfeat|fix|docs?|style|refactor|pref|chore|revert?${NC}"
		echo -e "\t${CYAN}${BOLD}Example:${NORMAL} "
		echo -e "\t\t${CYAN}revert(usersController): remove t3 validation${NC}"
		exit 1
	fi


# Examples
Navigate to your repo path
## Invalid commit message
    $ git commit -m "Invalid message"
    Invalid commit
	Follow the structure: "type(module): description, at lease 3 words"
	Available types:
		feat|fix|docs?|style|refactor|pref|chore|revert?
	Example:
		revert(usersController): remove t3 validation
## Valid commit message

    git commit -m "chore(BashCommitRule): write a valid message"
    [master ebdf4ef] chore(BashCommitRule): write a valid message
	 1 file changed, 1 insertion(+)
