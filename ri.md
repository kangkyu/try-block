Doc Rdoc ..
==

```
ri Array#each
```

documentations!

https://www.omniref.com/ruby/gems/httparty/0.13.1/symbols/HTTParty::ClassMethods/default_params

http://ruby-doc.org/stdlib-2.0.0/libdoc/minitest/rdoc/MiniTest/Assertions.html#method-i-assert_equal

http://www.rubydoc.info/gems/minitest/5.8.3/Minitest/Assertions#assert_equal-instance_method

ruby-doc.org
rubydoc.info

By the way, I found ri command useful http://www.jstorimer.com/blogs/workingwithcode/7766081-5-reasons-you-should-use-ri-to-read-ruby-documentation

all std-lib or core-ruby are already there, and for any gem use gem rdoc

You can't `ri rename` output out of Googling, for example.


Everyone has setup such as:
```
~/.gemrc
install: --no-document
update: --no-document
gem: --no-document

gem: --no-ri --no-rdoc
```

-​-[no-]document [TYPES] - Generate documentation for installed gems List the documentation types you wish to generate. For example: rdoc,ri

So the right setup would be:

```
--document ri
--no-document rdoc
```

How to get ri documentations

```
gem rdoc minitest --ri
ri Minitest::Assertions.assert_equal
```

If don't want it any more (or only temporarily)

```
gem rdoc activesupport --no-ri --overwrite
```
