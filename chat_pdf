
import os
import tempfile
import streamlit as st
from dotenv import load_dotenv  # For loading environment variables
from embedchain import App

# Load environment variables from .env file
load_dotenv()

def embedchain_bot(db_path, api_key):
    return App.from_config(
        config={
            "llm": {"provider": "openai", "config": {"api_key": api_key}},
            "vectordb": {"provider": "chroma", "config": {"dir": db_path}},
            "embedder": {"provider": "openai", "config": {"api_key": api_key}},
        }
    )

st.title("Chat with PDF")

openai_access_token = os.getenv("OPENAI_API_KEY")

if openai_access_token:
    db_path = tempfile.mkdtemp()
    app = embedchain_bot(db_path, openai_access_token)

    pdf_file = st.file_uploader("Upload a PDF file", type="pdf")

    if pdf_file:
        with tempfile.NamedTemporaryFile(delete=False, suffix=".pdf") as f:
            f.write(pdf_file.getvalue())
            app.add(f.name, data_type="pdf_file")
        os.remove(f.name)
        st.success(f"Added {pdf_file.name} to knowledge base!")

    prompt = st.text_input("Ask a question about the PDF")

    if prompt:
        answer = app.chat(prompt)
        st.write(answer)

# =======
# >>>>>>> c4c23c6701730191dcca2308935dfd4238e39338
st.markdown("Built by Farah")
