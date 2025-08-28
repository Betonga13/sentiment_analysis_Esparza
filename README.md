# sentiment_analysis_Esparza
Sentiment Analysis Steam Reviews Kafka im digitalen Spiel

import pandas as pd
import numpy as np
import nltk

nltk.download('vader_lexicon')
from nltk.sentiment.vader import SentimentIntensityAnalyzer as SIA

df = pd.read_csv('C:/Users/José/OneDrive/Documentos/sentiment_analysis/steam_reviews_kafka.csv')
sid = SIA()
msg=df.at[1, 'review']

print(msg)

scores = sid.polarity_scores(msg)

# Here we loop through the keys contained in scores (pos, neu, neg, and compound scores) and print the key-value pairs on the screen

#for key in sorted(scores):
        #print('{0}: {1}, '.format(key, scores[key]), end='')

#print(df.head())
total = 0
for r in df['review']:
    calculate = sid.polarity_scores(r)
    print(calculate)
    com = calculate['compound']
    total = total+com
    print(total)

total = total/df.shape[0]
print(total)

no_rew = 0 
total = 0 
for i in range(0, df.shape[0]):
    if df.at[i, 'review_language'] == "english":
        #code
        r = df.at[i, 'review']
        calculate = sid.polarity_scores(r)
        com = calculate['compound']
        total = total+com
        print(total)
        no_rew += 1
    else:
        pass

total = total/no_rew
print(total)

scores = []   # Liste für alle Compound-Scores
no_rew = 0    # Zähler für englische Reviews

for i in range(df.shape[0]):
    if df.at[i, 'review_language'] == "english":
        r = df.at[i, 'review']
        calculate = sid.polarity_scores(r)
        com = calculate['compound']
        scores.append(com)   # speichere den Score in der Liste
        no_rew += 1
    else:
        pass

# Durchschnitt berechnen
average = sum(scores) / len(scores)

print("Anzahl englischer Reviews:", no_rew)
print("Durchschnittlicher Compound-Score:", average)

positive = len([s for s in scores if s > 0.05])
negative = len([s for s in scores if s < -0.05])
neutral  = len([s for s in scores if -0.05 <= s <= 0.05])

print("Anzahl positiver Reviews:", positive)
print("Anzahl negativer Reviews:", negative)
print("Anzahl neutraler Reviews:", neutral)

# Prozente berechnen
total_reviews = len(scores)
print("\nProzentuale Verteilung:")
print("Positiv: ", round(positive / total_reviews * 100, 2), "%")
print("Negativ: ", round(negative / total_reviews * 100, 2), "%")
print("Neutral: ", round(neutral  / total_reviews * 100, 2), "%")
