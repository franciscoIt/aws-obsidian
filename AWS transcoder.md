Amazon Elastic Transcoder was a cloud-based media transcoding service that let developers and businesses convert video and audio files into formats optimized for various devices like smartphones, tablets, and PCs. It leveraged AWS infrastructure so users didn't need to manage complex transcoding hardware or software. [G2](https://www.g2.com/products/amazon-elastic-transcoder/reviews)[G2](https://www.g2.com/products/amazon-elastic-transcoder/reviews)

### Core Concepts

#### 1. Pipelines

Pipelines let you manage and run multiple transcoding workflows in parallel, enabling efficient processing of large volumes of media files. A pipeline was configured with: [G2](https://www.g2.com/products/amazon-elastic-transcoder/reviews)

- An input S3 bucket
- An output S3 bucket (and storage class)
- An IAM role used by the service to access your files [Amazon Web Services](https://aws.amazon.com/elastictranscoder/details/)

#### 2. Jobs

A job converts a single input file into multiple output formats, supporting up to 30 different formats per job. [G2](https://www.g2.com/products/amazon-elastic-transcoder/reviews)

#### 3. Presets

System and custom presets provided predefined settings optimized for various devices, or let you create custom presets tailored to specific requirements. [G2](https://www.g2.com/products/amazon-elastic-transcoder/reviews)

#### 4. Monitoring

- Job/pipeline status was viewable via the AWS Console, the Elastic Transcoder API, or SDKs. [Amazon Web Services](https://aws.amazon.com/elastictranscoder/details/)
- It automatically published operational metrics into CloudWatch (jobs completed, jobs errored, output minutes generated, standby time, API errors/throttles), appearing within minutes of a job running. [Amazon Web Services](https://aws.amazon.com/elastictranscoder/details/)
- It used Amazon SNS to send notifications about transcoding events. [Amazon Web Services](https://aws.amazon.com/documentation-overview/elastic-transcoder/)

#### Pricing (historical)

The Free Tier included up to 20 minutes of transcoding per month. [Amazon Web Services](https://aws.amazon.com/elastictranscoder/details/)

### Why It Was Deprecated

As media processing needs evolved (4K/8K, HDR, DRM, modern codecs like AV1/HEVC), AWS built **MediaConvert** as a more advanced, cheaper alternative and phased out Elastic Transcoder.

### Migrating to MediaConvert

|Elastic Transcoder|MediaConvert Equivalent|
|---|---|
|Pipeline|Queue|
|Preset|Job template / preset|
|SNS notifications on pipeline|CloudWatch Events (EventBridge) rules on job status changes|
|Basic codec support|AV1, HEVC, HDR, DRM, 8K, broadcast-grade features|

MediaConvert offers on-demand rates starting at $0.0075/minute, and AWS provides a migration guide with a script to convert existing Elastic Transcoder presets and jobs over.