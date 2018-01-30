![leveldb](https://user-images.githubusercontent.com/2289/35121759-6490fb0a-fc51-11e7-9dfd-dde9e2d09765.png)

LevelDB is a key-value store created by Google for use in the Chromium browser.
In typical Node.js style, some cool kids have run wild with the technology and
found many exciting new uses for it outside of the browser.

I sat down with LevelDB aficionado [Julian Gruber](http://juliangruber.com/)
to talk about what LevelDB is, how to use it, and where the project is headed.

As a GitHub employee, I have an annual budget that I can spend on 
"Learning and Development". In exchange for Julian's time I donated $100 to 
his charity of choice, 
[Code for Science & Society](https://donate.datproject.org/). 
Thanks to GitHub and Julian for making this possible. 🙏

🎧 [Listen to our 40-minute conversation on YouTube](https://youtu.be/-ofwZi9Xj44).

## Questions / Notes

#### I'm just getting started with LevelDB. Which module(s) should I install?

```sh
npm install level
```

[level](https://ghub.io) is the batteries-included module that most people should use. It bundles
[levelup](https://ghub.io/levelup) and
[leveldown](https://ghub.io/leveldown), 
and defaults to using the file system for storage.

#### Where are the good LevelDB modules?

The [@Level](https://github.com) org on GitHub has many, but there are more in 
npm userland. Most packages start with `level-`. GitHub search is handy for 
this.

#### Callbacks? Promises? Async/Await?

As of version 2, `level` supports Promises (and still supports node-style 
callbacks too). If you omit the callback argument, you'll get a Promise. 
This means you can do this:

```js
const main = async () => {
  const db = level('./my-db')

  await db.put('foo', 'bar')
  console.log(await db.get('foo'))
}
```

As for the rest of LevelDB userland, many modules still use callbacks. Your 
mileage may vary.

#### Avoiding race conditions

levelDB is a single-process library; That means it does create a folder on your disk that stores all the data, and only one instance can point to it. However, there's a module (created by Julian) called 
[multilevel](https://ghub.io/multilevel)
which exposes a levelDB over the network, to be used by multiple processes, with levelUp's API.

#### Native Modules

Many backends, like the default `leveldown`, use [prebuild](https://github.com/prebuild/prebuild),
for native code compilation. This means when you install `level`, your machine
downloads a precompiled binary for your system from GitHub Releases, rather
than compiling it on the fly.

#### How to save JSON?

To save JSON objects, set the `valueEncoding` option when initializing the
DB:

```js
const db = level('./db' {valueEncoding: 'json'})
```

This will allows you to save JSON with `db.put()` and retrieve it with 
`db.get()`, without stringifying and parsing it yourself.

You can alternatively also set this option on a call by call basis,
if that fits your use case better:

```js
db.put('key', { some: 'json' }, { valueEncoding: 'json' })
```

#### Timestamps

LevelDB doesn't have timestamps. Roll your own.

See also [level-timestamps](https://github.com/juliangruber/level-timestamps),
[level-ttl](https://ghub.io/level-ttl), and
[level-version](https://ghub.io/level-version).

#### Search

We didn't get too much into this, but it sounds like the essential things
is key design. Storing indexes is very cheap, and if space is not an issue,
you can store data redundantly to prevent additional lookups.

See also [level-search](https://ghub.io/level-search)

#### CLI / REPL

Tools for interacting with your LevelDB:

- [leveldb-repl](https://ghub.io/leveldb-repl)
- [lev](https://ghub.io/lev)
- [ldb](https://github.com/0x00A/ldb)
