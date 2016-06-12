for this

```
iex(1)> Rumbl.Repo.all Rumbl.Other
[]
iex(2)> Rumbl.Repo.all Rumbl.User     
[%Rumbl.User{id: "1", name: "Jose", password: "elixir",
  username: "josevalim"},
 %Rumbl.User{id: "2", name: "Bruce", password: "7langs",
  username: "redrapids"},
 %Rumbl.User{id: "3", name: "Chris", password: "phx",
  username: "chrismccord"}]
```

do this

```ex
defmodule Rumbl.Repo do
  def all(Rumbl.User) do
    [%Rumbl.User{id: "1", name: "Jose", username: "josevalim", password: "elixir"},
    %Rumbl.User{id: "2", name: "Bruce", username: "redrapids", password: "7langs"},
    %Rumbl.User{id: "3", name: "Chris", username: "chrismccord", password: "phx"}]
  end
  def all(_module) do
    []
  end
end
```

if you do this

```
alias Rumbl.Repo
alias Rumbl.User
```

then it's more simple

```
iex(3)> Rumbl.Repo.all Rumbl.User
[%Rumbl.User{id: "1", name: "Jose", password: "elixir", username: "josevalim"},
 %Rumbl.User{id: "2", name: "Bruce", password: "7langs", username: "redrapids"},
 %Rumbl.User{id: "3", name: "Chris", password: "phx", username: "chrismccord"}]
iex(4)> Repo.all User
** (UndefinedFunctionError) undefined function Repo.all/1 (module Repo is not available)
    Repo.all(User)
iex(4)> alias Rumbl.Repo
nil
iex(5)> Repo.all User
[]
iex(6)> alias Rumbl.User
nil
iex(7)> Repo.all User   
[%Rumbl.User{id: "1", name: "Jose", password: "elixir", username: "josevalim"},
 %Rumbl.User{id: "2", name: "Bruce", password: "7langs", username: "redrapids"},
 %Rumbl.User{id: "3", name: "Chris", password: "phx", username: "chrismccord"}]
```
 
something like this
 
```
Repo.get User, "1"
Repo.get_by User, name: "Bruce"
```
 
how to do that?

type on iex `h Enum.find` and `h Enum.all?` `h Map.get` etc for hint

