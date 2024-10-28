# **YouTube Video Search Application**

This application allows users to **extract transcripts from YouTube videos**, **create searchable vector databases**, and **perform semantic searches** using a **Gradio web interface** or **command-line interface (CLI)**. It's powered by `faster-whisper` for transcription, `FAISS` for vector search, and `sentence-transformers` for text embeddings.

---

## **Features**
- Extract transcripts from individual videos or playlists.
- Automatically download video thumbnails.
- Store transcripts and create a searchable vector database.
- Perform semantic searches on video content.
- Supports **Gradio web interface** and **CLI** for flexible usage.
- Easily add more videos to the dataset.

---

## **Web Interface**
<img width="520" alt="image" src="https://github.com/user-attachments/assets/81c20089-f786-4743-a151-307560acbe1a">
<img width="526" alt="image" src="https://github.com/user-attachments/assets/a7e8fe0b-d6d7-436d-ab44-857f143035c3">


## **Installation**

Ensure you have Python installed (>= 3.7). Then, install the required dependencies:

```bash
pip install -r requirements.txt
```

---

## **Usage**

The app provides **two ways to interact**:  
1. **Gradio Web Interface**  
2. **Command-Line Interface (CLI)**

### **1. Running the Gradio Web Interface**

Launch the web interface:

```bash
python app.py ui
```

or simply:

```bash
python app.py
```

Then, open the URL (usually `http://127.0.0.1:7860`) in your browser.

#### **Gradio Interface Tabs:**

- **Add Videos:**  
  - Add videos from playlists or individual links to the dataset.
  - Videos will be transcribed, and the database will be updated with the content.
  
- **Search:**  
  - Enter search queries to find relevant snippets from the video transcripts.
  - Results are ranked based on semantic similarity and include video thumbnails.

---

### **2. Command-Line Interface (CLI)**

The CLI provides more flexibility for programmatic use.

#### **Commands Overview**

Use the `--help` command to view available commands and examples:

```bash
python app.py --help
```

**Output:**

```
usage: app.py [-h] {add,search,ui} ...

YouTube Video Search Application

positional arguments:
  {add,search,ui}   Available commands
    add             Add videos to the database
    search          Search the video database
    ui              Launch the Gradio web interface

optional arguments:
  -h, --help        Show this help message and exit

Examples:
  # Add videos from a playlist
  python app.py add --type playlist --input "https://www.youtube.com/playlist?list=YOUR_PLAYLIST_ID"

  # Add specific videos
  python app.py add --type videos --input "https://www.youtube.com/watch?v=dQw4w9WgXcQ,https://www.youtube.com/watch?v=9bZkp7q19f0"

  # Search the database with a query
  python app.py search --query "machine learning tutorials" --top_k 5

  # Run the Gradio web interface
  python app.py ui
```

---

### **Examples of CLI Usage**

#### **1. Adding Videos**

- **Add a Playlist:**

   ```bash
   python app.py add --type playlist --input "https://www.youtube.com/playlist?list=YOUR_PLAYLIST_ID"
   ```

- **Add Specific Videos:**

   ```bash
   python app.py add --type videos --input "https://www.youtube.com/watch?v=dQw4w9WgXcQ,https://www.youtube.com/watch?v=9bZkp7q19f0"
   ```

#### **2. Searching the Database**

- **Perform a Search:**

   ```bash
   python app.py search --query "machine learning tutorials" --top_k 5
   ```

---

### **How It Works**

1. **Adding Videos:**
   - The app downloads video audio and transcribes it using `faster-whisper`.
   - Thumbnails are downloaded and saved locally.
   - The transcript data is saved in `datasets/transcript_dataset.csv`.
   - A vector database is created using FAISS with embeddings generated by `sentence-transformers`.

2. **Rebuilding the Vector Database:**
   - Every time new videos are added, the vector database is rebuilt using the **entire dataset** to ensure all videos are searchable.

3. **Searching the Database:**
   - When a query is entered, the app computes its embedding and searches the FAISS index for relevant video snippets.
   - The top results are displayed with thumbnails, titles, and links to the videos.

---

### **FAQ**

#### **1. Why aren’t new videos showing up in search results?**
Make sure the entire dataset is being read and the vector database is being rebuilt after adding videos. This is already handled in the provided solution.

#### **2. How do I update the database with more videos?**
Simply add videos through the Gradio interface or use the `add` command via the CLI. The database and vector index will be automatically updated.

#### **3. Can I search the database without launching the Gradio interface?**
Yes! Use the `search` command via the CLI:

```bash
python app.py search --query "Your query" --top_k 5
```

---

### **Project Structure**

```
.
├── app.py                 # Main application script (Gradio + CLI)
├── lib/
│   └── functions.py        # Helper functions for transcription, FAISS, etc.
├── datasets/
│   └── transcript_dataset.csv  # CSV file storing transcripts
│   └── vector_index.faiss  # FAISS vector index
├── thumbnails/             # Folder for storing video thumbnails
```

---

### **Known Limitations**

- **Large Datasets:** As the dataset grows, building the vector database may take longer. For very large datasets, consider incremental indexing strategies.
- **FAISS Index Size:** Ensure you have enough storage for both the dataset and FAISS index as they grow.

---

### **Contributing**

Feel free to fork the repository, open issues, or submit pull requests if you'd like to contribute to this project.

---

### **License**

This project is licensed under the MIT License. See the LICENSE file for details.

---

### **Acknowledgments**

- **faster-whisper** for fast transcription.
- **FAISS** for efficient vector search.
- **Gradio** for the interactive web interface.
- **yt-dlp** for downloading video content.
