import streamlit as st
import pandas as pd
import random

# Load data files
mood_df = pd.read_csv("mood_tracker.csv")
tasks_df = pd.read_csv("tasks.csv")
career_df = pd.read_csv("career_tips.csv")
support_df = pd.read_csv("emotional_support.csv")

# Set page title
st.set_page_config(page_title="Settlee: Your AI Companion")
st.title("🌸 Settlee - Your AI Companion")
st.markdown("Helping you navigate solo travel, career re-entry & emotional wellness 💼✈️💖")

# --- Mood Tracker Section ---
st.header("🧠 Mood Journal")
mood_input = st.selectbox("How are you feeling today?", ["", "Nervous", "Excited", "Overwhelmed", "Happy", "Lonely", "Curious"])
notes_input = st.text_area("Write a short note about your day")
if st.button("Get Tip Based on Mood"):
    if mood_input:
        match = mood_df[mood_df['Mood'] == mood_input]
        if not match.empty:
            tip = match.iloc[0]['Mood-Based Action']
        else:
            tip = "Take a deep breath. You're doing great 💛"
        st.success(f"Suggested Action: {tip}")

# --- Affirmation Section ---
st.header("💬 Daily Affirmation")
affirmations = [
    "You are capable of amazing things!",
    "One step at a time — you're getting there.",
    "Your courage is stronger than your fear.",
    "You are not alone. You've got this."
]
if st.button("Show Me An Affirmation"):
    st.info(random.choice(affirmations))

# --- Career Tips ---
st.header("💼 Career Prep Tips")
selected_cat = st.selectbox("Choose a topic:", ["All"] + list(career_df['Category'].unique()))
filtered_tips = career_df if selected_cat == "All" else career_df[career_df['Category'] == selected_cat]
for _, row in filtered_tips.iterrows():
    st.subheader(row['Tip Name'])
    st.write(row['Tip Description'])
    st.markdown(f"[Resource Link]({row['Link to Resource']})")

# --- Task Tracker ---
st.header("🗂️ Your Task Board")
status_filter = st.selectbox("Filter by status:", ["All"] + list(tasks_df['Status'].unique()))
filtered_tasks = tasks_df if status_filter == "All" else tasks_df[tasks_df['Status'] == status_filter]
st.dataframe(filtered_tasks[['Task Name', 'Priority', 'Due Date', 'Category', 'Status']])

# --- Emotional Support Library ---
st.header("🌿 Emotional Support Tips")
support_type = st.selectbox("Choose a category:", ["All"] + list(support_df['Category'].unique()))
filtered_support = support_df if support_type == "All" else support_df[support_df['Category'] == support_type]
for _, row in filtered_support.iterrows():
    st.subheader(row['Tip Name'])
    st.write(row['Description'])
    st.markdown(f"[Try This]({row['Link to Resource']})")

st.markdown("---")
st.caption("Designed with 💜 by you, for every woman finding her new beginning.")
