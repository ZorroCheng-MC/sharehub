# Make Better Charts with RAG on Video - Notes

**Video:** RAG on Video with LlamaIndex: Using Gemini 1.5 Pro and Flash  
**Source:** https://youtu.be/UCj5eHkhYg0?si=Fh_7fPNcT3Svvwbc  
**Date Captured:** 2026-03-06  
**Topics:** Retrieval-Augmented Generation, LlamaIndex, Google Gemini 1.5, Video Analysis, Multi-modal AI

---

## Overview

This video is a comprehensive tutorial on performing Retrieval-Augmented Generation (RAG) on video files using LlamaIndex and Google's Gemini 1.5 models (Pro and Flash). The tutorial demonstrates two distinct approaches:

1. **"Big Context" RAG** - Utilizing Gemini 1.5 Pro's 1M token context window to process entire videos for holistic tasks
2. **Indexed RAG** - Traditional RAG with frame extraction, vector embeddings, and targeted retrieval for Q&A

---

## Key Segments & Timestamps

| Timestamp | Topic | Description |
|-----------|-------|-------------|
| `0:00` | Introduction | RAG on video capabilities and Gemini 1.5 Pro's multi-modal understanding |
| `0:45` | Two Approaches | Overview of "Big Context" vs. Indexed RAG methods |
| `1:10` | Setup | Google Colab environment, library installation (`llama-index`, `moviepy`), API key config |
| `2:55` | Video Download | Demonstrating with Andrej Karpathy's Llama 3 launch video |
| `3:30` | **Demo 1: Big Context RAG** | ⭐ Loading video and structured summarization (Full video → JSON output) |
| `6:50` | **Demo 2: Indexed RAG (Pro)** | ⭐ Frame extraction, embedding, indexing for specific Q&A |
| `7:40` | Indexed RAG Code | Detailed walkthrough of VideoFrameReader, embeddings, VectorStoreIndex setup |
| `10:30` | Querying Demo | Running specific queries with Gemini 1.5 Pro against indexed frames |
| `12:45` | Gemini 1.5 Flash Intro | Faster, more cost-effective alternative to Pro |
| `13:10` | **Demo 3: Indexed RAG (Flash)** | ⭐ Same process with Flash model for comparison |
| `14:40` | Model Comparison | Pro vs. Flash performance and quality analysis |
| `15:40` | Conclusion | Summary of approaches and Pro/Flash trade-offs |

---

## Technical Demonstrations

### Demo 1: "Big Context" RAG (3:30) ⭐

**Goal:** Generate structured summary of entire video  
**Approach:** Pass entire video directly into Gemini 1.5 Pro's context

**Key Components:**
- `SimpleDirectoryReader` - Load video file
- `GeminiMultiModal` - LlamaIndex Gemini integration
- `MultiModalLLMCompletionProgram` with `PydanticOutputParser` - Force structured JSON output
- Pydantic schema `VideoSummary` with title, summary, key_takeaways

**Result:** Model successfully analyzes full video and returns clean JSON with comprehensive analysis

**Code Pattern:**
```python
class VideoSummary(BaseModel):
    title: str
    summary: str
    key_takeaways: List[str]

video_documents = SimpleDirectoryReader(input_files=["llama3.mp4"]).load_data()

program = MultiModalLLMCompletionProgram.from_defaults(
    output_parser=PydanticOutputParser(VideoSummary),
    multi_modal_llm=GeminiMultiModal(model_name="models/gemini-1.5-pro-latest"),
    prompt_template_str=prompt_template_str,
)

response = program(image_documents=video_documents)
```

---

### Demo 2: Indexed RAG with Gemini 1.5 Pro (6:50) ⭐

**Goal:** Answer specific time-based question: "When does Andrej start talking about the Llama 3 tokenizer?"  
**Approach:** Frame extraction → embeddings → indexed retrieval → synthesis

**Process:**
1. **Frame Extraction:** `VideoFrameReader` samples frames at intervals (e.g., 1 fps)
2. **Indexing:** `VectorStoreIndex` built from image documents using Gemini for embeddings
3. **Retrieval:** `QueryEngine` finds most relevant frames based on text query
4. **Synthesis:** LLM generates answer using retrieved frames as context

**Key Components:**
- `VideoFrameReader(interval=1)` - Extract one frame per second
- `GeminiMultiModal` as both embedding model and LLM
- `VectorStoreIndex.from_documents()` - Build index from frame documents
- `index.as_query_engine(similarity_top_k=5)` - Retrieve top 5 frames per query

**Code Pattern:**
```python
from llama_index.core import VectorStoreIndex, Settings
from llama_index.readers.file import VideoFrameReader
from llama_index.multi_modal_llms.gemini import GeminiMultiModal

# Configure Gemini for both LLM and embeddings
Settings.llm = GeminiMultiModal(model_name="models/gemini-1.5-pro-latest")
Settings.embed_model = GeminiMultiModal(model_name="models/gemini-1.5-pro-latest")

# Load frames (1 per second)
loader = VideoFrameReader(interval=1)
documents = loader.load_data(file_path="llama3.mp4")

# Create indexed vector store
index = VectorStoreIndex.from_documents(documents)

# Query engine
query_engine = index.as_query_engine(similarity_top_k=5)

# Execute query
response = query_engine.query("When does Andrej start talking about the Llama 3 tokenizer?")
```

