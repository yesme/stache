{stache} [DRAFT]
================

{stache} is yet another mustached template system.  It is named after the fact all {stache} specific expressions are quoted by {}, although it's configrable.

{stache} is inspired by [{{mustache}}](http://mustache.github.com/), [handlebar](http://handlebarsjs.com/), [dust](http://akdubya.github.com/dustjs/) (and its [LinkedIn port](https://github.com/linkedin/dustjs)), [Django](https://docs.djangoproject.com/en/1.4/topics/templates/), [doT](https://github.com/olado/doT), [Smarty](http://www.smarty.net/), [Juicer](http://juicer.name/docs/docs.html), [Velocity](http://velocity.apache.org/), [Jinja2](http://jinja.pocoo.org/docs/) and [Clearsilver](http://www.clearsilver.net/).

{stache} consists of two parts:

1. {stache} Template Language
2. Implementation for the target language

Design Principles
-----------------

- performance, performance, performance: compile template into target language; cache
- cross-language: C++, Java, Python, PHP, Javascript/Node, Javascript/Browser, Actionscript
- keep it simple and stupid: general-purpose; no i18n support; no xss escape
- powerful and pratical: no MVC fundamentalism; logic-enabled to solve common problems in an easy way;
- extendable: user defined functions
- configurable: delimiter
- thread-safety / immutable: compile once, render multiple times in parallel
- unicode supported

Some implementation details that may be considered:

- async function definition to enable possible parallel execution
- stream interface for rendering

Typical Workflows
-----------------

### In-place render:

    stache.render(tmpl, context)

### Compile once at runtime, render many times:

    tmpl_ = stache.compile(tmpl)
    tmpl_.render(context1)
    tmpl_.render(context2)

### Compile into file, load later then render:

    tmpl_ = stache.compile(tmpl)
    tmpl_.store("compiled.tmpl")

    tmpl_ = stache.load("compiled.tmpl")
    tmpl_.render(context1)
    tmpl_.render(context2)

