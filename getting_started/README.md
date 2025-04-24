# Getting Started with Amazon Polly

This directory contains Jupyter notebooks that demonstrate how to use Amazon Polly's text-to-speech capabilities. These notebooks provide a hands-on introduction to the service and guide you through various features and use cases.

## Prerequisites

Before running these notebooks, make sure you have:

- An AWS account with access to Amazon Polly
- Python 3.9+ installed
- Required Python libraries: `boto3`, `IPython`, `jupyter`
- AWS credentials configured (via AWS CLI or environment variables)

## Setting Up Your Environment

1. Install the required Python packages:
   ```
   pip install boto3 notebook ipython
   ```

2. Configure your AWS credentials:
   ```
   aws configure
   ```
   Or set environment variables:
   ```
   export AWS_ACCESS_KEY_ID=your_access_key
   export AWS_SECRET_ACCESS_KEY=your_secret_key
   export AWS_DEFAULT_REGION=your_preferred_region
   ```

## Notebooks

### 1. Basic Speech Synthesis (`1.Basic_SynthesizeSpeech.ipynb`)

This notebook introduces you to:
- Setting up the Amazon Polly boto3 client
- Generating speech with different engines (Standard, Neural, Long-form)
- Using various voices and languages
- Saving audio files locally in different formats (MP3, OGG, PCM)
- Enhancing speech with SSML (Speech Synthesis Markup Language)

### 2. Asynchronous Speech Synthesis (`2.SynthesizeSpeech_Task_S3.ipynb`)

This notebook covers more advanced scenarios:
- Setting up the Amazon Polly and S3 boto3 clients
- Creating and managing speech synthesis tasks
- Handling long-form content that exceeds synchronous API limits
- Monitoring task status and retrieving results from S3
- Processing multiple synthesis tasks in parallel

## Running the Notebooks

1. Start Jupyter Notebook:
   ```
   jupyter notebook
   ```

2. Navigate to the notebook you want to run and open it.

3. Run the cells in sequence, following the instructions in the notebook.

4. For the second notebook, make sure to replace the placeholder S3 bucket name with your actual bucket name.

## Additional Resources

- [Amazon Polly Documentation](https://docs.aws.amazon.com/polly/latest/dg/what-is.html)
- [Boto3 Polly Documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/polly.html)
- [SSML Reference](https://docs.aws.amazon.com/polly/latest/dg/ssml.html)
