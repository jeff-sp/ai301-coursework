I fixed a bug in Kaoto, an open source tool for designing data integrations, and the fix was merged. It was my first contribution to a project I had never worked on.

One of the project’s automated tests failed intermittently. I found that right-clicking twice left the old menu on the page for a few milliseconds. My fix gives every right-click a fresh menu.

A maintainer approved and merged my pull request on November 11, and the issue was closed.

What I learned is to say what I can’t prove. I could not make the test fail in 21 tries, so I reported that and asked the maintainers where to start. My first fix did nothing, and my own measurements caught it before review. I also picked the issue with a small checklist tool I built to grade open issues before I commit to one.

https://github.com/KaotoIO/kaoto/pull/4306
