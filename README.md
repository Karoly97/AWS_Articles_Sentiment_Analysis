Understanding Corporate Narratives: Sentiment Analysis of News Articles Using AWS AI

source: https://www.diepresse.com/19039294/unicredit-commerzbank-wird-das-noch-was

In today’s fast-paced digital world, the public perception of businesses is often shaped by the media. Understanding how companies are portrayed in the news is crucial for stakeholders, investors, and analysts. One powerful approach to extract these insights is through sentiment analysis, where we gauge the tone of articles to identify if the sentiment is positive, negative, or neutral.
For this project, I focused on analyzing news articles about Unicredit and Commerzbank, two major banks involved in discussions about a potential merger. By leveraging AWS AI services like Comprehend and Translate, I transformed German-language articles into actionable insights, uncovering sentiment trends and key phrases.
This blog post outlines the entire process, from data extraction to analysis, and demonstrates how AWS tools make it seamless to process large volumes of multilingual text.

Use Case
The merger discussions between Unicredit and Commerzbank have garnered significant media attention. These articles often include mixed sentiments, reflecting diverse opinions from stakeholders, analysts, and government officials. My objective was to:
1.	Extract and analyze the sentiment of German-language articles.
2.	Identify recurring themes and key phrases.
3.	Compare sentiment across article sections, highlighting positive or negative tones.

Tools and Technologies
The project relied on the following tools:
•	Python: For automation and web scraping using libraries like requests and BeautifulSoup.
•	AWS Translate: To convert German-language articles into English.
•	AWS Comprehend: To perform sentiment analysis and key phrase extraction.
•	Matplotlib: For data visualization

Step-by-Step Approach

1. Data Collection
The first step involved collecting articles from the German news website Die Presse, focusing on topics related to the Unicredit-Commerzbank merger. Using Python’s requests and BeautifulSoup, I extracted the page title, content from <p> and <h1> tags, and combined the text for further analysis.

response = requests.get(url, headers=headers)
webpage = BeautifulSoup(response.content, "html.parser")
texts = [element.get_text().strip() for element in webpage.select("p, h1")]
text_to_translate = "\n".join(texts)

2. Language Translation
Since the articles were in German, translating them into English was essential for sentiment analysis. Using AWS Translate, the extracted content was automatically translated into English with high accuracy. The translated text preserved the nuances of the original content, ensuring that the subsequent sentiment analysis was meaningful.

translate = boto3.client("translate")
response = translate.translate_text(Text=text_to_translate, SourceLanguageCode="auto", TargetLanguageCode="en")
translated_text = response["TranslatedText"]

Translation Insight Example:
•	Original German: "Unicredit & Commerzbank: Wird das noch was?"
•	Translated English: "Unicredit & Commerzbank: Will that be anything else?"

3. Sentiment Analysis
The translated text was analyzed using AWS Comprehend to determine the overall sentiment of each article. AWS Comprehend provides:
•	Sentiment Classification: Positive, Negative, Neutral, or Mixed.
•	Confidence Scores: A breakdown of how confident the service is in each sentiment type.
The results highlighted the mixed opinions in the articles. Positive sentiment was associated with merger benefits, while negative sentiment often stemmed from stakeholder criticisms or regulatory challenges.

comprehend = boto3.client("comprehend")
response = comprehend.detect_sentiment(Text=translated_text, LanguageCode="en")
print(response['Sentiment'], response['SentimentScore'])

Sentiment Results Example:
•	Overall Sentiment: Neutral
•	Confidence Scores: Positive (0.03%), Negative (5.16%), Neutral (94.80%), Mixed (0.01%)

4. Key Phrase Detection
To understand recurring themes, I used AWS Comprehend’s key phrase detection. This feature identifies phrases that are significant in the text, along with a confidence score for each. Key phrases such as "stakeholder," "merger value," and "Unicredit & Commerzbank" appeared frequently, shedding light on the focus areas of the articles.

response = comprehend.detect_key_phrases(Text=translated_text, LanguageCode="en")
for phrase in response["KeyPhrases"]:
    print(f"{phrase['Text']}: {phrase['Score']:.2%}")

Key Phrases Example:
•	Unicredit & Commerzbank: 98.62% confidence
•	das: 99.96% confidence
•	noch: 85.47% confidence

Results and Visualizations

Sentiment Distribution

A bar chart was created to visualize the sentiment distribution across the articles. This showed that negative sentiment dominated discussions, particularly when highlighting challenges in the merger process.
 
Key Phrase Analysis
A word cloud or bar chart showcasing frequently occurring key phrases was used to visualize recurring themes. This highlighted the focus areas of the discussion.

Challenges Faced
Context Preservation in Translation
While AWS Translate performed well, domain-specific financial terms sometimes required additional context to ensure accurate sentiment interpretation.
 
Applications and Implications

The insights derived from this project have far-reaching implications:
1.	Investor Analysis: Investors can gauge market sentiment around major corporate events.
2.	PR and Crisis Management: Companies can monitor negative sentiment spikes and respond proactively.
3.	Cross-Language Insights: Multilingual sentiment analysis broadens the scope of global media monitoring.
 


Conclusion

This project showcased the power of AWS AI services in analyzing unstructured text data. By combining AWS Translate and Comprehend, I efficiently extracted insights from German news articles, identified sentiment trends, and highlighted key phrases.
The results underline the potential of these tools in transforming raw text into actionable intelligence, making them invaluable for businesses, analysts, and researchers alike.
 
Future Directions
•	Extend the analysis to articles from multiple news outlets for broader comparisons.
•	Incorporate AWS Rekognition to analyze visual content alongside text.
•	Use AWS Bedrock to experiment with fine-tuned language models for financial datasets.

Summary of Sentiment Analysis Project on Unicredit and Commerzbank
This project used AWS AI services to analyze sentiment in German news articles about the Unicredit-Commerzbank merger. AWS Translate enabled accurate translation, preserving content nuances, while AWS Comprehend identified sentiment (Positive, Negative, Neutral, or Mixed) and extracted key phrases like "stakeholder" and "merger value."

Key findings revealed predominantly neutral sentiment with significant negative undertones, reflecting stakeholder criticism and regulatory hurdles. However, AWS Comprehend struggled to capture the nuanced blend of skepticism and cautious optimism present in the articles, as well as the underlying tension in strategic visions between the two banks. While the tools effectively highlighted major themes and general sentiment trends, the subtleties of complex narratives require further refinement.
This project demonstrated how AWS simplifies large-scale text analysis, offering actionable insights into corporate narratives while highlighting areas for improvement in detecting layered sentiments.

Estimated cost:  AWS Translate($0.10) + AWS Comprehend($0.10) x 5 ~$1
