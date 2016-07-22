# @Ching-Han Ho

Source code for my personal blog.

## Requirements

* ruby-2.0.0-p0

## Usage

```
git clone -b source git@github.com:chinghanho/chinghanho.github.com.git chinghanho
cd chinghanho
```

Create new post:

```
rake new_post\[title\]
rake gen_deploy
```

if you are new on a new machine:

```
rake setup_github_pages
rake gen_deploy
```
