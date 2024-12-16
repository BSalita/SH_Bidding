# Frequently Asked Questions (FAQ)

## Introduction
The Superhuman Bridge Bidding project is an ambitious initiative to develop an AI system capable of outperforming human players in the bidding phase of bridge. This FAQ provides insights into the project’s goals, methodologies, and progress while inviting collaboration from the community.

## General Questions

### What is this project?
The Superhuman Bridge Bidding project aims to develop an AI system capable of outperforming human players in bridge bidding. This involves creating algorithms and methodologies to mimic and surpass expert-level decision-making in the bidding phase of bridge.

### Why is it significant?
This project is significant because it pushes the boundaries of artificial intelligence in games of incomplete information, showcasing advancements in strategic decision-making, pattern recognition, and reasoning under uncertainty.

### In what ways does it advance SOTA (State of the Art)?
The project advances SOTA by introducing novel deterministic methodologies tailored to bridge bidding. It emphasizes scalability, transparency, interpretability, and strategic depth compared to prior art.

### What are the novel contributions?
- Methods of scaling huge amounts of bridge data.
- Open source foundational model of a bidding system (GIB).
- Reducing the seemingly intractable problem of exceeding human expert level bidding into a manageable form.
- Development of a complete deterministic bidding system.
- Integration of rule-based logic for enhanced decision-making.
- Creation of a comprehensive dataset specific to bridge bidding strategies.
- Creation of tools to vet bidding strategies for effectiveness, coverage, and disambiguation.
- Introduction of explainable AI techniques for better interpretability of decisions.
- Document superhuman methods of hand evaluation.

### What is the prior art?
Previous efforts include:
- Traditional neural network approaches for bridge bidding.
- Rule-based systems designed by human bridge experts.
- Research on AI in games like chess, poker, and Go, which have influenced this domain.

### Give examples of questions which the project will answer:
- What is the bidding criteria for each bid in the auction? Present a choice of bids based on a "goodness" metric.
- Show a probability distribution of next bids. Rank by highest probability to lowest.
- What is the likely par score or expected value? Predict as each bid is made.
- Who holds the key cards? Predict key cards in each hand as each bid is made.
- Which key cards must partner hold to ensure a highest expected value score? Show as each bid is made.
- What is the probability and expected value for opponents bids? Show expected value of pass, bid, double, or redouble.
- What are the probabilities and expected values in terms of match points or IMPs (International Match Points)?

## Technical Approach

### What is the general approach?
The project uses data driven deterministic algorithms to select the most effective bids.
- Employ scalable techniques to efficiently take full advantage of abundant data.
- Rule-based logic and statistical methods to select best bidding candidates.
- A framework for implementation of additional bidding systems beyond the foundational model (GIB).

### How is the approach better than a neural net (NN)?
Deterministic systems are:
- Transparent and explainable, offering clear reasoning for each decision.
- Effective and precise selection of bids.
- Consistent, reducing variability in performance.

### In what way is a NN better?
Neural networks excel in:
- Handling large datasets and complex patterns.
- Adapting to new scenarios through training.
- Learning subtleties and nuances which may not be captured by deterministic rules.

### Why is a deterministic approach better?
A deterministic approach is better for:
- Ensuring predictable outcomes.
- Building explainable systems.
- Codifying expert knowledge directly into the system.

## Project Roadmap

### Do you have a roadmap?
Yes. The roadmap includes:
1. Research and data collection.
2. Development of deterministic methods.
3. Testing and validation against real-world results.
4. Iterative improvement based on feedback.
5. Public release and community engagement.

### What is the current status?
The project is in the final development phase focusing on making the bidding table complete and accurate.

## Open Source and Collaboration

### Where is the source code?
The source code will be posted from now continuing through end of April 2025.

### Is it open source?
Yes, the project will be open-source (MIT license) for the community of developers and game researchers.

### Is the data open source?
Yes, datasets curated for the project will be made available for download.

### What is needed to complete the project?
- Completion and vetting of the bidding table.
- Additional data for ever-improving data integrity and testing.
- Collaboration with bridge experts.
- Community contributions for further testing and validation.

