# पारदर्शी टिप्पणी पुस्तक

यह रचना मेरे भविष्य के *मस्तिष्क की स्वास्थ्य हेतु* बनाई गयी है। 

इस पुरालेख का स्तुति परिचय `ssh` और `https` में। 

```
git@github.com:0x3508A/-.git
```

```
https://github.com/0x3508A/-.git
```

## कार्यविधि

```sh
# यह प्रणाली कोई भी लिनक्स के लिये प्रयुक्त है।
python3 -m venv ~/.cache/temp-env

# इस लागु करने की विधि।
source ~/.cache/temp-env/bin/activate

# अब सब निर्भर कारक गठरीओ को प्रबुद्धा करने की विधि।
pip install mkdocs-material pillow cairosvg mkdocs-minify-plugin \
	mkdocs-no-sitemap-plugin

# अब इन सभी गठरीओ का विन्यासित करने की विधि, पर ये आवश्यक नहीं है।
pip3 freeze > requirements.txt

# इस चलाने कि विधि।
mkdocs serve

# अंत मे सब समाप्त करने के लिए इस विधि प्रयोग करें।
source ~/.profile
rm -rf ~/.cache/temp-env
```

## अनुज्ञापत्र अथवा `लाइसेंस` (License)

```
(C) Copyright (C) 2023 Abhijit Bose - All Rights Reserved
This work is licensed under the Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License.
To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-nd/4.0/ or
send a letter to Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.
```

All software, documents, artifacts, files of any type and articles
found in this repository are governed by the following license -

**Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)**

<https://creativecommons.org/licenses/by-nc-nd/4.0/>

[LICENSE.txt file](./LICENSE.txt)

`SPDX: CC-BY-NC-ND-4.0`

Contents have been shared under the following terms specified by the above license:

- **Attribution** — You must give appropriate credit, provide a link to the license, and indicate if changes were made. You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use.

- **NonCommercial** — You may not use the material for commercial purposes.

- **NoDerivatives** — If you remix, transform, or build upon the material, you may not distribute the modified material.

- **No additional restrictions** — You may not apply legal terms or technological measures that legally restrict others from doing anything the license permits.

