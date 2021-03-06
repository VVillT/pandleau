# pandleau

A quick and easy way to convert a Pandas DataFrame to a Tableau .tde or .hyper extract.

## Getting Started

### Prerequisites

 - If you want to output as a .tde format, you'll need to install TableauSDK directly from Tableau's site: https://onlinehelp.tableau.com/current/api/sdk/en-us/help.htm#SDK/tableau_sdk_installing.htm%3FTocPath%3D_____3 . 
  - If you want to output as a .hyper format, you'll need to install Extract API 2.0 directly from Tableau's site: https://onlinehelp.tableau.com/current/api/extract_api/en-us/help.htm#Extract/extract_api_installing.htm%3FTocPath%3D_____3 . 
  - Although Tableau's site claims Python 3 is not supported, this module is tested to work fully functional on Python 3.6

### Installing

Once installing TableauSDK is done, download this repository, navigate to your downloads file and run the following in cmd:  
```bash
python -m setup.py install
```

You can also install pandleau using pip:
```bash
pip install pandleau
```
But note that this will throw a warning to install tableausdk using the above link in Prerequisites.

## Example

I grabbed the following Brazil flights data off of kaggle for this example: https://www.kaggle.com/microtang/exploring-brazil-flights-data/data.

```python
import pandas as pd
from pandleau import *

# Import the data
example_df = pd.read_csv(r'example/BrFlights2.csv', encoding = 'iso-8859-1')

# Format dates in pandas
example_df['Partida.Prevista'] = pd.to_datetime(example_df['Partida.Prevista'], format = '%Y-%m-%d')
example_df['Partida.Real'] = pd.to_datetime(example_df['Partida.Real'], format = '%Y-%m-%d')
example_df['Chegada.Prevista'] = pd.to_datetime(example_df['Chegada.Prevista'], format = '%Y-%m-%d')
example_df['Chegada.Real'] = pd.to_datetime(example_df['Chegada.Real'], format = '%Y-%m-%d')

# Set up a spatial column
example_df.loc[:, 'SpatialDest'] = example_df['LongDest'].apply( lambda x: "POINT (" + str( round(x, 6) ) ) + \
	example_df['LatDest'].apply( lambda x: " "+str( round(x, 6) ) + ")" )

# Change to pandleau object
df_tableau = pandleau(example_df)

# Define spatial column
df_tableau.set_spatial('SpatialDest', indicator=True)

# Write .tde or .hyper Extract!
df_tableau.to_tableau('test.hyper', add_index=False)

```

## Authors

* **Benjamin Wiley** - [jamin4lyfe](https://github.com/bwiley1)
* **Zhirui(Jerry) Wang**  - [zhiruiwang](https://github.com/zhiruiwang)
* **Aaron Wiegel** - [aawiegel](https://github.com/aawiegel)

## Related Project

[RTableau](https://github.com/zhiruiwang/RTableau) Convert R data.frame to Tableau Extract using pandleau

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details
