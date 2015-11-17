Title: Faster pastes with ix.io
Slug: faster-pastes-with-ix-io
Date: 2015-11-12 22:26:43
Tags: tools
Summary: Share pastes incredibly fast with ix.io
Status: Published
Schema: Article
Keywords: terminal, paste, pastebin, pastie, ix, vim

Pasting code or other text is so common that there's an insane amount of
paste sites available and while they all have their strength, the majority
of them rely on their web interface to post the code required and/or pack some
additional metadata that I don't particularly care for. (filename, filetype, etc..)

I've used many of them throughout my career daily, but none of them really stuck
as the one goto paste tool. The three I've used most are:

- [Pastie](http://www.pastie.org)
- [hastebin](http://www.hastebin.com)
- [GitHub Gist](http://gist.github.com)

Where `Gist` remains the one I use if I want to keep track of the pastes I make
and manage them as a repository in their own right. However, not every paste I
make needs to be persisted, most are just one-off and can be left to rot, which
it not what I'd like my `Gist` page to contain.

I live in the terminal. My entire development workflow revolves around
having the terminal open the entire time and that terminal occupying my entire
desktop space; now while it may be possible to still use these tools, I simply
needed something small, available as a command line interface with as little
noise as possible all the while being able to either paste and throw away or
keep them combined under a specific alias for future use. Several months back
I was introduced to [ix.io](http://ix.io) and that remains for now, my paste
tool of choice.

### So what exactly is ix.io?

`ix.io` is a command line pastebin, plain and simple. It provides you with the
ability to create pastes from the command line which in turn gives your other
terminal applications the power to do the same.

`ix.io` uses [curl](http://curl.haxx.se/) as the communication client and thus
uses HTTP with a somewhat restful approach to managing pastes. This is quite
apparent from their [main page](http://ix.io). Let's see how it works, we will be using
the [client](http://ix.io/client) available on the official site which is
a simple, yet powerful script.

First off, make sure you have `curl` installed, this is a hard requirement for
the client script.

Next download the client script by first inspecting the [client](http://ix.io/client)
code (make sure you get into the habit of doing this) and then running the following:

    :::text
    % curl ix.io/client > ix
    % chmod +x ix

_N.B. Everything from here on out assumes that `ix` is available in your path
so you'll have to either ensure it's there or replace any occurance of `ix` by
`path/to/your/ix`._

Let's get dirty!

### Pasting text

All you need to do is run the `ix` command by itself.

    :::text
    % ix
    ^C to cancel, ^D to send.
    class Foo(object):
        def bar(self):
            self.stuff()
    http://ix.io/mac

This will prompt you to type multi-line text and simply send using `^D` (CTRL+D)
or cancel using `^C` (CTRL+C). (Tip: `^D` should be on a new line and doesn't
need a following `Enter`)

The result of your paste will be a URL with a random ID that you can open in
order to get the pasted text.

<script type="text/javascript" src="https://asciinema.org/a/8s5hbhfyl3d5x8oib4uc6tzxg.js" id="asciicast-8s5hbhfyl3d5x8oib4uc6tzxg" async></script>

Most commonly you would use one of the following methods in order to paste:

    :::text
    % ix somefile.txt
    % echo "some text" | ix
    % cat some file1 file2 | ix
    % run_some_command | ix

### View the recently pasted text

Who needs a browser? Since we're in the terminal, let's just stay there!

    :::text
    % curl http://ix.io/mac
    class Foo(object):
        def bar(self):
            self.stuff()

<script type="text/javascript" src="https://asciinema.org/a/405sst73fogw22002263d4tsf.js" id="asciicast-405sst73fogw22002263d4tsf" async></script>

### How about some highlight?

One of my personal favorite features of `ix.io` is the ability to get highlighted
text right in the terminal.

    :::text
    % curl http://ix.io/mac+python

Don't believe me? See for yourself:

<script type="text/javascript" src="https://asciinema.org/a/cadujn9l3oy61rc8ptn96w1u4.js" id="asciicast-cadujn9l3oy61rc8ptn96w1u4" async></script>

You can achieve the same result by appending `/python` to the URL and opening
it in the browser.

### Persisting the pastes under an alias

`ix.io` also allows you to store your pastes under a user. It uses `.netrc` as
means to identify who you are with respect to the `ix.io` host. In order to
create a user, open `~/.netrc` in your favorite text editor and add the following:

    :::text
    machine ix.io
      login user123
      password pass456

The authentication method is HTTP `Basic Auth` so don't use any important
password. `ix.io` would now look at your credentials every time you run the `ix` client
and if it doesn't find your user, it will create it during your first paste.

You can now also view your available pastes at `http://ix.io/user/user123`. Sadly,
there's no way to get plain text out of that URL, so you have to either do that
in the browser or parse the resulting markup.

By having a user you can now also update your own pastes:

    :::text
    % ix -i ID ...

Or even deleting your own pastes:

    :::text
    % ix -d ID

### How can I use this with my text editor or IDE?

It really depends on the text editor's way of invoking external commands. You
will have to consult with the respective tools' documentation. I can offer
suggestion on how to paste from the following tools:

**Emacs** Install the following [plugin](https://github.com/theanalyst/ix.el) and use:
`M-x ix`

**Vim** Place the following in your `.vimrc`

    :::vimscript
    noremap <silent> <leader>i :w !ix<CR>

And use `\i` on a buffer or a visual block, where `\` is the default leader
(replace it with your configured leader key). Below is a sample of the Vim
binding used:

<script type="text/javascript" src="https://asciinema.org/a/93r53538bfluivwe8dbnmrh9b.js" id="asciicast-93r53538bfluivwe8dbnmrh9b" async></script>

Enjoy
