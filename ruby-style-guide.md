# The Ruby Style Guide

I've plucked a few choice sections from the [Ruby Style Guide](https://github.com/rubocop-hq/ruby-style-guide), I recommend giving it a read, there's lots of great information.

Hopefully we can use this is a base the we can all add to, and edited, depending on our preferences.  

## Table of Contents

  * [Source Code Layout](#source-code-layout)
  * [Syntax](#syntax)
  * [Naming](#naming)
  * [Comments](#comments)
  * [Classes & Modules](#classes--modules)
  * [Strings](#strings)
  * [Date & Time](#date--time)
  * [Misc](#misc)

## Source Code Layout

  * <a name="spaces-indentation"></a>
    Use two **spaces** per indentation level (aka soft tabs). No hard tabs.

    ```ruby
    # bad - four spaces
    def some_method
        do_something
    end

    # good
    def some_method
      do_something
    end
    ```

  * No spaces after `(`, `[` or before `]`, `)`.
    Use spaces around `{` and before `}`.

    ```ruby
    # bad
    some( arg ).other
    [ 1, 2, 3 ].each{|e| puts e}

    # good
    some(arg).other
    [1, 2, 3].each { |e| puts e }
    ```

  * Use spaces around the `=` operator when assigning default values to method
    parameters:

    ```ruby
    # bad
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end

    # good
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end
    ```

  * Align the parameters of a method call if they span more than one
    line. When aligning parameters is not appropriate due to line-length
    constraints, single indent for the lines after the first is also
    acceptable.

    ```ruby
    # starting point (line is too long)
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
    end

    # bad (double indent)
    def send_mail(source)
      Mailer.deliver(
          to: 'bob@example.com',
          from: 'us@example.com',
          subject: 'Important message',
          body: source.text)
    end

    # good
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com',
                     from: 'us@example.com',
                     subject: 'Important message',
                     body: source.text)
    end

    # good (normal indent)
    def send_mail(source)
      Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text
      )
    end
    ```

  * Align the elements of array literals spanning multiple lines.

    ```ruby
    # bad - single indent
    menu_item = %w[Spam Spam Spam Spam Spam Spam Spam Spam
      Baked beans Spam Spam Spam Spam Spam]

    # good
    menu_item = %w[
      Spam Spam Spam Spam Spam Spam Spam Spam
      Baked beans Spam Spam Spam Spam Spam
    ]

    # good
    menu_item =
      %w[Spam Spam Spam Spam Spam Spam Spam Spam
         Baked beans Spam Spam Spam Spam Spam]
    ```

## Syntax

  * Use `def` with parentheses when there are parameters. Omit the
    parentheses when the method doesn't accept any parameters.

    ```ruby
    # bad
    def some_method()
      # body omitted
    end

    # good
    def some_method
      # body omitted
    end

    # bad
    def some_method_with_parameters param1, param2
      # body omitted
    end

    # good
    def some_method_with_parameters(param1, param2)
      # body omitted
    end
    ```

  * Use parentheses around the arguments of method invocations,
    especially if the first argument begins with an open parenthesis `(`,
    as in `f((3 + 2) + 1)`.

    ```ruby
    # bad
    x = Math.sin y
    # good
    x = Math.sin(y)

    # bad
    array.delete e
    # good
    array.delete(e)

    # bad
    temperance = Person.new 'Temperance', 30
    # good
    temperance = Person.new('Temperance', 30)
    ```

    Always omit parentheses for

      * Method calls with no arguments:

        ```ruby
        # bad
        Kernel.exit!()
        2.even?()
        fork()
        'test'.upcase()

        # good
        Kernel.exit!
        2.even?
        fork
        'test'.upcase
        ```

      * Methods that have "keyword" status in Ruby:

        ```ruby
        class Person
          # bad
          attr_reader(:name, :age)
          # good
          attr_reader :name, :age

          # body omitted
        end
        ```

  * Define optional arguments at the end of the list of arguments.
    Ruby has some unexpected results when calling methods that have
    optional arguments at the front of the list.

    ```ruby
    # bad
    def some_method(a = 1, b = 2, c, d)
      puts "#{a}, #{b}, #{c}, #{d}"
    end

    some_method('w', 'x') # => '1, 2, w, x'
    some_method('w', 'x', 'y') # => 'w, 2, x, y'
    some_method('w', 'x', 'y', 'z') # => 'w, x, y, z'

    # good
    def some_method(c, d, a = 1, b = 2)
      puts "#{a}, #{b}, #{c}, #{d}"
    end

    some_method('w', 'x') # => '1, 2, w, x'
    some_method('w', 'x', 'y') # => 'y, 2, w, x'
    some_method('w', 'x', 'y', 'z') # => 'y, z, w, x'
    ```

  * Use keyword arguments when passing boolean argument to a method.

    ```Ruby
    # bad
    def some_method(bar = false)
      puts bar
    end

    # bad - common hack before keyword args were introduced
    def some_method(options = {})
      bar = options.fetch(:bar, false)
      puts bar
    end

    # good
    def some_method(bar: false)
      puts bar
    end

    some_method            # => false
    some_method(bar: true) # => true
    ```

  * Prefer keyword arguments over optional arguments.

    ```Ruby
    # bad
    def some_method(a, b = 5, c = 1)
      # body omitted
    end

    # good
    def some_method(a, b: 5, c: 1)
      # body omitted
    end
    ```

  * Use keyword arguments instead of option hashes.

    ```Ruby
    # bad
    def some_method(options = {})
      bar = options.fetch(:bar, false)
      puts bar
    end

    # good
    def some_method(bar: false)
      puts bar
    end
    ```

  * Favor modifier `if`/`unless` usage when you have a single-line body. Another
    good alternative is the usage of control flow `&&`/`||`.

    ```ruby
    # bad
    if some_condition
      do_something
    end

    # good
    do_something if some_condition

    # another good option
    some_condition && do_something
    ```

  * Favor `unless` over `if` for negative conditions (or control flow `||`).

    ```ruby
    # bad
    do_something if !some_condition

    # bad
    do_something if not some_condition

    # good
    do_something unless some_condition

    # another good option
    some_condition || do_something
    ```

  * Omit the outer braces around an implicit options hash.

    ```ruby
    # bad
    user.set({ name: 'John', age: 45, permissions: { read: true } })

    # good
    user.set(name: 'John', age: 45, permissions: { read: true })
    ```

  * Use the proc invocation shorthand when the invoked method is the only operation of a block.

    ```ruby
    # bad
    names.map { |name| name.upcase }

    # good
    names.map(&:upcase)
    ```

  * Prefer `{...}` over `do...end` for single-line blocks.  Avoid using `{...}`
    for multi-line blocks (multi-line chaining is always ugly). Always use
    `do...end` for "control flow" and "method definitions" (e.g. in Rakefiles and
    certain DSLs).  Avoid `do...end` when chaining.

    ```ruby
    names = %w[Bozhidar Steve Sarah]

    # bad
    names.each do |name|
      puts name
    end

    # good
    names.each { |name| puts name }

    # bad
    names.select do |name|
      name.start_with?('S')
    end.map { |name| name.upcase }

    # good
    names.select { |name| name.start_with?('S') }.map(&:upcase)
    ```

    Some will argue that multi-line chaining would look OK with the use of {...},
    but they should ask themselves&mdash;is this code really readable and can the
    blocks' contents be extracted into nifty methods?

  * Consider using explicit block argument to avoid writing block literal that
    just passes its arguments to another block. Beware of the performance impact,
    though, as the block gets converted to a Proc.

    ```ruby
    require 'tempfile'

    # bad
    def with_tmp_dir
      Dir.mktmpdir do |tmp_dir|
        Dir.chdir(tmp_dir) { |dir| yield dir }  # block just passes arguments
      end
    end

    # good
    def with_tmp_dir(&block)
      Dir.mktmpdir do |tmp_dir|
        Dir.chdir(tmp_dir, &block)
      end
    end

    with_tmp_dir do |dir|
      puts "dir is accessible as a parameter and pwd is set: #{dir}"
    end
    ```

  * Avoid `return` where not required for flow of control.

    ```ruby
    # bad
    def some_method(some_arr)
      return some_arr.size
    end

    # good
    def some_method(some_arr)
      some_arr.size
    end
    ```

  * Do not use `eql?` when using `==` will do. The stricter comparison semantics
    provided by `eql?` are rarely needed in practice.

    ```ruby
    # bad - eql? is the same as == for strings
    'ruby'.eql? some_str

    # good
    'ruby' == some_str
    1.0.eql? x # eql? makes sense here if want to differentiate between Integer and Float 1
    ```

  * Don't do explicit non-`nil` checks unless you're dealing with boolean
    values.

    ```ruby
    # bad
    do_something if !something.nil?
    do_something if something != nil

    # good
    do_something if something

    # good - dealing with a boolean
    def value_set?
      !@some_boolean.nil?
    end
    ```

  * Prefer `map` over `collect`, `find` over `detect`, `select` over `find_all`,
    `reduce` over `inject` and `size` over `length`. This is not a hard
    requirement; if the use of the alias enhances readability, it's ok to use it.
    The rhyming methods are inherited from Smalltalk and are not common in other
    programming languages. The reason the use of `select` is encouraged over
    `find_all` is that it goes together nicely with `reject` and its name is
    pretty self-explanatory.

  * Don't use `count` as a substitute for `size`. For `Enumerable` objects other
    than `Array` it will iterate the entire collection in order to determine its
    size.

    ```ruby
    # bad
    some_hash.count

    # good
    some_hash.size
    ```

## Naming

  * Do not separate numbers from letters on symbols, methods and variables.

    ```ruby
    # bad
    :some_sym_1

    some_var_1 = 1

    var_10  = 10

    def some_method_1
      # some code
    end

    # good
    :some_sym1

    some_var1 = 1

    var10    = 10

    def some_method1
      # some code
    end
    ```

  * Avoid prefixing predicate methods with the auxiliary verbs such as `is`,
    `does`, or `can`.  These words are redundant and inconsistent with the style of
    boolean methods in the Ruby core library, such as `empty?` and `include?`.

    ```ruby
    # bad
    class Person
      def is_tall?
        true
      end

      def can_play_basketball?
        false
      end

      def does_like_candy?
        true
      end
    end

    # good
    class Person
      def tall?
        true
      end

      def basketball_player?
        false
      end

      def likes_candy?
        true
      end
    end
    ```

## Comments

> Good code is its own best documentation. As you're about to add a
> comment, ask yourself, "How can I improve the code so that this
> comment isn't needed?" Improve the code and then document it to make
> it even clearer. <br>
> -- Steve McConnell

  * Avoid writing comments to explain bad code. Refactor the code to make it
    self-explanatory. ("Do or do not&mdash;there is no try." Yoda)

  * Annotations should usually be written on the line immediately above the
    relevant code.

  * The annotation keyword is followed by a colon and a space, then a note
    describing the problem.

  * If multiple lines are required to describe the problem, subsequent lines
    should be indented three spaces after the `#` (one general plus two for
    indentation purpose).

    ```ruby
    def bar
      # FIXME: This has crashed occasionally since v3.2.1. It may
      #   be related to the BarBazUtil upgrade.
      baz(:quux)
    end
    ```

  * In cases where the problem is so obvious that any documentation would be
    redundant, annotations may be left at the end of the offending line with no
    note. This usage should be the exception and not the rule.

    ```ruby
    def bar
      sleep 100 # OPTIMIZE
    end
    ```

  * Use `TODO` to note missing features or functionality that should be added at
    a later date.

  * Use `FIXME` to note broken code that needs to be fixed.

  * Use `OPTIMIZE` to note slow or inefficient code that may cause performance
    problems.

  * Use `HACK` to note code smells where questionable coding practices were used
    and should be refactored away.

  * Use `REVIEW` to note anything that should be looked at to confirm it is
    working as intended. For example: `REVIEW: Are we sure this is how the client
    does X currently?`

  * Use other custom annotation keywords if it feels appropriate, but be sure to
    document them in your project's `README` or similar.(Here!)

## Classes & Modules

  * Use a consistent structure in your class definitions.

    ```ruby
    class Person
      # extend and include go first
      extend SomeModule
      include AnotherModule

      # inner classes
      CustomError = Class.new(StandardError)

      # constants are next
      SOME_CONSTANT = 20

      # afterwards we have attribute macros
      attr_reader :name

      # followed by other macros (if any)
      validates :name

      # public class methods are next in line
      def self.some_method
      end

      # initialization goes between class methods and other instance methods
      def initialize
      end

      # followed by other public instance methods
      def some_method
      end

      # protected and private methods are grouped near the end
      protected

      def some_protected_method
      end

      private

      def some_private_method
      end
    end
    ```

  * Split multiple mixins into separate statements.

    ```ruby
    # bad
    class Person
      include Foo, Bar
    end

    # good
    class Person
      # multiple mixins go in separate statements
      include Foo
      include Bar
    end
    ```

  * Don't nest multi-line classes within classes. Try to have such nested
    classes each in their own file in a folder named like the containing class.

    ```ruby
    # bad

    # foo.rb
    class Foo
      class Bar
        # 30 methods inside
      end

      class Car
        # 20 methods inside
      end

      # 30 methods inside
    end

    # good

    # foo.rb
    class Foo
      # 30 methods inside
    end

    # foo/bar.rb
    class Foo
      class Bar
        # 30 methods inside
      end
    end

    # foo/car.rb
    class Foo
      class Car
        # 20 methods inside
      end
    end
    ```

  * Prefer modules to classes with only class methods. Classes should be used
    only when it makes sense to create instances out of them.

    ```ruby
    # bad
    class SomeClass
      def self.some_method
        # body omitted
      end

      def self.some_other_method
        # body omitted
      end
    end

    # good
    module SomeModule
      module_function

      def some_method
        # body omitted
      end

      def some_other_method
        # body omitted
      end
    end
    ```

  * Use the `attr` family of functions to define trivial accessors or mutators.

    ```ruby
    # bad
    class Person
      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def first_name
        @first_name
      end

      def last_name
        @last_name
      end
    end

    # good
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end
    ```

  * For accessors and mutators, avoid prefixing method names with
    `get_` and `set_`.
    It is a Ruby convention to use attribute names for accessors (readers) and
    `attr_name=` for mutators (writers).

    ```ruby
    # bad
    class Person
      def get_name
        "#{@first_name} #{@last_name}"
      end

      def set_name(name)
        @first_name, @last_name = name.split(' ')
      end
    end

    # good
    class Person
      def name
        "#{@first_name} #{@last_name}"
      end

      def name=(name)
        @first_name, @last_name = name.split(' ')
      end
    end
    ```

  * Consider using `Struct.new`, which defines the trivial accessors,
    constructor and comparison operators for you.

    ```ruby
    # good
    class Person
      attr_accessor :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end

    # better
    Person = Struct.new(:first_name, :last_name) do
    end
    ```

  * Assign proper visibility levels to methods (`private`, `protected`) in
    accordance with their intended usage. Don't go off leaving everything `public`
    (which is the default). After all we're coding in *Ruby* now, not in *Python*.

  * Indent the `public`, `protected`, and `private` methods as much as the method
    definitions they apply to. Leave one blank line above the visibility modifier
    and one blank line below in order to emphasize that it applies to all methods
    below it.

    ```ruby
    class SomeClass
      def public_method
        # some code
      end

      private

      def private_method
        # some code
      end

      def another_private_method
        # some code
      end
    end
    ```

  * Use `def self.method` to define class methods. This makes the code
    easier to refactor since the class name is not repeated.

    ```ruby
    class TestClass
      # bad
      def TestClass.some_method
        # body omitted
      end

      # good
      def self.some_other_method
        # body omitted
      end

      # Also possible and convenient when you
      # have to define many class methods.
      class << self
        def first_method
          # body omitted
        end

        def second_method_etc
          # body omitted
        end
      end
    end
    ```

### Constructors

  * Consider adding factory methods to provide additional sensible ways to
    create instances of a particular class.

    ```ruby
    class Person
      def self.create(options_hash)
        # body omitted
      end
    end
    ```

## Collections

  * Prefer literal array and hash creation notation (unless you need to pass
    parameters to their constructors, that is).

    ```ruby
    # bad
    arr = Array.new
    hash = Hash.new

    # good
    arr = []
    arr = Array.new(10)
    hash = {}
    hash = Hash.new(0)
    ```

  * Prefer `%w` to the literal array syntax when you need an array of words
    (non-empty strings without spaces and special characters in them).  Apply this
    rule only to arrays with two or more elements.

    ```ruby
    # bad
    STATES = ['draft', 'open', 'closed']

    # good
    STATES = %w[draft open closed]
    ```

  * Prefer `%i` to the literal array syntax when you need an array of symbols
    (and you don't need to maintain Ruby 1.9 compatibility). Apply this rule only
    to arrays with two or more elements.

    ```ruby
    # bad
    STATES = [:draft, :open, :closed]

    # good
    STATES = %i[draft open closed]
    ```

  * Avoid comma after the last item of an `Array` or `Hash` literal, especially
    when the items are not on separate lines.

    ```ruby
    # bad - easier to move/add/remove items, but still not preferred
    VALUES = [
               1001,
               2020,
               3333,
             ]

    # bad
    VALUES = [1001, 2020, 3333, ]

    # good
    VALUES = [1001, 2020, 3333]
    ```

  * When accessing the first or last element from an array, prefer `first` or
    `last` over `[0]` or `[-1]`.

  * Use `Set` instead of `Array` when dealing with unique elements. `Set`
    implements a collection of unordered values with no duplicates. This is a
    hybrid of `Array`'s intuitive inter-operation facilities and `Hash`'s fast
    lookup.

  * Prefer symbols instead of strings as hash keys.

    ```ruby
    # bad
    hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

  * Avoid the use of mutable objects as hash keys.

  * Use the Ruby 1.9 hash literal syntax when your hash keys are symbols.

    ```ruby
    # bad
    hash = { :one => 1, :two => 2, :three => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

  * Don't mix the Ruby 1.9 hash syntax with hash rockets in the same hash
    literal. When you've got keys that are not symbols stick to the hash rockets
    syntax.

    ```ruby
    # bad
    { a: 1, 'b' => 2 }

    # good
    { :a => 1, 'b' => 2 }
    ```

  * Use `Hash#key?` instead of `Hash#has_key?` and `Hash#value?` instead of
    `Hash#has_value?`.

    ```ruby
    # bad
    hash.has_key?(:test)
    hash.has_value?(value)

    # good
    hash.key?(:test)
    hash.value?(value)
    ```

  * Use `Hash#each_key` instead of `Hash#keys.each` and `Hash#each_value`
    instead of `Hash#values.each`.

    ```ruby
    # bad
    hash.keys.each { |k| p k }
    hash.values.each { |v| p v }
    hash.each { |k, _v| p k }
    hash.each { |_k, v| p v }

    # good
    hash.each_key { |k| p k }
    hash.each_value { |v| p v }
    ```

  * Use `Hash#fetch` when dealing with hash keys that should be present.

    ```ruby
    heroes = { batman: 'Bruce Wayne', superman: 'Clark Kent' }
    # bad - if we make a mistake we might not spot it right away
    heroes[:batman] # => 'Bruce Wayne'
    heroes[:supermann] # => nil

    # good - fetch raises a KeyError making the problem obvious
    heroes.fetch(:supermann)
    ```

  * Introduce default values for hash keys via `Hash#fetch` as opposed to using
    custom logic.

    ```ruby
    batman = { name: 'Bruce Wayne', is_evil: false }

    # bad - if we just use || operator with falsy value we won't get the expected result
    batman[:is_evil] || true # => true

    # good - fetch works correctly with falsy values
    batman.fetch(:is_evil, true) # => false
    ```

  * Prefer the use of the block instead of the default value in `Hash#fetch`
    if the code that has to be evaluated may have side effects or be expensive.

    ```ruby
    batman = { name: 'Bruce Wayne' }

    # bad - if we use the default value, we eager evaluate it
    # so it can slow the program down if done multiple times
    batman.fetch(:powers, obtain_batman_powers) # obtain_batman_powers is an expensive call

    # good - blocks are lazy evaluated, so only triggered in case of KeyError exception
    batman.fetch(:powers) { obtain_batman_powers }
    ```

  * Use `Hash#values_at` when you need to retrieve several values consecutively
    from a hash.
    

    ```ruby
    # bad
    email = data['email']
    username = data['nickname']

    # good
    email, username = data.values_at('email', 'nickname')
    ```

  * When providing an accessor for a collection, provide an alternate form
    to save users from checking for `nil` before accessing an element in
    the collection.
    
    ```ruby
    # bad
    def awesome_things
      @awesome_things
    end

    # good
    def awesome_things(index = nil)
      if index && @awesome_things
        @awesome_things[index]
      else
        @awesome_things
      end
    end
    ```

## Strings

  * Prefer string interpolation and string formatting instead of string
    concatenation:
    
    ```ruby
    # bad
    email_with_name = user.name + ' <' + user.email + '>'

    # good
    email_with_name = "#{user.name} <#{user.email}>"

    # good
    email_with_name = format('%s <%s>', user.name, user.email)
    ```

  * Adopt a consistent string literal quoting style. There are two popular
    styles in the Ruby community, both of which are considered good&mdash;single
    quotes by default (Option A) and double quotes by default (Option B).
    
      * **(Option A)** Prefer single-quoted strings when you don't need
        string interpolation or special symbols such as `\t`, `\n`, `'`,
        etc.

        ```ruby
        # bad
        name = "Bozhidar"

        name = 'De\'Andre'

        # good
        name = 'Bozhidar'

        name = "De'Andre"
        ```

      * **(Option B)** Prefer double-quotes unless your string literal
        contains `"` or escape characters you want to suppress.

        ```ruby
        # bad
        name = 'Bozhidar'

        sarcasm = "I \"like\" it."

        # good
        name = "Bozhidar"

        sarcasm = 'I "like" it.'
        ```

    The string literals in this guide are aligned with the first style.

  * Avoid using `String#+` when you need to construct large data chunks.
    Instead, use `String#<<`. Concatenation mutates the string instance in-place
    and is always faster than `String#+`, which creates a bunch of new string
    objects.
    
    ```ruby
    # bad
    html = ''
    html += '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html += "<p>#{paragraph}</p>"
    end

    # good and also fast
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

  * Don't use `String#gsub` in scenarios in which you can use a faster and more specialized alternative.

    ```ruby
    url = 'http://example.com'
    str = 'lisp-case-rules'

    # bad
    url.gsub('http://', 'https://')
    str.gsub('-', '_')

    # good
    url.sub('http://', 'https://')
    str.tr('-', '_')
    ```

## Date & Time

  * Prefer `Time.now` over `Time.new` when retrieving the current system time.

  * Don't use `DateTime` unless you need to account for historical calendar
    reform—and if you do, explicitly specify the `start` argument to
    clearly state your intentions.

    ```ruby
    # bad - uses DateTime for current time
    DateTime.now

    # good - uses Time for current time
    Time.now

    # bad - uses DateTime for modern date
    DateTime.iso8601('2016-06-29')

    # good - uses Date for modern date
    Date.iso8601('2016-06-29')

    # good - uses DateTime with start argument for historical date
    DateTime.iso8601('1751-04-23', Date::ENGLAND)
    ```

## Misc

  * Avoid hashes as optional parameters. Does the method do too much? (Object
    initializers are exceptions for this rule).

  * Avoid methods longer than 10 LOC (lines of code). Ideally, most methods will
    be shorter than 5 LOC. Empty lines do not contribute to the relevant LOC.

  * Avoid parameter lists longer than three or four parameters.

  * Do not mutate parameters unless that is the purpose of the method.

  * Avoid more than three levels of block nesting.

  * Be consistent. In an ideal world, be consistent with these guidelines

  * Use common sense.
