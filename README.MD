# Rob Bikmansurov. Personal blog 

Testing and learning.

[![pipeline status](https://gitlab.com/RobBikmansurov/robbikmansurov.github.io.jekyll/badges/master/pipeline.svg)](https://gitlab.com/RobBikmansurov/bp1step.jekyll/commits/master)


This awesome template sent me from https://pawelurbanek.com/jekyll-seo-template

To build the template run the following commands (requires Ruby 2.6.3 installed):


```
gem install bundler
bundle install
jekyll serve --watch
```

You should see the blog on `http://localhost:4000`

If something goes wrong you should consult jekyll documentation on https://jekyllrb.com/.

##Update site

```bash
jekyll hyde
jekyll build
```

Подключить услугу ssh на хостинге https://cp-hosting.jino.ru/services/#ssh за 0.84 руб. в сутки

`rsync -avz -e ssh _site/ 9194949874@9194949874.myjino.ru:/home/users/9/9194949874/domains/bikmansurov.ru`

Отключить услугу ssh на хостинге

##Hosting
Reminder! I have hostings at:

https://cp-hosting.jino.ru/ for domain bikmansurov.ru
https://my.radiushost.ru/billmgr?sfrom=loginform for domain bp1step.ru
