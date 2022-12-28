# molecule-galaxy-actions

```
molecule init role sfmalp.dependency
molecule init role sfmalp.dummy
```

Neither `side_effect.yml` nor `idempotence.yml` are created by the `init` commands.
There are 4 more possible playbooks which seem unnecessary on throwaway machines:

* `create.yml` and `destroy.yml` are created automatically, and can be removed
* `prepare.yml` and `cleanup.yml` are optional parts of the testing sequence, but not created automatically

Unfortunately, importing roles (that are not part of a collection) into galaxy seems to be cumbersome:

* each role needs to be at the root level of its own dedicated repository
* there does not seem to be any API for importing them

This seems to be different to collections.