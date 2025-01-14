# Textractor-Textract-Helper

amazon-textractor-textract-helper provides a collection of ready to use functions and sample implementations to speed up the evaluation and development for any project using Amazon Textract.


# Install

```bash
> python -m pip install amazon-textract-helper
```

Make sure your environment is setup with AWS credentials through configuration files or environment variables or an attached role. (https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)

# Samples

## Easy Start

```bash
> amazon-textract --example
```

this will run the examples document using the DetectDocumentText API.
Output will be printed to stdout and look similar to this:

```
{"DocumentMetadata": {"Pages": 1}, "Blocks": [{"BlockType": "PAGE", "Geometry": {"BoundingBox": {"Width": 1.0, "Height": 1.0, "Left": 0.0
, "Top": 0.0}, "Polygon": [{"X": 9.33321120033382e-17, "Y": 0.0}, {"X": 1.0, "Y": 1.6069064689339292e-16}, {"X": 1.0, "Y": 1.0}],
"HTTPHeaders": {"x-amzn-requestid": "12345678-1234-1234-1234-123456789012", "content-type": "a
pplication/x-amz-json-1.1", "content-length": "48177", "date": "Thu, 01 Apr 2021 21:50:29 GMT"}, "RetryAttempts": 0}}
```

It is working.

## Call with document on S3

```
> amazon-textract --input-document "s3://somebucket/someprefix/someobjectname.png"
```

Output similar to Easy Start

## Call with document on local file system

```
> amazon-textract --input-document "./somepath/somefilename.png"
```

Output similar to Easy Start

We will continue to use the ```--example``` parameter to keep it simple and easy to reproduce. S3 and local files work the same way, just instead of --example use --input-document <location>.

## Call with FORMS and TABLES

```bash
> amazon-textract --example --features FORMS TABLES
```

This will call the [AnalyzeDocument API] (https://docs.aws.amazon.com/textract/latest/dg/API_AnalyzeDocument.html) and output will include
Output will look similar to "Easy Start" but include  

## Pretty print the output

Pretty print outputs nicely formatted information for words, lines, forms or tables.

For example to print the tables identified by Amazon Textract to stdout, use

```bash
> amazon-textract --example --features TABLES --pretty-print TABLES
```

Output will look like this:

```
|------------|-----------|---------------------|-----------------|-----------------------|
|            |           | Previous Employment | History         |                       |
| Start Date | End Date  | Employer Name       | Position Held   | Reason for leaving    |
| 1/15/2009  | 6/30/2011 | Any Company         | Assistant Baker | Family relocated      |
| 7/1/2011   | 8/10/2013 | Best Corp.          | Baker           | Better opportunity    |
| 8/15/2013  | present   | Example Corp.       | Head Baker      | N/A, current employer |

```

to pretty print both, FORMS and TABLES:

```
> amazon-textract --example --features FORMS TABLES --pretty-print FORMS TABLES
```

will output

```
Phone Number:: 555-0100
Home Address:: 123 Any Street, Any Town, USA
Full Name:: Jane Doe
Mailing Address:: same as home address
|------------|-----------|---------------------|-----------------|-----------------------|
|            |           | Previous Employment | History         |                       |
| Start Date | End Date  | Employer Name       | Position Held   | Reason for leaving    |
| 1/15/2009  | 6/30/2011 | Any Company         | Assistant Baker | Family relocated      |
| 7/1/2011   | 8/10/2013 | Best Corp.          | Baker           | Better opportunity    |
| 8/15/2013  | present   | Example Corp.       | Head Baker      | N/A, current employer |
```

## Overlay

**At the moment overlay only works with images, we will add support for PDF soon.**

The following command runs DetectDocumentText, pretty prints the WORDS in the document to stdout and draws bounding boxes around each WORD and displays the result in a popup window and stores it to a folder called 'overlay-output-folder-name'.

```
amazon-textract --example --pretty-print WORDS --overlay WORD --pop-up-overlay-output --overlay-output-folder overlay-output-folder-name
```

<img src="./docs/employmentapp_boxed_WORD_.png" alt="Sample overlay WORD" width="50%" height="50%" border="1">



The following command runs AnalyzeDocument for FORMS and TABLES, pretty prints FORMS and TABLES to  to stdout and draws bounding boxes around each TABLE-CELL and FORM KEY/VALUE and displays the result in a popup window and stores it to a folder called 'overlay-output-folder-name'.

```
> amazon-textract --example --features TABLES FORMS --pretty-print FORMS TABLES --overlay FORM CELL --pop-up-overlay-output --overlay-output-folder ../docs/
```


<img src="./docs/employmentapp_boxed_FORM_CELL_.png" alt="Sample overlay FORM CELL" width="50%" height="50%" border="1">


