## Creating a Superhuman AI for Contract Bridge

This project aims to develop a foundational AI model capable of analyzing and bidding contract bridge deals at a superhuman level. The AI is rooted in advanced analytics and strives to enhance strategic decision-making in the complex world of bridge. The target of the foundational model is BBO's GIB bidding system

### Target Audience
This project is designed for software developers with an interest in artificial intelligence, machine learning, and bridge bidding systems. It has no end-user components.

### Prerequisites
#### General Prerequisites
- Python 3.12 or later
- Polars: A fast DataFrame library for data processing.

#### Bridge-Specific Prerequisites
- Endplay: A Python library for analyzing bridge hands.

### Description of Files
- `Pipelines to Superhuman Bridge Analysis and Bidding.md`: Describes the methodology for analyzing and bidding bridge hands.
- `bbo_bidding_vocabulary.txt`: Contains a glossary of bridge-related terms (e.g., "Rebiddable," "HCP," "Solid").
- `bbo_bidding_expressions.txt`: Lists expressions used to evaluate bidding criteria (e.g., "Rebiddable_C," "HCP >= 15").
- `requirements.txt`: Specifies project dependencies. Use the following command to install them:
  ```
  pip install -U -r requirements.txt
  ```

### References
- [GIB System Notes](https://www.bridgebase.com/doc/gib_system_notes.php): Documentation for BBO's GIB system.
- [GIB Descriptions](https://www.bridgebase.com/doc/gib_descriptions.php): Additional details on GIB bidding logic.
- [Ben GitHub Repository](https://github.com/lorserker/ben): A related open-source project for bridge analysis.
- [IJCAI Paper on AI and Bridge](https://www.ijcai.org/Proceedings/99-1/Papers/084.pdf): Academic research on AI strategies for bridge.

