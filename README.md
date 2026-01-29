k# CurateGPT

**NOTE**: This repository has been copied from the following repository O'Neil, S., & Mungall, C. (2023). CurateGPT [Computer software]. https://doi.org/10.5281/zenodo.8388002 and changes have been made to allow longer text length to run through the extract function in the streamlit app. We are using this repository to extract information on white matter tracks found in Alzheimer's and Alzheimer's related disease research papers through with the Uberon ontology already available in CurateGPT. Below are the instructions specifically for this project. 

[![DOI](https://zenodo.org/badge/645996391.svg)](https://zenodo.org/doi/10.5281/zenodo.8293691)

CurateGPT is a prototype web application and framework for performing general purpose AI-guided curation
and curation-related operations over *collections* of objects.

## Getting started

#### Clone the repository
#### Enter the Directory
`cd curategpt-for-ad/`
#### Install Developer and Dependencies
`sudo apt install python3-poetry`
`poetry install`
`poetry shell` ##You will need to run this every time you use curategpt. More on this below on Usage
`pip install curategpt`
`pip install paper-qa`
#### Export OpenAI Key. You will need to get a key from OpenAI yourself
In order to get the best performance from CurateGPT, we recommend getting an OpenAI API key, and setting it:
`export OPENAI_API_KEY=<enter_api_key_here>`
#### Generate Github token. You will need to retrieve this from Github yourself
General instructions: Go to Settings>Developer Settings>Personal Access Tokens>Fine Grained Tokens and then create a token name and under repositories select "public repositories" then select the generate the token green button. 

## Usage
### Step 1: In Command Line
```
poetry shell
```
```
make ont-uberon
```
This loads the Uberon ontology for our use into `ont_uberon`

Note that by default this loads into a collection set stored at `stagedb`, whereas the app works off
of `db`. You can and must copy the collection set to the db with for us to use in the app:
```
cp -r stagedb/* db/
```
You can then run the streamlit app with:
```
make app
```
### Step 2: In Streamlit app (after running make app)
- Under **Choose operation**, select **Extract**
- Under **Choose collection** select **ont_uberon**
- Under **Choose model** select **gpt-4o** (TO DO: find out if we can change curategpt to allow the newest GPT version)
- Under **Extraction Strategy** select **Basic** (TO DO: find out why we are getting errors with OAI Function and SPIRES)
- Under **Background knowledge** select **PubMED (via API)**. This we can play around with but this is what we had it set it to when practicing
- In the **Text** box copy and paste the text from the research paper into the box
- check the "Generate background" box
- In the **Additional Instructions** box, enter the prompt below
  - You are an expert in structured data extraction from scientific papers. You MUST output a JSON object with EXACTLY the following five required keys: "diseases", "white_matter_tracts", "diffusion_measures", "study_type", and "species". No other keys are allowed. Empty JSON {} is INVALID. Follow this procedure EXACTLY and IN ORDER. STEP 1 — STUDY CLASSIFICATION: Determine (A) whether the paper is a review paper or an original single study and (B) whether it is a human study or a non-human study (animal, in vitro, or computational). STEP 2 — FIELD-LEVEL EXTRACTION RULES: The restriction below applies ONLY to the field "white_matter_tracts". If the study is NON-HUMAN OR a REVIEW PAPER, you MUST set "white_matter_tracts" to " " and you are NOT allowed to list, infer, summarize, or extract white matter tracts. All other fields MUST still be filled if information is present. STEP 3 — DATA EXTRACTION: Populate all five keys. If information for any field is missing or uncertain, output " " for that field. IMPORTANT CONSTRAINTS: Do NOT hallucinate. Do NOT infer white matter tracts from anatomy, figures, or background text. Violating the white_matter_tracts rule is an error. Output JSON ONLY.
  - Set **Max examples** to 10. This we can play around with but this is what we had it set it to when practicing
  - Select **Extract**
