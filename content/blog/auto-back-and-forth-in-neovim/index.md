+++
title = 'Implementing auto back and forth in grapple.nvim'
description = 'Possibly my favorite feature of i3wm is workspace auto back and forth and for a good reason, It comes in handy a lot'
date = "2024-08-31"
[extra]
[taxonomies]
tags = ['neovim', 'grapple.nvim', 'workflow']
categories = ['tutorial']
+++

So I have a workspace-like workflow in neovim where I tag certain files and I switched to those files using certain shortcuts. Basically what primeagen does with [harpoon](https://github.com/ThePrimeagen/harpoon) but I use [grapple.nvim](https://github.com/cbochs/grapple.nvim) instead.

I switched to grapple.nvim from harpoon for a variety of reasons:

  1. The cursor position is actually saved after quitting[^1].
  2. It has different \*\*scopes\*\*. Meaning you can make it so that the list of tagged files changes as per cwd, git branch, initial directory etc.
  3. Has a clean and extensive API(we'd discuss that later.)

I was very happy and efficient with my workflow and this whole file tagging thing but there was one issue.

Suppose I'm on tag 1, I want to have just a quick peek at 3, maybe I forgot how I defined a certain function or variable or what I called it, so I press `<leader>3` to go to tag 3, all good and well, and finally I press `<leader>1` to go back to tag 1.

This works really well no problem whatsoever but I still wish switching to tagged files had an auto-back-and-forth mechanism like from [i3wm](https://i3wm.org/).

In case you don't know what that is it basically allows you to switch to the last tag upon hitting the keybinding for the current tag:

  1. Be on tag 1
  2. Press `<leader>3`
  3. On tag 3 now
  4. Press `<leader>3`
  5. Back to tag 1 again

Grapple.nvim doesn't support this out of the box. So I thought no biggie I'll just modify the source code a bit adding an optional auto back and forth feature and create a pr at last.
But as it turns out the source code for grapple.nvim is massive and I'm not familiar with neovim's API enough to be able to achieve that.

Thankfully though grapple.nvim has a very extensive and clean API which makes our job much easier.

Here are my bindings before the modifications:

```lua
  {
    "cbochs/grapple.nvim",
    dependencies = { "nvim-tree/nvim-web-devicons" },
    cmd = "Grapple",
    opts = {
      scope = "cwd",
      icons = true,
      status = true,
    },
    keys = {
      { "<leader>a", "<cmd>Grapple toggle<cr>", desc = "grapple toggle a file" },
      { "<C-e>", "<cmd>Grapple toggle_tags<cr>", desc = "grapple toggle tags menu" },
      { "<leader>1", "<cmd>Grapple select index=1<cr>", desc = "grapple select first tag" },
      { "<leader>2", "<cmd>Grapple select index=2<cr>", desc = "grapple select second tag" },
      { "<leader>3", "<cmd>Grapple select index=3<cr>", desc = "grapple select third tag" },
      { "<leader>4", "<cmd>Grapple select index=4<cr>", desc = "grapple select fourth tag" },
      { "<leader>5", "<cmd>Grapple select index=5<cr>", desc = "grapple select fifth tag" },
      { "<leader>6", "<cmd>Grapple select index=6<cr>", desc = "grapple select sixth tag" },
      { "<leader>7", "<cmd>Grapple select index=7<cr>", desc = "grapple select seventh tag" },
      { "<leader>8", "<cmd>Grapple select index=8<cr>", desc = "grapple select eighth tag" },
      { "<leader>9", "<cmd>Grapple select index=9<cr>", desc = "grapple select ninth tag" },
    },
  },
```

And here they are now:

```lua
return {
  {
    "cbochs/grapple.nvim",
    dependencies = { "nvim-tree/nvim-web-devicons" },
    cmd = "Grapple",
    opts = {
      scope = "cwd",
      icons = true,
      status = true,
    },
    keys = {
      { "<leader>a", "<cmd>Grapple toggle<cr>", desc = "grapple toggle a file" },
      { "<C-e>", "<cmd>Grapple toggle_tags<cr>", desc = "grapple toggle tags menu" },

      {
        "<leader>1",
        function()
          if
            require("grapple").exists()
            and require("grapple").find({ index = 1 }) == require("grapple").find({ buffer = 0 })
          then
            vim.cmd.norm("")
          else
            vim.cmd("Grapple select index=1")
          end
        end,
        desc = "grapple select first tag",
      },

      {
        "<leader>2",
        function()
          if
            require("grapple").exists()
            and require("grapple").find({ index = 2 }) == require("grapple").find({ buffer = 0 })
          then
            vim.cmd.norm("")
          else
            vim.cmd("Grapple select index=2")
          end
        end,
        desc = "grapple select second tag",
      },

      {
        "<leader>3",
        function()
          if
            require("grapple").exists()
            and require("grapple").find({ index = 3 }) == require("grapple").find({ buffer = 0 })
          then
            vim.cmd.norm("")
          else
            vim.cmd("Grapple select index=3")
          end
        end,
        desc = "grapple select third tag",
      },

      {
        "<leader>4",
        function()
          if
            require("grapple").exists()
            and require("grapple").find({ index = 4 }) == require("grapple").find({ buffer = 0 })
          then
            vim.cmd.norm("")
          else
            vim.cmd("Grapple select index=4")
          end
        end,
        desc = "grapple select fourth tag",
      },

      {
        "<leader>5",
        function()
          if
            require("grapple").exists()
            and require("grapple").find({ index = 5 }) == require("grapple").find({ buffer = 0 })
          then
            vim.cmd.norm("")
          else
            vim.cmd("Grapple select index=5")
          end
        end,
        desc = "grapple select fifth tag",
      },

      {
        "<leader>6",
        function()
          if
            require("grapple").exists()
            and require("grapple").find({ index = 6 }) == require("grapple").find({ buffer = 0 })
          then
            vim.cmd.norm("")
          else
            vim.cmd("Grapple select index=6")
          end
        end,
        desc = "grapple select sixth tag",
      },

      {
        "<leader>7",
        function()
          if
            require("grapple").exists()
            and require("grapple").find({ index = 7 }) == require("grapple").find({ buffer = 0 })
          then
            vim.cmd.norm("")
          else
            vim.cmd("Grapple select index=7")
          end
        end,
        desc = "grapple select seventh tag",
      },

      {
        "<leader>8",
        function()
          if
            require("grapple").exists()
            and require("grapple").find({ index = 8 }) == require("grapple").find({ buffer = 0 })
          then
            vim.cmd.norm("")
          else
            vim.cmd("Grapple select index=8")
          end
        end,
        desc = "grapple select eighth tag",
      },

      {
        "<leader>9",
        function()
          if
            require("grapple").exists()
            and require("grapple").find({ index = 9 }) == require("grapple").find({ buffer = 0 })
          then
            vim.cmd.norm("")
          else
            vim.cmd("Grapple select index=9")
          end
        end,
        desc = "grapple select ninth tag",
      },
    },
  },
}
```

I'm basically checking whether the current buffer/file is tagged using `require("grapple").exists()` and if so I move onto the next condition, which is whether the file at tag x is equal to the current file(the zeroth buffer), using `require("grapple").find({ index = x }) == require("grapple").find({ buffer = 0 })`, and if that too is true I move to the last buffer with control+caret or `vim.cmd.norm("")`[^2]

If neither of those conditions succeed I simply move onto the desired tag.

Here's how it looks like in action:

{{video(src="./auto_back_and_forth.mp4" muted=true)}}

[^1]: There's an open [issue](https://github.com/ThePrimeagen/harpoon/issues/441)
[^2]: Unfortunately the text inside the double quotes will not render. Same with code block. If you plan on copying what I have here you have to manually press `C-q` followed by `C-^` while in insert mode to insert the appropriate value.
