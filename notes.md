# Function Explanations

## 1. `get_text_chunks(raw_text)`
- **Purpose**:  
  Splits raw PDF text into smaller, manageable chunks for processing.
- **How it Works**:  
  - Uses `CharacterTextSplitter` from LangChain to divide text into chunks of 1000 characters.
  - Includes a 200-character overlap to ensure continuity between chunks.
- **Output**:  
  A list of text chunks.

---

## 2. `get_vectorstore(text_chunks)`
- **Purpose**:  
  Converts text chunks into embeddings and stores them in a vector database for semantic search.
- **How it Works**:  
  - Generates embeddings using `OpenAIEmbeddings`.
  - Stores these embeddings in a **FAISS vector store** for fast similarity-based retrieval.
- **Output**:  
  A **FAISS vector store object** containing the embeddings.

---

## 3. `get_conversation_chain(vectorstore)`
- **Purpose**:  
  Creates a conversational retrieval chain to handle user queries.
- **How it Works**:  
  - Initializes the **ChatOpenAI** model for conversational AI.
  - Uses `ConversationBufferMemory` to maintain chat history for follow-up questions.
  - Combines the ChatOpenAI model with the FAISS retriever using `ConversationalRetrievalChain`.
- **Output**:  
  A conversational chain that integrates retrieval and generative AI.

---

## 4. `get_pdf_text(pdf_docs, page_number=None)`
- **Purpose**:  
  Extracts text from uploaded PDFs, either from specific pages or the entire document.
- **How it Works**:  
  - Uses **PyPDF2’s PdfReader** to iterate through uploaded PDFs.
  - If `page_number` is provided, extracts text from that page.
  - If no `page_number` is provided, processes all pages in the PDF.
  - Displays warnings if pages are unreadable or empty.
- **Output**:  
  A **string** containing the extracted text from the PDFs.

---

## 5. `text_to_speech(text)`
- **Purpose**:  
  Converts the bot’s response into speech and plays the audio.
- **How it Works**:  
  - Uses **gTTS (Google Text-to-Speech)** to convert text into an MP3 audio file.
  - Saves the audio file in a temporary directory.
  - Plays the audio using **Streamlit’s st.audio**.
- **Output**:  
  Plays an audio version of the input text.

---

## 6. `handle_userinput(user_question)`
- **Purpose**:  
  Processes the user’s question, generates a response, and displays it in the chat interface.
- **How it Works**:  
  1. Displays a typing indicator while processing the question.
  2. Initializes or retrieves chat history from `st.session_state`.
  3. Passes the question and chat history to the conversational chain.
  4. Updates the chat history with the response.
  5. Displays chat messages using custom HTML templates (`user_template` and `bot_template`).
  6. Converts the bot’s response to audio using `text_to_speech`.
- **Output**:  
  Displays chat messages and plays the bot’s response.

---

## 7. `main()`
- **Purpose**:  
  Acts as the entry point for the Streamlit app, defining the overall user interface and workflow.
- **How it Works**:  
  1. **Load API Key**:  
     - Loads the OpenAI API key from a `.env` file using `dotenv`.
     - Displays an error if the key is missing.
  2. **Set Up the App**:  
     - Configures the Streamlit page and sidebar.  
     - Allows users to enable background music and set avatar URLs.
  3. **Process User Inputs**:  
     - Lets users upload PDFs.  
     - Extracts text, splits it into chunks, and creates a vector store.  
     - Sets up the conversational chain (`st.session_state.conversation`) to answer questions.
  4. **Chat Interface**:  
     - Provides a text input for user questions.
     - Processes questions using `handle_userinput`.
     - Allows users to specify page numbers for targeted text extraction.
- **Output**:  
  Renders the Streamlit app, enabling users to upload documents, ask questions, and interact with responses.
