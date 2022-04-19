# Amazon-Review-Analysis

In this project, we analyze online product reviews using network analysis and sentiment analysis.

### purpose
- We compare the differences between reviews recommended by several users and general reviews.
- We classify the attributes of the review through n-gram and then conduct an analysis that scores by attributes through sentiment analysis to provide more objective results than the ratings provided by Amazon.

<br>

Team-project of 'Unstructured Data Analysis' lecture in 2020-2




<br>

## Data 

### Amazon review data 
We used a review of the "Sunny Health & Fitness SF-B901 Pro Indoor Cycling Excellence Bike", a running machine product with a large amount of reviews and moderate ratings.


You can access at [here](https://www.amazon.com/Sunny-Spin-Bikes-Health-Fitness-Indoor-Cycling/product-reviews/B002CVU2HG/ref=cm_cr_arp_d_paging_btm_next_2?ie=UTF8&reviewerType=all_reviews)

<br>

## Data collection

We implement a crawler using ```selenium.webdriver``` and ```BeautifulSoup``` to collect data from Amazon websites.

**Crawling feature**
| Data    | Type |
| ------- | ---- |
| title   | str  |
| text    | str  |
| star    | int  |
| helpful | int  |

```crawler.ipynb``` contains crawling process.


<br>

## Data Pre-processing

Online product reviews require a preprocessing process because they are unrefined text data. We preprocessed textual data for use in network analysis and emotional analysis through the following process.

1. Remove unnecessary spaces and replace numbers of str type with int
2. Change to lowercase
3. Remove Stopword
4. Stemming
5. lemmatize
6. Extraction of noun,verb,adjectives and adverbs after pos-tagging
7. Rejoining Sentence Form

**Example**

![preprocessing](/image/preprocessing.PNG)


```Preprocessing.ipynb``` contains pre-processing process.


We also generate keyword matrices via TF-IDF for use in network analysis.

You can check this content through ```Network.ipynb``` and ```to_gephi.csv```

<br>

## Analysis

### Network Analysis

**All review keyword network**　　　　　　

![network_all](/image/network_all.PNG) 　 

<span style='background-color:#00D8FF'>Assembly</span> : assembly, manual

<span style='color:#1DDB16'>Mechanical</span> : usb, motnitor

<span style='color:#F361DC'>Durability</span> : solid, quiet

<span style='color:#FF5E00'>User Experience</span> : Motivation to use the product

<br>

We compare with reviews that have been recommended over a certain number.

Select only reviews that have been recommended at least twice.


<br>


**Helpful review keyword network**

![network_helpful](/image/network_heplful.PNG)

<span style='color:#00D8FF'>Assembly</span> : assembly, manual

<span style='color:#1DDB16'>Quality</span> : resistance, quality

<span style='color:#F361DC'>Durability & Practicality</span> : app, solid, control

<span style='color:#FF5E00'>User Experience</span> : defective, bad, uncomfortable (<span style= 'color : red'>negative experience</span>)

<span style='color:#664B00'>Screen</span> : screen, monitor

<br>

### Attribute classification by keyword

After combining the attributes provided by Amazon and the attributes we defined through network analysis, categories were divided through segmentation.

**Categories**
- Hardware
- Software
- Appearance
- Durability
- Assemble
- Noise level
- Shipment
- Service
- Price

After measuring the similarity of keywords through word2vec, noun keywords were classified according to categories to complete the attribute word dictionary.

<br>

**Attribute word dictionary**
| Attribute         | keyword                                                                                                                                                                                                                                                                            |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hardware    | seat, spin, pedal, chain, wheel, handlebar, bar, holder, flywheel, brake, tool, equipment, machine, frame, handle, spinner, knob, clip, hand, shape, size, cushion, metal, cage, plastic, silicone                                                                         |
| Software    | pad,  tension,  tv,  computer,  video,  monitor,  speed,  phone,  distance,  rate,  cardio,  music,  pound,  app,  electronics,  calorie, youtube,  sensor,  figure,  track,  ipad,  update,  headphone,  system,  tablet                                                  |
| Appearance  | smell,  simple,  design,  style,  movement,  convenience,  dust,  simplicity,  functionality,  dirty, cumbersome                                                                                                                                                             |
| Durability  | quality, sturdy, break, rock, damage, construction, guard, friction, condition, vibration, injury, intensity, secure, performance, fraction, strength, stability, lack, teflon,  risk,  fork,  error,  vibrates,  steal,  defect,  durability,  solid,  sturdiness,  fence |
| Assemble    | part,  instruction,  piece,  assembly,  bolt,  direction,  hole,  set,  wrench,  screw,  difficulty,  setup,  clipless,  installation,  breeze,  tie, effort,  install,  drill,  clamp,  attachment,  setting,  description,  weld                                         |
| Noise level | noise, sound, squeak, loud, noisy, volume, quieter, rattle, squeal, louder, quiet, silent, silence, clank, squeaky, clanking, clanky, banging, clang                                                                                                                          |
| Shipment    | shipping, delivery, package, packaging, manufacturer, stock, location, wait, ship, transport, factory, truck, address, shipment, styrofoam, arrival, receive, quick, pack, flight, arrives, transmission, delivers                                                         |
| Service     | complaint, service, replacement, fix, warranty, solution, membership, maintenance, repair, response, online, club, information, opinion, grade, assistance, shaft, swap, maintain, question, trust, complain, switch, confidence                                              |
| Price       | price, purchase, money, value, deal, buy, cost, pay, option, amount, buying, dollar, con, worth, choice, bargain, budget, spending, sale, bill, penny, purchasing, spent, refund, cheap, discount, cheaply, sell, exchange, expense, cash                                  |


<br>

### Sentiment Analysis

We use a ```VADER``` for sentiment analysis.

Two words before the keyword and six words after that are extracted and composed into sentences, and then emotional analysis of the sentence is conducted.

**Ex)** annoying clicking **sound** coming right whell spindle make unusable 
-> Noise level : Negative.

