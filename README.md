## An implementation of an AI which bids contract bridge deals at a superhuman level.

### Who is this for?
This project is for software developers needing an open source implementation of bridge superhuman game analysis and bidding.

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