### How can I contribute?
You can contribute by:
- Contributing to code development and documentation.
- Providing bridge-related datasets.
- Offering expertise in bridge strategies.
- Testing the system and providing feedback.

## Technical Details

### What are the scalability issues?
- Managability of millions of deal downloads.
- Efficient creation of 1000's of augmented columns. Efficient particularly in memory usage and clock-on-wall performance.
- Efficient when reading/writing of gigabytes of data, millions of rows of deals. Needs efficient structures for file, database.
- Efficient dataframe structures. Pandas couldn't scale but Polars does. There's millions of rows in dataframes.
- Efficient Polars wrangling: selecting, filtering, joining.

### What are the hardware requirements for the project?
To replicate the entire pipeline requires a beefy computer system preferably with 512GB of RAM (sic) and 2TB of fast storage. Possibly a more modest system can be used, one having 64GB of RAM, if the CPU and disk I/O are able to compensate for the reduced RAM size. The actual system used for development is a Dell R630 system with dual Intel Xeons (total of 12 cores), 1.5TB of RAM (sic) and dual 2TB SATA SSDs in RAID 0. While this may appear to be a daunting piece of hardware, it's less than the price of a high-end GPU and easily obtainable on eBay. No GPU is needed in the current phase of the project.

### What is the bidding table?
A structured representation of all known auction sequences broken down by individual bids and their criteria.

### What is a bidding expression list?
A list of predefined expressions, datatype string, used to encode and interpret bidding actions e.g. 1NT is `['Balanced', 'HCP >= 15', 'HCP <= 17']`.

### What is a bidding vocabulary list?
A comprehensive list of terms, datatype string, commonly used in bridge bidding. e.g. Balanced, HCP, Rebiddable_C.

### What is the purpose of the SQL interface?
SQL is a standards based method of quering data. We use it to query data held in dataframes. SQL provides the ability to select, filter, join and create new augmentations (features).

## Superhuman Performance

### In what way is the project superhuman now?
The system demonstrates:
- Superior consistency compared to human players.
- Ability to leverage information in vast datasets beyond human capacity.
- Employ sophisticated statistical methods to select efficient bids.
- Enhanced strategic reasoning in complex bidding scenarios.

### Why do you think the project will result in a superhuman AI?
By combining the strengths of deterministic logic and statistical methods, the system is expected to:
- Exploit subtle patterns and strategies unattainable by humans.
- Continuously improve through feedback.
- Offer unparalleled consistency and accuracy.

### Can an AI really bid better than a human?
Yes, AI can outperform humans by:
- Leveraging extensive datasets and simulations.
- Avoiding cognitive biases.
- Consistently applying optimal strategies.

### Are there examples of AI outperforming humans in other games?

**Chess (Deep Blue vs. Garry Kasparov, 1997)**  
- [New York Times: Machine Defeats Kasparov](https://www.nytimes.com/1997/05/11/nyregion/deep-blue-wins-rematch-with-kasparov.html)

**Go (AlphaGo vs. Lee Sedol, 2016)**  
- [Nature: AlphaGo Mastering the Game of Go](https://www.nature.com/articles/nature16961)  
- [DeepMind’s Official Case Study on AlphaGo](https://deepmind.com/research/case-studies/alphago-the-story-so-far)
- [Move 37: Artificial Intelligence, Randomness, and Creativity](https://www.johnmenick.com/writing/move-37-alpha-go-deep-mind.html)

**Poker (Libratus & Pluribus vs. Professionals)**  
- [Science Magazine: Superhuman AI for Multiplayer Poker](https://www.science.org/doi/10.1126/science.aay2400)  

**Real-Time Strategy Games (Dota 2 & StarCraft II)**  
- [OpenAI: OpenAI Five and Dota 2](https://openai.com/blog/openai-five/)  
- [DeepMind Blog: AlphaStar’s StarCraft II Achievements](https://deepmind.com/blog/article/alphastar-mastering-real-time-strategy-game-starcraft-ii)

**General Game AI (AlphaZero and Beyond)**  
- [AlphaZero’s General Game-Playing Performance (arXiv)](https://arxiv.org/pdf/1712.01815.pdf)
