![image](https://user-images.githubusercontent.com/26519123/120105902-1a28d380-c15b-11eb-8221-ec55f238bc3b.png)

# 1. SENTIMENTS

Zgodnie z [[1]](https://mfiles.pl/pl/index.php/Analiza_sentymentu):

> Analiza sentymentu – metoda analizy tekstu. Jej zadaniem jest wyszukać i zaklasyfikować w wypowiedzi słowa naznaczone emocjonalnie: zarówno takie, które świadczą o stanie emocjonalnym autora jak i te, które mogą wskazywać na efekt emocjonalny, jaki wypowiedź uzyska u odbiorcy (K.Tomanek, 2014, str. 120).

> Może być wykorzystywana do analizy ludzkich opinii, odczuć czy postaw na przykład wobec produktów, usług, organizacji, wydarzeń czy poszczególnych osób (Bing Liu, 2012, str. 7), co daje możliwość wykorzystywania jej w wielu obszarach, takich jak nauki społeczne, ekonomiczne, a także biznes (T.Turek, 2017, str. 286).

## vaderSentiment

```py
#!pip3 install vaderSentiment
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer

analyzer = SentimentIntensityAnalyzer()
r1 = analyzer.polarity_scores("I don't like the movie")
r2 = analyzer.polarity_scores("I hate the movie")
r3 = analyzer.polarity_scores("I like the movie")
r4 = analyzer.polarity_scores("I love the movie")
print(f's1 = {r1} \ns2 = {r2}')
print(f's3 = {r3} \ns4 = {r4}')
```
```
s1 = {'neg': 0.345, 'neu': 0.655, 'pos': 0.0, 'compound': -0.2755} 
s2 = {'neg': 0.552, 'neu': 0.448, 'pos': 0.0, 'compound': -0.5719}
s3 = {'neg': 0.0, 'neu': 0.545, 'pos': 0.455, 'compound': 0.3612} 
s4 = {'neg': 0.0, 'neu': 0.417, 'pos': 0.583, 'compound': 0.6369}
```

- metryka **neg** prawdopodobieństwo przynależności do klasy **negatywnej**
- metryka **neu** prawdopodobieństwo przynależności do klasy **neutralnej**
- metryka **pos** prawdopodobieństwo przynależności do klasy **pozytywnej**

### Definicja **compound**
```py
if sentiment_dict['compound'] >= 0.05 :
    print("Positive")

elif sentiment_dict['compound'] <= - 0.05 :
    print("Negative")

else :
    print("Neutral")
```

## TextBlob

```py
from textblob import TextBlob
test = TextBlob("I hate this movie")
print(test.sentiment)
```

```
Sentiment(polarity=-0.8, subjectivity=0.9)
```

Proszę przejrzeć dokumentację dla [TextBlob - Sentiment Analysis](https://textblob.readthedocs.io/en/dev/quickstart.html#sentiment-analysis)


### Wykorzystanie [[2]](geeksforgeeks.org/python-sentiment-analysis-using-vader/)

**Biznes**: firmy zajmujące się marketingiem używają go do rozwijania swoich strategii, zrozumienia uczuć klientów wobec produktów lub marki, tego, jak ludzie reagują na ich kampanie lub premiery produktów i dlaczego konsumenci nie kupują niektórych produktów.

**Polityka**: w dziedzinie polityki służy do śledzenia poglądów politycznych, wykrywania spójności i niespójności między oświadczeniami a działaniami na szczeblu rządowym. Może być również używany do przewidywania wyników wyborów! .

**Działania publiczne**: Analiza nastrojów służy również do monitorowania i analizowania zjawisk społecznych, wykrywania potencjalnie niebezpiecznych sytuacji i określania ogólnego nastroju blogosfery.



### Links
[NLTK sentiments](https://www.nltk.org/howto/sentiment.html)
[TextBlob](https://textblob.readthedocs.io/en/dev/)



# 2. spacy similarity for INTENTS

Obliczanie wyników podobieństwa może być pomocne w wielu sytuacjach, ale ważne jest również, aby zachować realistyczne oczekiwania co do tego, jakie informacje mogą dostarczyć. Słowa mogą być ze sobą powiązane na wiele sposobów, więc pojedynczy wynik "podobieństwa" zawsze będzie mieszanką różnych sygnałów, a wektory trenowane na różnych danych mogą dawać bardzo różne wyniki, które mogą nie być przydatne w realizowanym projekcie.
Dlatego przy wykrywaniu intencji należy przyjąć określone ograniczenia.

```py
import spacy

nlp = spacy.load("en_core_web_md")  # make sure to use larger package!
doc1 = nlp("I like salty fries and hamburgers.")
doc2 = nlp("Fast food tastes very good.")

# Similarity of two documents
print(doc1, "<->", doc2, doc1.similarity(doc2))
# Similarity of tokens and spans
french_fries = doc1[2:4]
burgers = doc1[5]
print(french_fries, "<->", burgers, french_fries.similarity(burgers))
```
```
###  I like salty fries and hamburgers. <-> Fast food tastes very good. 0.24146281347644855
###  salty fries <-> hamburgers 0.45291108
```

### [Example](https://betterprogramming.pub/the-beginners-guide-to-similarity-matching-using-spacy-782fc2922f7c) 

```py
def calculate_similarity(text1, text2):
    base = nlp(process_text(text1))
    compare = nlp(process_text(text2))
    return base.similarity(compare)
```

# 3. NER

Na stronie spacy.io jest wygodne rozwiązanie dla rozszerzania modelu encji [ExcelCy](https://spacy.io/universe/project/excelcy) 
```py
from excelcy import ExcelCy
# collect sentences, annotate Entities and train NER using spaCy
excelcy = ExcelCy.execute(file_path='https://github.com/kororo/excelcy/raw/master/tests/data/test_data_01.xlsx')
# use the nlp object as per spaCy API
doc = excelcy.nlp('Google rebrands its business apps')
# or save it for faster bootstrap for application
excelcy.nlp.to_disk('/model')
```
Plus obszerne wyjaśnienie oraz źródła projektu  [github excelcy](https://github.com/kororo/excelcy)


# Literatura
[1] https://mfiles.pl/pl/index.php/Analiza_sentymentu

[2] https://geeksforgeeks.org/python-sentiment-analysis-using-vader
 

