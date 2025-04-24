# Amazon Polly Samples

Notebooks and examples on how to onboard and use various features of Amazon Polly, AWS's text-to-speech service.

## What is Amazon Polly?

Amazon Polly is a cloud service that converts text into lifelike speech. It offers dozens of voices across multiple languages and a variety of neural and standard voices. With Polly, you can:

- Build applications that talk naturally
- Create voice-enabled products
- Generate speech for content such as news articles, customer service scripts, and educational materials
- Enhance accessibility of your applications

## Getting Started with Amazon Polly

The [getting_started/](getting_started/) folder contains Jupyter notebooks that walk you through the basics of using Amazon Polly:

1. **Basic Speech Synthesis**: Learn how to use the boto3 Polly client to call synthesizeSpeech for different engines and save the audio files locally.

2. **Asynchronous Speech Synthesis**: Discover how to create synthesis tasks, check their status, and retrieve results from S3 for longer content or batch processing.

These notebooks serve as templates for building your own text-to-speech applications with Amazon Polly.

## Repository Structure

- [PollyCheatSheet.md](PollyCheatSheet.md): A comprehensive reference guide with terminology, API details, SSML examples, cost optimization tips, and sample code snippets.

- [getting_started/](getting_started/): Beginner-friendly notebooks to start using Amazon Polly.

## Prerequisites

To use these examples, you'll need:

- An AWS account with access to Amazon Polly
- Python 3.9+ with boto3 installed
- AWS credentials configured locally
- For asynchronous examples: An S3 bucket with appropriate permissions

## Additional Resources

- [Amazon Polly Documentation](https://docs.aws.amazon.com/polly/latest/dg/what-is.html)
- [Amazon Polly Pricing](https://aws.amazon.com/polly/pricing/)
- [Amazon Polly Feature Updates](https://aws.amazon.com/about-aws/whats-new/machine-learning/?whats-new-content.sort-by=item.additionalFields.postDateTime&whats-new-content.sort-order=desc&awsf.whats-new-products=general-products%23amazon-polly)

## Future Enhancements

Future additions to this repository might include:

- Advanced SSML usage examples
- Custom lexicon creation and usage
- Integration examples with common frameworks and platforms
- Serverless deployment patterns with Lambda and API Gateway
- Batch processing of large text collections
- Multi-language applications

## License

This sample code is made available under a modified MIT license. See the LICENSE file for details.
