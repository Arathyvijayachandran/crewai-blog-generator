# crewai-blog-generator
import os
import streamlit as st
from crewai import Agent, Task, Crew

# ✅ Set your OpenAI API Key directly here
os.environ["OPENAI_API_KEY"] = "sk-xxxxx..."  # 🔐 Replace with your actual key
os.environ["OPENAI_MODEL_NAME"] = "gpt-3.5-turbo"

# Streamlit UI setup
st.set_page_config(page_title="CrewAI Blog Generator", layout="centered")
st.title("🧠 Multi-Agent Blog Creator with CrewAI")
st.markdown("Generate a complete blog article using multi-agent LLMs.")

# User input for topic
topic = st.text_input("Enter a blog topic", "Artificial Intelligence")

if st.button("Generate Blog"):
    if not topic.strip():
        st.error("Please enter a valid topic.")
    else:
        # Define agents
        planner = Agent(
            role="Content Planner",
            goal=f"Plan engaging and factually accurate content on {topic}",
            backstory="You're responsible for gathering insights and structuring content on the topic.",
            allow_delegation=False,
            verbose=True
        )

        writer = Agent(
            role="Content Writer",
            goal=f"Write an insightful opinion piece about {topic}",
            backstory="You use the planner’s outline to craft a compelling article.",
            allow_delegation=False,
            verbose=True
        )

        editor = Agent(
            role="Editor",
            goal="Edit the blog to ensure it’s clear, professional, and aligned with tone guidelines.",
            backstory="You make sure the article is grammatically correct and styled for publication.",
            allow_delegation=False,
            verbose=True
        )

        # Define tasks
        plan_task = Task(
            description=(
                f"Create a detailed content plan about '{topic}' including current trends, key points, "
                "SEO keywords, and target audience analysis."
            ),
            expected_output="A comprehensive content plan with an outline and insights.",
            agent=planner,
        )

        write_task = Task(
            description=(
                f"Write a full blog post using the plan about '{topic}'. Include intro, body, and conclusion. "
                "Incorporate SEO naturally and keep it engaging."
            ),
            expected_output="A complete markdown-formatted blog article.",
            agent=writer,
        )

        edit_task = Task(
            description="Proofread and refine the blog post to ensure clarity, flow, and correct grammar.",
            expected_output="A finalized blog article in markdown, ready for publishing.",
            agent=editor,
        )

        # Run the multi-agent crew
        crew = Crew(
            agents=[planner, writer, editor],
            tasks=[plan_task, write_task, edit_task],
            verbose=True
        )

        try:
            with st.spinner("🤖 Agents are working..."):
                result = crew.kickoff(inputs={"topic": topic})
            st.success("✅ Blog generated successfully!")
            st.markdown(result)
        except Exception as e:
            st.error(f"An error occurred: {e}")
