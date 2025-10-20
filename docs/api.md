# API Reference

REST APIs provide programmatic ways to submit new jobs and to download data from both [Michigan Imputation Server](https://imputationserver.sph.umich.edu) and the [TOPMed Imputation Server](https://imputation.biodatacatalyst.nhlbi.nih.gov). It identifies users using authentication tokens, responses are provided in JSON format.

## Authentication
The TOPMed Imputation Server uses a token-based authentication mechanism for all automated scripts that use our APIs. The token is required for all future interaction with the server. The token can be created and downloaded from your [user profile](https://imputation.biodatacatalyst.nhlbi.nih.gov/#!pages/profile) (username -> Profile):

![Activate API](https://raw.githubusercontent.com/genepi/imputationserver-docker/master/images/api.png)

!!! note
    Tokens will expire after 30 days. **Please keep your token secure** and never commit to a public repository (such as GitHub). We reserve the right to revoke API credentials at any time if an automated script is causing problems.


## Job Submission
The API allows setting several imputation parameters.

### TOPMed Imputation Server Job Submission

URL: https://imputation.biodatacatalyst.nhlbi.nih.gov/api/v2
POST /jobs/submit/imputationserver

The following parameters can be set:

| Parameter     | Values                                            | Default Value | Required |
| ------------- | ------------------------------------------------- | ------------- | -------- |
| files         | /path/to/file                                     | -             | **x**    |
| refpanel      | `apps@topmed-r3`                                  | -             | **x**    |
| population    | `all` <br> `off`                                  | -             | **x**    |
| mode          | `qconly`<br> `phasing` <br> `imputation`          | `imputation`  |          |
| job-name      | (user specified)                                  | (job ID)      |          |
| phasing       | `eagle`<br> `no_phasing`                          | `eagle`       |          |
| build         | `hg19`<br> `hg38`                                 | `hg19`        |          |
| r2Filter      | `0` <br> `0.001` <br> `0.1` <br> `0.2` <br> `0.3` | `0`           |          |
| aesEncryption | `no` <br> `yes`                                   | `no`          |          |
| meta          | `no` <br> `yes`                                   | `no`          |          |

* The _meta_ option generates a meta-imputation file.
* AES 256 encryption is stronger than the default option, but `.zip` files using AES 256 cannot be opened with common decompression programs. If you select this option, you will need a tool such as [7-zip](https://www.7-zip.org/download.html) to open your results.

### Examples

### Examples: curl

#### Submit file(s) using TOPMed

To submit a job please change `/path-to-file` to the actual path. This example can be adapted to send one, or multiple, files. (one per chromosome)

```sh
TOKEN="YOUR-API-TOKEN";

curl https://imputation.biodatacatalyst.nhlbi.nih.gov/api/v2/jobs/submit/imputationserver \
  -X "POST" \
  -H "X-Auth-Token: ${TOKEN}" \
  -F "job-name=Documentation example (1000G - chr1 and 2)" \
  -F "files=@/path-to/filename_chr1.vcf.gz" \
  -F "files=@/path-to/filename_chr2.vcf.gz" \
  -F "refpanel=apps@topmed-r3" \
  -F "build=hg38" \
  -F "phasing=eagle" \
  -F "population=all" \
  -F "meta=yes"
```


Response:

```json
{
  "id":"job-20160101-000001",
  "message":"Your job was successfully added to the job queue.",
  "success":true
}
```

### Examples: Python

#### Submit one or more vcf files

```python3
import requests

# imputation server url
base = 'https://imputation.biodatacatalyst.nhlbi.nih.gov/api/v2'
token = 'YOUR-API-TOKEN'

# add token to header (see documentation for Authentication)
headers = {'X-Auth-Token' : token }
data = {
  'job-name': 'Documentation example (1000G - chr1 and 2)',
  'refpanel': 'apps@topmed-r3',
  'population': 'all',
  'build': 'hg38',
  'phasing': 'eagle',
  'r2Filter': 0,
  'meta': 'yes',
}

# submit new job. This demonstrates multiple files, one per chromosome. Edit to send one or more chromosomes, as needed.
vcf1 = '/path/to/filename_chr1.vcf.gz'
vcf2 = '/path/to/filename_chr2.vcf.gz'

with open(vcf1, 'rb') as f1, open(vcf2, 'rb') as f2:
    files = [
        ('files', f1),
        ('files', f2)
    ]

    endpoint = "/jobs/submit/imputationserver"
    resp = requests.post(base + endpoint, files=files, data=data, headers=headers)

output = resp.json()

if resp.status_code != 200:
  print(output['message'])
  raise Exception('POST {} {}'.format(endpoint, resp.status_code))
else:
    # print message
    print(output['message'])
    print(output['id'])
```


## List all jobs
Return a list of all currently running jobs (for the user associated with the provided token).

### GET /jobs

### Examples: curl

Command:

```sh
TOKEN="YOUR-API-TOKEN";

curl -H "X-Auth-Token: ${TOKEN}" https://imputation.biodatacatalyst.nhlbi.nih.gov/api/v2/jobs
```

Response:

```json
{
  "count": 1,
  "page": 1,
  "pages": [
    1
  ],
  "data": [
    {
      "app": null,
      "application": "Genotype Imputation (Minimac4) 1.8.0",
      "canceld": false,
      "complete": true,
      "currentTime": 1687898833855,
      "endTime": 0,
      "finishedOn": 1687898825867,
      "id": "job-20230627-204701-307",
      "name": "job-20230627-204701-307",
      "positionInQueue": -1,
      "priority": 0,
      "progress": -1,
      "setupEndTime": 1687898822002,
      "setupRunning": false,
      "setupStartTime": 1687898821563,
      "startTime": 0,
      "state": 5,
      "submittedOn": 1687898821315,
      "userAgent": "curl/7.87.0",
      "workspaceSize": ""
    }
  ]
}
```

### Example: Python

```python
import requests

# imputation server url
url = 'https://imputation.biodatacatalyst.nhlbi.nih.gov/api/v2'
token = 'YOUR-API-TOKEN'

# add token to header (see authentication)
headers = {'X-Auth-Token' : token }

# get all jobs for the user associated with this token
endpoint = "/jobs"
resp = requests.get(url + endpoint, headers=headers)
if resp.status_code != 200:
    raise Exception('GET {} {}'.format(endpoint, resp.status_code))

# print all jobs
for job in resp.json()['data']:
    print('{} [{}]'.format(job['id'], job['state']))
```

## Monitor Job Status

### /jobs/{id}/status
This endpoint includes basic information about a job, such as execution status (waiting = 1, running = 2, waiting to export results = 3, success = 4, failed = 5, canceled = 6). See `/jobs/{id}` for another endpoint that provides information about individual step progress, similar to the logs in the UI.

### Example: curl

Substitute your job ID into the examples as requested. This is commonly used to monitor the status of one specific submitted job. To avoid overwhelming the server, we ask that you rate-limit your automated scripts to check no more often than once per minute. (not included in the example below)

Command:

```sh
TOKEN="YOUR-API-TOKEN";

curl -H "X-Auth-Token: $TOKEN" https://imputation.biodatacatalyst.nhlbi.nih.gov/api/v2/jobs/job-20160101-000001/status
```

Response:

```json
{
  "app": null,
  "application": "Genotype Imputation (Minimac4) 1.8.0",
  "applicationId": "imputationserver",
  "canceld": false,
  "complete": true,
  "currentTime": 1687898968305,
  "deletedOn": -1,
  "endTime": 0,
  "finishedOn": 1687898825867,
  "id": "job-20160101-000001",
  "logs": "",
  "name": "job-20160101-000001",
  "outputParams": [],
  "positionInQueue": -1,
  "priority": 0,
  "progress": -1,
  "running": false,
  "setupEndTime": 1687898822002,
  "setupRunning": false,
  "setupStartTime": 1687898821563,
  "startTime": 0,
  "state": 5,
  "steps": [],
  "submittedOn": 1687898821315,
  "workspaceSize": ""
}
```

## Monitor Job Details

This endpoint includes more specific information about a job and completed steps, similar to what is shown in the website job details page. Generally, this endpoint is less helpful for automated scripts; the information in this endpoint is easier to read from within the website UI.

### /jobs/{id}

### Example: curl

```sh
TOKEN="YOUR-API-TOKEN";

curl -H "X-Auth-Token: $TOKEN" https://imputation.biodatacatalyst.nhlbi.nih.gov/api/v2/jobs/job-20160101-000001/
```
