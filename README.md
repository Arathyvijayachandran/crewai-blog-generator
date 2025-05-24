# Multi-Agent Blog Creator with CrewAI
This project is a Streamlit-based application that uses the CrewAI multi-agent framework powered by OpenAI GPT models to automatically generate, write, and edit a blog post based on a given topic.

## Features
Accepts a blog topic from the user.

Uses three AI agents:

Planner: Researches and outlines the content.

Writer: Composes the full article based on the plan.

Editor: Proofreads and finalizes the blog post.

Entire blog is generated in a single click with collaboration between agents.

Returns a complete Markdown-formatted blog.

# 🧩 Project Structure
app.py
This is the main Streamlit app file. It does the following:

Takes user input for the blog topic.

Initializes CrewAI agents with defined roles:

Content Planner

Content Writer

Editor

Defines tasks for each agent based on their roles.

Runs the Crew using the Crew.kickoff() method.

Displays the final generated blog post using Streamlit.

