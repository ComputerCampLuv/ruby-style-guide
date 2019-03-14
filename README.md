[Style Guide](https://github.com/ComputerCampLuv/ruby-style-guide/blob/master/ruby-style-guide)
[Recent Changes](https://github.com/ComputerCampLuv/ruby-style-guide/blob/master/recent-changes)

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

  * <a name="spaces-braces"></a>
    No spaces after `(`, `[` or before `]`, `)`.
    Use spaces around `{` and before `}`.
    <sup>[[link](#spaces-braces)]</sup>

    ```ruby
    # bad
    some( arg ).other
    [ 1, 2, 3 ].each{|e| puts e}

    # good
    some(arg).other
    [1, 2, 3].each { |e| puts e }
    ```

  * <a name="spaces-around-equals"></a>
    Use spaces around the `=` operator when assigning default values to method
    parameters:
    <sup>[[link](#spaces-around-equals)]</sup>

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

  * <a name="no-double-indent"></a>
    Align the parameters of a method call if they span more than one
    line. When aligning parameters is not appropriate due to line-length
    constraints, single indent for the lines after the first is also
    acceptable.
    <sup>[[link](#no-double-indent)]</sup>

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

  * <a name="align-multiline-arrays"></a>
    Align the elements of array literals spanning multiple lines.
    <sup>[[link](#align-multiline-arrays)]</sup>

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

  * <a name="method-parens"></a>
    Use `def` with parentheses when there are parameters. Omit the
    parentheses when the method doesn't accept any parameters.
    <sup>[[link](#method-parens)]</sup>

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

  * <a name="method-invocation-parens"></a>
    Use parentheses around the arguments of method invocations,
    especially if the first argument begins with an open parenthesis `(`,
    as in `f((3 + 2) + 1)`.
    <sup>[[link](#method-invocation-parens)]</sup>

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

  * <a name="optional-arguments"></a>
    Define optional arguments at the end of the list of arguments.
    Ruby has some unexpected results when calling methods that have
    optional arguments at the front of the list.
    <sup>[[link](#optional-arguments)]</sup>

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

  * <a name="boolean-keyword-arguments"></a>
    Use keyword arguments when passing boolean argument to a method.
    <sup>[[link](#boolean-keyword-arguments)]</sup>

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

  * <a name="keyword-arguments-vs-optional-arguments"></a>
    Prefer keyword arguments over optional arguments.
    <sup>[[link](#keyword-arguments-vs-optional-arguments)]</sup>

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

  * <a name="keyword-arguments-vs-option-hashes"></a>
    Use keyword arguments instead of option hashes.
    <sup>[[link](#keyword-arguments-vs-option-hashes)]</sup>

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

  * <a name="if-as-a-modifier"></a>
    Favor modifier `if`/`unless` usage when you have a single-line body. Another
    good alternative is the usage of control flow `&&`/`||`.
    <sup>[[link](#if-as-a-modifier)]</sup>

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

  * <a name="unless-for-negatives"></a>
    Favor `unless` over `if` for negative conditions (or control flow `||`).
    <sup>[[link](#unless-for-negatives)]</sup>

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

  * <a name="no-braces-opts-hash"></a>
    Omit the outer braces around an implicit options hash.
    <sup>[[link](#no-braces-opts-hash)]</sup>

    ```ruby
    # bad
    user.set({ name: 'John', age: 45, permissions: { read: true } })

    # good
    user.set(name: 'John', age: 45, permissions: { read: true })
    ```

  * <a name="single-action-blocks"></a>
    Use the proc invocation shorthand when the invoked method is the only operation of a block.
    <sup>[[link](#single-action-blocks)]</sup>

    ```ruby
    # bad
    names.map { |name| name.upcase }

    # good
    names.map(&:upcase)
    ```

  * <a name="single-line-blocks"></a>
    Prefer `{...}` over `do...end` for single-line blocks.  Avoid using `{...}`
    for multi-line blocks (multi-line chaining is always ugly). Always use
    `do...end` for "control flow" and "method definitions" (e.g. in Rakefiles and
    certain DSLs).  Avoid `do...end` when chaining.
    <sup>[[link](#single-line-blocks)]</sup>

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

  * <a name="block-argument"></a>
    Consider using explicit block argument to avoid writing block literal that
    just passes its arguments to another block. Beware of the performance impact,
    though, as the block gets converted to a Proc.
    <sup>[[link](#block-argument)]</sup>

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

  * <a name="no-explicit-return"></a>
    Avoid `return` where not required for flow of control.
    <sup>[[link](#no-explicit-return)]</sup>

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

  * <a name="eql"></a>
    Do not use `eql?` when using `==` will do. The stricter comparison semantics
    provided by `eql?` are rarely needed in practice.
    <sup>[[link](#eql)]</sup>

    ```ruby
    # bad - eql? is the same as == for strings
    'ruby'.eql? some_str

    # good
    'ruby' == some_str
    1.0.eql? x # eql? makes sense here if want to differentiate between Integer and Float 1
    ```

  * <a name="no-non-nil-checks"></a>
    Don't do explicit non-`nil` checks unless you're dealing with boolean
    values.
    <sup>[[link](#no-non-nil-checks)]</sup>

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

  * <a name="map-find-select-reduce-size"></a>
    Prefer `map` over `collect`, `find` over `detect`, `select` over `find_all`,
    `reduce` over `inject` and `size` over `length`. This is not a hard
    requirement; if the use of the alias enhances readability, it's ok to use it.
    The rhyming methods are inherited from Smalltalk and are not common in other
    programming languages. The reason the use of `select` is encouraged over
    `find_all` is that it goes together nicely with `reject` and its name is
    pretty self-explanatory.
    <sup>[[link](#map-find-select-reduce-size)]</sup>

  * <a name="count-vs-size"></a>
    Don't use `count` as a substitute for `size`. For `Enumerable` objects other
    than `Array` it will iterate the entire collection in order to determine its
    size.
    <sup>[[link](#count-vs-size)]</sup>

    ```ruby
    # bad
    some_hash.count

    # good
    some_hash.size
    ```

## Naming

  * <a name="snake-case-symbols-methods-vars-with-numbers"></a>
    Do not separate numbers from letters on symbols, methods and variables.
    <sup>[[link](#snake-case-symbols-methods-vars-with-numbers)]</sup>

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

  * <a name="bool-methods-prefix"></a>
    Avoid prefixing predicate methods with the auxiliary verbs such as `is`,
    `does`, or `can`.  These words are redundant and inconsistent with the style of
    boolean methods in the Ruby core library, such as `empty?` and `include?`.
    <sup>[[link](#bool-methods-prefix)]</sup>

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

  * <a name="refactor-dont-comment"></a>
    Avoid writing comments to explain bad code. Refactor the code to make it
    self-explanatory. ("Do or do not&mdash;there is no try." Yoda)
    <sup>[[link](#refactor-dont-comment)]</sup>

  * <a name="annotate-above"></a>
    Annotations should usually be written on the line immediately above the
    relevant code.
    <sup>[[link](#annotate-above)]</sup>

  * <a name="annotate-keywords"></a>
    The annotation keyword is followed by a colon and a space, then a note
    describing the problem.
    <sup>[[link](#annotate-keywords)]</sup>

  * <a name="indent-annotations"></a>
    If multiple lines are required to describe the problem, subsequent lines
    should be indented three spaces after the `#` (one general plus two for
    indentation purpose).
    <sup>[[link](#indent-annotations)]</sup>

    ```ruby
    def bar
      # FIXME: This has crashed occasionally since v3.2.1. It may
      #   be related to the BarBazUtil upgrade.
      baz(:quux)
    end
    ```

  * <a name="rare-eol-annotations"></a>
    In cases where the problem is so obvious that any documentation would be
    redundant, annotations may be left at the end of the offending line with no
    note. This usage should be the exception and not the rule.
    <sup>[[link](#rare-eol-annotations)]</sup>

    ```ruby
    def bar
      sleep 100 # OPTIMIZE
    end
    ```

  * <a name="todo"></a>
    Use `TODO` to note missing features or functionality that should be added at
    a later date.
    <sup>[[link](#todo)]</sup>

  * <a name="fixme"></a>
    Use `FIXME` to note broken code that needs to be fixed.
    <sup>[[link](#fixme)]</sup>

  * <a name="optimize"></a>
    Use `OPTIMIZE` to note slow or inefficient code that may cause performance
    problems.
    <sup>[[link](#optimize)]</sup>

  * <a name="hack"></a>
    Use `HACK` to note code smells where questionable coding practices were used
    and should be refactored away.
    <sup>[[link](#hack)]</sup>

  * <a name="review"></a>
    Use `REVIEW` to note anything that should be looked at to confirm it is
    working as intended. For example: `REVIEW: Are we sure this is how the client
    does X currently?`
    <sup>[[link](#review)]</sup>

  * <a name="document-annotations"></a>
    Use other custom annotation keywords if it feels appropriate, but be sure to
    document them in your project's `README` or similar.(Here!)
    <sup>[[link](#document-annotations)]</sup>

## Classes & Modules

  * <a name="consistent-classes"></a>
    Use a consistent structure in your class definitions.
    <sup>[[link](#consistent-classes)]</sup>

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

  * <a name="mixin-grouping"></a>
    Split multiple mixins into separate statements.
    <sup>[[link](#mixin-grouping)]</sup>

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

  * <a name="file-classes"></a>
    Don't nest multi-line classes within classes. Try to have such nested
    classes each in their own file in a folder named like the containing class.
    <sup>[[link](#file-classes)]</sup>

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

  * <a name="modules-vs-classes"></a>
    Prefer modules to classes with only class methods. Classes should be used
    only when it makes sense to create instances out of them.
    <sup>[[link](#modules-vs-classes)]</sup>

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

  * <a name="attr_family"></a>
    Use the `attr` family of functions to define trivial accessors or mutators.
    <sup>[[link](#attr_family)]</sup>

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

  * <a name="accessor_mutator_method_names"></a>
    For accessors and mutators, avoid prefixing method names with
    `get_` and `set_`.
    It is a Ruby convention to use attribute names for accessors (readers) and
    `attr_name=` for mutators (writers).
    <sup>[[link](#accessor_mutator_method_names)]</sup>

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

  * <a name="struct-new"></a>
    Consider using `Struct.new`, which defines the trivial accessors,
    constructor and comparison operators for you.
    <sup>[[link](#struct-new)]</sup>

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

  * <a name="visibility"></a>
    Assign proper visibility levels to methods (`private`, `protected`) in
    accordance with their intended usage. Don't go off leaving everything `public`
    (which is the default). After all we're coding in *Ruby* now, not in *Python*.
    <sup>[[link](#visibility)]</sup>

  * <a name="indent-public-private-protected"></a>
    Indent the `public`, `protected`, and `private` methods as much as the method
    definitions they apply to. Leave one blank line above the visibility modifier
    and one blank line below in order to emphasize that it applies to all methods
    below it.
    <sup>[[link](#indent-public-private-protected)]</sup>

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

  * <a name="def-self-class-methods"></a>
    Use `def self.method` to define class methods. This makes the code
    easier to refactor since the class name is not repeated.
    <sup>[[link](#def-self-class-methods)]</sup>

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

  * <a name="factory-methods"></a>
    Consider adding factory methods to provide additional sensible ways to
    create instances of a particular class.
    <sup>[[link](#factory-methods)]</sup>

    ```ruby
    class Person
      def self.create(options_hash)
        # body omitted
      end
    end
    ```

## Collections

  * <a name="literal-array-hash"></a>
    Prefer literal array and hash creation notation (unless you need to pass
    parameters to their constructors, that is).
    <sup>[[link](#literal-array-hash)]</sup>

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

  * <a name="percent-w"></a>
    Prefer `%w` to the literal array syntax when you need an array of words
    (non-empty strings without spaces and special characters in them).  Apply this
    rule only to arrays with two or more elements.
    <sup>[[link](#percent-w)]</sup>

    ```ruby
    # bad
    STATES = ['draft', 'open', 'closed']

    # good
    STATES = %w[draft open closed]
    ```

  * <a name="percent-i"></a>
    Prefer `%i` to the literal array syntax when you need an array of symbols
    (and you don't need to maintain Ruby 1.9 compatibility). Apply this rule only
    to arrays with two or more elements.
    <sup>[[link](#percent-i)]</sup>

    ```ruby
    # bad
    STATES = [:draft, :open, :closed]

    # good
    STATES = %i[draft open closed]
    ```

  * <a name="no-trailing-array-commas"></a>
    Avoid comma after the last item of an `Array` or `Hash` literal, especially
    when the items are not on separate lines.
    <sup>[[link](#no-trailing-array-commas)]</sup>

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

  * <a name="first-and-last"></a>
    When accessing the first or last element from an array, prefer `first` or
    `last` over `[0]` or `[-1]`.
    <sup>[[link](#first-and-last)]</sup>

  * <a name="set-vs-array"></a>
    Use `Set` instead of `Array` when dealing with unique elements. `Set`
    implements a collection of unordered values with no duplicates. This is a
    hybrid of `Array`'s intuitive inter-operation facilities and `Hash`'s fast
    lookup.
    <sup>[[link](#set-vs-array)]</sup>

  * <a name="symbols-as-keys"></a>
    Prefer symbols instead of strings as hash keys.
    <sup>[[link](#symbols-as-keys)]</sup>

    ```ruby
    # bad
    hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

  * <a name="no-mutable-keys"></a>
    Avoid the use of mutable objects as hash keys.
    <sup>[[link](#no-mutable-keys)]</sup>

  * <a name="hash-literals"></a>
    Use the Ruby 1.9 hash literal syntax when your hash keys are symbols.
    <sup>[[link](#hash-literals)]</sup>

    ```ruby
    # bad
    hash = { :one => 1, :two => 2, :three => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

  * <a name="no-mixed-hash-syntaces"></a>
    Don't mix the Ruby 1.9 hash syntax with hash rockets in the same hash
    literal. When you've got keys that are not symbols stick to the hash rockets
    syntax.
    <sup>[[link](#no-mixed-hash-syntaces)]</sup>

    ```ruby
    # bad
    { a: 1, 'b' => 2 }

    # good
    { :a => 1, 'b' => 2 }
    ```

  * <a name="hash-key"></a>
    Use `Hash#key?` instead of `Hash#has_key?` and `Hash#value?` instead of
    `Hash#has_value?`.
    <sup>[[link](#hash-key)]</sup>

    ```ruby
    # bad
    hash.has_key?(:test)
    hash.has_value?(value)

    # good
    hash.key?(:test)
    hash.value?(value)
    ```

  * <a name="hash-each"></a>
    Use `Hash#each_key` instead of `Hash#keys.each` and `Hash#each_value`
    instead of `Hash#values.each`.
    <sup>[[link](#hash-each)]</sup>

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

  * <a name="hash-fetch"></a>
    Use `Hash#fetch` when dealing with hash keys that should be present.
    <sup>[[link](#hash-fetch)]</sup>

    ```ruby
    heroes = { batman: 'Bruce Wayne', superman: 'Clark Kent' }
    # bad - if we make a mistake we might not spot it right away
    heroes[:batman] # => 'Bruce Wayne'
    heroes[:supermann] # => nil

    # good - fetch raises a KeyError making the problem obvious
    heroes.fetch(:supermann)
    ```

  * <a name="hash-fetch-defaults"></a>
    Introduce default values for hash keys via `Hash#fetch` as opposed to using
    custom logic.
    <sup>[[link](#hash-fetch-defaults)]</sup>

    ```ruby
    batman = { name: 'Bruce Wayne', is_evil: false }

    # bad - if we just use || operator with falsy value we won't get the expected result
    batman[:is_evil] || true # => true

    # good - fetch works correctly with falsy values
    batman.fetch(:is_evil, true) # => false
    ```

  * <a name="use-hash-blocks"></a>
    Prefer the use of the block instead of the default value in `Hash#fetch`
    if the code that has to be evaluated may have side effects or be expensive.
    <sup>[[link](#use-hash-blocks)]</sup>

    ```ruby
    batman = { name: 'Bruce Wayne' }

    # bad - if we use the default value, we eager evaluate it
    # so it can slow the program down if done multiple times
    batman.fetch(:powers, obtain_batman_powers) # obtain_batman_powers is an expensive call

    # good - blocks are lazy evaluated, so only triggered in case of KeyError exception
    batman.fetch(:powers) { obtain_batman_powers }
    ```

  * <a name="hash-values-at"></a>
    Use `Hash#values_at` when you need to retrieve several values consecutively
    from a hash.
    <sup>[[link](#hash-values-at)]</sup>

    ```ruby
    # bad
    email = data['email']
    username = data['nickname']

    # good
    email, username = data.values_at('email', 'nickname')
    ```

  * <a name="provide-alternate-accessor-to-collections"></a>
    When providing an accessor for a collection, provide an alternate form
    to save users from checking for `nil` before accessing an element in
    the collection.
    <sup>[[link](#provide-alternate-accessor-to-collections)]</sup>

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

  * <a name="string-interpolation"></a>
    Prefer string interpolation and string formatting instead of string
    concatenation:
    <sup>[[link](#string-interpolation)]</sup>

    ```ruby
    # bad
    email_with_name = user.name + ' <' + user.email + '>'

    # good
    email_with_name = "#{user.name} <#{user.email}>"

    # good
    email_with_name = format('%s <%s>', user.name, user.email)
    ```

  * <a name="consistent-string-literals"></a>
    Adopt a consistent string literal quoting style. There are two popular
    styles in the Ruby community, both of which are considered good&mdash;single
    quotes by default (Option A) and double quotes by default (Option B).
    <sup>[[link](#consistent-string-literals)]</sup>

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

  * <a name="concat-strings"></a>
    Avoid using `String#+` when you need to construct large data chunks.
    Instead, use `String#<<`. Concatenation mutates the string instance in-place
    and is always faster than `String#+`, which creates a bunch of new string
    objects.
    <sup>[[link](#concat-strings)]</sup>

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

  * <a name="dont-abuse-gsub"></a>
    Don't use `String#gsub` in scenarios in which you can use a faster and more specialized alternative.
    <sup>[[link](#dont-abuse-gsub)]</sup>

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

  * <a name="time-now"></a>
    Prefer `Time.now` over `Time.new` when retrieving the current system time.
    <sup>[[link](#time-now)]</sup>

  * <a name="no-datetime"></a>
    Don't use `DateTime` unless you need to account for historical calendar
    reform—and if you do, explicitly specify the `start` argument to
    clearly state your intentions.
    <sup>[[link](#no-datetime)]</sup>

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

  * <a name="always-warn"></a>
    Write `ruby -w` safe code.
    <sup>[[link](#always-warn)]</sup>

  * <a name="no-optional-hash-params"></a>
    Avoid hashes as optional parameters. Does the method do too much? (Object
    initializers are exceptions for this rule).
    <sup>[[link](#no-optional-hash-params)]</sup>

  * <a name="short-methods"></a>
    Avoid methods longer than 10 LOC (lines of code). Ideally, most methods will
    be shorter than 5 LOC. Empty lines do not contribute to the relevant LOC.
    <sup>[[link](#short-methods)]</sup>

  * <a name="too-many-params"></a>
    Avoid parameter lists longer than three or four parameters.
    <sup>[[link](#too-many-params)]</sup>

  * <a name="no-param-mutations"></a>
    Do not mutate parameters unless that is the purpose of the method.
    <sup>[[link](#no-param-mutations)]</sup>

  * <a name="three-is-the-number-thou-shalt-count"></a>
    Avoid more than three levels of block nesting.
    <sup>[[link](#three-is-the-number-thou-shalt-count)]</sup>

  * <a name="be-consistent"></a>
    Be consistent. In an ideal world, be consistent with these guidelines.
    <sup>[[link](#be-consistent)]</sup>

  * <a name="common-sense"></a>
    Use common sense.
    <sup>[[link](#common-sense)]</sup>

# Recent Changes
#### Replacing the using of the Checks class with the CheckSave module where the transaction screen is concerned.
Saving a check with the default check name.
```
@transaction_screen.checks.save_check_with_prefilled_name  # old

@transaction_screen.save_new_check  # new
```
Saving and naming a check.
```
@transaction_screen.checks.save_and_name_check('Check Name')  # old

@transaction_screen.save_new_check('Check Name')  # new
```
`checkname` is an optional parameter
```
def save_new_check(checkname = nil)
  click_save
  name_check(checkname) unless checkname.nil?
  complete_saving_check
end
```

#### PaymentMethod class replacing some of the functunality currently provided by the SaleTotal class
This is part of making our classes and modules more focussed. Their functionality should be obvious.

```ruby
# old
@transaction_screen.sale_total.pay_cash
@transaction_screen.sale_total.pay_split_tender

# new
@transaction_screen.payment_method.pay_cash
@transaction_screen.payment_method.pay_split_tender
```
If in doubt, have a look at the PaymentMethod class. Chances are if the methods is there, you should probably be using that in place of the SaleTotal equivilent.


# Syntax preferences
#### Blocks
Single line blocks wrapped in curly braces with single spacing either end

```ruby
2.times { @transaction_screen.transaction_grid.add_generic_item }
```
Multi line blocks wrapped in a `do ... end`

```ruby
2.times do
  @transaction_screen.transaction_grid.add_generic_item
  @transaction_screen.save_new_check
end
```

### The dumping ground
need to agree on usage, what is worth pulling out as helper methods.
are these local or global
```
RSpec.describe 'example template' do
  let(:cashier_passcode)  { '3456' }
  let(:manager_passcode)  { @appium_config.manager_code }
  let(:gift_card)         { @appium_config.gift_card_id_1 }
```
I'll put this here but it's not exactly relevant yet
```
# this will be useful to us at some point in the future.
# good to have it clear in your head what happends and in what order
# and this doesn't take into consideration the that the same hooks can be used multiple times
# so nested contexts could provide you with some unexpected behaviour if you're not careful
# to the best of my knowledge `around :all` has been deprecated
# I'm not sure if `around :suite` is a thing!

before :suite {...} 
  
before :all {...}
    
around :each do
      
  before :each {...}
        
  example.run
          
  after :each {...}

end
    
after :all {...}

after :suite {...}

end
```
Dumping this here for the time being. I will clean this up
```
before :each do
    receipt_settings.on_screen_tips_enabled
    BrowserlessBo::Endpoints::Bo::ReceiptSettings.update(bo_context, receipt_settings) # seperate actions that are not related
      
    @bo_actions.select_register_type('Restaurant')
    # get and set are conventions for instance variables however this isn't a ruby convention
    # we have a decision to make on our own convention here
    # guest_count_prompt?
    # guest_count_prompt=
    @bo_actions.set_guest_count_prompt(false)

    @shift_screen.open_shift.sidebar.close # @shift_screen.open_shift('100.00').sidebar.close
  end
  
  after :each do 
    @bo_actions.select_register_type('Retail')
    # consider the actions you take in the back office and be sure to reverse any actions that could effect another test.
    # assume that any test may follow this one
  end
    
  it 'will perform said task', :android, :skip_bo do # bevity, should vs will, case_id's?, flags
    @shift_screen.sidebar.sign_out
    # gaps between screen changes
    @signin_screen.sign_in @appium_config.manager_code
    @signin_screen.sign_in '1111'
    @signin_screen.sign_in(cashier_passcode)

    # needless indentation and curly backets used on a multiline block
    3.times { @transaction_screen.stock_item_grid.add_item '$10 Discount Only General'
              @transaction_screen.save_new_check } 
    3.times do 
      @transaction_screen.stock_item_grid.add_item '$10 Discount Only General'
      @transaction_screen.save_new_check
    end 
    @checks_screen.select_checks_and_begin_merge('1', '2') # parentheses

    expect(@checks_screen.does_element_exist?('Check Name Placeholder')).to eql true
    expect(@checks_screen.does_element_exist?('Check Name Placeholder')).to be true
    # be true, eq for everything else. most everything is a string, does this need to be the case

    @checks_screen.name_check 'Custom'
    @checks_screen.complete_merge
    # gaps bewteen 
    expect(@checks_screen.get_check_name '1').to eql 'Custom'
    expect(@checks_screen.check_name('1')).to eq('Custom')

    # grouped as they represent one operation
    # granular is okay because it represents part of the test
    @checks_screen.enter_select_mode
    @checks_screen.select_checks('1','2')
    @checks_screen.open_actions_menu
    @checks_screen.begin_merge

    expect(@checks_screen.does_element_exist?('Check Name Placeholder')).to eql true

    # and used as part of a method name is generally a no go
    # consider merge_checks(*check_indecies)
    @checks_screen.select_checks_and_begin_merge '1', '2'

    # use symbols where possible for keys
    # symbols used here for percent and amount as it highlights the non arbatrary nature of them
    # percent, and amount are the only valid options here.
    # it wont always be clear if ints and symbols will work in place of strings
    suggested_tips = [
      { field: 1, type: :percent, value: 60 },
      { field: 2, type: :amount, value: 10 },
      { field: 3, type: :percent, value: 80 }
    ]

    receipt_settings.set_tips(suggested_tips)
    BrowserlessBo::Endpoints::Bo::ReceiptSettings.update(bo_context, receipt_settings) 

    expect(@shift_screen.correct_menu_options_displayed_to_cashier?).to be true
    # here the expectation is carried out with the page object. which isn't very upfront and doesn't
    # provide great debugging potential
    visible_menu_options = @shift_screen.sidebar.visible_menu_options
    cashier_menu_options = @shift_screen.sidebar.cashier_menu_options
    expect(visible_menu_options).to match_array(cashier_menu_options)
    # actual vs expected compared within the example
    # a failure would inspect both elements and highight discrepancies
    # in place of 'expected true, got false
    expect(@transaction_screen.sale_total.get_total).to eql '8.70'
    # are we happy with the comparrisons to all be on strings, could get total return a float?
    # needless step?!
    expect(@transaction_screen.sale_total.get_total).to eq(8.70)

    # not sure about this expectation, seems like this is more likely to turn a 'broken' test into a 'failing' test
    expect(@split_tenders_payment_screen.sale_complete?).to be true

    @transaction_screen.wait_until_visible 'Split 51'
    # wait for something to be true and then confirm just to be sure!!
    expect(@split_screen.does_element_exist? 'Split 51').to eql true
  
    sign_out_and_sign_back_in '3456'
    # i wouldn't mind helpers returning a variable but I'd rather helper that bundled actions were called on a page object
  end
end
# with all the comments its hard to get spacing across

RSpec.describe '...' do 
  let(:variable) { '...' }
  
  context '...' do 
    it '...' do
      @transaction_screen.do_a_thing
      @transaction_screen.do_another_thing
      @transaction_screen.access_checks

      @checks_screen.do_some_things
      @checks_screen.merge_some_checks
      @checks_screen.access_mergered_check

      @transaction_screen.add_item
      @transaction_screen.pay_cash

      @transaction_screen.add_items('this', 'that')
      @transaction_screen.save_new_check
    end
  end

  context '...' do 
    before :each do 
      [...]
    end

    it '...' do
      [...]
    end
  end
end
```

