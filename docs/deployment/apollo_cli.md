The Apollo command line interface (CLI) is useful to streamline or automate
tasks. The CLI is executed via the `apollo` command which includes a number of
subcommands.

# Setting up the CLI

## With npm package manager

The Apollo CLI is available via
[npm](https://www.npmjs.com/package/@apollo-annotation/apollo-cli). At the time
of this writing it requires node version 18. Once installed, the CLI will be
available as:

```
apollo --help
```

## With docker

The Apollo CLI is also available on GitHub as a [docker
image](https://github.com/gmod/Apollo3/pkgs/container/apollo-cli):

```
docker run ghcr.io/gmod/apollo-cli --help
```

To run the CLI from docker we need to bind local directories to volumes inside
docker using the option `-v/--volume`. In particular, we need to bind the local
directory containing the Apollo configuration file to the corresponding docker
directory `/root/.config/apollo-cli`. For example:

```
docker run --network host -v ~/.config/apollo-cli:/root/.config/apollo-cli ghcr.io/gmod/apollo-cli config --get-config-file

/root/.config/apollo-cli/config.yaml
```

Similarly, to read local files we need to bind the directory containing the
files to read to a directory inside docker. For example, to upload file
`ref/genome.fa`:

```
docker run --network host \
    -v ~/.config/apollo-cli:/root/.config/apollo-cli \
    -v ref:/data \
    ghcr.io/gmod/apollo-cli \
    add-fasta -i data/genome.fa ...
```

# Working with the CLI

For brevity, we use here the command `apollo` as installed via `npm` instead of the docker counterpart.

## Configuration and login

We start by registering a new user profile with access details and credentials
matching the way Apollo has been setup. The default profile is called
`default`, use the option `--profile` to add a new one.

In this example we setup the default profile wth "root" access to Apollo on
address `http://localhost:3999`:

```
apollo config address http://localhost:3999
apollo config accessType root
apollo config rootCredentials.username admin
apollo config rootCredentials.password pass
```

With `accessType` other than `root`, e.g. `google`, username and password
are not needed.

Next we login to Apollo:

```
apollo login
```

`login` stores an access token in the configuration file so that a profile
stays logged-in until the token is deleted or expires. See also commands
`apollo status` and `apollo logout`.


## Adding assemblies

Similar to the web interface, we can add assemblies from fasta files or from
GFF files containing contig sequences. We call the new assembly `myAssembly`:

```
apollo assembly add-fasta -i genome.fa -a myAssembly
# Or
apollo assembly add-gff -i genome.gff -a myAssembly
```

If the assembly comes from a fasta file, we probably want to add some existing
annotation:

```
apollo feature import -i annotation.gff -a myAssembly
```

-----

See `apollo <command> --help` for further commands and options.
