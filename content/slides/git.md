+++
Categories = []
Tags = []
author = "Cong Nguyen"
authorfacebook = "http://facebook.com/ntcong.it"
authorgoogleplus = "https://plus.google.com/+CongNguyen"
authorlink = "http://ntcong.it"
date = "2017-01-17T14:26:06+07:00"
thumbnail = "/img/avatar.png"
title = "git"

+++

class: animated center middle fadeIn 

![test](/img/FPT_logo.png)

---

class: animated left middle fadeIn inverse

<div class='gdbar'><img src="/img/FPT_logo.png"></div>

# Git
## Introduction and use cases
@ntcong

---

class: animated fadeIn 

# Agenda

1. Introduction
2. Deep-dive
3. ...

---

background-image: url("https://git-scm.com/book/en/v2/book/10-git-internals/images/data-model-4.png")

# Introduction

---

# Code Test

```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    app.run()
```
```bash
export FLASK_APP=app.py
flask runserver
flask shell
```

