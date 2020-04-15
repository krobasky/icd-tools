# icd-tools
Short applications to aid in icd code conversions 

# icdjson.py
This is a simple script that can be used  to automate portions of the process for mapping ICD9 and/or ICD10 codes to a common, ICD11 code.

## example:
PYTHONWARNINGS="ignore:Unverified HTTPS request" python icd.py --icd 10 --code Z98.0 --client_id ${id} --client_secret ${secret}

See more about how to obtain CLIENT_ID and CLIENT_SECRET here: https://icd.who.int/icdapi/docs2/API-Authentication/

## usage:
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
## Algorithm:
1. naively query the name of the input code from the appropriate endpoint, depending the system (e.g., ICD9 vs ICD10)
2. naively query the ICD11 codes for matches to the input code's name
3. return a json containing the query code and name along with the best matching target code and name, with the name annotated based on the query results.

### ICD9 code name query
Uses `https://clinicaltables.nlm.nih.gov/apidoc/icd9cm_dx/v3` to query the name of the input ICD9 code. If the code isn't found, a warning is output to stderr.

### ICD10 code name query
First queries `https://id.who.int/icd/release/10/2016/`, if not found then authenticates and queries `http://icd10api.com/?code="+code+"&desc=long&r=json`

### ICD11 code lookup
Authenticates at icd10api.com and searches for the input code's query name (described above), excluding ICD11 chapters 21, 25-V. 
e.g.,
`http://icd10api.com/icd/release/11/2019-04/mms/search?includeKeywordResult=false&chapterFilter=01%3B02%3B03%3B04%3B05%3B06%3B07%3B08%3B09%3B10%3B11%3B12%3B13%3B14%3B15%3B16%3B17%3B18%3B19%3B20%3B22%3B23%3B24&useFlexisearch=true&flatResults=false&q=<name>`

