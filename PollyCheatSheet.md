# Amazon Polly Cheat Sheet

## Service Overview

### What is Amazon Polly?
Amazon Polly turns your text into natural-sounding speech. It's a cloud service that gives your apps realistic voices for all kinds of content.

### Key Benefits
- Lifelike speech with natural emotion
- Fast responses for real-time apps
- Cost-effective solution
- Many voices and languages to choose from
- Works for apps of any size

### Use Cases
- Making content accessible for people with reading difficulties
- Voice menus and customer service systems
- Learning platforms and educational content
- Public announcements and news
- GPS and navigation voices
- Voice assistants and chatbots
- Game characters and narration
- Audio books and podcasts

### Notable Features
- **Brand Voice**: Create custom voices for your organization (requires AWS team engagement)
- **Newscaster Style**: Make your content sound like a professional news broadcast
- **Speech Marks**: Get timing info to sync animations with speech
- **Whispered Speech**: Add whispered voice for certain content
- **Multiple Formats**: Output as MP3, OGG, or PCM
- **Streaming**: Stream audio directly to your users
- **Language Mixing**: Mix multiple languages in one speech request

## Core Concepts

### Speech Synthesis
The process of turning written text into spoken words. Amazon Polly creates lifelike speech using different voices and languages.

### Voice Types
Digital personalities with unique speaking styles. Choose from voices across multiple languages to fit your needs.

### SSML (Speech Synthesis Markup Language)
Use SSML to control how your text sounds. You can adjust pronunciation, volume, pitch, speech rate, and more with simple tags.

### Lexicon
Your custom pronunciation guide that tells Polly exactly how to say specific words or phrases.

### Synthesis Task
A speech job for longer content. Perfect for articles, books, or batch processing needs.

## Engine Types and Capabilities

### Engine Types
- **Standard**: Basic voice that's fast and affordable
- **Neural**: Higher-quality voice with natural intonation
- **Long-Form**: Optimized for reading articles and books
- **Generative**: Most human-like voices for realistic conversations

### Engine Comparison

| Feature | Standard | Neural | Long-form | Generative |
|---------|----------|--------|-----------|-----------|
| Sound Quality | Good | Better | Best for long content | Most human-like | 
| Speed | Fastest | Fast | Slower | Slower |
| Cost | $4 / million char | $16 / million char | $100 / million char | $30 / million char |
| Char Limit (Live) | 3,000 | 3,000 | 3,000 | 3,000 |
| Char Limit (Batch) | 100,000 | 100,000 | 100,000 | 100,000|
| SSML Support | Full | Partial | Partial | Partial |
| Special Features | Basic | newscaster, speaking styles | Better paragraphs | Emotional speech |

