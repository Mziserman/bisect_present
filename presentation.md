# Git bisect

Comment trouver des bugs

```bash
$ git clone https://github.com/Mziserman/bisect_demo
```
---
## Le code buggé

```ruby
def add(a, b)
  a + b
end

def multiply(a, b)
  b.times.sum(a)
end

def divide(a, b)
  return a if b == 1

  count = 0
  acc = 0
  loop do
    count += 1
    acc += b

    break if acc >= substract(a, b)
  end
  add(acc, a % b)
end

def substract(a, b)
  a - b
end
```

---
## les tests qui passaient

```ruby
#!/usr/bin/env ruby

require_relative 'calculations'

tests = [
  -> { add(2, 1) == 3 },
  -> { substract(2, 1) == 1 },
  -> { multiply(2, 1) == 2 },
  -> { divide(2, 1) == 2 }
]

if tests.all? &:call
  exit 0
else
  puts tests.select { |t| !t.call }
  exit 1
end
```

---
## les nouveaux tests qui ne passent pas

```ruby
#!/usr/bin/env ruby

require_relative 'calculations'

tests = [
  -> { multiply(12, 3) == 36 },
  -> { divide(1, 2) == 0.5 },
  -> { divide(21, 3) == 7 }
]

if tests.all? &:call
  exit 0
else
  puts tests.select { |t| !t.call }
  exit 1
end
```
