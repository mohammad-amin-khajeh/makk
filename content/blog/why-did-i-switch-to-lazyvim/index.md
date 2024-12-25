+++
title = "Why I switched to lazyvim(and you should too)"
description = "With so many good neovim distributions and the ability to create your own config from scratch, why did I switch to and stay on lazyvim?"
date = "2024-12-25"
[extra]
toc = true
[taxonomies]
tags = ['neovim']
categories = ['thought']
+++

I used to have this love hate relationship with Neovim,
well to be honest it was love for the most part,
but I always had this idea in the back of my head that
"damn I wish this was behaving more like that".

And to be fair I did try; I did try to make Neovim perfect for me and me alone.
But if you have life and responsibilities and can't keep up with the forever changing
ecosystem of Neovim, or the sheer amount of configuration you need to set up and maintain,
then I don't know about you but I for one did not have a "perfect" time.

This is where a Neovim distribution, and more specifically [lazyvim](https://www.lazyvim.org/) shines.

{{admonition(type='note' text='This blog post does not intend to say there are no downsides to using a Neovim distribution, one has to be competent in Neovim and its lua api in the first place.')}}

## 1.Is a good base

Now don't lie to me here I know you're going set up that lsp server.
You're also going to set up treesitter. Probably need to toss in [telescope](https://github.com/nvim-telescope/telescope.nvim)[^1] as well.
Ah! Who can write code without the new hotness that is [blink.cmp](https://cmp.saghen.dev/)?
And lets also not forget [mason](https://github.com/williamboman/mason.nvim).

There are certain plugins and configurations that you are going to need anyway if you're going to use Neovim as your code editor[^2].

Lazyvim takes care of all that.

Some may call it bloated but I beg to differ. My stance on minimalism and suckless-ish software
probably needs a blog post of its own, but I for the most part use everything provided by Lazyvim
and do not think the added startup time(among other factors of course) is going to waste.

## 2. Is opinionated

There are many ways to get the job done. Take vim motions for instance; why would anyone use `vawd` when they can `daw` directly?

Ideal ways exist and unideal ways exist, while we may only know of one or the other.

What I personally love about lazyvim is that it takes care of my opinions.
Because I can rest assured that despite its complexities,
many thoughts have been put into the creation and maintenance of it.

I especially like the [lazyExtras](https://www.lazyvim.org/extras) feature of it shown below.
Makes it extremely trivial and simple to set up all the configuration
needed to have a 10x environment for a given language/framework as
it merely requires a click to be toggled on. I mean hell I even code in java with it.

{{full_width_image(src="./lazyExtras.png")}}

Of course there are times where I don't like something and so decide to add
my own touch to it. But even then lazyvim has made it extremely simple and easy
to spice it up and add you own touch to the config.

## 3. No constant maintenance on my part

The plugin ecosystem of neovim as I mentioned has been moving quickly for some time now.
Quite a while now that I think of it.

And while the community has settled for some plugins and made them the standard[^4]
a large chunk of it is still moving and developing.
I can think of nvim-cmp getting replaced[^3] by [blink.cmp](https://cmp.saghen.dev/)
in the past month.

No complaints though; I'm all for progress.
Hell that's why I'm using arch Linux(by the way) in the first place.
But when there is a constant need of being on and adapting to
the latest and greatest, the time it takes to do that quickly adds up
and the whole thing turns into a chore.

With lazyvim however, it takes at most a few hours to merge a fixing pull request.
In the meantime you can check the issues and most probably find a decent solution there.

## 4. Cured my tinkering addiction

I honestly just want to get job done.

But when I can switch up my configuration
and get that sweet dopamine rush since bare Neovim is meant to be infinitely malleable,
it's hard to resist the temptations; I was pretty much stuck in a constant loop of rabit holes.

Considering that lazyvim has a good base and takes care of the maintenance part,
I really don't feel the urge to modify my config all that often.

Maybe once every two weeks or so I'd discover something new and play around with it for half an hour at most;
and that's as far as it goes. No more infinite lua files,
no more reading entire docs when I just wanted to install the thing and get job done,
no more theme switching and not letting my brain get used to one(tokyo-night rocks).

## Summary

I love lazyvim because it is a good base for your own config
and is opinionated. Not only that but it also has an active community
maintaining the plugin configs and updating them in case of a breaking change.
Lastly, lazyvim allowed me to treat my text editor like a text editor and not a toy
by curing my tinkering addiction.

[^1]: Although lazyvim has switched to fzf at the time of writing.
[^2]: Unless you're one of those weirdo C devs [writing a kernel driver from scratch in vi](https://www.youtube.com/watch?v=IXBC85SGC0Q) in which case, hats off.
[^3]: Courtesy of [lazyvim](https://github.com/LazyVim/LazyVim/releases/tag/v14.0.0)?.
[^4]: lazy.nvim, the package manager for instance.
