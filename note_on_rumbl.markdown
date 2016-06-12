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
