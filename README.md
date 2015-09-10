#README 
```
hugo new site hugo_blog
cd hugo_blog

hugo new first.md
hugo undraft content/first.md

mkdir themes
cd themes
git clone https://github.com/spf13/hyde.git
cd hyde
rm -rf .git

cd ..
cd ..
hugo server -t hyde --watch
```