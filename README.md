# Building

* Install [Nix](https://nixos.org/nix/download.html) or [NixOS](http://nixos.org/nixos/download.html)
* Clone my supplementary packages [repo](https://github.com/maurer/nixlocal)
* Clone [nixpkgs](https://github.com/nixos/nixpkgs) if you have not done so during your installation
* Set `NIX_PATH` to have nixpkgs pointing at the master repo, and local pointing at my nixlocal repository
* Run `nix-build` in the source directory to do an in place build, or `nix-env -f . -i` to install.

# Operation

## Training

For each believed benign binary, run
```
generate_db ${target} > ${target}.db
```
to generate a callgraph, dump constants, cluster the callgraph, and convert the results in a constant database.

Finally, merge all the benign binaries together with
```
merge-db ${target1}.db ${target2}.db ... > merged.db
```

## Testing

For a potentially tampered with program ${target}, run
```
test_program ${target} merged.db > out
```

The resulting output file will contain a list of abberant constants, ranked by weirdness (a constant in a component which matched well with a component in the database, but which did not appear in that component, will be the highest weirdness) with addresses to jump to.
