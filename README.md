## 🤝 协作者

**我们欢迎社区做出贡献，以帮助增强 GraphRAG Local Ollama！请参阅我们的[贡献指南](CONTRIBUTING.md)，了解有关如何参与的更多详细信息。**

# 🚀 GraphRAG-Local-Ollama - 知识图谱

使用Ollama在本地运行GraphRAG

## 📄 参考论文

有关 GraphRAG 实现的更多详细信息，请参阅[GraphRAG 论文](https://arxiv.org/pdf/2404.16130).

**Paper Abstract**

The use of retrieval-augmented generation (RAG) to retrieve relevant information from an external knowledge source enables large language models (LLMs)to answer questions over private and/or previously unseen document collections.However, RAG fails on global questions directed at an entire text corpus, suchas “What are the main themes in the dataset?”, since this is inherently a queryfocused summarization (QFS) task, rather than an explicit retrieval task. PriorQFS methods, meanwhile, fail to scale to the quantities of text indexed by typicalRAG systems. To combine the strengths of these contrasting methods, we proposea Graph RAG approach to question answering over private text corpora that scaleswith both the generality of user questions and the quantity of source text to be indexed. Our approach uses an LLM to build a graph-based text index in two stages:first to derive an entity knowledge graph from the source documents, then to pregenerate community summaries for all groups of closely-related entities. Given aquestion, each community summary is used to generate a partial response, beforeall partial responses are again summarized in a final response to the user. For aclass of global sensemaking questions over datasets in the 1 million token range,we show that Graph RAG leads to substantial improvements over a na¨ıve RAGbaseline for both the comprehensiveness and diversity of generated answers. 

## 📦 安装与设置

按照以下步骤设置此存储库并将 GraphRag 与 Ollama 提供的本地模型一起使用：


1. **创建并激活新的 conda 环境：（请使用给定的 python 版本 3.10 以避免出现错误）**
    ```bash
    conda create -n graphrag-ollama-local python=3.10
    conda activate graphrag-ollama-local
    ```
    conda指令通过安装Anaconda, Miniconda, conda-forge等均可，我安装了Anaconda3

2. **安装 Ollama：**
    - 请访问[Ollama 官网](https://ollama.com/)获取安装说明
    - 或者运行：
    ```bash
    curl -fsSL https://ollama.com/install.sh | sh #ollama for linux
    pip install ollama
    ```

3. **使用 Ollama 下载所需的模型，我们可以从 llm 的（mistral、gemma2、qwen2）和 Ollama 下提供的任何嵌入模型中进行选择：**
    ```bash
    ollama pull mistral  #llm
    ollama pull nomic-embed-text  #embedding
    ```

4. **克隆存储库：**
    ```bash
    git clone https://github.com/Wusiyuan0330/graphrag-ollama.git
    ```

5. **导航到目录**
    ```bash
    cd graphrag-local-ollama/
    ```

6. **安装 graphrag 包 **这是最重要的一步：**
    ```bash
    pip install -e .
    ```


7. **创建所需的input目录：存储实验数据和结果 - ./ragtest**
    ```bash
    mkdir -p ./ragtest/input
    ```
    windows命令提示符中需要用"mkdir ragtest\input"
    
8. **将示例数据文件夹 input/ 复制到 ./ragtest。Input/ 包含运行设置的示例数据。您可以在此处以 .txt 格式添加自己的数据。**
    ```bash
    cp input/* ./ragtest/input
    ```
    windows命令提示符中需要用"copy input\* .\ragtest\input"
    
9. **初始化./ragtest 文件夹以创建所需的文件：**
    ```bash
    python -m graphrag.index --init --root ./ragtest
    ```

10. **移动 settings.yaml 文件，这是使用 ollama 本地模型配置的主要预定义配置文件：**
    ```bash
    cp settings.yaml ./ragtest
    ```

用户可以通过更改模型进行实验。llm 模型需要语言模型，如 llama3、mistral、phi3 等，而嵌入模型部分需要嵌入模型，如 mxbai-embed-large、nomic-embed-text 等，这些模型由 Ollama 提供。您可以在此处https://ollama.com/library找到 Ollama 提供的完整模型列表，这些模型可以在本地部署。LLM 的默认 API 基本 URL 为http://localhost:11434/v1，嵌入的默认 API 基本 URL 为http://localhost:11434/api，因此它们被添加到相应的部分。

![LLM Configuration](<Screenshot 2024-07-09 at 3.34.31 AM-1.png>)

![Embedding Configuration](<Screenshot 2024-07-09 at 3.36.28 AM.png>)

11. **运行索引，创建图表：**
    ```bash
    python -m graphrag.index --root ./ragtest
    ```

12. **运行查询：仅支持全局方法**
    ```bash
    python -m graphrag.query --root ./ragtest --method global "What is machine learning?"
    ```
    准备修改以支持本地(local)方法

**可以保存图表，并通过在 settings.yaml 中将 graphml 更改为“true”来进一步用于可视化：**
    
    snapshots:
    graphml: true
    
**为了可视化生成的 graphml 文件，您可以使用：https://gephi.org/users/download/或 repo visualize-graphml.py 中提供的脚本：**

将.graphml 文件的路径传递给 visualize-graphml.py 中的以下行：

    graph = nx.read_graphml('output/20240708-161630/artifacts/summarized_graph.graphml') 

13. **可视化.graphml：**

    ```bash
    python visualize-graphml.py
    ```



## 引用

- Original GraphRAG repository by Microsoft: [GraphRAG](https://github.com/microsoft/graphrag)
- Ollama: [Ollama](https://ollama.com/)
- graphrag-local-ollama：https://github.com/TheAiSingularity/graphrag-local-ollama
---
