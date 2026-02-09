# Understanding the Attack

Flow of Downloading When Public Repo does not Contain Package
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Understanding-the-Attack-1.png)

Flow of Downloading when Public Repo does Contain Package
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Understanding-the-Attack-2.png)

Version Specifier for hackshort-util Requirment
>``` shell
>hackshort-util~=1.1.0
>```

Importing utils Submodule from hackshort-util Package
>``` shell
>from hackshort_util import utils
>```

Lab 1 - Which configuration makes pip vulnerable to a dependency chain attack?
>--extra-index-url

Lab 2 - Which of the following given version options will satisfy the below requirement? hackshort-util==2.*
>A) 2.0.1

Lab 3 - What does the ~= version specifier in pip indicate?
>D) Versions that are compatible with the specified version can be used

Lab 4 - Why do developers often replace dashes with underscores in Python package names?
>B) Dashes cause issues in Python syntax.
