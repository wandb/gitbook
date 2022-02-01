# Molecule



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.10/wandb/sdk/data_types.py#L856-L1058)



Wandb class for 3D Molecular data

```python
Molecule(
    data_or_path: Union[str, 'TextIO'],
    **kwargs
) -> None
```





| Arguments |  |
| :--- | :--- |
|  `data_or_path` |  (string, io) Molecule can be initialized from a file name or an io object. |



## Methods

<h3 id="from_rdkit"><code>from_rdkit</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.10/wandb/sdk/data_types.py#L922-L983)

```python
@classmethod
from_rdkit(
    data_or_path: "RDKitDataType",
    convert_to_3d_and_optimize: bool = (True),
    mmff_optimize_molecule_max_iterations: int = 200
) -> "Molecule"
```

Convert RDKit-supported file/object types to wandb.Molecule


| Arguments |  |
| :--- | :--- |
|  `data_or_path` |  (string, rdkit.Chem.rdchem.Mol) Molecule can be initialized from a file name or an rdkit.Chem.rdchem.Mol object. |
|  `convert_to_3d_and_optimize` |  bool Convert to rdkit.Chem.rdchem.Mol with 3D coordinates. This is an expensive operation that may take a long time for complicated molecules. |
|  `mmff_optimize_molecule_max_iterations` |  int Number of iterations to use in rdkit.Chem.AllChem.MMFFOptimizeMolecule |



<h3 id="from_smiles"><code>from_smiles</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.10/wandb/sdk/data_types.py#L985-L1019)

```python
@classmethod
from_smiles(
    data: str,
    sanitize: bool = (True),
    convert_to_3d_and_optimize: bool = (True),
    mmff_optimize_molecule_max_iterations: int = 200
) -> "Molecule"
```

Convert SMILES string to wandb.Molecule


| Arguments |  |
| :--- | :--- |
|  `data` |  string SMILES string. |
|  `sanitize` |  bool Check if the molecule is chemically reasonable by the RDKit's definition. |
|  `convert_to_3d_and_optimize` |  bool Convert to rdkit.Chem.rdchem.Mol with 3D coordinates. This is an expensive operation that may take a long time for complicated molecules. |
|  `mmff_optimize_molecule_max_iterations` |  int Number of iterations to use in rdkit.Chem.AllChem.MMFFOptimizeMolecule |







| Class Variables |  |
| :--- | :--- |
|  `SUPPORTED_RDKIT_TYPES`<a id="SUPPORTED_RDKIT_TYPES"></a> |   |
|  `SUPPORTED_TYPES`<a id="SUPPORTED_TYPES"></a> |   |

