Railway oriented programming
===

Implementation of module described here:
http://zohaib.me/railway-programming-pattern-in-elixir/

Same thing can be achieved using Error monad:
http://zohaib.me/monads-in-elixir-2/

## tl;dr

Instead of `|>` we can use `>>>` to pipe `{:ok, val} | {:error, error}` returning functions.

## Usage

As written in `test/rop_test.exs`:

```ex
use Rop
```

Using Rop defines `>>>` operator.

```ex
test "Count to 3" do
  assert (ok(0) >>> ok >>> ok) == {:ok, 3}
end
```

We increase `0` three times to end up with `{:ok, 3}`.

```ex
test "Error" do
  assert (ok(0) >>> error) == {:error, "Error at 1"}
end
```

Error function always return error.

```ex
test "Error propagation" do
  assert (ok(0) >>> error >>> ok) == {:error, "Error at 1"}
end
```

Error was propagated and second `ok` was not called.

```ex
test "First error is returned" do
  assert (ok(0) >>> error >>> error) == {:error, "Error at 1"}
end
```

Only first error is returned, since next `error` is never called.

```ex
defp ok(cnt) do
  {:ok, cnt + 1}
end

```
`ok/1` function takes value and increases it, returning standard `{:ok, val}` response.

```ex
defp error(cnt) do
  {:error, "Error at #{cnt}"}
end
```

`error/1` function takes value and returns standard `{:error, string}` that identifies where error was called.


