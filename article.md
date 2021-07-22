# Exploring the Algorand Sandbox: An Exploratory Journey

In this tutorial, you will explore the [Algorand sandbox](https://github.com/algorand/sandbox) to get a deeper understanding of its functioning. The goal is to show you what information certain files contain, access the Docker containers and their files, or change network configurations. This tutorial complements the [introduction tutorial for the sandbox](https://developer.algorand.org/articles/introducing-sandbox-quick-way-get-started-algorand/) created by Sam Abbassi. Make sure to follow the installation instructions in Sam's tutorial to set up the Algorand sandbox.

Here's an overview of what you will do:

1. Start a default network
2. Explore the `sandnet-v1` genesis file inside the Algod container
3. Explore configuration files 
4. Set up a custom network

Let's get started!

## Step 1: Start the Algorand sandbox with the default configuration

First, let's make sure that your installation is working as expected. Therefore, start the node with the default configuration using the following command inside the sandbox folder.

```bash
./sandbox up
```

By default, the script will execute some commands like retrieving the node status. Here's an overview of the node status:

[sandbox node status](https://i.ibb.co/tPF4PPQ/node-status.png)

As you can see, the node contains only one committed block because this is a new Algorand network. Also, note that the `Genesis ID` points to an identifier called `sandnet-v1`. In the next step, you'll explore this genesis file.

Besides the node status, it will print all active accounts using `goal account list`. You'll find those accounts later in the genesis file.

```bash
[online]	T5SVL76GT3NCUPV3J7D4QCFCWFAGZO6CTPZGSU5SXPISOR55ESOWFL26MY	4000000000000000 microAlgos
[offline]	2XMFQ6F5DREAA5MEP7WTMY276YMQS4HOLSOIVEPUAZ3477G6KSLAR4267A	1000000000000000 microAlgos
[offline]	4NRRJYWM7NYVAVPPW3XU5YI3A3RTZPXRIK7DNXDRP3NWUCKKWDQHXZ4RCA	4000000000000000 microAlgos
```

Next, let's explore the `genesis.json` file.


## Step 2: Exploring the genesis.json file

Next, we want to retrieve the `sandnet-v1` genesis file. Let's learn how to enter a Docker container to explore the files inside the container, such as the genesis file and other configuration files. To do so, we have to tell the sandbox script to enter the Algod container. You can use the same command to enter other containers such as the `indexer` or `indexer-db` containers.

```bash
./sandbox enter algod
```

If you're wondering how the enter command work, you can look at the `sandbox` executable in your editor. At [line 292 in the script](https://github.com/algorand/sandbox/blob/master/sandbox#L292), you'll find the implementation details of the command. The command requires you to pass a container name, and the container name must match one of the active container's names. Furthermore, the script will use the `docker exec` command to open a Bash shell inside the container.

Now, let's print all items inside the folder using the `ls` command.

```bash
ls
```

You'll get the following result.

[sandbox container files](https://i.ibb.co/gM4fzdf/sandbox.png)

Let's take a look at the genesis file.

```bash
cat genesis.json
```

This command will print the contents of the genesis file. It contains all accounts that the `goal account list` account has printed. On top of that, the network is named `sandnet`, and the id equals `v1`, which combined make up the genesis ID.

```json
{
  "alloc": [
    {
      "addr": "7777777777777777777777777777777777777777777777777774MSJUVU",
      "comment": "RewardsPool",
      "state": {
        "algo": 125000000000000,
        "onl": 2
      }
    },
    {
      "addr": "A7NMWS3NT3IUDMLVO26ULGXGIIOUQ3ND2TXSER6EBGRZNOBOUIQXHIBGDE",
      "comment": "FeeSink",
      "state": {
        "algo": 100000,
        "onl": 2
      }
    },
    {
      "addr": "5AO6OPZKSA3W3QXFFOA7ITXFFWZS4MFRTOVMXSFFWQWJLOYSQ44XYPKX6Y",
      "comment": "Wallet1",
      "state": {
        "algo": 1000000000000000,
        "onl": 1,
        "sel": "OMpQJpMuambHxQ9QIq31B4mOaXNcyLXMl9TpYu3KzWE=",
        "vote": "dLw/06AurPLaDWzLlwG5sUeqboFeQS3cFtZE+oL+Du8=",
        "voteKD": 100,
        "voteLst": 3000000
      }
    },
    {
      "addr": "T5SVL76GT3NCUPV3J7D4QCFCWFAGZO6CTPZGSU5SXPISOR55ESOWFL26MY",
      "comment": "Wallet2",
      "state": {
        "algo": 4000000000000000,
        "onl": 1,
        "sel": "TL1+M4igP4jJVjVdqL4+tj6pDPHfiRoGBNPK38pPaXg=",
        "vote": "zmXZ8jodrv+UhKGYpqUQmadkJdCF57r/MMw4DsvHN2I=",
        "voteKD": 100,
        "voteLst": 3000000
      }
    },
    {
      "addr": "4NRRJYWM7NYVAVPPW3XU5YI3A3RTZPXRIK7DNXDRP3NWUCKKWDQHXZ4RCA",
      "comment": "Wallet3",
      "state": {
        "algo": 4000000000000000
      }
    },
    {
      "addr": "2XMFQ6F5DREAA5MEP7WTMY276YMQS4HOLSOIVEPUAZ3477G6KSLAR4267A",
      "comment": "Wallet4",
      "state": {
        "algo": 1000000000000000
      }
    }
  ],
  "fees": "A7NMWS3NT3IUDMLVO26ULGXGIIOUQ3ND2TXSER6EBGRZNOBOUIQXHIBGDE",
  "id": "v1",
  "network": "sandnet",
  "proto": "https://github.com/algorandfoundation/specs/tree/65b4ab3266c52c56a0fa7d591754887d68faad0a",
  "rwd": "7777777777777777777777777777777777777777777777777774MSJUVU"
}
```

So, what else can we find?

## Step 3: Looking up Algorand config

Interestingly, we can find other files besides the genesis file. Inspect the contents of the `algod.net` file using `cat algod.net` results in the following output. 

```txt
[::]:4001
```

This output refers to `localhost:4001` which is the configured address to which the Algod API is exposed.

Alternatively, we can also inspect the `algod.token` file. It will print the authorization token for making requests to the Algod API. 

```txt
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

However, we don't have to look for this information inside our container. Take a look at the `sandbox/docker-compose.yml` file, which sets all these configuration parameters.

```yml
version: '3'

services:
  algod:
    container_name: "algorand-sandbox-algod"
    build:
      context: .
      dockerfile: ./images/algod/Dockerfile
      args:
        CHANNEL: "${ALGOD_CHANNEL}"
        URL: "${ALGOD_URL}"
        BRANCH: "${ALGOD_BRANCH}"
        SHA: "${ALGOD_SHA}"
        BOOTSTRAP_URL: "${NETWORK_BOOTSTRAP_URL}"
        GENESIS_FILE: "${NETWORK_GENESIS_FILE}"
        TEMPLATE: "${NETWORK_TEMPLATE:-/tmp/images/algod/template.json}"
        TOKEN: "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
        ALGOD_PORT: "4001"
        KMD_PORT: "4002"
    ports:
      - 4001:4001
      - 4002:4002
```

Actually, we can use this information to query the Algod API and retrieve, for instance, the node status manually instead of using `goal node status`. It will print the same information as the info shown in the screenshot when starting the network for the first time. 

```bash
curl localhost:4001/v1/status -H "X-Algo-API-Token: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
```

Cool, let's try to set up a custom network using your own genesis.json file. 

## Step 4: Setting up a custom network with the Algorand sandbox

Let's get our hands dirty! Create a new folder inside the `sandbox/genesis` folder called `customnet`. Further, create a new file called `genesis.json`.

```bash
cd genesis
mkdir customnet && cd customnet
touch genesis.json
```

Now, let's add the following configuration to the genesis.json file you've created. You can change the configuration as you wish, like adding more addresses or changing the number of ALGO tokens each address owns. 

```json
{
  "alloc": [
    {
      "addr": "7777777777777777777777777777777777777777777777777774MSJUVU",
      "comment": "RewardsPool",
      "state": {
        "algo": 125000000000000,
        "onl": 2
      }
    },
    {
      "addr": "A7NMWS3NT3IUDMLVO26ULGXGIIOUQ3ND2TXSER6EBGRZNOBOUIQXHIBGDE",
      "comment": "FeeSink",
      "state": {
        "algo": 100000,
        "onl": 2
      }
    },
    {
      "addr": "SEMQ3EH6AHVCXZDHCDH3OYBJJGGLHDUFGYVCTJUUJMLO4J5F4E6AIXH3R4",
      "comment": "Wallet1",
      "state": {
        "algo": 500000000000000,
        "onl": 1,
        "sel": "kRtZiTWtKjFbx58DxEllyZ0/7mQYjT4gdilgNLEEcIQ=",
        "vote": "QN1+DlX+2JE8O0NiFTSH+50sXpTYT79HVIYf0YRp1P4=",
        "voteKD": 10000,
        "voteLst": 3000000
      }
    },
    {
      "addr": "IDUTJEUIEVSMXTU4LGTJWZ2UE2E6TIODUKU6UW3FU3UKIQQ77RLUBBBFLA",
      "comment": "bank",
      "state": {
        "algo": 100000000000000,
        "onl": 1,
        "sel": "6Rax2zpOehfDXHyyvMFJwc22A//Pm6rxW4MiEBGIwog=",
        "vote": "5JbOqqGn1Ypw+uH3t2i61DcjWYo1AslT0jyOGgcVC6Y=",
        "voteKD": 10000,
        "voteLst": 3000000
      }
    },
    {
      "addr": "PNWOET7LLOWMBMLE4KOCELCX6X3D3Q4H2Q4QJASYIEOF7YIPPQBG3YQ5YI",
      "comment": "pp1",
      "state": {
        "algo": 50000000000000
      }
    },
    {
      "addr": "F5FALDSUFYO5LQU4OQY2HPZTXSUPRHUAERZ7IDG5QCBC76AOHEPK2VUJ34",
      "comment": "pp2",
      "state": {
        "algo": 50000000000000
      }
    }
  ],
  "fees": "A7NMWS3NT3IUDMLVO26ULGXGIIOUQ3ND2TXSER6EBGRZNOBOUIQXHIBGDE",
  "id": "v1.0",
  "network": "devnet",
  "proto": "https://github.com/algorand/spec/tree/a26ed78ed8f834e2b9ccb6eb7d3ee9f629a6e622",
  "rwd": "7777777777777777777777777777777777777777777777777774MSJUVU",
  "timestamp": 1560210489
}
```

Furthermore, we need to create a new `config.*` file that matches the name of our network. Therefore, create a new file called `config.customnet` in the root of the `/sandbox` folder.

```bash
touch config.customnet
```

Here, we'll add our custom configuration to point to the newly created `genesis.json` file. The configuration contains a variable `NETWORK_GENESIS_FILE` that points to the custom `genesis.json` file. Note that the path starts from the root of the `/sandbox` folder. Additionally, we've set the `INDEXER_DISABLED` variable to `false` to ensure that our custom network spins up an indexer container. However, if you don't need the indexer, you can set the variable to `true`. As an advantage, your network will spin up faster because it doesn't have to spin up the indexer software.

```bash
export ALGOD_CHANNEL="beta"
export ALGOD_URL=""
export ALGOD_BRANCH=""
export ALGOD_SHA=""
export NETWORK=""
export NETWORK_BOOTSTRAP_URL=""
export NETWORK_GENESIS_FILE="genesis/customnet/genesis.json"
export INDEXER_URL="https://github.com/algorand/indexer"
export INDEXER_BRANCH="develop"
export INDEXER_SHA=""
export INDEXER_DISABLED="false"
```

Finally, let's start our custom network. However, make sure to stop any other Algorand containers that are running. Use the `clean` command for that:

```bash
./sandbox clean
```

Next, you can start your custom network by adding the network's name to the `up` command.

```bash
./sandbox up customnet
```

That's it! 