**Result:** System correctly identifies and retrieves relevant frames with accurate, text-based answers

---

### Demo 3: Indexed RAG with Gemini 1.5 Flash (13:10) ⭐

**Goal:** Compare Gemini 1.5 Flash performance vs. Pro on same task  
**Difference:** Only model name changes from `gemini-1.5-pro-latest` to `gemini-1.5-flash-latest`

**To Switch Models:**
```python
# Change these lines to use Flash
Settings.llm = GeminiMultiModal(model_name="models/gemini-1.5-flash-latest")
Settings.embed_model = GeminiMultiModal(model_name="models/gemini-1.5-flash-latest")
```

**Result:** Significantly faster query response while maintaining high answer quality  
**Trade-off:** Slightly less nuanced responses compared to Pro, but acceptable for most use cases

---

## Environment Setup

**Installation:**
```bash
!pip install llama-index-multi-modal-llms-gemini llama-index-readers-file moviepy
```

**Configuration:**
```python
import os
from google.colab import userdata

os.environ["GOOGLE_API_KEY"] = userdata.get('GOOGLE_API_KEY')
```

---

## Key Technical Concepts

### Multi-Modal Native Support
- **Gemini 1.5 is natively multi-modal:** Can process video, audio, and images directly
- Simplifies architecture compared to models requiring separate vision components
- Same model can function as both LLM and embedding generator

### Two RAG Strategies Compared

| Aspect | Big Context RAG | Indexed RAG |
|--------|-----------------|------------|
| **Best For** | Holistic analysis, summarization | Scalable Q&A over large video corpus |
| **Scalability** | Limited (full video processing) | High (query-time efficiency) |
| **Cost** | Higher (processes entire context) | Lower (only processes relevant frames) |
| **Latency** | Slower (full analysis) | Faster (frame retrieval only) |
| **Use Case** | Single video summaries | Building searchable video libraries |

### Gemini 1.5 Pro vs. Flash

| Feature | Gemini 1.5 Pro | Gemini 1.5 Flash |
|---------|----------------|-----------------|
| **Context Window** | 1M tokens | 1M tokens |
| **Speed** | Slower (~baseline) | Significantly faster |
| **Cost** | Higher | Lower |
| **Accuracy** | Highest, most nuanced | Very high, slightly less nuance |
| **Best For** | High-accuracy requirements | Real-time apps, budget-conscious |

---

## Important Takeaways

### 1. Native Multi-Modal Architecture
- Gemini 1.5 handles video/audio/images natively
- No need for separate vision encoders
- Single model can embed and generate text

### 2. RAG Approach Selection
- **Use Big Context** for: summarizing single videos, holistic analysis
- **Use Indexed RAG** for: building Q&A systems, searching video libraries, scale operations
- Indexed RAG is superior for production scalability

### 3. Dual-Purpose Embeddings
- Gemini can simultaneously act as:
  - **LLM** - Generate answers (synthesis)
  - **Embedding Model** - Create frame vectors for similarity search
- This co-location is convenient and eliminates model compatibility issues

### 4. Frame Sampling Strategy
- 1 frame per second is effective baseline
- Adjust interval based on content (slower for static content, faster for dynamic)
- More frames = more context but slower indexing and querying

### 5. Production Considerations
- **For accuracy-critical:** Use Gemini 1.5 Pro
- **For speed/cost-critical:** Use Gemini 1.5 Flash
- **For real-time:** Flash model preferred despite minor quality tradeoff
- Both share same 1M token context window

### 6. Practical Application Pattern
```
Video → Frame Extraction → Embedding → Vector Index → User Query → Retrieval → Synthesis → Answer
```

---

## Related Technologies & Concepts

- **LlamaIndex** - Framework for indexing and querying with LLMs
- **VectorStoreIndex** - Vector database integration for semantic search
- **Pydantic** - Type validation and structured outputs
- **MultiModalLLMCompletionProgram** - LlamaIndex structured completion
- **VideoFrameReader** - Automated frame extraction utility
- **Gemini 1.5** - Google's latest multi-modal frontier model family

---

## Next Steps & Extensions

1. **Experiment with frame intervals** - Optimize sampling for different video types
2. **Build a video Q&A API** - Indexed RAG pattern scales to web service
3. **Compare with Flash for your use cases** - Test quality/speed tradeoff
4. **Extend to multi-video indexing** - Create searchable video library
5. **Implement caching** - Cache frequently accessed frames or queries

---

**Last Updated:** 2026-03-06  
**Status:** Complete capture with full technical demonstrations and code examples
