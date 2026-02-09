# Creating Our Malicious Package

Structure of a Python Package
>``` shell
>└── hackshort-util
>    ├── setup.py
>    └── hackshort_util
>        └── __init__.py
>```

Creating Most Basic Python Package
>``` shell
>kali@kali:~$ mkdir hackshort-util
>
>kali@kali:~$ cd hackshort-util           
>                                                                                                        
>kali@kali:~/hackshort-util$ nano setup.py
>
>kali@kali:~/hackshort-util$ cat -n setup.py
>
># ========== Expected Result ==========
>01  from setuptools import setup, find_packages
>02
>03  setup(
>04      name='hackshort-util',
>05      version='1.1.4',
>06      packages=find_packages(),
>07      classifiers=[],
>08      install_requires=[],
>09      tests_require=[],
>10  )
># =====================================
>
>kali@kali:~/hackshort-util$ mkdir hackshort_util
>
>kali@kali:~/hackshort-util$ touch hackshort_util/__init__.py
>```

Running the Newly Created Python Package
>``` shell
>kali@kali:~/hackshort-util$ python3 ./setup.py sdist
>
># ========== Expected Result ==========
>running sdist
>running egg_info
>writing hackshort_util.egg-info/PKG-INFO
>writing dependency_links to hackshort_util.egg-info/dependency_links.txt
>writing top-level names to hackshort_util.egg-info/top_level.txt
>reading manifest file 'hackshort_util.egg-info/SOURCES.txt'
>writing manifest file 'hackshort_util.egg-info/SOURCES.txt'
>warning: sdist: standard file not found: should have one of README, README.rst, README.txt, README.md
>
>running check
>creating hackshort-util-1.1.4
>creating hackshort-util-1.1.4/hackshort_util
>creating hackshort-util-1.1.4/hackshort_util.egg-info
>copying files to hackshort-util-1.1.4...
>copying setup.py -> hackshort-util-1.1.4
>copying hackshort_util/__init__.py -> hackshort-util-1.1.4/hackshort_util
>copying hackshort_util/utils.py -> hackshort-util-1.1.4/hackshort_util
>copying hackshort_util.egg-info/PKG-INFO -> hackshort-util-1.1.4/hackshort_util.egg-info
>copying hackshort_util.egg-info/SOURCES.txt -> hackshort-util-1.1.4/hackshort_util.egg-info
>copying hackshort_util.egg-info/dependency_links.txt -> hackshort-util-1.1.4/hackshort_util.egg-info
>copying hackshort_util.egg-info/top_level.txt -> hackshort-util-1.1.4/hackshort_util.egg-info
>Writing hackshort-util-1.1.4/setup.cfg
>Creating tar archive
>removing 'hackshort-util-1.1.4' (and everything under it)
># =====================================
>```

Installing hackshort-util Locally
>``` shell
>kali@kali:~/hackshort-util$ pip install dist/hackshort_util-1.1.4.tar.gz
>
># ========== Expected Result ==========
>Defaulting to user installation because normal site-packages is not writeable
>Looking in indexes: http://pypi.offseclab.io, http://127.0.0.1
>Processing ./dist/hackshort_util-1.1.4.tar.gz
>  Preparing metadata (setup.py) ... done
>Building wheels for collected packages: hackshort-util
>  Building wheel for hackshort-util (setup.py) ... done
>  Created wheel for hackshort-util: filename=hackshort_util-1.1.4-py3-none-any.whl size=1188 sha256=2b00a9631c7fb9e1094b6c6ac70bd4424f1ecc3110e05dc89b6352229ed58f93
>  Stored in directory: /home/kali/.cache/pip/wheels/da/63/05/afd9e305b95f17a67a64eaa1e62f8acfd4fe458712853c2c3d
>Successfully built hackshort-util
>Installing collected packages: hackshort-util
>Successfully installed hackshort-util-1.1.4
># =====================================
>```

Importing and Using hackshort_util Package
>``` shell
>kali@kali:~$ python3
>
># ========== Expected Result ==========
>Python 3.11.2 [GCC 12.2.0] on linux
>Type "help", "copyright", "credits" or "license" for more information.
>>>> import hackshort_util
>>>> print(hackshort_util)
><module 'hackshort_util' from '/home/kali/.local/lib/python3.11/site-packages/hackshort_util/__init__.py'>
># =====================================
>```

Uninstalling hackshort-util Package
>``` shell
>kali@kali:~/hackshort-util$ pip uninstall hackshort-util 
>
># ========== Expected Result ==========
>Found existing installation: hackshort-util 1.1.4
>Uninstalling hackshort-util-1.1.4:
>  Would remove:
>    /home/kali/.local/lib/python3.11/site-packages/hackshort_util-1.1.4.dist-info/*
>    /home/kali/.local/lib/python3.11/site-packages/hackshort_util/*
>Proceed (Y/n)? Y
>  Successfully uninstalled hackshort-util-1.1.4
># =====================================
>```
