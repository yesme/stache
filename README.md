{stache} [DRAFT]
================

{stache} is yet another mustached template system.  It is named after the fact all {stache} specific expressions are quoted by {}, although it's configrable.

{stache} is inspired by [{{mustache}}](http://mustache.github.com/), [handlebar](http://handlebarsjs.com/), [dust](http://akdubya.github.com/dustjs/) (and its [LinkedIn port](https://github.com/linkedin/dustjs)), [Django](https://docs.djangoproject.com/en/1.4/topics/templates/), [doT](https://github.com/olado/doT), [Smarty](http://www.smarty.net/), [Juicer](http://juicer.name/docs/docs.html), [Velocity](http://velocity.apache.org/), [Jinja2](http://jinja.pocoo.org/docs/) and [Clearsilver](http://www.clearsilver.net/).

{stache} consists of two parts: (1) {stache} language spec and (2) target language implementation.

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

{stache} Template Language
--------------------------

### Values
- value: 
  - {=val}
  - {=this} - refer to the whole context
  - {=val.field}: equals to val["field"]
  - {=val[expression]}: the expression can be anything. E.g.: given field="name", val[field] => val["name"] => val.name
  - {=../upper_context_val}
  - {=/absolute_context_val}
- function:
  - name as same as {$function}, resolving follow the context chain
  - func_name(param1, param2, ...)
  - nested function enabled
  - shortcut for single param function: filter (or say pipe): value|func_name, can be chained
  - no block function (see "why" section)
- expression:
  - +, -, *, /, %
  - no bit operator
  - no logic operator
  - function call (and filter)
  - (, )

### Controls
- {@keyword ...; @keyword ...}
- controls and their "end" are paired
- context: {@with expression:name}, {@end}
  - it enables: {@with func(a, b) | filter | filter : name}
  - name is optional, if expression is trival or it will not be refered by name in the future
- control flow: {@if expression-statement}, {@elseif expression-statement}, {@else}, (@end}
  - it enables: {@if val; @with val}
  - context v.s field: they are different!
- loop (TBD):
  - integer loop: {@for i [expression : expression]}, {@end}
  - array: {@for i [0 : val|length]; @with val[i] : elem}, {@end; @end}
  - object: {@for key val; @with val.key : elem}, {@end; @end}
  - array and object needs to be distinguished as strong typing language...

### Code Reuse
- inclusion: {>"path/to/{=whatever}/abc.html” expression} - only value is allowed in the inclusion
- no block (see "why" section)
- no extension (see "why" section)
- how to locate the specific template is language/platform/scenario dependent

### Comments
- {# ...}
