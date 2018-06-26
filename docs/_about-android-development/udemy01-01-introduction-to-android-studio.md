---
title: "[Udemy01] 01. Introduction to Android Studio" 
excerpt: The complete android N developer course-Introduction
mathjax : false
tags : 
    - Udemy01
---

### 1. Install and make a new project but...
- I face a problem....even though I just installed and do nothing....
- That is
>Failed to load AppCompat ActionBar with unknown error. 
- This solution may help you. Modify style.xml from:
```
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
</style>
to:

<style name="AppTheme" parent="Base.Theme.AppCompat.Light.DarkActionBar">
</style>
```
- I got this answer from [stackoverflow](https://stackoverflow.com/a/30011016/9928325).

### 2. Another error...
>Hardcoded string "TextView", should use @string resource
- I also found an answer.. That was not an error, just warning.
[answer](https://stackoverflow.com/a/11795265/9928325)
