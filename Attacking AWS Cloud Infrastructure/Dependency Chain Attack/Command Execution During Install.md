# Command Execution During Install

Adding Custom Code to Run During Install
>``` shell
>kali@kali:~/hackshort-util$ cat -n setup.py  
>
># ========== Expected Result ==========
>01  from setuptools import setup, find_packages
>02  from setuptools.command.install import install
>03
>04  class Installer(install):
>05      def run(self):
>06          install.run(self)
>07          with open('/tmp/running_during_install', 'w') as f:
>08              f.write('This code was executed when the package was installed')
>09
>10  setup(
>11      name='hackshort-util',
>12      version='1.1.4',
>13      packages=find_packages(),
>14      classifiers=[],
>15      install_requires=[],
>16      tests_require=[],
>17      cmdclass={'install': Installer}
>18  )
>19
># =====================================
>```

Removing the Existing Package and Building the New Package
>``` shell
>kali@kali:~/hackshort-util$ rm ./dist/hackshort_util-1.1.4.tar.gz
>
>kali@kali:~/hackshort-util$ cat /tmp/running_during_install  
>
># ========== Expected Result ==========
>cat: /tmp/running_during_install: No such file or directory
># =====================================
>
>kali@kali:~/hackshort-util$ python3 ./setup.py sdist 
>
># ========== Expected Result ==========
>...
># =====================================
>```

Installing the New Package and Checking if Custom Code Executed
>``` shell
>kali@kali:~/hackshort-util$ pip install ./dist/hackshort_util-1.1.4.tar.gz
>
># ========== Expected Result ==========
>...
># =====================================
>
>kali@kali:~/hackshort-util$ cat /tmp/running_during_install 
>
># ========== Expected Result ==========
>This code was executed when the package was installed   
># =====================================
>```
