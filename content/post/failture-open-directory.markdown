---

title: "Macで open . が失敗する"
date: 2014-10-28T14:27:00+09:00
comments: true
categories: ["tmux","mac"]
---

`open .`をしようとしたら `LSOpenURLsWithRole() failed`が発生した。

```
$ open .
LSOpenURLsWithRole() failed with error -600 for the file
```

tmux のペインを全部消して開きなおしたら治った。

### 参考文献

* [osx - open Safari and Finder failed in tmux - Super User](http://superuser.com/questions/746606/open-safari-and-finder-failed-in-tmux)
