[Back to Homepage](https://linuxcloudadmin.github.io)

## GIT
- To sync git repository.

```
git pull
```

- To add a new file to git repository.

```
git add file-name.txt
```

- Commit created file.

```
git commit -m "specify changes done here" file-name.txt
```

- Push file to git master.

```
git push origin master
```

- Shows changes done to the file.

```
# git log file-name.txt
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    Change version number

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    Remove unnecessary test

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700

    Initial commit
```

- To find the changes happend between two commits

```
git diff ca82a6dff817ec66f44342007202690a93763949 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
```

[Back to Homepage](https://linuxcloudadmin.github.io)
