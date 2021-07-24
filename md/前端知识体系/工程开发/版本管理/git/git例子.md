## Create a new repository
```
git clone http://gitlab.365youlian.com/juluo/crv_web.git
cd crv_web
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
```

## Push an existing folder
```
cd existing_folder
git init
git remote add origin http://gitlab.365youlian.com/juluo/crv_web.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

## Push an existing Git repository
```
cd existing_repo
git remote rename origin old-origin
git remote add origin http://gitlab.365youlian.com/juluo/crv_web.git
git push -u origin --all
git push -u origin --tags
```