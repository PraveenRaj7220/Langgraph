LangGraph Agentic Workflow with Together API
This project implements a dual-loop agentic workflow using LangGraph and Together API, inspired by the architecture diagram provided. It processes a user query by decomposing it into subtasks, solving them one by one, validating results, and allowing user refinement.

🧠 Architecture Overview
Outer Loop: Planning & Iteration
Takes a User Query and breaks it into subtasks via the PlanAgent.

Subtasks are solved one-by-one using a ToolAgent.
After solving, the ReflectionAgent (an LLM call) validates the results.
Based on the feedback, subtasks can be:

Modified
Deleted
New tasks added

The user is also prompted to manually refine the task list after each full iteration.

Inner Loop: Task Execution
Each subtask is passed to the ToolAgent.

If the result includes Python code, it is:
Dynamically executed using exec().
Output captured and stored.
Then, the ReflectionAgent evaluates the result to decide if the task is complete or needs refinement.

🛠 Technologies Used
LangGraph – Building modular agent workflows.

Together API – Using mistralai/Mistral-7B-Instruct-v0.1 to generate responses.

Python – Executes code when required.

Requests – Handles API calls to Together.

Try/Except Handling – Captures any code execution errors cleanly.

🔐 Setting the API Key
Your Together API key is required for all LLM interactions.

In the code, it's stored like this:

python
Copy
Edit
TOGETHER_API_KEY = "your_api_key_here"

🔒 Security Note:
Do not hardcode secrets in production environments. Use .env files or secret managers (e.g., Python dotenv, GitHub secrets, or Vault) for better safety.

🔄 Execution Flow
Install Required Libraries

bash
Copy
Edit
pip install langgraph requests
Run the Notebook / Script

python
Copy
Edit
# Everything is initiated with:
langgraph_workflow()
Query Example

python
Copy
Edit
query = "Analyze sentiment from a set of tweets and visualize results with a bar graph"
Subtask Planning

LLM splits your query into 2–5 atomic tasks:

pgsql
Copy
Edit
[1] Gather tweets
[2] Perform sentiment analysis
[3] Visualize using a bar graph
Subtask Execution

Each subtask is processed with:

🛠 ToolAgent – Calls Together API.

🧠 ReflectionAgent – Validates the result.

✅ Code is automatically run (if generated) and output saved.

Manual Refinement Loop

After each full iteration, the system prompts:

text
Copy
Edit
🧠 Would you like to add / modify / delete any task? (type 'no' to finish):
Add new subtasks

Modify existing ones

Delete tasks

This continues until you type no.

📦 Output Format
After each iteration, results are clearly printed:

csharp
Copy
Edit
📦 Final Task Outputs:

[1] Gather tweets from Twitter
📝 Result:
['"Great product!"', '"Horrible service."', ...]

[2] Perform sentiment analysis
📝 Result:
{"Positive": 3, "Negative": 2, "Neutral": 1}

[3] Visualize with bar graph
📝 Result:
<matplotlib bar chart or URL to graph>
Each result field stores both the final response and any execution results (as a dictionary).

✅ Features
Modular: Plug in different LLMs or toolchains.

Agentic: Uses reflection and feedback to improve accuracy.

Transparent: You see every subtask and can guide the system.

Executable: Handles Python execution safely.

🧪 Example Use Case
Query: "Analyze sentiment from a set of tweets and visualize results."

Automatically becomes:

Gather sample tweets

Analyze their sentiment

Visualize results

Format the plot

Interpret the graph

Each step runs through tool agents, validations, and optional corrections.

📤 Suggested Enhancements
Save all outputs to .json or .md reports.

Store execution logs for reproducibility.

Add memory to improve long-chain reasoning (e.g., LangChain agents or ReAct).

