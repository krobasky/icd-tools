# icd-tools
Short applications to aid in icd code conversions 

# icdjson.py
```
$ python icdjson.py --help
usage: icdjson.py [-h] [--icd ICD] [--code CODE] [--client_id CLIENT_ID]
                  [--client_secret CLIENT_SECRET]

Get the description of this ICD code. ICD9 and ICD10 may be in output, but ICD11 will always be in output. Output format looks like this:
{
    "icd9": {},
    "icd10": {
        "code": "Z98.0",
        "name": "Intestinal bypass and anastomosis status"
    },
    "icd11": {
        "chapter": "24",
        "property": "Synonym",
        "label": "<em class='found'>intestinal</em> <em class='found'>bypass</em> and <em class='found'>anastomosis</em> <em class='found'>status</em>",
        "score": 0.00016951415
    }
}

optional arguments:
  -h, --help            show this help message and exit
  --icd ICD             9, 10, or 11
  --code CODE           the code
  --client_id CLIENT_ID
                        for icd10 api access, get it from https://icd.who.int/icdapi
  --client_secret CLIENT_SECRET
                        for icd10 api access, get it from https://icd.who.int/icdapi
```
