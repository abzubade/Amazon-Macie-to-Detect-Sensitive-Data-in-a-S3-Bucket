# Using Amazon Macie to detect Sensitive Data in S3 buckets

## Introduction

In this lab, I used Amazon Macie to detect sensitive data inside my AWS Account. Macie applies machine learning and pattern matching techniques to identify and alert you to sensitive data, such as personally identifiable information (PII). 

## Creating S3 Bucket

Connected to Cloud Shell in AWS and ran the below commands 
1. This command pulls in the data from a REPO and created a folder named amazon-macie-detect-sensitive-data-lab :

WORKDIR="$([ -d "$HOME/environment" ] && echo "$HOME/environment" || echo "$HOME")" && \
cd "$WORKDIR" && \
REPO=amazon-macie-detect-sensitive-data-lab && \
rm -rf "$REPO" && \
git clone --depth 1 https://github.com/aws-samples/$REPO.git --quiet && \
cd "$REPO" && \
python3 -m pip install --user --quiet boto3 && \
python3 setup_lab.py && \
cd "$WORKDIR"

2. Created the S3 bucket:
<br> aws s3 mb s3://my-new-bucket-name

3. Copied contents into S3 bucket
<br> aws s3 cp ./data s3://macie-devlab-anz-dba/ --recursive

4. Now that we have the S3 bucket. next we'll create a Macie Job, this job will analyze objects in any number of S3 buckets in this account to detect sensitive data.

To do this, select 'Get Started' and from then select Create Job under Analyze Buckets.

<img width="1059" height="637" alt="Screenshot 2025-09-28 at 5 41 07 PM" src="https://github.com/user-attachments/assets/48ab9243-018f-4c77-b55c-b9e9eca0d324" />

6. On the next screen, select 'Select specific buckets' to specify the bucket you wish to scan. Then, select the bucket whose name begins with 'macie-devlab'. This bucket has been prepopulated with test data for us to scan. There is a chance that it may take some time before the bucket appears under the listed buckets:

<img width="1657" height="644" alt="Screenshot 2025-09-28 at 5 40 57 PM" src="https://github.com/user-attachments/assets/ccb42722-19bd-402c-a086-e673e335b8c6" />

7. The next step is to review our buckets. You should see a single bucket with some estimated cost (the estimated cost might be $0.00). Click on Next.

<img width="1655" height="652" alt="Screenshot 2025-09-28 at 5 42 03 PM" src="https://github.com/user-attachments/assets/3742a2bc-5acc-4cd0-a410-79bd6911d1c3" />

8. Next, we'll need to refine the scope of our discovery job. For this lab, we'll use a one-time job with 100% sampling depth. In your production environments, you might run daily jobs that scan only new objects. Similarly, for very large buckets, e.g. buckets wih millions of objects, customers can choose a sampling depth to run analysis on a subset of the objects in the bucket.

For the purpose of the lab, the test data is small (<10 objects), so we'll run a one-time job with 100% sampling depth.

<img width="1657" height="652" alt="Screenshot 2025-09-28 at 6 24 10 PM" src="https://github.com/user-attachments/assets/09f30461-0ac2-4acb-ab60-545699be447a" />

9. Now, we can select managed data identifiers. A set of built-in criteria and techniques that are designed to detect a specific type of sensitive data. Examples of sensitive data include credit card numbers, AWS secret access keys, or passport numbers for a particular country or region. These identifiers can detect a large and growing list of sensitive data types for many countries and regions. Select "Recommended," which will select all 35 in that list.

https://github.com/user-attachments/assets/188d6e39-4197-4dc1-87ec-0417814fecf7

10. Additionally, you can also specify custom data identifiers to help Macie identify sensitive data outside of what is provided by the managed data identifiers. As part of this lab I pre-populated a custom data identifier called 'Gotham Passport Number', which will detect any data based on a regular expression we specified. Part of the test data will have one instance of this to demonstrate how Macie will analyze the finding. You will have to create the "Custom Data Identifier" outside of the job creation.

<img width="1905" height="653" alt="Screenshot 2025-09-28 at 6 38 25 PM" src="https://github.com/user-attachments/assets/6677c66e-41f1-4d3f-aca8-1ec8aee52f90" />

Finally we can review and scroll down to click submit. The job might not have any information on the bucket yet, and hence would estimate the cost to be $0.00, we can ignore this for now.

<img width="1398" height="793" alt="Screenshot 2025-09-28 at 6 43 32 PM" src="https://github.com/user-attachments/assets/a73069a7-bc4b-40f4-b5ce-e1cff20893dd" />

## Reviewing Test data

The Macie job will typically 5-10 minutes before completing. While we're waiting, let's use this time to analyze the test data in the bucket.

To do this, go back the Cloud9 IDE, and on the left side of the pane, you can select `macie-ANZ-devlabs/test-data`. You can also view the test data on this repo directly under the `test-data` directory. 

All test data is purely made up and do not reference any real person, phone number of credit card data.

## Reviewing Macie Finding

After about 5-10 minutes, findings should begin to appear in your Macie Console. Click on the findings to learn more about what sensitive data Macie discovered. To view the findings, click on the Findings link in the Macie menu. 

Each finding includes the severity level, contextual details—such as the specific resource where sensitive data was detected and relevant metadata—enabling you to assess potential exposure, evaluate risk, and make informed security decisions.

<img width="1709" height="744" alt="Screenshot 2025-09-28 at 7 04 33 PM" src="https://github.com/user-attachments/assets/777564d8-19fd-4d9d-8200-c17a1d286f6a" />

<img width="1683" height="733" alt="Screenshot 2025-09-28 at 7 07 59 PM" src="https://github.com/user-attachments/assets/c435fbb9-3a61-4ffc-a39e-7ede6f24f51e" />


## Conclusion

In this lab, I used Macie to analyze test objects in an S3 bucket. We used manage data identifiers together with a custom data identifier based on a regular expression we specified. We then started the Macie Data Discovery Job, and finally reviewed the results.

As next steps, you might want to test Macie in your own environments using the same methods we walked through today, or test Macie with other test data you might think off.




O
