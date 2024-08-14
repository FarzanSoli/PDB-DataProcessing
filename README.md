A detailed review of recent deep learning advancements, focusing on frameworks that integrate diffusion models and equivariant graph neural networks for protein and small molecule generation, is provided in this [paper][10].

If you use this work please cite my work:

```
@article{soleymani2023protinteract,
  title={ProtInteract: A deep learning framework for predicting protein--protein interactions},
  author={Soleymani, Farzan and Paquet, Eric and Viktor, Herna Lydia and Michalowski, Wojtek and Spinello, Davide},
  journal={Computational and Structural Biotechnology Journal},
  volume={21},
  pages={1324--1348},
  year={2023},
  publisher={Elsevier}
}
```

```
@article{soleymani2024structure,
  title={Structure-based protein and small molecule generation using EGNN and diffusion models: A comprehensive review},
  author={Soleymani, Farzan and Paquet, Eric and Viktor, Herna Lydia and Michalowski, Wojtek},
  journal={Computational and Structural Biotechnology Journal},
  year={2024},
  publisher={Elsevier}
}
```

# PDB Dataset retrieval for training the diffusion model from scratch
This repository contains the codes to retrieve 3-D coordinates of the protein backbone from [PDB database][1]. 
## To prepare the dataset for training PeptiDiff, follow these steps:

1 - Fetch Dataset: The Fetch_Dataset.py script retrieves the necessary dataset from the PDB database and extracts the files into a specified folder.

2 - Process Protein Data: Next, the Data_Processing.py script processes the extracted dataset. It extracts 3-D coordinates of alpha-carbons and assigns physicochemical features to each protein, utilizing functions from AA_features.py. You can customize the padding length of the protein directly within Data_Processing.py. This script saves two dictionaries: one for backbone coordinates and another for backbone features. You can select which features to include by modifying the AA_features.py script.

3 - Utility Functions: The Functions.py script contains essential functions for data processing, normalization, training, and other related tasks.

# Pre-processed datasets for training the models.

The raw datasets, [alpha-carbon coordinates.][2] and the [amino acid sequences of the proteins][3] are available for access.
 
The "AA_features.py" code includes physicochemical features of the 20 standard amino acids. Additionally, the physicochemical features of the unknown amino acid (X) are computed using the median of the known features. Each amino acid can be encoded using their respective [physicochemical features][4] (Table 1). 

Two pre-processed datasets are provided: a [backbone 3-D coordinates dictionary][5] and a [backbone feature dictionary][6] comprising physicochemical properties. For each protein, the data in both dictionaries are normalized and padded to a specified length to ensure uniformity across the dataset.

The [training][7], [validation][8] and [testig][9] datasets are available to download. These datasets are augmented by randomly rotating the original datasets (20 times). 


In order to fetch the dataset from PDB database, follow these steps:

```
docker build --no-cache -f Dockerfile_data -t dataset_image .
```
```
docker run --rm -v /path/to/host:/app/Code/Dataset dataset_image
```
# Train the diffusion model and generate n-samples of new protein sequence and structure.

If you'd like to use the pre-existing datasets mentioned above, please download the [backbone 3-D coordinates dictionary][5] and [backbone feature dictionary][6], then move them to the 'Dataset' folder within the 'Code' directory. After that, follow these steps to implement the training and inference processes:


```
docker build --no-cache -f Dockerfile_train -t training_inference_image .
```

```
docker run --rm -it --gpus all training_inference_image
```

This code trains the model and generates 1000 new instance (default - you can increase or decrease it based on the available computational resources) as well as density, coverage and Frechet distance metrics for both generated 3-D coordinates and physicochemical features. This framework implements both DDIM and DDPM which can be selected by chaning the parameter ($eta$) where $eta = 0$ reresents DDIM and $eta = 1$ denotes DDPM.





[1]: https://files.wwpdb.org/pub/pdb/data/biounit/PDB/divided/

[2]: https://uottawa-my.sharepoint.com/personal/fsole078_uottawa_ca/_layouts/15/guestaccess.aspx?share=ERw4N-f4U6BNutxBZ67JtbUBF29r45VJifBIzTVFaCvcew&e=79FvMR

[3]: https://uottawa-my.sharepoint.com/personal/fsole078_uottawa_ca/_layouts/15/guestaccess.aspx?share=Eescxh5uKtRGtBtdVZ7BSc8BGGvR9GXhhaw_2mKNKMQtzg&e=EpZjyQ

[4]: https://www.sciencedirect.com/science/article/pii/S2001037023000296

[5]: https://uottawa-my.sharepoint.com/personal/fsole078_uottawa_ca/_layouts/15/guestaccess.aspx?share=EaL9M84AeBlJrGhWozXEIMMBrVWLzDBBrYXpD_mJeFqDfg&e=LHQheE

[6]: https://uottawa-my.sharepoint.com/personal/fsole078_uottawa_ca/_layouts/15/guestaccess.aspx?share=EWajMPllOqRDuOFgfvucGREBJApruAi07NXg6VmuXnYX3g&e=ieN7SJ

[7]: https://uottawa-my.sharepoint.com/personal/fsole078_uottawa_ca/_layouts/15/guestaccess.aspx?share=Efrj4s-s7IhHmRmQJahgGUAB3c7AWxCB3LH2sPDcGZqtNw&e=xDl7Hg

[8]: https://uottawa-my.sharepoint.com/personal/fsole078_uottawa_ca/_layouts/15/guestaccess.aspx?share=ERI8YR-laKVEtW75QRXNkCwBtPkJdPAVOjir-eAEXBQPVg&e=1VsVKC

[9]: https://uottawa-my.sharepoint.com/personal/fsole078_uottawa_ca/_layouts/15/guestaccess.aspx?share=ESUT9WMvnnlNjGP3QUqFl1YBKZPS4eEobvftjXk4p0BiRA&e=JbR2j3

[10]: https://www.sciencedirect.com/science/article/pii/S2001037024002228
