# comments app
A simple, self-hosted comment engine

The [demo](https://netlify-comments.dyuproject.com) is running on a $2.5 vultr plan located in SG, with the app [configured](ARGS.txt) to use a max memory of 128mb (to leave most of the available memory to the filesystem cache).

![screenshot](https://github.com/dyu/comments/raw/master/screenshot.png)

## Instant comments on your site/blog
Put this anywhere in the html body (although it is advisable to put it last)
```html
<div id="comments"></div>
<script>
window.rpc_host = 'https://rpc.dyuproject.com';
window.comments_max_depth = 10; // max: 127
window.comments_collapse_depth = 7; // the depth where comments get collapsed by default
</script>
<script src="https://netlify-comments.dyuproject.com/build.js"></script>
<link rel="stylesheet" href="https://netlify-comments.dyuproject.com/build.css" />
```
> Note: Uses the same instance powering the demo. No tracking whatsoever.

1. The [css](https://dyu.github.io/comments/build.css) is 6.2kb minified, built with [pavilion](https://github.com/getpavilion/pavilion) core.
2. The [js](https://dyu.github.io/comments/build.js) is 79.7kb minified, built with:
   - [sveltjs](https://github.com/sveltejs/svelte)
   - [showdown](https://github.com/showdownjs/showdown)
   - [dompurify](https://github.com/cure53/DOMPurify)
   - [string-hash](https://github.com/darkskyapp/string-hash)
   - [color-hash](https://github.com/zenozeng/color-hash)

## Server runtime dependencies
- jdk7

## Dev requirements
- [node](https://nodejs.org/en/download/) 6.9.0 or higher
- yarn (npm install -g yarn)
- jdk7 (at /usr/lib/jvm/java-7-oracle)
- [maven](https://maven.apache.org/download.cgi)
- [protostuffdb](https://gitlab.com/dyu/protostuffdb) (downloaded below)

## Setup
```sh
mkdir -p target/data/main
echo "Your data lives in user/ dir.  Feel free to back it up." > target/data/main/README.txt

# download protostuffdb
yarn add protostuffdb@0.10.2 && mv node_modules/protostuffdb/dist/* target/ && rm -f package.json yarn.lock && rm -r node_modules

wget -O target/fbsgen-ds.jar https://repo1.maven.org/maven2/com/dyuproject/fbsgen/ds/fbsgen-ds-fatjar/1.0.5/fbsgen-ds-fatjar-1.0.5.jar
./modules/codegen.sh
mvn install

npm install -g http-server clean-css-cli
cd comments-ts
yarn install
```

## Dev mode
```sh
# produces a single jar the first time (comments-all/target/comments-all-jarjar.jar)
./run.sh

# on another terminal
cd comments-ts
# serves the ui via http://localhost:8080/
yarn run dev
```

## Production mode
```sh
cd comments-ts
# produces a single js and other assets in comments-ts/dist/
yarn run build
```

