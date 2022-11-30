# NLP Bible and Quran Analysis


## Project Overview

In this project I will be exploring the world of NLP (Natural Language Processing) to see how negative or positive the text in the Bible and Quran are. By adding the ascent and spaceytextblog pipelines to the spaCy model, I will be able to extract the polarity which is a number on a scale from -1 to 1 denoting how "positive" or "negative" text are. Once aggregated and grouped by book or Juz, I should be able to see the polarity of the texts from start to finish.

## Imports

![image](https://user-images.githubusercontent.com/10213983/204729422-18447f21-f3a1-4798-a404-6996895edf91.png)

## Methods
In order to meaningfully analyze text data, which comprises over 75% of all data online, you have to be able to analyze each word separately. 
This method of separating all the words from one another is called tokenization. I created a spaCy model and added the asent, sentensizer, 
and spacytextblob pipelines to the model.

![image](https://user-images.githubusercontent.com/10213983/204728941-ad66452a-0913-4ba8-b057-a3180d1fdaa0.png)


## Findings

## American Standard Version
![asv](https://user-images.githubusercontent.com/10213983/204638424-82b5eb01-b76f-4f83-9bdb-ff6a9b9b0e5d.png)

## Quran
![enq](https://user-images.githubusercontent.com/10213983/204638427-b0b98c99-dd69-45e6-872e-b49dc846cf7d.png)

## King James Version
![kjv](https://user-images.githubusercontent.com/10213983/204638434-00d4da77-77ec-40ad-966d-cf065561550d.png)

## World English Bible
![web](https://user-images.githubusercontent.com/10213983/204638437-e281d828-9c11-4cee-bcf9-2d7d1175d0a8.png)

# WordClouds

## Quran
![enq_wordcloud](https://user-images.githubusercontent.com/10213983/204638500-edebfe18-bf4e-4fd2-8815-48f89f1ad7de.png)

## King James Version
![kjv_wordcloud](https://user-images.githubusercontent.com/10213983/204638503-cb8b88ce-5e3b-43ca-aee6-53c72ed5ca86.png)

## World English Bible
![web_wordcloud](https://user-images.githubusercontent.com/10213983/204638505-1972f3b2-8e6b-4d2c-af52-66f479088703.png)

## American Standard Version
![asv_wordcloud](https://user-images.githubusercontent.com/10213983/204638507-e8d39300-1c1a-47ca-ae68-6b0185a43e85.png)
