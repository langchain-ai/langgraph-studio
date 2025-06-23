![LangGraph Studio](./cover.svg)

# LangGraph Studio

LangGraph Studio is a specialized agent IDE that enables visualization, interaction, and debugging of agentic systems that implement the LangGraph Server API protocol. Studio also integrates with LangSmith to enable tracing, evaluation, and prompt engineering.

![](img/lg_studio.png)

## Features

Key features of LangGraph Studio:

- Visualize your graph architecture
- [Run and interact with your agent](https://langchain-ai.github.io/langgraph/cloud/how-tos/invoke_studio/)
- [Manage assistants](https://langchain-ai.github.io/langgraph/cloud/how-tos/studio/manage_assistants/)
- [Manage threads](https://langchain-ai.github.io/langgraph/cloud/how-tos/threads_studio/)
- [Iterate on prompts](https://langchain-ai.github.io/langgraph/cloud/how-tos/iterate_graph_studio/)
- [Run experiments over a dataset](https://langchain-ai.github.io/langgraph/cloud/how-tos/studio/run_evals/)
- Manage [long term memory](https://langchain-ai.github.io/langgraph/concepts/memory/)
- Debug agent state via [time travel](https://langchain-ai.github.io/langgraph/concepts/time-travel/)

LangGraph Studio works for graphs that are deployed on [LangGraph Platform](https://langchain-ai.github.io/langgraph/cloud/quick_start/) or for graphs that are running locally via the [LangGraph Server](https://langchain-ai.github.io/langgraph/tutorials/langgraph-platform/local-server/).

Studio supports two modes:

### Graph mode

Graph mode exposes the full feature-set of Studio and is useful when you would like as many details about the execution of your agent, including the nodes traversed, intermediate states, and LangSmith integrations (such as adding to datasets an playground).

### Chat mode

Chat mode is a simpler UI for iterating on and testing chat-specific agents. It is useful for business users and those who want to test overall agent behavior. Chat mode is only supported for graph's whose state includes or extends [`MessagesState`](https://langchain-ai.github.io/langgraph/how-tos/graph-api/#messagesstate).

## Learn more

- See this guide on how to [get started](http://127.0.0.1:8000/langgraph/cloud/how-tos/studio/quick_start/) with LangGraph Studio.

## Troubleshooting

### Desktop App (deprecated)

Note: The MacOS Studio desktop app, which was previously the main way to run Studio, has now been deprecated. Please use the command-line interface (`langgraph up` or `langgraph dev`) to create a compatible server and interact with the studio. The CLI is compatible with MacOS, Windows, and Linux and supports a superset of the functionality originally built into this desktop app.

#### How do I access local services and models such as Ollama, Chroma, etc?

LangGraph Studio relies on Docker Compose to run the API, Redis and Postgres, which in turn creates its own network. Thus, to access local services you need to use `host.docker.internal` as the hostname instead of `localhost`. See [#112](https://github.com/langchain-ai/langgraph-studio/issues/112) for more details.
3
### Failing to install native dependencies during build

By default, we try to make the image as small as possible, thus some dependencies such as `gcc` or `build-essentials` are missing from the base image. If you need to install additional dependencies, you can do so by adding additional Dockerfile instructions in the `dockerfile_lines` section of your `langgraph.json` file:

```
{
    "dockerfile_lines": [
        "RUN apt-get update && apt-get install -y gcc"
    ]
}
```

See [How to customize Dockerfile](https://langchain-ai.github.io/langgraph/cloud/deployment/custom_docker) for more details.

###  Safari Connection Issues

Safari blocks plain-HTTP traffic on localhost. When running Studio with `langgraph dev`, you may see "Failed to load assistants" errors.

#### Solution 1: Use Cloudflare Tunnel

=== "Python"

    ```shell
    pip install -U langgraph-cli>=0.2.6
    langgraph dev --tunnel
    ```

=== "JS"

    ```shell
    # Requires @langchain/langgraph-cli>=0.0.26
    npx @langchain/langgraph-cli dev
    ```

The command outputs a URL in this format:

```shell
https://smith.langchain.com/studio/?baseUrl=https://hamilton-praise-heart-costumes.trycloudflare.com
```

Use this URL in Safari to load Studio. Here, the `baseUrl` parameter specifies your agent server endpoint.

#### Solution 2: Use Chromium Browser

Chrome and other Chromium browsers allow HTTP on localhost. Use `langgraph dev` without additional configuration.

###  Brave Connection Issues

Brave blocks plain-HTTP traffic on localhost when Brave Shields are enabled. When running Studio with `langgraph dev`, you may see "Failed to load assistants" errors.

#### Solution 1: Disable Brave Shields

Disable Brave Shields for LangSmith using the Brave icon in the URL bar.

![Brave Shields](./img/brave-shields.png)

#### Solution 2: Use Cloudflare Tunnel

=== "Python"

    ```shell
    pip install -U langgraph-cli>=0.2.6
    langgraph dev --tunnel
    ```

=== "JS"

    ```shell
    # Requires @langchain/langgraph-cli>=0.0.26
    npx @langchain/langgraph-cli dev
    ```

The command outputs a URL in this format:

```shell
https://smith.langchain.com/studio/?baseUrl=https://hamilton-praise-heart-costumes.trycloudflare.com
```

Use this URL in Brave to load Studio. Here, the `baseUrl` parameter specifies your agent server endpoint.

### Graph Edge Issues

Undefined conditional edges may show unexpected connections in your graph. This is
because without proper definition, LangGraph Studio assumes the conditional edge could access all other nodes. To address this, explicitly define the routing paths using one of these methods:

#### Solution 1: Path Map

Define a mapping between router outputs and target nodes:

=== "Python"

    ```python
    graph.add_conditional_edges("node_a", routing_function, {True: "node_b", False: "node_c"})
    ```

=== "Javascript"

    ```ts
    graph.addConditionalEdges("node_a", routingFunction, { true: "node_b", false: "node_c" });
    ```

#### Solution 2: Router Type Definition (Python)

Specify possible routing destinations using Python's `Literal` type:

```python
def routing_function(state: GraphState) -> Literal["node_b","node_c"]:
    if state['some_condition'] == True:
        return "node_b"
    else:
        return "node_c"
```

