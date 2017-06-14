## Buildit Gems

This repo contains a file `build` that is a script for building gems using the [buildit-buildpack](https://github.com/schneems/buildit-buildpack).


## Run

Clone the repo.

```
$ git clone git@github.com:schneems/buildit-gems.git
$ cd buildit-gems
```

Create a heroku app

```
$ heroku create
```

Set your buildpack

```
$ heroku buildpacks:add heroku/ruby
$ heroku buildpacks:add https://github.com/schneems/buildit-buildpack
```

The `heroku/ruby` is needed otherwise Ruby is in a different directory such as `/usr/bin/ruby2.3` and that will get added to the shebang lines of the gem i.e.

```
#!/usr/bin/env ruby2.3
```

Which is not correct. It should be:

```
#!/usr/bin/env ruby
```

To get this, we force an install of Ruby earlier on the path using the `heroku/ruby` buildpack which puts a `ruby` executable path.

Set config vars needed for this script:

```
$ heroku config:set GEM=bundler
$ heroku config:set VERSION=1.9.6
```

Deploy:

```
$ git push heroku master

Writing objects: 100% (1/1), 179 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)
remote: Compressing source files... done.
remote: Building source:
remote:
remote: -----> Fetching custom git buildpack... done
remote: -----> We can build it! app detected
remote: running: $ gem install bundler --version 1.9.6 --no-ri --no-rdoc --env-shebang
remote: Successfully installed bundler-1.9.6
remote: 1 gem installed
remote: tar: Removing leading `/' from member names
remote: -----> Discovering process types
remote:        Procfile declares types            -> (none)
remote:        Default types for We can build it! -> web
remote:
remote: -----> Compressing... done, 490K
remote: -----> Launching... done, v8
remote:        https://schneems-buildit-gems.herokuapp.com/ deployed to Heroku
remote:
remote: Verifying deploy... done.
To https://git.heroku.com/crazy-mountain-1984.git
   c9f2ee8..d2fea6c  master -> master
```

Protip: If you need to re-deploy without changing your script you can force a commit by running `$ git commit --allow-empty -mempty`.


Now visit your site to download the results in a `builds.tar`

```
$ heroku open
```