<br>

<!-- $$
Score = Positive / (Positive + Negative + neutrality)
$$ -->

Sentence sentiment are classified through ```VADER```, and scores are calculated for each attribute through the following fomula.

![score](/image/score.PNG)

<br>

For codes related to the analysis, refer to the ```all_sentiment.ipynb``` and ```helpful_sentiment.ipynb``` 


<br>

## Result

**All Reiview**
| attribute                 | positive | negative | neutrality | score |
| ----------                | ---- | ---- | ---------- | ----- |
| hardware                  | 4643 | 1945 | 2344       | 51.98 |
| software                  | 1534 | 586  | 1000       | 49.17 |
| appearnace                | 147  | 76   | 85         | 47.73 |
| durability                | 1379 | 387  | 313        | 66.33 |
| assemble                  | 1252 | 423  | 453        | 58.83 |
| noise  level              | 544  | 381  | 340        | 43.00 |
| shipment                  | 314  | 108  | 151        | 54.80 |
| service                   | 442  | 251  | 219        | 48.46 |
| price                     | 1847 | 366  | 465        | 68.97 |


<br>

**Helpful Review (recommended by many allows users)**
| attribute   | positive | negative | neutrality | score |
| ----------- | -------- | -------- | ---------- | ----- |
| hardware    | 1345     | 699      | 970        | 44.63 |
| software    | 583      | 277      | 414        | 45.76 |
| appearnace  | 49       | 25       | 33         | 45.79 |
| durability  | 389      | 193      | 115        | 55.81 |
| assemble    | 426      | 233      | 250        | 46.86 |
| noise level | 167      | 177      | 165        | 32.81 |
| shipment    | 122      | 55       | 83         | 46.92 |
| service     | 200      | 108      | 110        | 47.85 |
| price       | 604      | 157      | 182        | 64.05 |



<br>

<!-- ![chart](/image/chart.PNG) -->

**Radial Chart Visualization**

![chart2](/image/chart2.PNG)

The scores of reviews recommended more than once are clearly different from the overall reviews

<br>



## Conclusion
- Visualization by attribute allows the user to understand the product at a glance
- Analysis of reviews recommended by many allows users to gain more objective and useful information than the average rating of existing systems

<br>
<br>

---




## Configuration

```
python
selenium
BeautifulSoup
pandas
Vader
gephi
```