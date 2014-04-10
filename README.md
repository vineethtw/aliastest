# aliastest

__This repo exists solely to test how symbolic links (file aliases) are treated by github.__

There are two files:

  * `.catalog` is a symbolic link to sample.yaml
  * `sample.yaml` is a very basic Heat template

The steps are simple:
  1 create the actual file (in my case, `sample.yaml`)
  2 `ln -s sample.yaml .catalog`
  3 git add .
  4 git commit

The symbolic link doesn't quite just work. However, if you:

```shell

curl -L https://github.com/Pablosan/aliastest/raw/master/.catalog

```

_note the `-L`. This http request results in a single redirect._

The response will be:

`sample.yaml`

So, we now know the name of the file `.catalog` points to and we can retrieve that file.


This is fugly, but you could do this in a single command:

```shell

curl -L https://github.com/Pablosan/aliastest/raw/master/`curl -L https://github.com/Pablosan/aliastest/raw/master/.catalog`

```

The response from this command is the actual template:

```yaml

heat_template_version: 2013-05-23

resources:
  compute_instance: # You can name this whatever you prefer
    type: "OS::Nova::Server"
    properties:
      flavor: 1GB Standard Instance
      image: CentOS 6.5
      name: Single Compute Instance

```

I think I like this approach.

