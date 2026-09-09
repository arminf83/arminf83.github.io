+++
title = "Git واقعاً چطور کار می‌کنه؟ Snapshot، Delta و Plumbing"

[extra]
technologies = ["Git", "Version Control", "Internals"]
+++

<div dir="rtl" style="text-align: right;">

به نظرتون Git وقتی یه فایل رو تغییر می‌دیم، فقط تغییرات همون فایل رو ذخیره می‌کنه؟ یا هر بار از کل پروژه یه Snapshot می‌گیره؟

جواب شاید کمی خلاف چیزیه که اکثرمون فکر می‌کنیم: **Git در مدل مفهومی خودش با Snapshot کار می‌کنه، نه Delta.**

## پس چرا حجم Repository منفجر نمی‌شه؟

اگر Git قراره در هر Commit یه Snapshot کامل از پروژه بگیره، چرا حجم Repository بعد از چند صد یا چند هزار Commit منفجر نمی‌شه؟

اینجا می‌رسیم به یکی از قسمت‌های واقعاً هوشمندانه‌ی معماری Git — معماری‌ای که لینوس توروالدز پایه‌گذاری کرد.

Git فایل‌ها رو مثل یه Backup معمولی کپی نمی‌کنه. محتوای فایل‌ها به شکل **Object** ذخیره می‌شه و هر Object با Hash خودش شناخته می‌شه. پس اگه یه فایل تغییر نکرده باشه، Git لازم نیست دوباره همون محتوا رو ذخیره کنه — از همون Object قبلی استفاده می‌کنه.

جالب‌تر اینکه در لایه‌ی ذخیره‌سازی و Packfileها، Git می‌تونه برای فشرده‌سازی از Delta Compression هم استفاده کنه. یعنی:

- مدل مفهومی Git → **Snapshot**
- نحوه‌ی فشرده‌سازی داخلی → می‌تونه **Delta-based** هم باشه

## Porcelain در برابر Plumbing

ما معمولاً Git رو از طریق دستورات سطح بالا می‌شناسیم:
git add
git commit
git push
git pull
git branch

این‌ها بخش **Porcelain** هستن — همون رابط خوش‌دست و کاربرپسند که برای کار روزمره طراحی شده. بالای این لایه حتی GUIها، اکستنشن‌های VS Code و در نهایت سرویس‌هایی مثل GitHub قرار می‌گیرن.

اما زیر Porcelain، Git یه سری دستور low-level داره:
git hash-object
git cat-file
git write-tree
git commit-tree

اینجا دیگه به‌جای رابط‌های آماده، مستقیماً با Objectها و ساختار داخلی Git کار می‌کنیم. با همین primitiveها می‌شه کارهای جالبی انجام داد؛ مثلاً حتی یه Commit ساخت بدون استفاده از `git commit`.

به نظرم این بخش از Git واقعاً underrated ـه. چون وقتی با Plumbing کار می‌کنی، دیگه فقط از Git استفاده نمی‌کنی — شروع می‌کنی به فهمیدن اینکه Git واقعاً چطور ساخته شده.
</div>
<img src="/img/blog/git-internals/1.png" alt="اسکرین‌شات ۱" style="max-width: 100%; width: 300px; border-radius: 8px;">

