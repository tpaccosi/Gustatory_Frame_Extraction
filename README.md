# Gustatory-Frame-Extraction

This is the repository to use the models in English for gustatory frame extraction.

## __Step 1 - Convert Books__

The script in the `books-converter` folder converts plain text files into a format compatible with either multitask or single-task classification. To prepare your documents for frame element extraction, run the script `books_converter.py` on the folder containing the texts you wish to convert. This converter filters the content by retaining only text segments (specified by the `--window` parameter) surrounding the seed words. The `SeedLists` folder contains seed lists chosen by the author, as described in *Paccosi, T., & Tonelli, S. (2024, May). A New Annotation Scheme for the Semantics of Taste. In Proceedings of the 20th Joint ACL-ISO Workshop on Interoperable Semantic Annotation@ LREC-COLING 2024 (pp. 39-46)*. Note that the script does not perform lemmatization, so you need to include all inflected forms of the seeds in the list.

Here's a refined description of the parameters for `books_converter.py`:

- `--folder`: Specifies the input folder containing the books or documents in plain text format (no metadata or tags).
  
- `--output`: Defines the output folder where the converted documents will be saved.

- `--seeds`: Path to the file containing the seed list, e.g., `'SeedLists/seed_taste_pos_en.txt'`.

- `--books`: Controls the merging of multiple books into a single file. Setting this to `1` will create a separate file for each book. The default value is `100`.

- `--window`: The number of sentences to retain around each seed word. For example, setting this to `3` keeps three sentences before and after each occurrence of a seed word. The default value is `3`.

- `--label`: A short label used to assign a unique ID to each document, allowing it to be matched with metadata later.

The script also generates a `-mapping` file outside the output folder to map each document ID to its corresponding original book.

Usage example:

```
python3 books_converter-filter.py --folder books_folder --output output_folder --seeds SeedLists/seed_taste_pos_en.txt --label abc --books 1000
```

## __Step 2 - Taste prediction__

The `run-predictions` folder contains the classifier (`predict.py`) used to extract taste frame elements from the books that were converted in the previous step.

### Setup Instructions:

1. **Download the Model**  
   First, download the model you want to use from [link provided] and move it into the `run-predictions/models` folder.

2. **Python Version**  
   The code has been tested with Python 3.8.

3. **Install Required Packages**  
   To install the necessary packages, navigate to the `run-predictions` folder and run the following command:

   ```bash
   pip install -r requirements.txt
   ```

#### Optional Parameter:
- `--device`: Specifies the device to use for computation:
  - `0` for CUDA-based GPUs.
  - `1` for MPS (Apple M1/M2 chips).
  - `-1` for CPU (default).

The `test-files` folder contains a sample file you can use to check if the classifier is working correctly.

#### Usage Example:
```bash
python3 predict.py models/en.pt test-files/test-en.tsv predictions/predictions-test-en.tsv --device 0
```

After running the script, you can compare your output with the example provided in `predictions/sample-predictions-test.tsv` to ensure your setup is correct.

## __Step 3 - Frames Extraction__

The `extract-annotations.py` script, located in the `frames-extraction` folder, extracts frame elements from the classifier's predictions and outputs a TSV file with all identified frames and corresponding sentences.

### Parameters

- `--folder`: Specifies the folder containing the predictions generated by the classifier.
- `--output`: Defines the path to the output `.tsv` file that will store the extracted frames.

### Setup Instructions

The code has been tested with Python 3.8. To install the required packages, navigate to the `frames-extraction` folder and run:

```bash
pip install -r requirements.txt
```

### Usage Example

To execute the script, use the following command:

```bash
python3 extract-annotations.py --folder ../run-predictions/predictions/ --output test-frames.tsv
``` 

This command will process the predictions and create a `test-frames.tsv` file containing the extracted frames and sentences.

## Publication

If you use this resource, please cite:

`Paccosi T. & Tonelli S. (2024). Benchmarking the Semantics of Taste: Towards the Automatic Extraction of Gustatory Language. In CLiC-it, Italian Conference on Computational Linguistics 2024`

## Funding acknowledgement

Funded by the European Union under grant agreement 101088548 -TRIFECTA (https://trifecta.dhlab.nl/). 
