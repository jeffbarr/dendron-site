---
id: 2976e5df-c8b4-4176-9d3f-fe4220dfa9b6
title: Deploy
desc: ''
updated: 1617039146949
created: 1613863275374
---



## pre-req
```sh
npm install -g verdaccio
npm install -g yarn
npm install -g vsce
sudo apt-get install jq -y

git clone git@github.com:dendronhq/dendron.git dendron
git clone git@github.com:dendronhq/dendron.git dendron-build
```

- run verdaccio
```
verdaccio
```


## build and source

```sh
yarn
yarn bootstrap:build 
# initialize scripts
. bootstrap/scripts/helpers.sh
```

## test local

- publish from local
```sh
setRegLocal
verdaccio

gbD integ-publish
gco -b integ-publish

version=prepatch
lerna version $version --no-git-tag-version 

git add .
git commit -m "integ: publish $version"
git push --set-upstream origin integ-publish --force

lerna publish from-package --ignore-scripts
node bootstrap/scripts/genMeta.js
```

- pull in new changes
```sh
WORKSPACE_BUILD=/mnt/one/dendron-build
WORKSPACE_BUILD=/Users/kevinlin/projects/dendronv2/dendron/build/dendron/packages/plugin-core
cd $WORKSPACE_BUILD

git reset --hard
git clean -f
gco master
git fetch
gbD integ-publish
git checkout --track origin/integ-publish
./scripts/pack_and_install.sh

# combine the above
./scripts/pack_and_install.sh
./scripts/publish-insider.sh
```

- test changes
    - [ ] any packages installed will be with verdaccio so don't use dendron-site

## publish

- checkout
```
gco master
setRegRemote
```

- [ ] cherry pick if needed

```
version=minor
lerna version $version 
lerna publish from-package 
lerna publish from-package --ignore-scripts
```

- pull in new changes
```
sleep 60
git reset --hard
git clean -f
git fetch
gco master 
./scripts/pack_and_install.sh
```

## re-publish

```sh
./unpublish.sh
lerna publish from-package --ignore-scripts
```

## cleanup
```
cd $WORKSPACE_BUILD
gbD integ-publish

cd $WORKSPACE
gbD integ-publish
```


## cook

### unpublish a package

```
./unpublish.sh
```

## rest

### git 
```sh

yarn
yarn bootstrap:build 
. bootstrap/scripts/helpers.sh

```
