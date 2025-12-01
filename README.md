# selinux-policy

This is secureblue's SELinux policy based on [Fedora's policy](https://github.com/fedora-selinux/selinux-policy).

The override below simply provides the correct context if trivalent ended up 
installing in /usr/lib64 instead of /usr/lib. The priority is set at 401 just
above 400, the default trivalent policy.
This potentially happens if you did not use secureblue's ISO and installed
trivalent from the copr repo directly, and then migrated to secureblue
using the unofficial install_secureblue.sh script
 
```
make trivalent-lib64.pp
```

## install the override
```
semodule -X 401 -i trivalent-lib64.pp
```

## restorecon on all relevant directories, restorecon on /usr/lib64/trivalent if needed...
```
restorecon -Rv ~/.cache/trivalent
restorecon -Rv ~/.config/trivalent
restorecon -Rv /usr/lib64/trivalent
```

## verify that all context are accurate
```
ls -alhZ ~/.config/trivalent | grep trivalent
ls -alhZ ~/.cache/trivalent | grep trivalent
ls -alhZ /usr/lib64/trivalent | grep trivalent
```

## home directory output
```
drwxr-x---. 1 user user unconfined_u:object_r:trivalent_home_t:s0 1.5K Dec  1 23:31 .
drwx------. 1 user user unconfined_u:object_r:trivalent_home_t:s0    0 Dec  1 16:50 AmountExtractionHeuristicRegexes
...
...
```

## /usr/lib64/trivalent directory output
```
-rwxr-xr-x.   1 root root system_u:object_r:trivalent_exec_t:s0        1.9M Jan  1  1970 chrome_crashpad_handler
-rwxr-xr-x.   1 root root system_u:object_r:trivalent_script_exec_t:s0 1.7K Jan  1  1970 install_filter.sh
-rwxr-xr-x.   1 root root system_u:object_r:trivalent_exec_t:s0        274M Jan  1  1970 trivalent
-rwxr-xr-x.   1 root root system_u:object_r:trivalent_script_exec_t:s0 3.7K Jan  1  1970 trivalent.sh
```
