## need setup
+ Erlang
+ Elixir
+ Mix
+ Hex
+ Phoenix
+ Node
+ Brunch
+ Postgres

## commands similar to rails

| phoenix  | rails |
| ------------- | ------------- |
| mix phoenix.new  | rails new  |
| mix deps.get  | bundle install  |
| mix ecto.create  | rake db:create  |
| mix phoenix.server  | rails server  |
| mix phoenix.routes  | rake routes  |
| iex -S mix phoenix.server  | rails console  |
| mix phoenix.gen.html  | rails generate scaffold  |
| mix ecto.migrate  | rake db:migrate  |
| mix -h  | rails -h  |
| eex  | erb  |


```
mix phoenix.new appName
mix deps.get
mix ecto.create
mix phoenix.server
mix phoenix.routes
iex -S mix phoenix.server
```

```
mix phoenix.gen.html Post posts title body:text
mix ecto.migrate
```

```ex
resources "/posts", PostController
```

## repl

| elixir | ruby |
| ------------- | ------------- |
| iex  | irb  |
| v  | _  |
| Ctrl-D | Ctrl-C twice |

> There are several ways of exiting from iex—none are tidy. The easiest two are typing Ctrl-C twice or typing Ctrl-G followed by q and Return.

## elm-brunch

```
npm install --save elm-brunch
```

```
    elmBrunch: {
      elmFolder: "web/static/elm",
      mainModules: ["App.elm"],
      outputFolder: "../vendor/elm"
    },
```

```
cd web/static/elm
subl App.elm
elm package install evancz/elm-html
```

```
brunch build
mix phoenix.server
```

## elm-brunch

#### grunt-cli

```
    npm install -g grunt-cli --save-dev
    npm install grunt --save-dev
    npm install grunt-contrib-watch --save-dev
    npm install grunt-contrib-clean --save-dev
    npm install grunt-elm --save-dev
```

```
/* Gruntfile.js */

module.exports = function(grunt) {

  grunt.initConfig({
    elm: {
      compile: {
        files: {
          "hello.js": ["Hello.elm"]
        }
      }
    },
    watch: {
      elm: {
        files: ["Hello.elm"],
        tasks: ["elm"]
      }
    },
    clean: ["elm-stuff/build-artifacts"]
  });

  grunt.loadNpmTasks('grunt-contrib-watch');
  grunt.loadNpmTasks('grunt-contrib-clean');
  grunt.loadNpmTasks('grunt-elm');

  grunt.registerTask('default', ['elm']);

};

```

```
grunt elm
grunt clean
grunt watch
```

#### gulp-cli

```
    npm install -g gulp-cli --save-dev
    npm install gulp --save-dev
    npm install gulp-elm --save-dev
    npm install gulp-rename --save-dev
```

```
/* gulpfile.js */

var gulp = require('gulp'),
    elm = require('gulp-elm'),
    rename = require('gulp-rename');

gulp.task('elm-init', elm.init);

gulp.task('elm', ['elm-init'], function () {
    return gulp.src('./Hello.elm')
        .pipe(elm())
        .on('error', swallowError)
        .pipe(rename('hello.js'))
        .pipe(gulp.dest('./'))
});

function swallowError (error) {
    console.log(error.toString());
    this.emit('end');
}

gulp.task('default', ['elm'], function () {
    gulp.watch('./*.elm', ['elm']);
});

```

```
gulp
```

#### brunch

```
  npm init
  npm install -g brunch --save-dev
  npm install elm-brunch --save-dev
```

```
/* brunch-config.js */

module.exports = {
  config: {
    paths: {
      watched: ["app", "src"]
    },
    server: {
      port: 3000
    },
    files: {
      javascripts: {
        joinTo: "app.js"
      },
      stylesheets: {
        joinTo: "app.css"
      }
    },
    plugins: {
      elmBrunch: {
        mainModules: ["src/Hello.elm"],
        outputFolder: "public/"
      }
    }
  }
};

```

```
    brunch watch --server
```

https://github.com/kangkyu/hello-elm

```
module Hello where

import Html

main =
  Html.h1 [ ] [ Html.text "Hello Elm!" ]
```

```css
h1 {
  font-family: Verdana;
  font-size: 36px
}
```

## elm-reactor

https://github.com/kangkyu/hello-elm-reactor

```elm
module Hello where

import Graphics.Element exposing (Element, container, middle, centered)
import Window
import Text exposing (Text, typeface, fromString)

helloText : Text
helloText =
  "Hello Elm Reactor!"
    |> fromString
    |> typeface ["Verdana"]
    |> Text.height 36

showMiddle : Int -> Int -> Element
showMiddle w h =
  helloText
    |> centered
    |> container w h middle

main : Signal Element
main =
  Signal.map2 showMiddle Window.width Window.height
```
