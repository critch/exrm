# Elixir Release Manager

Thanks to @tylerflint for the Makefile, rel.config, and runner script!

## Getting Started

This project's goal is to make releases with Elixir projects a breeze. It is composed of a mix task, and all build files required to successfully take your Elixir project and perform a release build. All you have to do to get started is the following:

#### Add `exrm` as a dependency to your project

```elixir
  defp deps do
    [{:exrm, github: "bitwalker/exrm"}]
  end
```

#### Fetch and Compile

- `mix deps.get`
- `mix deps.compile`

#### Perform a release

- `mix release`

#### Run your app! (my example is based on a simple ping server, see the appendix for more info)

```
> rel/test/bin/test
Erlang/OTP 17 [RELEASE CANDIDATE 1] [erts-6.0] [source-fdcdaca] [64-bit] [smp:4:4] [async-threads:10] [hipe] [kernel-poll:false]

Interactive Elixir (0.12.5) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)> :gen_server.call(:test, :ping)
:pong
iex(2)>
```

## Features

- `mix release`

This task constructs the complete release for you. The output is sent to `rel/project_name_here`.

- `mix release.clean [--rel | --all]`

Without args, this will clean up any relese builds.

With `--rel`, it will also clean up any generated tools (rebar, relx,
etc.), useful when you've changed project information like version or
name, and need the build configuration updated.

With `--all`, it will implode all files created by `exrm`, and will
leave your project in the same state as it was prior to your first
build.

## TODO

- Make Elixir version configurable (currently locked to stable)
- Store Makefile in the `rel/files` directory, so that conflicts aren't possible
- Allow `release.clean` to be more granular (i.e. don't wipe `_elixir` on clean by default)

If you run into problems, this is still early in the project's development, so please create an issue, and I'll address ASAP.

## Appendix

The example server I setup was as simple as this:

- `mix new test`
- `cd test && touch lib/test/server.ex`

Then put the following in `lib/test/server.ex`

```elixir
defmodule Test.Server do
  use GenServer.Behaviour

  def start_link() do
    :gen_server.start_link({:local, :test}, __MODULE__, [], [])
  end

  def init([]) do
    { :ok, [] }
  end
  
  def handle_call(:ping, _from, state) do
    { :reply, :pong, state }
  end

end
```

You should be able to replicate my example using these steps. If you can't, please let me know.
