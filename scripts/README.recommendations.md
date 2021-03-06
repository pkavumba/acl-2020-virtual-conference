# How to get similar paper recommendations

In this guide we can see how to get paper recommendations using the pretrained model provided
from [ICLR webpage](https://github.com/ICLR/iclr.github.io/tree/master/recommendations) and abstract embeddings.



## Create a visualization based on BERT embeddings

1. Grab ACL2020
   [papers.csv](https://github.com/acl-org/acl-2020-virtual-conference-sitedata/blob/add_acl2020_accepted_papers_tsv/papers.csv)
   from this branch or a more recent version and copy it to `sitedata_acl2020`.
2. Run `python scripts/embeddings.py sitedata_acl2020/papers.csv` to produce the BERT embeddings
   for the paper abstracts.
3. Run `python scripts/reduce.py --projection-method [tsne|umap] sitedata_acl2020/papers.csv embeddings.torch > sitedata_acl2020/papers_projection.json`
   to produce a 2D projection of the BERT embeddings for visualization. `--projection-method`
   selects which dimensionality reduction technique to use.
4. Rerun `make run` and go to the paper visualization page


## Produce similar paper recommendations

1. Run `python scripts/create_recommendations_pickle.py --inp sitedata_acl2020/papers.csv --out cached_or.pkl` to produce `cached_or.pkl`.
   This file is compatible with the inference scripts provided in [https://github.com/ICLR/iclr.github.io/tree/master/recommendations](https://github.com/ICLR/iclr.github.io/tree/master/recommendations)
2. Clone [https://github.com/ICLR/iclr.github.io](https://github.com/ICLR/iclr.github.io). You will
   need `git-lfs` installed.
3. `cp cached_or.pkl iclr.github.io && cd iclr.github.io/recommendations`
4. Install missing requirements
5. `python recs.py`. This will run inference using a pretrained similarity model and produce the
   `rec.pkl` file that contains the paper similarities.
6. You can use the `iclr.github.io/data/pkl_to_json.py` script to produce the `paper_recs.json`
   file that contains the similar paper recommendations that can be displayed to the website. Make
   sure to modify the filepaths to point to the correct `cached_or.pkl`, `rec.pkl`.
7. Grab the produced `paper_recs.json` file and copy it to `sitedata_acl2020`. A version of this file
   produced using this method is [here](https://github.com/acl-org/acl-2020-virtual-conference-sitedata/blob/add_acl2020_accepted_papers_tsv/paper_recs.json)