### Voice Capabilities
- Choose from 60+ voices in 30+ languages
- Check which voices work with each engine using the `describe_voices` [API](https://docs.aws.amazon.com/polly/latest/dg/API_DescribeVoices.html)
- Neural and Generative voices available in select languages
- Long-form works with select neural voices
- Generative voices limited to certain regions

### Popular Voices

| Voice | Language | Gender | Engines |
|-------|----------|--------|---------|
| Joanna | US English | Female | Standard, Neural, Generative |
| Matthew | US English | Male | Standard, Neural |
| Stephen | US English | Male | Neural, Generative |
| Emma | UK English | Female | Standard, Neural |
| Brian | UK English | Male | Standard, Neural |
| Lupe | US Spanish | Female | Standard, Neural |
| Hans | German | Male | Standard, Neural |
| Léa | French | Female | Standard, Neural |
| Takumi | Japanese | Male | Standard, Neural |

# Quick Start

### Setup
```bash
# Install required packages
pip install boto3 ipython

# For Jupyter notebook support
pip install jupyter
```

Make sure you've configured your AWS credentials either through:
- AWS CLI (`aws configure`)
- Environment variables
- Credentials file (`~/.aws/credentials`)

### Your First Polly Script
```python
import boto3
from io import BytesIO
import pygame

# Create Amazon Polly client
polly = boto3.client('polly')

# Request speech synthesis
response = polly.synthesize_speech(
    Text="Hello, welcome to Amazon Polly!",
    OutputFormat="mp3",
    VoiceId="Joanna",
    Engine="neural"
)

# Play the audio using pygame
if "AudioStream" in response:
    pygame.mixer.init()
    pygame.init()
    
    with BytesIO(response["AudioStream"].read()) as audio_file:
        pygame.mixer.music.load(audio_file)
        pygame.mixer.music.play()
        
        while pygame.mixer.music.get_busy():
            pygame.time.Clock().tick(10)
```

This simple example:
1. Creates a connection to the Amazon Polly service
2. Sends a short text to be converted to speech using Joanna's neural voice
3. Receives the audio stream in the response
4. Plays the audio using the pygame library

Try changing the text or using different voices and engines to hear the differences!

# Advanced Polly Usage

## Implementation Guide

### API Operations

#### Real-time API
- [**SynthesizeSpeech**](https://docs.aws.amazon.com/polly/latest/dg/API_SynthesizeSpeech.html): Convert text to speech quickly (3,000 character limit)

#### Batch Processing API
- [**StartSpeechSynthesisTask**](https://docs.aws.amazon.com/polly/latest/dg/API_StartSpeechSynthesisTask.html): Process longer text (100,000 character limit)
- [**GetSpeechSynthesisTask**](https://docs.aws.amazon.com/polly/latest/dg/API_GetSpeechSynthesisTask.html): Check job status
- [**ListSpeechSynthesisTasks**](https://docs.aws.amazon.com/polly/latest/dg/API_ListSpeechSynthesisTasks.html): View all your speech jobs

#### Custom Pronunciation Tools
- [**PutLexicon**](https://docs.aws.amazon.com/polly/latest/dg/API_PutLexicon.html): Save your custom pronunciations
- [**GetLexicon**](https://docs.aws.amazon.com/polly/latest/dg/API_GetLexicon.html): Get a saved pronunciation guide
- [**ListLexicons**](https://docs.aws.amazon.com/polly/latest/dg/API_ListLexicons.html): See all your pronunciation guides
- [**DeleteLexicon**](https://docs.aws.amazon.com/polly/latest/dg/API_DeleteLexicon.html): Remove a pronunciation guide

#### Voice Finder
- [**DescribeVoices**](https://docs.aws.amazon.com/polly/latest/dg/API_DescribeVoices.html): Find voices that match your needs

### Save Money

#### Choose the Right Engine
- Use Standard for internal tools and less critical content
- Save Neural and Generative for customer-facing experiences
- Only use Long-form when reading longer content like articles

#### Optimize Your Content
- Remove unnecessary text before sending
- Break long content into logical chunks
- Save and reuse common phrases
- Format text efficiently to use fewer characters

#### Smart Architecture
- Save frequently used phrases instead of regenerating them
- Pre-generate audio for content that doesn't change
- Set up smart storage rules for speech files
- Deliver audio globally with CloudFront

#### Manage Character Usage
- Track how many characters you're processing
- Set limits to prevent unexpected usage
- Set up alerts for unusual spikes
- Group similar requests when possible

### SSML Tips

#### Common Tags Reference

```xml
<speak>
    <!-- Add pauses -->
    This is a sentence <break time="1s"/> followed by a pause.
    
    <!-- Emphasize words -->
    This is <emphasis level="strong">really</emphasis> important.
    
    <!-- Control speech rate -->
    <prosody rate="slow">This text is spoken slowly.</prosody>
    <prosody rate="fast">This text is spoken quickly.</prosody>
    
    <!-- Control pitch -->
    <prosody pitch="high">This text is spoken in a higher pitch.</prosody>
    <prosody pitch="low">This text is spoken in a lower pitch.</prosody>
    
    <!-- Specify how to pronounce words -->
    You say tomato, I say <phoneme alphabet="ipa" ph="təˈmeɪtoʊ">tomato</phoneme>.
    
    <!-- Specify how to read certain types of text -->
    <say-as interpret-as="date" format="mdy">12-25-2021</say-as>
    <say-as interpret-as="telephone" format="1">2025550142</say-as>
    <say-as interpret-as="characters">SSML</say-as>
    
    <!-- Add whispered speech (standard voices only) -->
    <amazon:effect name="whispered">This is a secret.</amazon:effect>
    
    <!-- Add newscaster speaking style (some neural voices only) -->
    <amazon:domain name="news">Today's top story: Amazon Polly launches new voices.</amazon:domain>
</speak>
```

#### Best Practices
- Use SSML only where needed for special pronunciation
- Test your SSML with each voice you plan to use
- Make sure to close all tags properly
- Remember that SSML tags count toward your character limits
- Start with basic tags before using more advanced ones
- Create templates for common speech patterns in your app

#### Quick Examples
- **Acronyms**: `<say-as interpret-as="characters">AWS</say-as>`
- **Numbers**: `<say-as interpret-as="cardinal">42</say-as>`
- **Dates**: `<say-as interpret-as="date" format="mdy">12-25-2021</say-as>`
- **Phone Numbers**: `<say-as interpret-as="telephone">202-555-0142</say-as>`
- **Addresses**: `<say-as interpret-as="address">123 Main St.</say-as>`

## Integration Patterns

### Web Applications
#### Architecture
- Client-side: Generate speech directly in the browser
- Server-side: Create audio on your server, then send to users
- Hybrid: Use server for common phrases, client for personalized content

#### Best Practices
- Add good error handling and fallbacks
- Save frequently used phrases
- Add audio controls for better user experience
- Load longer audio progressively

### Mobile Applications
- Stream audio directly to phones
- Save common phrases on the device
- Use native audio players
- Process large requests in the background
- Be mindful of data usage

### IVR Systems
- Create dynamic responses based on user input
- Use custom pronunciations for specialized terms
- Add fallbacks for synthesis failures
- Design natural-sounding conversation flows
- Test with real scenarios

### Content Accessibility
- Turn articles into audio automatically
- Create audio versions of documents
- Make learning materials accessible to everyone
- Provide audio options for visual content
- Let users control playback speed and voice

### Batch Processing
- Process large amounts of content with the batch API
- Run batch jobs during off-hours
- Use S3 to distribute audio files
- Set up notifications for job completion
- Handle errors properly

### AWS Integration
- Use Lambda for serverless speech processing
- Store audio in S3
- Deliver content globally with CloudFront
- Manage high volumes with SQS queues
- Track usage with CloudWatch

# Practical Examples

### Sample Code Snippets

#### Asynchronous Synthesis with S3 (Python)
```python
import boto3
import time

# Create clients
polly = boto3.client('polly')
s3 = boto3.client('s3')

# Start a speech synthesis task
response = polly.start_speech_synthesis_task(
    Text="This is a long piece of text that exceeds the limits of the synchronous API...",
    OutputFormat="mp3",
    OutputS3BucketName="your-bucket-name",
    OutputS3KeyPrefix="speech/",
    VoiceId="Matthew",
    Engine="neural"
)

# Get the task ID
task_id = response['SynthesisTask']['TaskId']
print(f"Task ID: {task_id}")

# Wait for the task to complete
while True:
    task = polly.get_speech_synthesis_task(TaskId=task_id)
    status = task['SynthesisTask']['TaskStatus']
    
    if status == 'completed':
        output_uri = task['SynthesisTask']['OutputUri']
        print(f"Task completed! Audio available at: {output_uri}")
        break
    elif status == 'failed':
        print("Task failed!")
        break
    else:
        print(f"Task status: {status}")
        time.sleep(2)
```

#### Using SSML (Python)
```python
import boto3

# Create Amazon Polly client
polly = boto3.client('polly')

# SSML text
ssml_text = """
<speak>
    Hello! <break time="1s"/> Welcome to <emphasis level="strong">Amazon Polly</emphasis>.
    You can use SSML to add <prosody rate="slow">dramatic pauses</prosody>,
    control <prosody pitch="high">the pitch of the voice</prosody>,
    and even spell out <say-as interpret-as="characters">SSML</say-as>.
</speak>
"""

# Request speech synthesis with SSML
response = polly.synthesize_speech(
    Text=ssml_text,
    TextType="ssml",
    OutputFormat="mp3",
    VoiceId="Joanna",
    Engine="neural"
)

# Save the audio to a file
if "AudioStream" in response:
    with open("output.mp3", "wb") as file:
        file.write(response["AudioStream"].read())
    print("Audio saved to output.mp3")
```

### Common Use Case Solutions

#### Multilingual Content
```python
# Example for handling multilingual content
ssml_text = """
<speak>
    Here is some English text.
    <lang xml:lang="es-ES">Y aquí hay texto en español.</lang>
    <lang xml:lang="fr-FR">Et voici du texte en français.</lang>
</speak>
"""

response = polly.synthesize_speech(
    Text=ssml_text,
    TextType="ssml",
    OutputFormat="mp3",
    VoiceId="Joanna",  # Must be a voice that supports language mixing
    Engine="neural"
)
```

#### Dynamic Voice Selection
```python
def get_appropriate_voice(language_code, gender_preference=None):
    """Select appropriate voice based on language and gender preference"""
    response = polly.describe_voices(LanguageCode=language_code)
    voices = response['Voices']
    
    # Filter by gender if preference is specified
    if gender_preference:
        voices = [v for v in voices if v['Gender'].lower() == gender_preference.lower()]
    
    # Prioritize neural voices
    neural_voices = [v for v in voices if v['SupportedEngines'] and 'neural' in v['SupportedEngines']]
    
    if neural_voices:
        return neural_voices[0]['Id']
    elif voices:
        return voices[0]['Id']
    else:
        raise Exception(f"No voice found for language {language_code} and gender {gender_preference}")
```

#### Pronunciation Lexicon Implementation
```python
# Example lexicon in PLS format
lexicon_content = """
<?xml version="1.0" encoding="UTF-8"?>
<lexicon version="1.0" 
      xmlns="http://www.w3.org/2005/01/pronunciation-lexicon"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
      xsi:schemaLocation="http://www.w3.org/2005/01/pronunciation-lexicon 
        http://www.w3.org/TR/2007/CR-pronunciation-lexicon-20071212/pls.xsd"
      alphabet="ipa" 
      xml:lang="en-US">
  <lexeme>
    <grapheme>AWS</grapheme>
    <alias>Amazon Web Services</alias>
  </lexeme>
  <lexeme>
    <grapheme>SQL</grapheme>
    <alias>Sequel</alias>
  </lexeme>
</lexicon>
"""

# Store the lexicon
polly.put_lexicon(
    Name="tech-terms",
    Content=lexicon_content
)

# Use the lexicon in synthesis
response = polly.synthesize_speech(
    Text="AWS provides SQL database services.",
    OutputFormat="mp3",
    VoiceId="Matthew",
    LexiconNames=["tech-terms"]
)
```

# Troubleshooting Guide

#### Common Issues and Solutions

1. **Issue**: Technical terms sound wrong
   **Solution**: 
   - Create a custom dictionary for your special terms
   - Use the `<say-as>` tag to control pronunciation
   - Break down complex terms with `<phoneme>` tags

2. **Issue**: "TextSizeLimitExceeded" error
   **Solution**:
   - Check your character count (SSML tags included)
   - Split into smaller chunks
   - Use the batch API for longer content

3. **Issue**: Speech sounds unnatural or has odd pauses
   **Solution**:
   - Try a different voice or engine
   - Add SSML breaks and emphasis
   - Check your punctuation

4. **Issue**: "InvalidSsml" error
   **Solution**:
   - Check that all tags are properly closed
   - Make sure your XML is correctly formed
   - Verify the tags work with your chosen engine
   - Escape special characters (`&`, `<`, `>`, `'`, `"`)

5. **Issue**: Slow response in real-time apps
   **Solution**:
   - Save frequently used phrases
   - Use the Standard engine for faster responses
   - Pre-generate common audio clips
   - Use the same AWS region as your app

6. **Issue**: Unexpected high costs
   **Solution**:
   - Monitor usage with CloudWatch
   - Set character limits in your app
   - Review your caching strategy
   - Remove unnecessary spaces and content

# Reference

### FAQ

#### Technical Questions
1. **What's the difference between real-time and batch APIs?**
   - Real-time API: 3,000 character limit, returns audio instantly
   - Batch API: 100,000 character limit, saves results to S3

2. **How do I choose the right engine?**
   - Standard: For internal tools or when cost matters most
   - Neural: For customer-facing content that needs to sound good
   - Long-form: For articles, books, or other long content
   - Generative: For the most human-like speech experience

3. **How do I make acronyms sound right?**
   - Use `<say-as interpret-as="characters">AWS</say-as>`
   - Add a custom pronunciation to your lexicon
   - Spell with periods (e.g., "A.W.S.")

4. **Which languages are available?**
   - Over 30 languages with 60+ voices
   - Neural engines work with major languages
   - Check AWS docs for the current list

5. **Can Polly handle technical terms?**
   - Yes, but you'll need custom pronunciations or SSML
   - Break down complex terms phonetically
   - Test and refine how things sound

#### Implementation Questions
1. **Why does some text sound unnatural?**
   - Unusual words, acronyms, or numbers can sound odd
   - Try SSML tags to guide pronunciation
   - Create custom pronunciations for special terms

2. **Can I use Polly commercially?**
   - Yes, it's designed for commercial use
   - Check AWS Terms for any restrictions

3. **What's the best way to handle multiple languages?**
   - Create separate requests for each language
   - Use the right voice for each language
   - Consider auto-detecting languages in your app

4. **How do I integrate with other AWS services?**
   - Use AWS SDK in your code
   - Consider Lambda for serverless processing
   - Store audio in S3
   - Distribute with CloudFront

#### Cost Questions
1. **How can I keep costs down?**
   - Save frequently used audio
   - Use Standard voices when quality isn't critical
   - Watch your character usage
   - Use Standard for internal content, Neural for customer-facing

2. **Is there a free tier?**
   - AWS Free Tier includes some Polly usage
   - Standard voices cost less than Neural
   - Long-form and Generative aren't in the free tier

3. **How do I track usage?**
   - Use CloudWatch for usage metrics
   - Set up billing alerts
   - Log synthesis requests in your app

### Documentation Links
- [Amazon Polly Documentation](https://docs.aws.amazon.com/polly/latest/dg/what-is.html)
- [Amazon Polly Pricing](https://aws.amazon.com/polly/pricing/)
- [Amazon Polly Feature Updates](https://aws.amazon.com/about-aws/whats-new/machine-learning/?whats-new-content.sort-by=item.additionalFields.postDateTime&whats-new-content.sort-order=desc&awsf.whats-new-products=general-products%23amazon-polly)
- [SSML Reference Guide](https://docs.aws.amazon.com/polly/latest/dg/supportedtags.html)
- [Voice Characteristics Guide](https://docs.aws.amazon.com/polly/latest/dg/voices-in-polly.html)

### API Reference
- [API Guide](https://docs.aws.amazon.com/polly/latest/dg/API_Reference.html)
- [AWS CLI Reference for Polly](https://docs.aws.amazon.com/cli/latest/reference/polly/index.html)
- [AWS SDK Examples](https://docs.aws.amazon.com/polly/latest/dg/examples-python.html)

### Support Resources
- [AWS Knowledge Center](https://aws.amazon.com/premiumsupport/knowledge-center/)
- [AWS Support Plans](https://aws.amazon.com/premiumsupport/plans/)
- [AWS Community Forums](https://forums.aws.amazon.com/)
- [AWS Architecture Center](https://aws.amazon.com/architecture/)
- [AWS Workshops](https://workshops.aws/)
