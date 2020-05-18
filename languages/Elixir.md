# From "The Little Elixir and OTP Guidebook"

## Functions

### Function Clauses

```elixir
defmodule MeterToLengthConverter do
     # syntax for single lined functions :point-down:
    def convert(:feet, m), do: m * 3.28084
    def convert(:inch, m), do: m * 39.3701
    def convert(:yard, m), do: m * 1.09361
end
```

### Optional argument

Optional arguments can be specified using `\\`. For e.g. 
```elixir
def fetch_something(opts \\ []) do
  ## an empty list as default options
end
```

**Note: ** function clauses in a module should be placed together like the above example.

### Nested Modules

To flatten a nested module use the `.` operator

```elixir
defmodule FirePokemon.Charizard do
  def attack do
    "fireball"
  end
end

def FirePokemon.Magma do
  def attack do
    "lava"
  end
end
```

## OTP

### GenServer

Writing client-server code using GenServer is pretty straightforward in terms of code organization. The functions can be divided into 3 categories
- Client API functions: as the name states they are the entrypoint of a request from the client
- Server Callbacks: Genserver callbacks invoked by the client side functions
- Helper functions: Code that doesn't belong in either of the two types

For e.g.
```elixir
defmodule Gentex.Worker do
  use GenServer

  ## Client API

  def start_link(opts \\ []) do
    GenServer.start_link(__MODULE__, :ok, opts)
  end

  def reset_stats(pid) do
    GenServer.cast(pid, :reset_stats)
  end

  def temperature(pid, location) do
    GenServer.call(pid, {:location, location})
  end

  def stop(pid) do
    GenServer.cast(pid, :stop)
  end

  ## Server callbacks
  def init(:ok) do
    {:ok, %{}}
  end

  def handle_call({:location, location}, _from, stats) do
    case temperature_of(location) do
      {:ok, temp} ->
        new_stats = update_stats(stats, location)
        {:reply, "#{temp}°C", new_stats}
      _ ->
        {:reply, :error, stats}
    end
  end

  def handle_cast(:reset_stats, _stats) do
    {:noreply, %{}}
  end

  def handle_cast(:stop, stats) do
    {:stop, :normal, :ok, stats}
  end

  def terminate(reason, stats) do
    IO.puts "server stopped because of #{inspect reason}"
      inspect stats
    :ok
  end

  ## Helper functions

  defp update_stats(old_stats, location) do
    case Map.has_key?(old_stats, location) do
      true->
        Map.update!(old_stats, location, &(&1+1))
      false->
        Map.put_new(old_stats, location, 1)
    end
  end

  defp temperature_of(location) do
    url_for(location) |> HTTPoison.get |> parse_response
  end

  defp url_for(location) do
    # do something
  end

  defp parse_response({:ok, %HTTPoison.Response{body: body, status_code: 200}}) do
    body |> JSON.decode! |> compute_temp
  end

  defp parse_response(_) do
    :error
  end

  defp compute_temp(json) do
    try do
      temp = (json["main"]["temp"] - 273.15) |> Float.round(1)
      {:ok, temp}
    rescue
      _ -> :error
    end
  end
end

```

`GenServer.start_link`: the first argument is the module name that contains the `init` function and invokes it. Our client `start_link` is just a wrapper around the function provided by the GenServer to provide a way for clients to invoke GenServer functions instead of manually calling Genserver functions and pass the API module to it.

`handle_call`: The first argument declares the expected request to be handled. The second argument returns a tuple in the form of {pid, tag}, where the pid is the pid of the client
and tag is a unique reference to the message. 

`handle_call`(s) & `handle_cast`(s) should be grouped together.

good use case for GenServer.cast/2? A
fine example is a command that’s issued to a server and that causes a side effect in the
server’s state. In that case, the client issuing the command shouldn’t care about a
reply.

Messages may arrive from processes that aren’t defined in handle_call/3/handle
_cast/2. That’s where handle_info/2 comes in. It’s invoked to handle any other
messages that are received by the process, sometimes referred to as out-of-band messages. You don’t need to supply a client API counterpart for handle_info/2.


In case of distributed nodes you can use a name attribute for your GenServers
```elixir
defmodule Metex.Worker do
  use GenServer
  @name MW
  ## Client API
  def start_link(opts \\ []) do
    GenServer.start_link(__MODULE__, :ok, opts ++ [name: MW])
  end

  def get_temperature(location) do
    GenServer.call(@name, {:location, location})
  end
end
```

Things to keep in mind while writing GenServers
 The state with which you want to initialize the server
 The kinds of messages the server handles
 When to reply to the client
 What message to use to reply to the client
 What resources to clean up after termination

### Processes
- There’s no shared memory. The only way a change
of state can occur within a process is when a message is sent to it. This is different from
threads, because threads share memory. 
- messages can come in the form of any valid Elixir
term. This means tuples, lists, and atoms are all fair game.

#### Links and Monitors

### Agents

Agents are simple wrappers around state. If all you want from a process is to keep state, agents are a great fit.

### Different OTP behaviours and their uses

GenServer => Implementing the server of a client-server relationship
GenEvent => Implementing event-handling functionality
Supervisor => Implementing supervision functionality
Application => Working with applications and defining application callbacks