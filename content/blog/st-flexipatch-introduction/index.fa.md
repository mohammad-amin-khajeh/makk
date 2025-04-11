+++
title = "آموزش شروع با ترمینال اس تی"
description = "چیزی که توی کامیونیتی لینوکس مثل مور و ملخ زیاده ترمیناله ولی اس تی همه رو ده هیچ میزنه"
date = "2024-10-15"
[extra]
[taxonomies]
tags = ["ترمینال", "ساکلس", "مینیمالیسم"]
categories = ["آموزشی"]
+++

## اس تی؟ اسمشم نشنیدم

اس تی یکی از اپلیکیشن های گروه ساکلس([suckless](suckless.org)) هست و مثل بقیه اپلیکیشن های این گروه فوق العاده مینیمال و ساده و سبک هستش.

توی [ویدیوی جدیدم](https://www.youtube.com/watch?v=gnJdoq9G6jw) در مورد اس تی صحبت کردم و اینکه چطوری با [اس تی-فلکسی پچ](https://github.com/bakkeby/st-flexipatch) میتونید به قوی ترین و در عین حال سریع ترین ترمینال ممکن تبدیلش کنید.

مشکل اس تی خام اینه که بیش از حد مینیمال و سادس، طوری که حتی قابلیت اسکرول کردن به بالا رو هم نداره! برای اضافه کردن قابلیت های مختلف هم باید پَچ بهش اضافه کنید(که شامل ادیت سورس کد و کمپایل دوباره میشه.)

معمولا هم بعد از اضافه کردن دو سه تا پچ، پچ های بعدی با سورس کد جدید تداخل پیدا میکنن و باید برید لاین هارو دستی به سورس کد اضافه کنید که یکم زمان بر هست. اما با اس تی-فلکسی پچ هیچ کدوم از این دردسرا وجود ندارن.

ساختار اس تی-فلکسی پچ اینطوریه که شما میتونید پچ ها رو توی یک فایل آف و آن کنید و توی یک فایل دیگه مقادیرشون رو تغییر بدید مثل فایل های کانفیگ معمولی. تقریبا هیچ نیازی به ادیت سورس کد نیست.

## چطور اس تی-فلکسی پچ رو نصب کنم؟

سورس کد رو بگیر و کمپایلش کن. شاید ترسناک بنظر بیاد ولی خیلی سادس.

```sh
git clone https://github.com/bakkeby/st-flexipatch
cd st-flexipatch
sudo make install
```

همین:) حالا میتونید با دستور st اجراش کنید.

## بعدش چکار کنم؟

بعد از کمپایل کردن دوتا فایل جدید به پوشه st-flexipatch تون اضافه میشه. یکی `patches.h` و اون یکی `config.h`

کاری به بقیه فایلا نداشته باشید، اکثر ادیتامون قراره توی همین دو فایل باشن.[^1]

مثلا بیاین قابلیت transparency یا شفافیت رو به اس تی اضافه کنیم.

برای این کار فایل `patches.h` رو باز میکنیم و بخش زیری رو پیدا میکنیم:

```c, name=st-flexipatch/patches.h
/* The alpha patch adds transparency for the terminal.
 * You need to uncomment the corresponding line in config.mk to use the -lXrender library
 * when including this patch.
 * https://st.suckless.org/patches/alpha/
 */
#define ALPHA_PATCH 0
```

عدد 0 رو به 1 تغییر بدید تا پچ فعال شه:

```c, name=st-flexipatch/patches.h
/* The alpha patch adds transparency for the terminal.
 * You need to uncomment the corresponding line in config.mk to use the -lXrender library
 * when including this patch.
 * https://st.suckless.org/patches/alpha/
 */
#define ALPHA_PATCH 1
```

بعد از اون به فایل `config.mk` برید و خط زیر رو پیدا کنید:

```make, name=st-flexipatch/config.mk
# Uncomment this for the alpha patch / ALPHA_PATCH
#XRENDER = `$(PKG_CONFIG) --libs xrender`
```

و خط دوم رو آنکامنت کنید:

```make, name=st-flexipatch/config.mk
# Uncomment this for the alpha patch / ALPHA_PATCH
XRENDER = `$(PKG_CONFIG) --libs xrender`
```

به پوشه ی `st-flexipatch` تون برید و یه `sudo make clean install` بزنید و تامام.

حالا ترمینالتون transparency داره:)! [^2]

{{ image_toggler(default_src="./before.png", toggled_src="./after.png", default_alt="St before alpha patch", toggled_alt="St after alpha patch", raw_path = true ) }}

## نکته

میتونید توی فایل `config.h` بخش زیر رو پیدا کنید و تغییرش بدید تا مقدار transparency تغییر کنه:

```c, name=st-flexipatch/patches.h
#if ALPHA_PATCH
/* bg opacity */
float alpha = 0.8;
...
#endif // ALPHA_PATCH
```

به طور پیشفرض 0.8 هست. میتونید کمترش کنید تا مقدار transparency افزایش پیدا کنه و به قولی شفاف تر شه[^2]:

```c, name=st-flexipatch/patches.h
#if ALPHA_PATCH
/* bg opacity */
float alpha = 0.3;
...
#endif // ALPHA_PATCH
```

{{ image_toggler(default_src="./0.3-transparency.png", toggled_src="./after.png", default_alt="St at 0.3 alpha", toggled_alt="St at 0.8 alpha", raw_path = true ) }}

[^1]: یه فایل `config.mk` هم هست که بعضی پچ ها برای فعال شدن نیاز به ادیتش دارن.
[^2]: روی عکس کلیک کنید تا قبل و بعد رو ببینید.
