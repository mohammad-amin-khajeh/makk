+++
title = "fix your pictures' modified date ffs"
description = "When ADHD hits hard, you have to battle it!"
date = "2025-03-14"
[extra]
toc = true
[taxonomies]
tags = ['qol', 'script']
categories = ['blog', 'tutorial']
+++

## What exactly is the issue!?!?

{{put_image(url='./before.png' caption='wrong modified dates!' width='800')}}

Notice how the times of the creation of the pictures are literally in the filenames, yet the modification dates are wrong?(in this case, February 14th 14:28)

That's just unacceptable to me! I want my pictures to be chronologically sorted otherwise I can't enjoy my memories! Can't we just take the dates in filenames and change the modification dates accordingly?

Yes we can! And luckily for us, there's a very quick and hacky solution.

## Introducing [mtimefix.py](https://github.com/mohammad-amin-khajeh/dotfiles/blob/main/scripts/.local/bin/mtimefix.py)

### prerequisites

- coreutils
- python
- a unix shell(Meaning it probably wouldn't work on windows)

Just run [the script](https://github.com/mohammad-amin-khajeh/dotfiles/blob/main/scripts/.local/bin/mtimefix.py) in your directory of choice and voila!

{{put_image(url='./after.png' caption='hurray correct modified dates!' width='800')}}

## How does it work?

I'm sure you can just skim through it and find out for yourself as it's merely 25 lines of python.\
but all we're doing is taking all the files that end in `png`, `jpg`, and `mp4` like so:.

```python
files = set(glob("*.jpg") + glob("*.png") + glob("*.mp4"))
intab = "۱۲۳۴۵۶۷۸۹۰١٢٣٤٥٦٧٨٩٠"
outtab = "12345678901234567890"
translation_table = str.maketrans(intab, outtab)
```

{{admonition(type='tip' text='the python regex library detects arabic and persian numerals automatically and treats them as regular numbers.\
but not the `touch` command.\
so we have to "translate" them into the roman numerals like above.')}}

And then checking if they **meet the criteria**.\
That is, for each file we check whether they have the correct date in the filename or not.

We do this by running the following regex expression on each filename:

```python
for file in files:
    if dates := re.search(
        r".*(\d{4})-*(\d{2})-*(\d{2})_(\d{2})-*(\d{2})-*(\d{2})", file
    ):
```

If they do, we assign the year, month, etc to the variable `dates`

And in the body of the for loop, change the date accordingly using the `touch` command:

```python
        year    = dates.group(1).translate(translation_table)
        month   = dates.group(2).translate(translation_table)
        day     = dates.group(3).translate(translation_table)
        hour    = dates.group(4).translate(translation_table)
        minute  = dates.group(5).translate(translation_table)
        second  = dates.group(6).translate(translation_table)

        call(
            f"touch -m -d '{year}/{month}/{day} {hour}:{minute}:{second}' '{file}'",
            shell=True,
        )
```

{{admonition(type='tip' text="I've used an `fstring` to make passing the date variables easier.")}}

And that is it!

Enjoy your correct modification dates:)!
