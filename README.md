# Multimodal RAG

## Problem Statement
### Competition to Improve the Accuracy of Natural Language Processing<br>
Currently, RAG chatbots utilizing generative AI are attracting attention. However, RAG sometimes faces issues with response accuracy, leading to hallucinations or incorrect answers. Therefore, this competition focuses on the preprocessing pipeline of RAG to improve its accuracy.<br>
### Duration
The competition period is two weeks, from July 1 to July 11.<br>
Four members will each develop a preprocessing pipeline. Evaluations will be conducted on July 12.<br>
### Rules
Extract data from a specific PDF and complete the preprocessing pipeline. The sample PDF is the "Attention is All You Need" paper (link).
The PDF includes tables, figures, and graphs. These must be properly incorporated into the preprocessing pipeline, cropped to the appropriate size, and made suitable for input into an LLM (Large Language Model).
The preprocessing pipeline should enhance accuracy by adding summaries to the original text and incorporating various elements and ideas such as questions and keywords.
You can use either a laptop or Google Colab as your environment.
For commercial AI, you can use ChatGPT3.5 and Embedding E5.
For other AIs, use OSS (Open Source Software) AIs with no specific restrictions.<br>
 
Evaluation Test
The dataset for evaluating the pipeline processing will be prepared by Otsuka Corporation.
The evaluation test dataset's PDF is confidential but is similar to the "Attention is All You Need" paper.
Evaluation will mainly focus on accuracy, but efforts and ideas will also be considered.<br>
- Accuracy: 60%
- Ideas: 20%
- Technicality and completeness: 20%<br>
<!-- -->
Evaluation will be conducted by using the preprocessing pipeline to search with E5 and determining if GPT3.5 can answer up to 40 natural language questions. The evaluation method will use the overall score of RAGAS.
The questions will be distributed as follows:<br>
- Natural language questions (questions answerable within 1024 bytes): 25%<br>
- Questions from tables: 25%<br>
- Questions from graphs/figures: 25%<br>
- Complex questions (questions requiring references to multiple chunks): 25%<br>
### Sample Questions
Who are the authors of "Attention Is All You Need"?<br>
What are the BLEU score and training cost of the ByteNet [18] model?<br>
Explain the flow when Q and K are input in Scaled Dot-Product Attention.<br>
### Results
The first-place winner will be invited to dinner and awarded at the final day party (July 12, 18:00~).
Additionally, if you wish to join Otsuka Corporation, the results of this competition will be considered.

## Proposed Solution
The main objective was to preprocess PDF files of scientific papers, extract images, tables, text and equations and create a Multimodal Retrieval Augmented Generation (RAG) chatbot using Langchain, OpenAI ChatGPT 3.5 Turbo, MiniCPM-Llama3-V-2_5 (VLM) and OpenAI text embedding-3-large. I made use of open-source software like Marker and Nougat to parse through pdf files and 
convert it into a markdown format. Implemented a Hierarchical Chunking strategy to chunk the pdf files and store it in a ChromaDB along with its embeddings generated using OpenAI text-embedding-3-large. Designed and developed an RAG pipeline using Ensemble Retrieval methods and generated image context using the open-source VLM MiniCPM-Llama3-V-2_5.<br><br>
![Pipeline Overview](https://github.com/RasButAss/Multimodal-RAG/blob/main/images/RAG.drawio.github3.png)
The PDFs were parsed into markdowns using the Marker and Nougat models.​ The Marker model was able to extract images and tables from pdfs and parse the pdf into markdown format.​ The Nougat model was able to extract important mathematical equations and complex tables and parse the pdfs into a special mathpix markdown format. An additional preprocessing step was done to get a detailed description of images with relevant context provided. All the images extracted from the markdown files were sent over to a VLM to generate individual image descriptions.<br>
![Preprocessing](https://github.com/RasButAss/Multimodal-RAG/blob/main/images/RAG.drawio.github1.png)<br>
Input question is queried into a Parent Document Retriever and a Multiquery Retriever​. The relevant context is retrieved through the two different retrievers. Relevance Embedding Filter and Redundant Embedding Filter was used for Contextual Compression.​<br>
![Retrieval](https://github.com/RasButAss/Multimodal-RAG/blob/main/images/RAG.drawio.github2.png)<br>
The user input is passed through the retriever to get the relevant context.​ A check is made if there is an image in retrieved context.​ If there is an image we then query the VLM to generate a summary of the image.​ If no image was found then we just pass the context onto the system prompt​. This processing is illustrated in the figure below.<br>
![Pipeline Overview](https://github.com/RasButAss/Multimodal-RAG/blob/main/images/RAG.drawio.github3.png)
