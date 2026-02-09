# Downloading the Bucket

Listing Bucket (1)
>``` shell
>kali@kali:~$ aws s3 ls staticcontent-lgudbhv8syu2tgbk
>
># ========== Expected Result ==========
>                           PRE .git/
>                           PRE images/
>                           PRE scripts/
>                           PRE webroot/
>2023-04-04 13:00:52        972 CONTRIBUTING.md
>2023-04-04 13:00:52         79 Caddyfile
>2023-04-04 13:00:52        407 Jenkinsfile
>2023-04-04 13:00:52        850 README.md
>2023-04-04 13:00:52        176 docker-compose.yml
># =====================================
>```

Listing Bucket (2)
>``` shell
>kali@kali:~$ aws s3 cp s3://staticcontent-lgudbhv8syu2tgbk/README.md ./
>
># ========== Expected Result ==========
>download: s3://staticcontent-lgudbhv8syu2tgbk/README.md to ./README.md
># =====================================
>```

Review README.md
>``` shell
>kali@kali:~$ cat README.md
>
># ========== Expected Result ==========
># Static Content Repository
>
>This repository holds static content.
>
>While it only hold images for now, later it will hold PDFs and other digital assets.
>
>Git probably isn't the best for this, but we need to have some form of version control on these assets later. 
>
>## How to use
>
>To use the content in this repository, simply clone or download the repository and access the files as needed. If you have access to the S3 bucket and would like to upload the content to the bucket, you can use the provided script:
>
>./scripts/upload-to-s3.sh
>
>This script will upload all the files in the repository to the specified S3 bucket.
>
>## Contributing
>
>If you would like to contribute to this repository, please fork the repository and submit a pull request with your changes. Please make sure to follow the contribution guidelines outlined in CONTRIBUTING.md.
>
># Collaborators
>Lucy
>Roger
># =====================================
>```

Downloading the S3 bucket
>``` shell
>kali@kali:~$ mkdir static_content                                     
>
>kali@kali:~$ aws s3 sync s3://staticcontent-lgudbhv8syu2tgbk ./static_content/
>
># ========== Expected Result ==========
>download: s3://staticcontent-lgudbhv8syu2tgbk/.git/COMMIT_EDITMSG to static_content/.git/COMMIT_EDITMSG
>...
>download: s3://staticcontent-lgudbhv8syu2tgbk/images/kittens.jpg to static_content/images/kittens.jpg
># =====================================
>
>kali@kali:~$ cd static_content
>
># ========== Expected Result ==========
>kali@kali:~/static_content$ 
># =====================================
>```

Review S3 upload script
>``` shell
>kali@kali:~/static_content$ cat scripts/upload-to-s3.sh
>
># ========== Expected Result ==========
># Upload images to s3
>
>SCRIPT_DIR=$( cd -- "$( dirname -- "${BASH_SOURCE[0]}" )" &> /dev/null && pwd )
>
>AWS_PROFILE=prod aws s3 sync $SCRIPT_DIR/../ s3://staticcontent-lgudbhv8syu2tgbk/ 
># =====================================
>```

Review update-readme Script
>``` shell
>kali@kali:~/static_content$ ls scripts   
>
># ========== Expected Result ==========
>update-readme.sh  upload-to-s3.sh
># =====================================
>
>kali@kali:~/static_content$ cat -n scripts/update-readme.sh 
>
># ========== Expected Result ==========
>01  # Update Readme to include collaborators images to s3
>02
>03  SCRIPT_DIR=$( cd -- "$( dirname -- "${BASH_SOURCE[0]}" )" &> /dev/null && pwd )
>04
>05  SECTION="# Collaborators"
>06  FILE=$SCRIPT_DIR/../README.md
>07
>08  if [ "$1" == "-h" ]; then
>09    echo "Update the collaborators in the README.md file"
>10    exit 0
>11  fi
>12
>13  # Check if both arguments are provided
>14  if [ "$#" -ne 2 ]; then
>15    # If not, display a help message
>16    echo "Usage: $0 USERNAME PASSWORD"
>17    exit 1
>18  fi
>19
>20  # Store the arguments in variables
>21  username=$1
>22  password=$2
>23
>24  auth_header=$(printf "Authorization: Basic %s\n" "$(echo -n "$username:$password" | base64)")
>25
>26  USERNAMES=$(curl -X 'GET' 'http://git.offseclab.io/api/v1/repos/Jack/static_content/collaborators' -H 'accept: application/json' -H $auth_header | jq .\[\].username |  tr -d '"')
>27
>28  sed -i "/^$SECTION/,/^#/{/$SECTION/d;//!d}" $FILE
>29  echo "$SECTION" >> $FILE
>30  echo "$USERNAMES" >> $FILE
>31  echo "" >> $FILE
># =====================================
>```

Lab 1 - Based on the directory listing, which of the following files indicates that the S3 bucket might be part of a CI/CD pipeline?
>C) Jenkinsfile

Lab 2 - Which command is used to synchronize the entire contents of the S3 bucket with a local directory?
>B) aws s3 sync
