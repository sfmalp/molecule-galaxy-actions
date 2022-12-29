# molecule-galaxy-actions

```
molecule init role sfmalp.dependency # just to see if and how dependencies are handled
molecule init role sfmalp.dummy
```

Neither `side_effect.yml` nor `idempotence.yml` are created by the `init` commands.
There are 4 more possible playbooks which seem unnecessary on throwaway machines:

* `create.yml` and `destroy.yml` are created automatically, and can be removed
* `prepare.yml` and `cleanup.yml` are optional parts of the testing sequence, but not created automatically

Unfortunately, importing roles (that are not part of a collection) into galaxy seems to be cumbersome:

* each role needs to be at the root level of its own dedicated repository
* there does not seem to be any API for importing them

So far, the best degree of automation I achieved:

* `ansible-galaxy collection init sfmalp.dummy_collection`
* I see to reason to include the namespace (== github name) in the path, so put repo inside it
* do molecule steps described above in `dummy_collection.roles`
* galaxy requires an api token to publish, use https://github.com/marketplace/actions/publish-ansible-role-to-galaxy if you trust github with it, but do not include it under `.github/workflows/`
* in the pipeline:
```
    steps:
    - name: delegated ensure pip
      run: |
        pip install molecule
    - name: delegated checkout
      uses: actions/checkout@v3
    - name: delegated test
      run: |
        cd dummy_collection/roles/dummy_role
        molecule test
```

note that dependency is from same collection, can this be resolved locally to avoid cyclic dependencies?