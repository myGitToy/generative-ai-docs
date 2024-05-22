# Docs Agent CLI reference

This page provides a list of the Docs Agent command lines and their usages
and examples.

The Docs Agent CLI helps developers to manage the Docs Agent project and
interact with language models. It can handle various tasks such as
processing documents, populating vector databases, launching the chatbot,
running benchmark test, sending prompts to language models, and more.

**Important**: All `agent` commands need to run in the `poetry shell`
environment.

## Processing documents

### Chunk Markdown files into small text chunks

The command below splits Markdown files (and other source files) into small
chunks of plain text files:

```sh
agent chunk
```

### Populate a vector database using text chunks

The command below populates a vector database using plain text files (created
by running the `agent chunk` command):

```sh
agent populate
```

### Populate a vector database and delete stale text chunks

The command below deletes stale entries in the existing vector database
before populating it with the new text chunks:

```sh
agent populate --enable_delete_chunks
```

### Show the Docs Agent configuration

The command below prints all the fields and values in the current
[`config.yaml`][config-yaml] file:

```sh
agent show-config
```

### Clean up the Docs Agent development environment

The command below deletes development databases specified in the
`config.yaml` file:

```sh
agent cleanup-dev
```

### Write logs to a CSV file

The command below writes the summaries of all captured debugging information
(in the `logs/debugs` directory) to  a `.csv` file:

```sh
agent write-logs-to-csv
```

## Launching the chatbot web app

### Launch the Docs Agent web app

The command below launches Docs Agent's Flask-based chatbot web application:

```sh
agent chatbot
```

### Launch the Docs Agent web app using a different port

The command below launches the Docs Agent web app to run on port 5005:

```sh
agent chatbot --port 5005
```

### Launch the Docs Agent web app as a widget

The command below launches the Docs Agent web app to use
a widget-friendly template:

```sh
agent chatbot --app_mode widget
```

### Launch the Docs Agent web app with a log view enabled

The command below launches the Docs Agent web app while enabling
a log view page (which is accessible at `<APP_URL>/logs`):

```sh
agent chatbot --enable_show_logs
```

## Running benchmark test

### Run the Docs Agent benchmark test

The command below runs benchmark test using the questions and answers listed
in the [`benchmarks.yaml`][benchmarks-yaml] file:

```sh
agent benchmark
```

## Interacting with language models

### Ask a question

The command below reads a question from the arguments, asks the Gemini model,
and prints its response:

```sh
agent tellme <QUESTION>
```

Replace `QUESTION` with a question written in plain English, for example:

```sh
agent tellme does flutter support material design 3?
```

**Note**: This `agent tellme` command is used to set up the `gemini` command
in the [Set up Docs Agent CLI][set-up-docs-agent-cli] guide.

### Ask a question to a specific product

The command below enables you to ask a question to a specific product in your
Docs Agent setup:

```sh
agent tellme <QUESTION> --product <PRODUCT>
```

The example below asks the question to the `Flutter` product in your
Docs Agent setup:

```sh
agent tellme which modules are available? --product=Flutter
```

You may also specify multiple products, for example:

```sh
agent tellme which modules are available? --product=Flutter --product=Angular --product=Android
```

### Ask for advice

The command below reads a request and a filename from the arguments,
asks the Gemini model, and prints its response:

```sh
agent helpme <REQUEST> --file <PATH_TO_FILE>
```

Replace `REQUEST` with a prompt and `PATH_TO_FILE` with a file's
absolure or relative path, for example:

```sh
agent helpme write comments for this C++ file? --file ../my-project/test.cc
```

### Ask for advice in a session

The command below starts a new session (`--new`), which tracks responses,
before running the `agent helpme` command:

```sh
agent helpme <REQUEST> --file <PATH_TO_FILE> --new
```

For example:

```sh
agent helpme write a draft of all features found in this README file? --file ./README.md --new
```

After starting a session, use the `--cont` flag to include the previous
responses as context to the request:

```sh
agent helpme <REQUEST> --cont
```

For example:

```sh
agent helpme write a concept doc that delves into more details of these features? --cont
```

### Ask for advice using RAG

The command below uses a local or online vector database (specified in
the `config.yaml` file) to retrieve relevant context for the request:

```sh
agent helpme <REQUEST> --file <PATH_TO_FILE> --rag
```

## Managing online corpora

### List all existing online corpora

The command below prints the list of all existing online corpora created
using the [Semantic Retrieval API][semantic-api]:

```sh
agent list-corpora
```

### Share an online corpora with a user

The command below enables `user01@gmail.com` to read text chunks stored in
`corpora/example01`:

```sh
agent share-corpus --name corpora/example01 --user user01@gmail.com --role READER
```

The command below enables `user01@gmail.com` to read and write to
`corpora/example01`:

```sh
agent share-corpus --name corpora/example01 --user user01@gmail.com --role WRITER
```

### Share an online corpora with everyone

The command below enables `EVERYONE` to read text chunks stored in
`corpora/example01`:

```sh
agent open-corpus --name corpora/example01
```

### Remove a user permission from an online corpora

The command below remove an existing user permission set in `corpora/example01`:

```sh
agent remove-corpus-permission --name corpora/example01/permissions/123456789123456789
```

### Delete an online corpora

The command below deletes an online corpus:

```sh
agent delete-corpus --name corpora/example01
```

<!-- Reference links -->

[config-yaml]: ../config.yaml
[benchmarks-yaml]: ../docs_agent/benchmarks/benchmarks.yaml
[set-up-docs-agent-cli]: ../docs_agent/interfaces/README.md
[semantic-api]: https://ai.google.dev/docs/semantic_retriever
