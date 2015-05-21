title: Using Environment Variables In SQL*Plus/SQLcl
tags:
- Oracle
- SQL*Plus
- SQLcl
---

In response to Blaine Carter

{% blockquote @OraBlaineOS https://twitter.com/orablaineos/status/601144702692237312 %}
Have you tried dbms_system.get_env()?
{% endblockquote %}

I didn't before. Thanks for that! Now I did and still it doesn't solve my problem as it 

* needs connection to server
* has access only to server environment variables

````
$ export foo=bar
$ sql scott/tiger@some_remote_host:1521:orcl

SQL> var x varchar2(255);
SQL> exec dbms_system.get_env('foo', :x);

PL/SQL procedure successfully completed.

SQL> print x

X
------

````

**What I've meant and didn't clearly explain is client side functionality and that's SQL\*Plus/SQLcl for me.**

What I need, and I think can be useful for many people, is something like this:

````
$ export foo=bar
$ sql /nolog

SQL> prompt &&__foo
bar
````

I don't know how it can be done in SQL\*Plus/SQLcl. Well I do, but it's

* ugly 
* not applicable for many security reasons
* isn't OS independent
* it may break some legacy SQL\*Plus code

````
$ export foo=bar
$ sql /nolog

SQL> host echo "define __foo = "$foo"" > .tmp
SQL> @.tmp
SQL> prompt &&__foo
bar

````

So the fallback is to use some scripting language like Python, Perl, ... Which brings some issues

* security might not approve it
* developers need to learn some other language
* code get's overcomplicated
* environment needed to run scripts is getting more complicated

@krisrice: It would be great feature new of SQLcl.
