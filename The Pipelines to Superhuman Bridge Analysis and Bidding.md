# Pipelines to Achieve Superhuman Abilities in Contract Bridge

This document describes a methodology being used to attain a superhuman level of analysis and bidding success in the game of contract bridge. The project relies on deterministic methods to accomplish its objectives. The process can easily be tweaked, measured, optimized, replicated, and explained. This approach contrasts sharply with using a neural network (NN) model. That said, the project naturally organizes its data into a form that is easily consumable for training a NN. The author believes that both deterministic methods and NN models can achieve a synergistic fusion.

An AI bidding at a superhuman level has eluded all efforts, whether in open or closed-source forms. Attempts at using NN models for bidding have not achieved expert-level performance or acceptable consistency. Approaches using deterministic processes, meanwhile, have never reached completion and have appeared intractable.

This document describes how a set of well-crafted data pipelines can create a superhuman AI that is both deterministic and performant. The development process has established that 99% of the project's work has been in data engineering and only 1% in applying AI techniques. The focus of the data engineering work has been directed at creating scalable processes for game data collection, coalescence of data, data cleaning, data augmentation, statistical analysis, and storage. The key to scalability has been selecting appropriate software, such as Polars for data organization, Parquet files for storage, and vectorization techniques to evaluate billions of bidding criteria.

The rest of this document describes five data pipelines that collectively enable superhuman analysis and bidding in the game of contract bridge.

### **Pipeline 1: Creating the Training Dataset**

Develop a dataset of structured game data suitable for training AI.

**Steps:**

1. **Downloading .lin files from BBO (Bridge Base Online):**
   - Retrieve game data from BBO.
   - The AI training process benefits from a large volume of data.
   - Downloads are in .lin file format, containing records of deals, auctions, bid descriptions, card play, and results.

2. **Reading .lin files, structuring the data, cleaning:**
   - Use Endplay to read the .lin files.
   - Endplay parses the files into Board objects.
   - Validate the data and reject invalid entries.

3. **From Board objects to an Endplay-native dataframe:**
   - Coalesce Board objects into an Endplay-native dataframe.
   - Map all data into columns, which may require flattening or unnesting operations.

4. **Extracting bidding criteria from bid descriptions:**
   - Lin files include bid descriptions (announcements), detailing the criteria for each bid. These descriptions are mandated by bridge rules, which require full disclosure of the bidding system to opponents.
   - Parse, clean, and transform descriptions into structured lists of bidding criteria expressions. These expressions should use familiar vernacular, typically comprising 1 to 3 Python-like terms. For example, a 1NT opening bid might have a bidding expression list of: `['Balanced', 'HCP >= 15', 'HCP <= 17']`.
   - Create a dictionary of bidding expressions and their postfix equivalents for efficient evaluation.
   - Currently, there are 55 terms in the bidding vocabulary (e.g., Balanced, HCP, Solid) and 400 bidding expressions (e.g., `Balanced`, `HCP >= 15`, `SL_H < 5`) used by BBO.
   - Each bidding expression must evaluate to `True` when applied to a hand for the bid to be valid. If no bids evaluate to `True`, the default action is to pass.

5. **Transforming the Endplay-native dataframe into a project-native dataframe:**
   - Transform data from its source-native format (e.g., ACBL, BBO, French Bridge) into a standardized project-native format dictated by the project’s analytical tools and methodologies.

6. **Cleaning and augmenting data:**
   - Augment the data with over 1,000 columns derived from a deal (e.g., hands, suit lengths, HCP, individual cards). This includes creating columns for every bidding expression (e.g., `Balanced`, `HCP`, `SL_[CDHS]`, `Solid_[CDHS]`, `Rebiddable_[CDHSN]`).

7. **Creating a bidding table:**
   - Compile a bidding table of all BBO auctions extracted from .lin files. Each entry represents a unique bid in an auction. The table is currently 1.7 million entries and will eventually grow to 2 to 5 million. Entries have the structure: `(index number, tuple(prior bids), tuple(candidate bid), 'textual description', [bidding expressions*])`.
   - The table exists as both Python and pickled files in various data structures for efficiency.

### **Pipeline 2: Creating a Target Dataset**

Develop a structured target dataset optimized for inference. This dataset is augmented with numerous columns, including those needed to infer bidding auctions. The steps are similar to Pipeline 1.

**Steps:**

1. **Download game data:**
   - Reliable sources include ACBL, BBO, ffbridge, or PBN files. BBO data is particularly useful as its auctions serve as a near ground-truth reference.

2. **Download to source-native dataframe:**
   - Read, clean, and coalesce the data into a source-native dataframe. Endplay can read .lin and .pbn files. At a minimum, the dataframe should include a `Deal` column, with additional board results columns for richer analysis.

3. **Transform to project-native dataframe:**
   - Convert the source-native dataframe into the project-native format.

4. **Clean and augment:**
   - Augment the data as described in Pipeline 1.

### **Pipeline 3: Creating a Dataframe of Bidding Table Results**

For each entry in the bidding table, apply the bidding expressions to the target dataset using vectorized Boolean operations. For a target dataset with 1 million deals and a bidding table of 2 million entries, this results in a dataframe of 1 million rows by 2 million columns (all Booleans). Despite the scale, Polars’ efficiency allows this process to complete in just 5 minutes.

### **Pipeline 4: Cascading Bidding Criteria Booleans to Qualify Candidate Bids**

Starting with the opening bid columns (approximately 30 out of 2 million columns without prior bids), recursively apply vectorized Boolean operations to subsequent bid columns. Avoid redundant computations for already processed partial bidding sequences. While computationally intensive, this process completes in 45 minutes with Polars. Improved hardware (e.g., GPUs) and algorithms could reduce this to 20 minutes.

### **Pipeline 5: Instantly Finding Successful Auctions**

Since all 2 million bidding sequences for the target dataset are precomputed, querying for possible bidding sequences is instantaneous. Filter the `True` values in a deal’s dataframe row (a vectorized horizontal operation) to extract corresponding column names. Use these column names to look up bidding table entries, yielding a list of all possible auctions guaranteed to meet the bidding criteria.

