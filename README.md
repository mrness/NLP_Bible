
# NLP BIBLE

asv_df = pd.read_csv("Resources/asv.csv")
asv_df['Combined'] = asv_df['Book Number'].astype(str) + " " + asv_df['Chapter'].astype(str) + ":" + asv_df['Verse'].astype(str)
asv_df.drop(['Chapter','Verse','Book Number'],axis=1, inplace=True)
asv_df = asv_df[["Verse ID","Book Name","Combined","Text"]]
asv_df.set_index("Verse ID",inplace=True)
asv_df.head(1)
# reading Qyran csv's to dataframes for NLP processing 


enq_df = pd.read_csv("Resources/enq.csv")
#enq_df["Verse Number"] = list(range(6236))
#enq_df.head(2)

# Reading King James bible csv's to dataframes for NLP processing 

kjv_df = pd.read_csv("Resources/kjv.csv", index_col="Verse ID")
kjv_df['Combined'] = kjv_df['Book Number'].astype(str) + " " + kjv_df['Chapter'].astype(str) + ":" + kjv_df['Verse'].astype(str)
kjv_df.drop(['Chapter','Verse','Book Number'],axis=1, inplace=True)
kjv_df = kjv_df[["Book Name","Combined", "Text"]]
kjv_df.head(1)
# Reading World English bible csv's to dataframes for NLP processing 

web_df = pd.read_csv("Resources/web.csv", index_col="Verse ID")
web_df['Combined'] = web_df['Book Number'].astype(str) + " " + web_df['Chapter'].astype(str) + ":" + web_df['Verse'].astype(str)
web_df.drop(['Chapter','Verse','Book Number'],axis=1, inplace=True)
web_df = web_df[["Book Name","Combined", "Text"]]
web_df.head(1)
# Choosing the correct library to analyze the text data. I chose the small library for github transfer size issues

spacy_model = 'en_core_web_sm'
# Adding spacy model and the sentencizer and asent/textblob pipelines

nlp = spacy.load(spacy_model)
nlp.add_pipe('sentencizer')
nlp.add_pipe('asent_en_v1')
nlp.add_pipe('spacytextblob')

# Doing the analysis and adding the values computed (polarity) for the two pipelines into their respective colums

verse_sentiments_asv = list(nlp.pipe(asv_df.Text.astype(str)))
asv_df["sentiment_asent"] = [verse_nlp._.polarity.compound for verse_nlp in verse_sentiments_asv]
asv_df['sentiment_textblob'] = [verse_nlp._.blob.polarity for verse_nlp in verse_sentiments_asv]
asv_df['average_sentiment'] = asv_df[["sentiment_asent", "sentiment_textblob"]].mean(axis=1)
# Doing the same steps but for the King James version

verse_sentiments_kjv = list(nlp.pipe(kjv_df.Text.astype(str)))
kjv_df["sentiment_asent"] = [verse_nlp._.polarity.compound for verse_nlp in verse_sentiments_kjv]
kjv_df['sentiment_textblob'] = [verse_nlp._.blob.polarity for verse_nlp in verse_sentiments_kjv]
kjv_df['average_sentiment'] = kjv_df[["sentiment_asent", "sentiment_textblob"]].mean(axis=1)
# same as above

verse_sentiments_web = list(nlp.pipe(web_df.Text.astype(str)))
web_df["sentiment_asent"] = [verse_nlp._.polarity.compound for verse_nlp in verse_sentiments_web]
web_df['sentiment_textblob'] = [verse_nlp._.blob.polarity for verse_nlp in verse_sentiments_web]
web_df['average_sentiment'] = web_df[["sentiment_asent", "sentiment_textblob"]].mean(axis=1)
#same steps taken for the Quran as the 3 different bible versions.

verse_sentiments_enq = list(nlp.pipe(enq_df.EnglishTranslation.astype(str)))
enq_df["sentiment_asent"] = [verse_nlp._.polarity.compound for verse_nlp in verse_sentiments_enq]
enq_df['sentiment_textblob'] = [verse_nlp._.blob.polarity for verse_nlp in verse_sentiments_enq]
enq_df['average_sentiment'] = enq_df[["sentiment_asent", "sentiment_textblob"]].mean(axis=1)
enq_df

# doing aggregation analysis on the dataframes after grouping the verses in a meaningful way. The bibles were grouped by book name 
# and the Quran was grouped by Juz

average_sentiment_asv_df = asv_df.groupby("Book Name", sort=False).mean()
average_sentiment_kjv_df = kjv_df.groupby("Book Name", sort=False).mean()
average_sentiment_web_df = web_df.groupby("Book Name", sort=False).mean()
average_sentiment_enq_df = enq_df.groupby("JuzNo", sort=False).mean()

#resetting indexes for plotting

av_sent_kjv_noindex = average_sentiment_kjv_df.reset_index()
av_sent_asv_noindex = average_sentiment_asv_df.reset_index()
av_sent_web_noindex = average_sentiment_web_df.reset_index()
av_sent_enq_noindex = average_sentiment_enq_df.reset_index()
kjv_sent_plot = av_sent_kjv_noindex.plot(x="Book Name", y=["average_sentiment","sentiment_asent","sentiment_textblob"],ylim=(-.5,.5), kind='bar', figsize=(30,15))

web_sent_plot = av_sent_web_noindex.plot(x="Book Name", y=["average_sentiment","sentiment_asent","sentiment_textblob"], kind='bar', ylim = (-.5,.5), figsize=(30,15))

asv_sent_plot = av_sent_asv_noindex.plot(x="Book Name", y=["average_sentiment","sentiment_asent","sentiment_textblob"], kind = 'bar', ylim = (-.5,.5), figsize=(30,15))

enq_sent_plot = av_sent_enq_noindex.plot(y=["average_sentiment","sentiment_asent","sentiment_textblob"],ylim=(-.5,.5), kind='bar', figsize=(30,15))

### Wordclouds
stopwords = set(STOPWORDS)
wordcloud = WordCloud(
                          background_color='white',
                          stopwords=stopwords,
                          max_words=300,
                          max_font_size=90, 
                          random_state=0
                         ).generate(str(asv_df['Text']))

fig = plt.figure()
plt.imshow(wordcloud)
plt.axis('off')
plt.show()
fig.savefig("asv_wordcloud.png", dpi=900)
stopwords = set(STOPWORDS)
wordcloud = WordCloud(
                          background_color='white',
                          stopwords=stopwords,
                          max_words=30,
                          max_font_size=100, 
                          random_state=0
                         ).generate(str(kjv_df['Text']))

fig = plt.figure()
plt.imshow(wordcloud)
plt.axis('off')
plt.show()
fig.savefig("kjv_wordcloud.png", dpi=900)
stopwords = set(STOPWORDS)
wordcloud = WordCloud(
                          background_color='white',
                          stopwords=stopwords,
                          max_words=30,
                          max_font_size=100, 
                          random_state=0
                         ).generate(str(web_df['Text']))

fig = plt.figure()
plt.imshow(wordcloud)
plt.axis('off')
plt.show()
fig.savefig("web_wordcloud.png", dpi=900)
stopwords = set(STOPWORDS)
wordcloud = WordCloud(
                          background_color='white',
                          stopwords=stopwords,
                          max_words=30,
                          max_font_size=100, 
                          random_state=1
                         ).generate(str(enq_df['EnglishTranslation']))

fig = plt.figure()
plt.imshow(wordcloud)
plt.axis('off')
plt.show()
fig.savefig("enq_wordcloud.png", dpi=900)
