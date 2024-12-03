# Retrieval augmented generation for building datasets from scientific literature
Contains the notebooks `Prompting.ipynb` `RAG.ipynb` used for creating datasets.\
The test sets are `test_100_metal_gemma2_9B.xlsx` and `test_100_metal_llama3_8B.xlsx`.\
Final dataset is `final_extracted_1611.xlsx`

## Preprint
The paper is available here [ChemRxiv](https://chemrxiv.org/engage/chemrxiv/article-details/673c541a7be152b1d0981fd7).


## RAG
Open `RAG.ipynb` in Google Colab.
<a target="_blank" href="https://colab.research.google.com/github/catastropiyush/RAG-dataset-gen/blob/main/RAG.ipynb">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>\
Connect to T4 GPU.\
After installing the libraries open the terminal.
```python
#open the terminal
!pip install colab-xterm
%load_ext colabxterm
%xterm
```
Run the following commands in the terminal to load the LLM.
```bash
curl -fsSL https://ollama.com/install.sh | sh
ollama serve & ollama pull llama3
ollama serve & ollama run llama3
```
Load the file containing the abstracts.
```python
selected_df = pd.read_csv("selected_df.xls")
```
Define the query.
```python
    query = """
      Describe all the parameters of the material discussed in the text.
      If no information is available just write "N/A".
      The output should be concise and in the format as below:

      Name of Alloy                        :
      Hydrogen storage capacity            :
      Temperature                          :
      Pressure                             :
      Experimental Conditions              :
    """
```
Follow the notebook to extract the data.

## Prompting
Open `Prompting.ipynb` in Google Colab
<a target="_blank" href="https://colab.research.google.com/github/catastropiyush/RAG-dataset-gen/blob/main/Prompting.ipynb">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

Load the LLM.
```python
model, tokenizer = FastLanguageModel.from_pretrained(
    model_name = "unsloth/llama-3-8b-bnb-4bit",
    max_seq_length = max_seq_length,
    dtype = dtype,
    load_in_4bit = load_in_4bit,
)
```
Use this prompt or modify it.
```python
text_instruct = """
  Describe all the parameters of the material discussed in the text.
  If no information is available just write "N/A".
  The output should be concise and in the format as below:

  Name of Alloy                        :
  Hydrogen storage capacity            :
  Temperature                          :
  Pressure                             :
  Experimental Conditions              :
  """
```
Load the file containing the abstracts.
```python
selected_df = pd.read_csv("selected_df.xls")
```
Follow the notebook to extract the specified data.
