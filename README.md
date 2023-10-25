# RAG LangServe chain template

A basic chain template showing the RAG pattern using
a vector store on Astra DB / Apache Cassandra®.

## Setup:

You need:

- an [Astra](https://astra.datastax.com) Vector Database (free tier is fine!). **You need a [Database Administrator token](https://awesome-astra.github.io/docs/pages/astra/create-token/#c-procedure)**, in particular the string starting with `AstraCS:...`;
- likewise, get your [Database ID](https://awesome-astra.github.io/docs/pages/astra/faq/#where-should-i-find-a-database-identifier) ready, you will have to enter it below;
- an **OpenAI API Key**. (More info [here](https://cassio.org/start_here/#llm-access), note that out-of-the-box this demo supports OpenAI unless you tinker with the code.)

_Note:_ you can alternatively use a regular Cassandra cluster: to do so, make sure you provide the `USE_CASSANDRA_CLUSTER` entry as shown in `.env.template` and the subsequent environment variables to specify how to connect to it.

You need to provide the connection parameters and secrets through environment variables. Please refer to `.env.template` for what variables are required.

## Running the chain

### Populate the vector store

For a standalone usage (i.e. outside of LangServe), first clone this repo
and then, in its root directory:

Make sure you have the environment variables all set (see previous section),
then, from this directory, launch the following just once:

```
poetry env use /usr/bin/python3.11    # adjust to your system
poetry install
poetry run python setup.py 
```

The output will be something like `Done (29 lines inserted).`.

> **Note**: In a full application, the vector store might be populated in other ways:
> this step is to pre-populate the vector store with some rows for the
> demo RAG chains to sensibly work.


### Run the chain test

The environment has already been set up in the previous section:
to test the chain, simply run:

```
poetry run python main.py 
```

## Adding to LangServe

To add this chain to your LangServe app,

```
poe add --repo=hemidactylus/langserve_cassandra_entomology_rag
```

(you may need to prepend `poetry run` to `poe` commands).

Then, after setting the environment variables as specified for the standalone usage above, you can start LangServe:

```
poe start
```

and test the new endpoints by opening `http://127.0.0.1:8000/docs`.

Suggestions:

- for a negative test, try `Do birds have wings?`
- for a positive test, try `Do Odonata have wings?`
