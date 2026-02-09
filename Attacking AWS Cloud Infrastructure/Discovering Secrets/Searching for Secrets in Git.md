# Searching for Secrets in Git

Installing gitleaks
>``` shell
>kali@kali:~/static_content$ sudo apt update    
>
># ========== Expected Result ==========
>...
># =====================================
>
>kali@kali:~/static_content$ sudo apt install -y gitleaks    
>
># ========== Expected Result ==========
>Reading package lists... Done
>Building dependency tree... Done
>Reading state information... Done
>The following NEW packages will be installed:
>  gitleaks
>...
># =====================================
>```

Using gitleaks to Search for Secrets
>``` shell
>kali@kali:~/static_content$ gitleaks detect  
>
># ========== Expected Result ==========
>    ○
>    │╲
>    │ ○
>    ○ ░
>    ░    gitleaks 
>
>1:58PM INF no leaks found
>1:58PM INF scan completed in 61.787205ms
># =====================================
>```

Review Git History
>``` shell
>kali@kali:~/static_content$ git log
>
># ========== Expected Result ==========
>commit 07feec62e57fec8335e932d9fcbb9ea1f8431305 (HEAD -> master, origin/master)
>Author: Jack <jack@offseclab.io>
>
>    Add Jenkinsfile
>
>commit 64382765366943dd1270e945b0b23dbed3024340
>Author: Jack <jack@offseclab.io>
>
>    Fix issue
>
>commit 54166a0803785d745d68f132cde6e3859f425c75
>Author: Jack <jack@offseclab.io>
>
>    Add Managment Scripts
>
>commit 5c22f52b6e5efbb490c330f3eb39949f2dfe2f91
>Author: Jack <jack@offseclab.io>
>
>    add Docker
>
>commit 065abcd970335c35a44e54019bb453a4abd59210
>Author: Jack <jack@offseclab.io>
>
>    Add index.html
>
>commit 6e466ede070b7fb44e0ef38bef3504cf87e866d0
>Author: Jack <jack@offseclab.io>
>
>    Add images
>
>commit 85c736662f2644783d1f376dcfc1688e37bd1991
>Author: Jack <jack@offseclab.io>
>
>    Init Repo
># =====================================
>```

Review Git Diff
>``` shell
>kali@kali:~/static_content$ git show 64382765366943dd1270e945b0b23dbed3024340
>
># ========== Expected Result ==========
>commit 64382765366943dd1270e945b0b23dbed3024340
>Author: Jack <jack@offseclab.io>
>
>    Fix issue
>
>diff --git a/scripts/update-readme.sh b/scripts/update-readme.sh
>index 94c67fc..c2fcc19 100644
>--- a/scripts/update-readme.sh
>+++ b/scripts/update-readme.sh
>@@ -1,4 +1,5 @@
> # Update Readme to include collaborators images to s3
>+
> SCRIPT_DIR=$( cd -- "$( dirname -- "${BASH_SOURCE[0]}" )" &> /dev/null && pwd )
> 
> SECTION="# Collaborators"
>@@ -9,9 +10,22 @@ if [ "$1" == "-h" ]; then
>   exit 0
> fi
> 
>-USERNAMES=$(curl -X 'GET' 'http://git.offseclab.io/api/v1/repos/Jack/static_content/collaborators' -H 'accept: application/json' -H 'authorization: Basic YWRtaW5pc3RyYXRvcjo5bndrcWU1aGxiY21jOTFu' | jq .\[\].username |  tr -d '"')
>+# Check if both arguments are provided
>+if [ "$#" -ne 2 ]; then
>+  # If not, display a help message
>+  echo "Usage: $0 USERNAME PASSWORD"
>+  exit 1
>+fi
>+
>+# Store the arguments in variables
>+username=$1
>+password=$2
>+
>+auth_header=$(printf "Authorization: Basic %s\n" "$(echo -n "$username:$password" | base64)")
>+
>+USERNAMES=$(curl -X 'GET' 'http://git.offseclab.io/api/v1/repos/Jack/static_content/collaborators' -H 'accept: application/json' -H $auth_header | jq .\[\].username |  tr -d '"')
> 
> sed -i "/^$SECTION/,/^#/{/$SECTION/d;//!d}" $FILE
> echo "$SECTION" >> $FILE
> echo "$USERNAMES" >> $FILE
>-echo "" >> $FILE
>+echo "" >> $FILE
>\ No newline at end of file
># =====================================
>```

Decoding the header
>``` shell
>kali@kali:~/static_content$ echo "YWRtaW5pc3RyYXRvcjo5bndrcWU1aGxiY21jOTFu" | base64 --decode
>
># ========== Expected Result ==========
>administrator:9nwkqe5hlbcmc91n
># =====================================
>```

Logging into gitea
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Searching-for-Secrets-in-Git-1.png)

Logged in as Administrator
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Searching-for-Secrets-in-Git-2.png)

Lab 1 - What is the username of the user who committed the credentials to the repository
>``` shell
>
>```
>

Lab 2 - What is the username of the user who committed the credentials to the repository
>``` shell
>
>```
>
