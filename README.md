# Natural-Home-Remedies
A simple yet powerful Streamlit-based app that helps users find natural, chemical-free treatments for common health issues. Just enter your name, age, and health condition, and the app provides safe and effective home remedies. It also supports Urdu translation of remedies, making it more user-friendly and accessible.

import streamlit as st
import pandas as pd
from googletrans import Translator

st.set_page_config(page_title="Home Remedies Assistant", page_icon="🌿", layout="centered")

# Title
st.title("🌿 Home Remedies Assistant")

# Intro
st.markdown("""
Welcome to the Home Remedies Assistant – your guide to safe, natural, and chemical-free treatments for common health issues.  
These remedies use ingredients found easily at home, helping you avoid harsh chemicals and costly medications.  
Embrace a healthier lifestyle with trusted, affordable solutions passed down through generations.
---
""")

# Load dataset
try:
    df = pd.read_excel("Complete_Home_Remedies_Dataset.xlsx")
except Exception:
    st.error("Error loading dataset. Please keep the file name exactly 'Complete_Home_Remedies_Dataset.xlsx' and try again.")
    st.stop()

# Inputs
name = st.text_input("Enter your name:")
age = st.text_input("Enter your age:")

if name and age:
    st.write(f"Hello **{name}** (age {age})! I'm here to help you with home remedies.")

    issue = st.text_input("Enter your health issue (or type 'no' to exit):")

    if issue:
        if issue.lower() == "no":
            st.success(f"Thank you for using the Home Remedies Assistant, {name}! Stay healthy! 🌟")
        else:
            results = df[df["Condition"].str.lower() == issue.lower()]
            if results.empty:
                st.warning(f"Sorry {name}, no remedy found for '{issue}'.")
                st.info(f"Thank you for using the Home Remedies Assistant, {name}! Stay healthy!")
            else:
                st.subheader(f"Home Remedies for {issue.title()}:")
                translate = st.radio("Would you like to see the remedies in Urdu?", ("No", "Yes"))
                translator = Translator()

                for _, row in results.iterrows():
                    st.markdown(f"**{row['Method']}**: {row['Details']}")
                    if translate == "Yes":
                        try:
                            ur = translator.translate(row["Details"], dest="ur").text
                            st.markdown(f"*Urdu Translation*: {ur}")
                        except Exception:
                            st.warning("Urdu translation failed. Please check your internet connection or try again later.")

                st.success(f"Thank you for using the Home Remedies Assistant, {name}! Stay healthy! 🌿")
else:
    st.info("Please enter your name and age to continue.")
