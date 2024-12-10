# The Pipelines to Superhuman Analysis and Bidding in Contract Bridge

This document describes a methodology being used to attain a superhuman level of analysis and bidding success in the game of contract bridge. The project uses deterministic methods to achieve its goal. The process can easily be tweeked, measured, optimized, duplicated and explained. This is quite different from using a neural network (NN) model. That said, the project naturally makes its data into a form that's easily consumable for the training of a NN. The author believe that both deterministic methods and NN models can have a synergistic fushion.

An AI bidding at superhuman level has eluded all efforts whether in the open or closed source form. Attempts at using a NN for bidding have not achieved expert levels or acceptable consistancy. While approaches using deterministic processes have never achieved completion and appeared intractable. 

This document describes how a set of well crafted data pipelines can be used to create a superhuman AI which is both deterministic and performant. The development process has established that 99% of the projects work been data engineering and only 1% has been applying AI techniques. The focus of the data engineering work has been directed at creating scalable processes for game data collection, coalesence of data, data cleaning, data augmenting, statistical analysis and storage. The key to scalability has been selecting scalable software such as using polars for data organization, parquet files for storage, vectorization when evaluating billions of bidding criteria.

The rest of this document describes five data pipelines which collectively will result in superhuman analysis and bidding in the game of contract bridge.  

### **Pipeline 1: Creating the Training Dataset**

Create a dataset of structured game data suitable for training AI to bid at superhuman level.

**Steps:**

1. **Downloading .lin files from BBO (Bridge Base Online):**

     - Retrieve game data from BBO.
     - The AI training process favors lots of data.
     - Downloads are in .lin file format, which contains records of deals, auction, bid's descriptions, card play and results.

2. **Reading .lin files, structuring the data, cleaning:**

     - Use Endplay to read the .lin files.
     - Endplay will parse the files into Board objects.
     - Validate the data
     - Reject invalid data.

3. **From Board objects to an Endplay-native dataFrame:**

    - Coalesce Board objects into an Endplay-native dataframe.
    - All data must be mapped into columns, might require flattening or unnesting operations.

4. **Extracting bidding criteria from bid descriptions:**

    - Lin files contain bid descriptions (announcements) which are a description of the criteria for making the bid (e.g., point ranges, suit distributions). Descriptions exist because the rules of bridge require complete revelation of bidding system to opponents.

    - The descriptions are parsed, cleaned, and transformed into a structured list (strings) of bidding criteria expressions. The expressions should use familiar vernacular. Each expression may be 1 to 3 python-like terms. The list of expressions may be empty or as many as 16 items. For example, 1NT opening bid might have a bidding expression list of 3 items: ['Balanced', 'HCP >= 15', 'HCP <= 17'].
  
    - Create a dict of bidding expressions to their postfix counterpart. Postfix is simplier when applying bidding expressions to the actual data.

    - Currently there are 55 terms in the bidding vocabulary (e.g. Balanced, HCP, Solid) and 400 bidding expressions (e.g. 'Balanced', 'HCP >= 15', 'SL_H < 5') used by BBO.

    - For a bid to be used in an auction, every bidding expression in the list, when applied to the current hand, must evaluate to True. Otherwise the next candidate bid is evaluated. If no bids have all True evaluation results, the default action is to pass.

5. **Transforming the Endplay native dataframe into a project native dataFrame:**

    - Project native format is dictated by the specific requirements of this project’s analytical tools and methodologies. No matter the source of the game data (ACBL, BBO, French Bridge, etc.), it's first coalesced into a dataframe of the source's native format and then transformed into a dataframe of the project's native format.

6. **Cleaning and augmenting data:**
    - There are over 1000 columns which can be augmented from a deal (e.g. Hands, suit lengths, HCP, individual cards, etc.). Augmenting includes creating columns for every bidding expression (e.g. Balanced, HCP, SL\_[CDHS], Solid\_[CDHS], Rebiddable\_[CDHSN].

7. **Creating a bidding table:**
    - Create a bidding table of all BBO auctions extracted from .lin files. The table has a unique entry for every bid of every auction. The table is currently 1.7 million entries but, when complete, will have 2 to 5 million entries. Entries have a structure of tuple(index number, tuple(prior bids), tuple(candidate bid,), 'textual description', ['bidding expression'*]. There may be zero or more bidding expressions. The bidding table exists as both a python file and pickled files in various helpful data structures.

### **Pipeline 2: Creating a target dataset**

Create a target dataset of structured game data suitable for inference. The dataset will augmented with lots of columns including columns needed to infer bidding auctions. The steps are very similar to Pipleline 1 above.

#### **Steps:**

1. **Download game data:**
    - Good sources of plentiful and robust data include ACBL, BBO, ffbridge, or PBN files. A benefit of using BBO data is that its auction serves as ground-truthish for inferred auctions.
2. **Download to source-native dataframe:**
    - Read the downloaded data, clean, coalesce into a source-native dataframe. Endplay can be used to read .lin, .pbn files. Minimum necessary columns are ['Deal'] but board results columns make for more interesting analysis.
3. **Transform to project-native dataframe:**
    - Transform the source-native dataframe into project-native dataframe.
4. **Clean and augment:**
    - Clean and augment the data. There are over 1000 columns which can be augmented from a deal (e.g. Hands, suit lengths, HCP, individual cards, etc.). Augmenting includes creating columns for every bidding expression as described in previous section.

### **Pipeline 3: Creating a dataframe of bidding table results:**

For each entry in the bidding table, apply the entry's bidding expression list to the target dataset (vectorized boolean AND operations). This is for a batch processing use case. Processing a small number of deal needs a different approach. If the target dataset consists of 1 million deals and the bidding table has 2 million entries, the result of this process is a dataframe of 1 million rows by 2 million columns -- all booleans. Performance looks scary but the time required for this process, thanks to polars dataframe efficiency, is 5 minutes.

### **Pipeline 4: Cascading bidding criteria booleans to qualify candidate bids:**

Starting with the opening bid columns (about 30 of the 2 million columns which have no prior bid), recursively apply the column (vectorized boolean AND operations) to the next bids columns. Do so in a way that never repeats computations for an already computed partial bidding sequence. Performance looks scary but the time required for this process, thanks to polars dataframe efficiency, is currently 45 minutes including writing the parquet file. With improved hardware (GPU) and algorithms, this process might be driven down to 20 minutes.

### **Pipeline 5: Instantly finding successful auctions:**

Since we've already computed all 2 million bidding sequences for the entire target dataset, we are well positioned to get all sorts of no cost data analysis. Want to know all possible bidding sequences for a deal? Simply filter all the True values in the deal's dataframe row, a vectorized horizontal operation, to obtain a list of True's column names. Use the list of column names to lookup the bidding table entries. Now you have a list of all possible auctions each guarenteed to meet all bidding criteria.



