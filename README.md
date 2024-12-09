# The Pipelines to Superhuman Bidding in  Contract Bridge

### **Pipeline 1: Create Training Dataset**

Create a dataset of structured game data suitable for training AI to bid at superhuman level.

**Steps:**

1. **Download .lin files from BBO (Bridge Base Online):**

   - Retrieve game data from BBO.
   - The AI training process favors lots of data.
   - Downloads are in .lin file format, which contains records of deals, bids, bid's descriptions and results.

2. **Read .lin files, structure the data, clean:**

   - Use Endplay to read the .lin files.
   - Endplay will parse the files into Board objects.
   - Validate the data
   - Reject invalid data.

3. **Flatten Board objects into an Endplay-Native DataFrame:**

   - Coalesce Board objects into an Endplay-native dataframe.
   - All data must be mapped into columns, might require flattening or unnesting operations.

4. **Extract bidding criteria from bid descriptions:**

   - Lin files contain bid descriptions (announcements) which are a description of the criteria for making the bid (e.g., point ranges, suit distributions). The rules of bridge require complete revelation of bidding system to opponents.

   - The descriptions are parsed, cleaned, and transformed into a structured list (strings) of bidding criteria expressions. The expressions should use familiar vernacular. Each expression may be 1 to 3 python-like terms. The list of expressions may be empty or as many as 16 items. For example, 1NT opening bid might have a bidding expression list of 3 items: ['Balanced', 'HCP >= 15', 'HCP <= 17'].
  
   - Create a dict of bidding expressions to their postfix counterpart. Postfix is simplier when applying bidding expressions to the actual data.

   - Currently there are 55 terms in the bidding vocabulary (e.g. Balanced, HCP, Solid) and 400 bidding expressions (e.g. 'Balanced', 'HCP >= 15', 'SL_H < 5') used by BBO.

   - For a bid to be used in an auction, every bidding expression in the list, when applied to the current hand, must evaluate to True. Otherwise the next candidate bid is evaluated. If no bids have an all True expression list, the default default action is to pass.

5. **Transform the Endplay Native DataFrame into a Project Native DataFrame:**

   - Project native format is dictated by the specific requirements of this project’s analytical tools and methodologies. No matter the source of the game data (ACBL, BBO, French Bridge, etc.), it's first coalesced into the source's native format and then transformed into this project's native format.

6. Clean and augment the data. There are over 1000 columns which can be augmented from a deal (e.g. Hands, suit lengths, HCP, individual cards, etc.). Augmenting includes creating columns for every bidding expression (e.g. Balanced, HCP, SL\_[CDHS], Solid\_[CDHS] Rebiddable\_[CDHSN].

7. Create a bidding table of all BBO auctions extracted from .lin files. The table has a unique entry for every bid of every auction. The table is currently 1.7 million entries but, when complete, will have 2 to 5 million entries. Entries have a structure of tuple(index number, tuple(prior bids), tuple(candidate bid,), 'textual description', ['bidding expression'*]. There may be zero or more bidding expressions. The bidding table exists as both a python file and is pickled in various forms.


### **Pipeline 2: Create Target Dataset**

Create a target dataset of structured game data suitable for inference. The dataset will be used to infer bidding auctions. The steps are very similar to Process 1 above.

#### **Steps:**

1. Download game data. Good sources of plentiful and robust data include ACBL, BBO, ffbridge, or PBN files.
2. Read the downloaded data, clean, coalesce into a source-native dataframe. Endplay can be used to read .lin, .pbn files. Minimum necessary columns are Deal.
3. Transform the source-native dataframe into project-native dataframe.
4. Clean and augment the data. There are over 1000 columns which can be augmented from a deal (e.g. Hands, suit lengths, HCP, individual cards, etc.). Augmenting includes creating columns for every bidding expression as described in previous section.

### **Pipeline 3: Apply Bidding E:**

####

**Steps:**

1. **For Every Bid:**

   - Create a table containing:
     - Bid details.
     - Context (e.g., previous bid).
     - Announcement text.
     - Canonical criteria expressions.

2. **Aggregate Bidding Data:**

   - Construct and validate the results DataFrame, ensuring that criteria expressions align with each corresponding deal and bid.

---

## **Create the Bidding Table Results DataFrame with Boolean Outcomes**



