
# Ansible Issue #7353 Reproduction

This repo provides an example which reproduces the issue.

## Expected Result

As role *child* is a dependency of role *y* it should be run.

```
$> ansible-playbook -i inventory.ini playbook.yml

PLAY [all] ******************************************************************** 

TASK: [child | Echo something] ************************************************ 
ok: [127.0.0.1] => {
    "item": "", 
    "msg": "This task should be run as a dependency of role 'y'"
}

PLAY RECAP ******************************************************************** 
127.0.0.1                  : ok=1    changed=0    unreachable=0    failed=0   
```

## Actual Result

As role *x* is skipped, the *child* role is also skipped, and is then skipped
for role *y*.

```
$> ansible-playbook -i inventory.ini playbook.yml

PLAY [all] ******************************************************************** 

TASK: [child | Echo something] ************************************************ 
skipping: [127.0.0.1]

PLAY RECAP ******************************************************************** 
127.0.0.1                  : ok=0    changed=0    unreachable=0    failed=0   
```

Toggling the *when* condition in _roles/z/meta/main.yml_ will produce the
expected result.

