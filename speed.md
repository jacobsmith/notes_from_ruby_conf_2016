## Get there faster by slowing down...
  Measure it

```ruby
StackProf.run(mode: :wall, out: "tmp/stackprof.dump") do
  Rake::Task["assets:precompile"].invoke
end
```

Inspect stackprof output

```ruby
stackprof tmp/stackprof.dump --method Set#include?
```
  returns information of callers, etc.

Set is just a wrapper around Hash


See the kernel calls Ruby is making
```ruby
code = "
  foo = Hash.new
  foo[:bar]
"

puts RubyVM::InstructionSequence.compile(code).disasm
```

insdef - opt_aref
Ruby optimizes hash calls in C
Don't subclass hash! (skips utilizing C bindings)

if/elsif is hidden iteration

## Refinements (monkey patch *only in your code*)

```ruby
Module Foo
  refine String do
    def say_hello(s)
      s + " hello!"
    end
  end
end
```

"Ruby Under a Microscope" - Book to read

- Identify a goal
- Gather potentially useful tools (StackProf, DerailedBenchmarks, fasterer gem)
- Iterate on the problem

Find a speed buddy

"Don't abandon your language for speed"

Have you tried JRuby?

bootscale - caches require lookups
