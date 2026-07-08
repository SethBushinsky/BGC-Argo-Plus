To update the BGC-Argo+ website after a new set of processing:

- Make new processed figure plots, called from main Float_Processing notebook
- In "code/plotting_scripts/Website_Pages_Summary_Table.ipynb"
    - Create a new  "website_float_outlier_meta_" + dataset_version + ".csv"
    - from that table, create a markdown page containing all float metadata
    - Make new markdown pages for each float (will automatically read in figure titles)
    - Copy figures into ftp site after first deleting old figures (note that this is incredibly slow, currently doing this via FileZilla. Not sure if there is a better way. Maybe if I have all files in one directory it would be faster.
    - 
