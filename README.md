## An implementation of an AI which bids contract bridge deals at a superhuman level

### What is this project?
The project's goal is to create an AI foundational model which exceeds human abilities in analytics and bidding. The foundational model is largely based on BBO's GIB bidding system.

### Who is this project for?
This project is for software developers. It is not for end user consumption.

### General Prerequisites
    python
    polars

### Bridge Specific Prerequisites
    endplay

### Description of Files

The Pipelines to Superhuman Bridge Analysis and Bidding.md describes the methodology used to bid bridge hands.

File bbo_bidding_vocabulary.txt contains a list of bridge terms used to evaluate hands. e.g. Rebiddable, HCP, Solid.

File bbo_bidding_expressions.txt contains a list of expressions used to evaluate bidding criteria. e.g. Rebiddable_C, HCP >= 15, HCP <= 17, Solid_C.

File requirements.txt contains project's prerequisites. It's used to install python packages into the development environment.
    pip install -U -r requirements.txt

### References
https://www.bridgebase.com/doc/gib_system_notes.php

https://www.bridgebase.com/doc/gib_descriptions.php

https://github.com/lorserker/ben

https://www.ijcai.org/Proceedings/99-1/Papers/084.pdf


