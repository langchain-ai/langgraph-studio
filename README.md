![LangGraph Studio](./cover.svg)

# LangGraph Studio

LangGraph Studio is a desktop app for prototyping and debugging LangGraph applications locally. LangGraph visualizes your agent graph, lets you interact with the agent, and lets you modify the agent state directly. It speeds up development by giving developers more insight into what the agent looks like, the steps it is taking, and allowing you to modify the agent halfway through and then test out those changes.

![](./img/intro.gif)

## Setup

To use LangGraph Studio, make sure you have a [project with a LangGraph app](https://langchain-ai.github.io/langgraph/cloud/deployment/setup/) set up.

For this example, we will use this example repository [here](https://github.com/langchain-ai/langgraph-example):

```shell
git clone https://github.com/langchain-ai/langgraph-example.git
```

You will then want to create a `.env` file with the relevant environment variables:

```shell
cp .env.example .env
```

You should then open up the `.env` file and fill in with relevant OpenAI, Anthropic, and [Tavily](https://app.tavily.com/sign-in) API keys.

Once you've set up the project, you can use it in LangGraph Studio. Let's dive in!

## Open a project

When you open LangGraph Studio desktop app for the first time, you need to login via LangSmith.

![Login Screen](./img/login_screen.png)

Once you have successfully authenticated, you can choose the LangGraph application folder to use — you can either drag and drop or manually select it in the file picker. If you are using the example project, the folder would be `langgraph-example`.

> [!IMPORTANT] 
> The application directory you select needs to contain correctly configured `langgraph.json` file. See more information on how to configure it [here](https://langchain-ai.github.io/langgraph/cloud/reference/cli/#configuration-file) and how to set up a LangGraph app [here](https://langchain-ai.github.io/langgraph/cloud/deployment/setup/).

![Select Project Screen](./img/select_project_screen.png)

Once you select a valid project, LangGraph Studio will start a LangGraph API server and you should see a UI with your graph rendered.

![Graph Screen](./img/graph_screen.png)

## Invoke graph

Now we can run the graph! LangGraph Studio lets you run your graph with different inputs and configurations.

### Start a new run

To start a new run:

1. In the dropdown menu (top-left corner of the left-hand pane), select a graph. In our example the graph is called `agent`. The list of graphs corresponds to the `graphs` keys in your `langgraph.json` configuration.
1. In the bottom of the left-hand pane, edit the `Input` section.
1. Click `Submit` to invoke the selected graph.
1. View output of the invocation in the right-hand pane.

The following video shows how to start a new run:

https://github.com/user-attachments/assets/e0e7487e-17e2-4194-a4ad-85b346c2f1c4

### Configure graph run

To change configuration for a given graph run, press `Configurable` button in the `Input` section. Then click `Submit` to invoke the graph.

> [!IMPORTANT] 
> In order for the `Configurable` menu to be visible, make sure to specify `config_schema` when creating `StateGraph`. You can read more about how to add config schema to your graph [here](https://langchain-ai.github.io/langgraph/cloud/how-tos/cloud_examples/configuration_cloud/).
   
The following video shows how to edit configuration and start a new run:



https://github.com/user-attachments/assets/8495b476-7e33-42d4-85cb-2f9269bea20c


## Create and edit threads

### Create a thread

When you open LangGraph Studio, you will automatically be in a new thread window. If you have an existing thread open, follow these steps to create a new thread:

1. In the top-right corner of the right-hand pane, press `+` to open a new thread menu.

The following video shows how to create a thread:

https://github.com/user-attachments/assets/78d4a692-2042-48e2-a7e2-5a7ca3d5a611

### Select a thread

To select a thread:

1. Click on `New Thread` / `Thread <thread-id>` label at the top of the right-hand pane to open a thread list dropdown.
1. Select a thread that you wish to view / edit.

The following video shows how to select a thread:

https://github.com/user-attachments/assets/5f0dbd63-fa59-4496-8d8e-4fb8d0eab893

### Edit thread state

LangGraph Studio allows you to edit the thread state and fork the threads to create alternative graph execution with the updated state. To do it:

1. Select a thread you wish to edit.
1. In the right-hand pane hover over the step you wish to edit and click on "pencil" icon to edit.
1. Make your edits.
1. Click `Fork` to update the state and create a new graph execution with the updated state.

The following video shows how to edit a thread in the studio:

https://github.com/user-attachments/assets/47f887e7-2e3f-46ce-977c-f474c3cd797e

## How to add interrupts to your graph

You might want to execute your graph step by step, or stop graph execution before/after a specific node executes. You can do so by adding interrupts. Interrupts can be set for all nodes (i.e. walk through the agent execution step by step) or for specific nodes. An interrupt in LangGraph Studio means that the graph execution will be interrupted both before and after a given node runs.

### Add interrupts to a list of nodes

To walk through the agent execution step by step, you can add interrupts to a all or a subset of nodes in the graph:

1. In the dropdown menu (top-right corner of the left-hand pane), click `Interrupt`.
2. Select a subset of nodes to interrupt on, or click `Interrupt on all`.

The following video shows how to add interrupts to all nodes:

https://github.com/user-attachments/assets/db44ebda-4d6e-482d-9ac8-ea8f5f0148ea

### Add interrupt to a specific node

1. Navigate to the left-hand pane with the graph visualization.
1. Hover over a node you want to add an interrupt to. You should see a `+` button show up on the left side of the node.
1. Click `+` to invoke the selected graph.
1. Run the graph by adding `Input` / configuration and clicking `Submit`

The following video shows how to add interrupts to a specific node:

https://github.com/user-attachments/assets/13429609-18fc-4f21-9cb9-4e0daeea62c4

To remove the interrupt, simply follow the same step and press `x` button on the left side of the node.

## Edit project config

LangGraph Studio allows you to modify your project config (`langgraph.json`) interactively.

To modify the config from the studio, follow these steps:

1. Click `Configure` on the bottom right. This will open an interactive config menu with the values that correspond to the existing `langgraph.json`.
1. Make your edits.
1. Click `Save and Restart` to reload the LangGraph API server with the updated config.

The following video shows how to edit project config from the studio:

https://github.com/user-attachments/assets/86d7d1f7-800c-4739-80bc-8122b4728817

## Edit graph code

With LangGraph Studio you can modify your graph code and sync the changes live to the interactive graph.

To modify your graph from the studio, follow these steps:

1. Click `Open in VS Code` on the bottom right. This will open the project that is currently opened in LangGraph studio.
1. Make changes to the `.py` files where the compiled graph is defined or associated dependencies.
1. LangGraph studio will automatically reload once the changes are saved in the project directory.

The following video shows how to open code editor from the studio:

https://github.com/user-attachments/assets/8ac0443d-460b-438e-a379-182ec9f68ff5
