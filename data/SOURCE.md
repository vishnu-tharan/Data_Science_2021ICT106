# Dataset source and credit
Becker, B. and Kohavi, R. (1996). **Adult** [Dataset]. UCI Machine Learning Repository.

- Dataset page: https://archive.ics.uci.edu/dataset/2/adult
- DOI: https://doi.org/10.24432/C5XW20
- Download: https://archive.ics.uci.edu/static/public/2/adult.zip
- License: Creative Commons Attribution 4.0 International (CC BY 4.0).
- License text: https://creativecommons.org/licenses/by/4.0/
- Downloaded: 19 November 2026.
- Origin: 1994 US Census database.

Original files `adult.data`, `adult.test`, and `adult.names` are kept in `raw/`. File checksums are included in `results/source_checksums.json`. Cleaned CSV copies are included as evidence. Running the notebook refreshes these files. Changes include trimming text, standardising test labels, treating question marks as missing, removing exact duplicate rows, and removing test input profiles present in training. No private records have been added. Credit and the source license should stay with shared data copies.
