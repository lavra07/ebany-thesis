# ebany-thesis
import pandas as pd
import spacy
from collections import Counter
import docx

# Load the Spanish NLP model
nlp = spacy.load('es_core_news_sm')

# Data based on identified slang terms with translations
data = {
    'Spanish Slang': ["chavales", "tío", "tía", "vale", "mola", "venga", "guay", "flipar", "peña", "currar", 
                      "colega", "fiesta", "mono", "pasta", "chungo", "pilas", "rollo", "jefe", "jefa", "enrollado", "chapuza"],
    'English Translation': ["kids", "dude", "dude", "okay", "cool", "come on", "cool", "freak out", "group of friends", "work",
                            "buddy", "party", "cute", "money", "bad", "get ready", "vibe", "boss", "boss", "cool (easygoing)", "botched job"],
    'Russian Translation': ["дети", "парень", "парень", "ладно", "круто", "давай", "круто", "ошалеть", "группа друзей", "работать",
                            "друг", "вечеринка", "милый", "деньги", "плохо", "готовься", "атмосфера", "босс", "босс", "крутой", "халтура"]
}

# Create a DataFrame
df_slang = pd.DataFrame(data)

# Define a function to find slang terms in a document
def find_slang_in_document(document, slang_df):
    doc = nlp(document)
    results = []

    for token in doc:
        if token.text.lower() in slang_df['Spanish Slang'].values:
            slang_info = slang_df[slang_df['Spanish Slang'] == token.text.lower()]
            results.append({
                'Slang Term': token.text,
                'English Translation': slang_info['English Translation'].values[0],
                'Russian Translation': slang_info['Russian Translation'].values[0],
                'Context': token.sent.text
            })
    
    return pd.DataFrame(results)

# Read the document
def read_docx(file_path):
    doc = docx.Document(file_path)
    full_text = []
    for paragraph in doc.paragraphs:
        full_text.append(paragraph.text)
    return '\n'.join(full_text)

# Function to process the uploaded document and generate results
def process_document(uploaded_file_path):
    document_text = read_docx(uploaded_file_path
