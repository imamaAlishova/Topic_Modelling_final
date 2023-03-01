 
## “What are the most discussed themes in articles on Special Needs Education?”

My research idea was to select fifty articles from the category of “most read articles” from "European Journal 
of Special Needs Education, Volume 37, Issue 6 (2022)",(https://www.tandfonline.com/action/showOpenAccess?journalCode=rejs20)
and make topic modeling with LDA. The research question is “What are the most discussed themes/topics in articles on 
Special Needs Education?”. I was particularly interested in the topics of the parental involvement in the teaching of 
special needs children and to find out the most prominent issues in special needs education across European countries. 
I ended up having only fifteen articles as a dataset due to the open access restriction.Therefore, I changed the category 
from “most read” to “open access” and was able to download fifty-two articles to use as my dataset.
My second step is to compare one review paper on Special Needs Education to the LDA results.(https://files.eric.ed.gov/fulltext/EJ1126490.pdf)

*

#  -------------------------------------------------------

### Data Preprocessing  
#### Data Loading
1. Data was loaded to the Jupyter notebook and all texts from 52 pdfs were extracted and saved under a single list.

2. In the next step, I convert this list to the data frame with 'paper text' as a column of the dataframe.This will be
 used for further preprocessing. 
#### Data cleaning:
1. Cleaning the data involves removing punctuations and converting all text to lowercase.Here I remove all the 
characters except the alphabet letters and the lowercase all the text using `re(regular expression library)` from Python.
2. The processed data is added to the data frame under a separate column, with a name “paper_ text_processed”.
#### Preparing data for LDA analysis
1. Removing the stopwords: This is the most important step of the whole pipeline as we prepare the final data for 
LDA training. First, NLTK(Natural Language Toolkit) “stopwords” package is used to remove the meaningless words that
the search engine is programmed to ignore. We would not want these words to take up space in our database
or take longer processing time.
2. Additional to the default list that the package contains, later the list was further
extended with most common shared terms, as well as the words has no meaning,for instance: 'use','doi','sen','et','al' etc.
throughout the 20 topics based on LDA results.Otherwise, this would lead to the problem of not being able to capture 
all possible topics within the text and resulting the same word list for each topic.
3. “simple_preprocess” package is used for tokenization of text, where the text is split into sentences and later to words.
4.  Tokenized object is converted into a corpus and dictionary metrics. 


#  -------------------------------------------------------
### LDA Training 
1. We should decide the number of topics for analysis of the LDA model training result. 
`corpus`, `id2words`(a dictionary that maps the word “id” to a token ),
`num_topic`(number of topics) are passed as arguments to the LDA model training in a first trial.
#### Improving the LDA performance
After several trials and analysis of the results two more parameters are chosen to improve the result. 
1. Parameter `alpha='asymmetric'` is used to capture the different characteristics of the documents in the corpus.
2. Parameter `passes` is used to define the number of iteration.The number of passes refers to the number of times 
the algorithm goes through the entire dataset. More passes over the data allow the algorithm to converge on better 
topic mixtures, but also increases the computational cost and runtime.
3. `id2word.filter_extremes` method is used to filter out the words, as it removes rare and overly common words that are 
unlikely to be useful for identifying topics in the text. 

#### Visualisation of LDA results
For visualisation purposes `pyLDAvis` which is a package is used for visualizing and interpreting the topics.
It plots the `Intertopic Distance Map` and visualizes the most 30 relevant words for chosen number of topics.
The circles represent each topic. The distance between the circles visualizes topic relatedness. 
These are mapped through dimensionality reduction (PCA/t-sne) on distances between each topic’s probability distributions into 2D space.
This shows whether our model developed distinct topics.


#  -------------------------------------------------------


### Results and Analysis
1. Running the LDA model with default stopwords in English language from `stopwords`package:
As a result we get the exact same words dominating for all 20 topics and also observing some words that have no meaning
for the result interpretation.For example: 'al','et','doi','sen' etc. 
Model is not properly identifying distinct topics within text data.In order to tackle this problem, we need to further 
**preprocess the data** or **adjusting the Hyperparameters** of the model.
2. Running the model  with parameter  `alpha` and extended `stopwords` list:
Parameter `alpha='asymmetric'` is used to capture the different characteristics of the documents in the corpus,
allowing for more flexibility in modeling the data.It allows for different levels of sparsity for different topics,
depending on the specific characteristics of the data.
Using extended list of `stopwords` to remove the meaningless words that observed as a result of previous trial.
As a result we get most of the circles representing the topics are overlapping due to the high number of common words
for all topics, which means that `alpha='asymmetric'` parameter may not improve the result. On the other hand,meaningless 
words were removed from the text as a result of extended stopwords list.
After looking into top 10 word of each topic, following words were found to be appearing in all the topics:
[education, special, children, students, teachers, school]
It means that if the same terms are appearing in multiple topics, it can be difficult to understand the unique context
and meaning of each topic. Having these results in hand we cannot contextualize each of the topics in a distinct manner. 
3. Running the model with including the commonly shared words among the topics to extended `stopwords` list:
Additionally, I added a parameter `passes` to train the LDA for more iteration . 
I plotted the word similarity among 20 topics based on 10 top words for each topic. Most of the words from all topics
are still common by 30%-50%.Manually adding all the stopwords to the list would be a tedious task.Therefore, in the next
step I am using a function/method to automatically define a threshold to remove the most common and most rare words from 
the text.
4. Running a model with a method from the `gensim` library for filtering out words: As mentioned in **Improving LDA model
performance** section above,this method is typically used before training a topic model, as it removes rare and overly 
common words that are unlikely to be useful for identifying topics in the text. By keeping only the most common words 
that appear in a sufficient number of documents, the method aims to improve the performance of the topic model by 
focusing on the most meaningful words in the text.
As a result of using this method, unique number of words increased to 157 among all the topics,where 2 words seems 
to be appearing in only 4 topics, 7 words are observed in 3 topics ,23 words appears in 2 topics and the rest of the words
are almost unique for each different topics in dataset.Moreover, a decline in word similarity to 7% was also noticed 
among all topics in word similarity plot.

#  -------------------------------------------------------

##  Interpretation of the Results


### Topic 1 :Parental involvement/governement role
The words: 'parenta','involvement','unesco', 'accessed', 'art', 'equal', 'provision', 'acceptance', 'government', 'right'
The topic 1 appears to be focused on parental involvement and equal access to education for children with special needs. Words such as 'parental involvement', 'equal', 'provision', 'acceptance' suggest that the topic is concerned with the role of parents and the provision of equal educational opportunities for children with special needs. The word 'government' suggests that the topic may also address the responsibilities of governments in ensuring equal access to education. 

### Topic 2 :Decision making in special needs education curriculum?
The words:'decision', 'test', 'mathematics', 'belonging', 'decisions', 'og', 'emotions', 'subjects', 'och', 'taking'
Topic 2 seems to be related to decision making in the context of special needs education. The words "decision" and "decisions" suggest that the topic is centered around the process of making choices. The word "mathematics" and "subjects" may imply that the decisions being discussed are related to course selection or curriculum. The words "belonging" and "emotions" may be realted to the psychological and emotional aspects of decision making, perhaps the impact of decisions on a student's sense of belonging and emotional well-being. The word "test" may be refered to the assessment factor in the decision making process.

### Topic 3 :Finnish young peoples' mental health and how it is supported
The words:'finnish', 'finland', 'youth', 'adolescents', 'mental', 'local', 'involvement', 'association', 'bj', 'supports'
These words suggest that the topic being discussed is related to Finnish youth and adolescents, in the finnish context, and their experiences with mental health. This may include their experiences with local organizations and associations that provide support for mental health issues. The use of the words "involvement" and "supports" suggests that there may be a focus on the importance of community and social support for youth and adolescents with mental health challenges.


### Topic 4 : Friendship in the german context of special needs education in the preeschool years.
The words: 'attitudes', 'friendship', 'friendships', 'preschool', 'german', 'friends', 'germany', 'values', 'phase', 'cultural'
The topic may focus on the attitudes and beliefs towards friendship among preschoolers, with a particular focus on Germany and the German social cultural context because of the words as "friendship," "friends," "preschool," and "german" . The relationships, focus on the beliefs and cultural norms about friendship  in early childhood education within the german context is the center of topic. 

### Topic 5 : Effectiveness of special needs education in the home environment for middle school children
The words: 'efficacy', 'items', 'home', 'middle', 'scores', 'achieving', 'feedback', 'achievement', 'beliefs', 'prior'
This topic seems to be related to the efficacy and effectiveness of special education provision in the home environment for middle school-aged children. The words "home," "middle," "scores," "achieving," "feedback," and "achievement"  may suggest that the focus of the topic is on educational outcomes and the impact of special education on students' abilities.

### Topic 6 : Reading abilities/learning difficulties of Swedish special needs children
The words: 'reading', 'swedish', 'grade', 'van', 'average', 'test', 'grades', 'profound', 'tests', 'direct'
The detected words may suggest of interpretation of reading skills of students in the Swedish education system. Topic is most probably about the assessment of reading abilities in this context. The presence of "profound" and "direct" may indicate that the focus of the topic is on students with profound learning difficulties or support measures aimed at improving their reading skills.


### Topic 7 : ADHD with gender differences
The words:'adhd', 'girls', 'boys', 'gender', 'disorder', 'hyperactivity', 'deficit', 'symptoms', 'likely', 'diagnosis'
Topic 7 seems to be focused on "Attention-Deficit/Hyperactivity Disorder (ADHD)" with a particular emphasis on the gender aspect of the disorder. The words suggest that the topic might be exploring the differences in symptoms and diagnosis between boys and girls with ADHD. The topic might also discuss the hyperactivity and deficit symptoms associated with ADHD.

### Topic 8 : The transition of preschool children with special needs to another educational settings/program in Swedish and north countries conetxt
The words: 'preschool', 'members', 'unit', 'transitions', 'transition', 'swedish', 'north', 'pattern', 'agency', 'confidence'
The words in Topic 8  may be related to the transition of preschool-aged children with special needs to another educational setting or program. The words "unit", "agency", and "confidence" could be interpreted as a focus on the role of the educational institution and its support of the child during this transition. We can assume from the word "swedish" and "north" that the focus is on the Swedish special needs education context or overall the Swedish context within the North countries.

### Topic 9 : Attitudes and cooperation of special needs student during their transition to job
The wrods: 'attitudes', 'transition', 'cooperation', 'assessed', 'youth', 'job', 'showing', 'explained', 'attitude', 'significantly'
The topic 9 seems to be focused on the attitudes and cooperation of special needs students during their transition from school to job because of the words “job”, “transition”, “cooperation” and e.t.c.The focus could be on  assessing the attitudes of these students and the factors that contribute to their attitudes, such as cooperation with others. 


### Topic 10 :Autism/anxiety disorder and how education system can support this children
The wrods: 'autistic', 'interests', 'spectrum', 'reviews', 'anxiety', 'disorder', 'achievement', 'efficacy', 'provision', 'range'
The topic may be related to autism and the education of individuals on the autism spectrum. Words such as "autistic," "spectrum," and "anxiety disorder" all suggest that the topic is related to autism. Additionally, words like "interests," "provision," and "achievement" may be refering to how the education system can support and accommodate the needs of individuals on the autism spectrum.


### Topic 11 : Interpersonal relatioinship and trust between students and educators
The words: 'relational', 'educators', 'post', 'test', 'interpersonal', 'video', 'informants', 'trust', 'theme', 'quotes'
The topic may be related to the interpersonal relationships and trust between educators and students with special needs. The topic is presumably discussing the importance of positive relationships and trust between educators and students with special needs(trust, interpersonal, relational, educators). The words "post",and "test" could be associated with the impact of these relationships and trust on student outcomes and progress. The words "educators", "video" and "informants" could refer the implementation of video lessons, their impact on students' outcome, what informatio do they give to educatiors and e.t.c.

### Topic 12: Instruction of Mathematics in the home context.
 The words:'circle', 'members', 'instruction', 'families', 'home', 'mathematics', 'family', 'space', 'week', 'per'
The topic may be about the instruction and support provided to families and children with special needs in the context of mathematics education.The words "home","family" and "space" may suggest that the topic is discussing the role of families and the home environment in providing instruction and support to children with special needs in mathematics education.

### Topic 13 : Sweden and Finland's approach to special needs education in the preschool context
The words:'strengths', 'efficacy', 'lower', 'sources', 'swedish', 'preschool', 'finnish', 'validity', 'observations', 'states'
The topic is pesumably related to the strengths, efficacy and validity of special needs education in preschool settings in the context of Sweden and Finland(efficacy, strentgh, validity). The words "sources", "Swedish", "Finnish" may refer to the specific approach and strategies used by educators in Sweden and Finland when working with children with special needs in preschool, as well as the words as "preschool", "observations" and "states" could be intepreted as such that this topic may be discussing the characteristics and observations of special needs education in preschool settings in Sweden and Finland.

### Topic 14 :
The words: 'educators', 'sweden', 'swedish', 'themes', 'expected', 'reports', 'away', 'ransson', 'consultation', 'tools'
The topic may be related to the role of educators in special needs education in the context of Sweden, also may be related to some themes or reports regarding special needs education. The words "educators", "Sweden", "Swedish" suggests that this topic is discussing the specific approach and strategies used by educators in Sweden when working with children with special needs. From the the words "consultation" and "tools" we may assume that this topic may be discussing the methods and strategies that educators use to consult with parents and other experts to provide effective support for children with special needs in Sweden, or some tools that could be of help with this children. 

### Topic 15 : Inormed decision making in specail needs education
Topic 15: 'choice', 'collaborative', 'provision', 'parental', 'search', 'real', 'placement', 'category', 'informed', 'decision'
These are more general words that can be found also in other topics and is hard to drive any conlusion from, however, we can assume that it is mostly about the concept of choice, collaborative provision, and informed decision-making in special needs education.The word "category" may suggest that the topic may be discussing the classification of children with special needs and the provision of services that are appropriate for the specific needs of each child. The words of "informed" and "decision" may be about the involvement of children and parents in the decision-making process and providing them with the information they need to make informed choices about their education. This topic may also be discussing the role of collaboration and partnership between parents, educators. 

### Topic 16 : The agency and the role of parents/expert/team in provision of special needs education at home
The words: 'agency', 'reviews', 'home', 'expert', 'parental', 'involvement', 'team', 'shared', 'documents', 'provision'
Topic may be related to the agency and the role of parents, experts and team in provision of special needs education at home. The words "agency", "reviews", "home", "expert", "parental", "involvement", "team", "shared" may discuss the role of different stakeholders, such as educational agencies, experts, and parents, in providing special needs education in a home setting. The words "documents" and "provision" may be about the use of different documents and resources, such as individualized education plans, in order to provide appropriate support and accommodations for children with special needs in a home setting.

### Topic 17 : Resarch on youth with impairmenet/hyperactivity
The words: 'reports', 'impairment', 'prior', 'score', 'conduct', 'longitudinal', 'hyperactivity', 'youth', 'scores', 'brown'
The topic may be related to the research and reports on special needs education, specifically focusing on youth with impairment, such as hyperactivity. The words "hyperactivity", "youth" and "scores" suggest that this topic is about the educational outcomes and performance of young people with hyperactivity disorder. The use of the word "longitudinal" may refer to the research studies which have been conducted over a longer period of time. 

### Topic 18:German context of special needs education with visual impairmenet
The words:'visual', 'barriers', 'impairment', 'germany', 'cooperation', 'responsibility', 'team', 'german', 'always', 'assignments
may be related to the German context of special education and particularly on visual impairment. The presence of the words "german", "germany" shows a connection to German culture and society, while the words "visual", "barriers", "impairment" point to a focus on the challenges and difficulties that students with visual impairments may face in the German education system. Some of the words as "cooperation", "responsibility", "team" may focus on the role of collaboration and teamwork in addressing these challenges and providing support to students with visual impairments. The words "always" and "assignments" could be intepreted as the continuous support, such as special assignments or materials, that may be necessary to ensure that students with visual impairments can fully participate in the German education system.


### Topic 19 : Instructional assistants/technology use in special needs education
The words: 'assistants', 'instructional', 'pupil', 'von', 'employed', 'coding', 'models', 'roles', 'video', 'mentioned' are mostly about the instructional assistants and technology, such as coding and video, in the context of special needs education.The use of different tools and techniques to support and enhance the education of students with special needs is on the center of the topic.
It may also discuss the roles and responsibilities of instructional assistants and how they are employed in the education of students with special needs, as well as the use of technology and its effectiveness in this context.

### Topic 20 :Norway context/a sense of belonging in special needs education
The words: 'idea', 'belonging', 'organisation', 'norway', 'outside', 'placement', 'goal', 'organisational', 'seemed', 'human'
appears to be focused on the concepts of belonging and organizational placement in the context of special needs education, specifically with reference to Norway. The words such as "idea", "belonging", "organisation", "norway", "outside", "placement", "goal", and "organisational" may suggest that the focus is on how individuals with special needs can feel a sense of belonging and feel included in the educational system.


### Short analysis 

Looking back at my research question and what the LDA model has generated, I can assume that with the help of some additional attempts, it was possible to find out the topics/themes that are discussed in the selected articles, although in some topics, some words seem to be reappearing and made it hard to extract meaning. However, in almost every topic, there are specific words shaping the topic's central context in this analysis. On the other hand, I am not sure how it could have been possible with a large dataset to extract meaning or name each topic accordingly, with many repetitive words.
As a summary, the first part of my research question (What are the most discussed themes/topics) is mostly answered. From the numbered topic words above, we can see the most discussed themes in the open-access articles, ranging from autism spectrum to hyperactivity, and parent-teacher relationship to home support, technology assistant teaching to the transformation of special needs children from school to job environment. In addition, the topics that I was particularly interested in (parental involvement in special needs education)seem to be appearing as well in many of the topics. Another part of my research question (the most prominent special needs education issues in European countries), has been answered partly, as the focus was more on northern countries, Finland, Sweden, Norway, and Germany. Therefore, it is hard to generalize the topics for all European countries, as the context may fail to be very relevant for every country. It could be related to the infrastructural, and bureaucratic reasons in other European countries. Unfortunately, special needs education has yet to receive the required attention in every educational system, because of the shortage of experts in the fields, lack of governmental support, and financial issues. The countries that have been giving a great deal of attention to special needs education recently, are obviously focusing on more research and studies in this field based on their experience. 


Comparing the results with review paper on Special Needs Education. (https://files.eric.ed.gov/fulltext/EJ1126490.pdf)

The paper is called  ”Review of the Literature on Children with Special Educational Needs”. 
This paper discusses the literature related to present research on special needs education. The review divided into four sections, and covers the most important issues in special needs education. 
The first section refers to cross-cultural factors, as its broader spectrum, it covers a lot of problems. 
The second part gives information on legislation about special needs education around the world, major changes and development since the establishment of Special Needs Education legislation. 
The third part led light how the main terminology seem to refer to the same issues even though they are formed differently in USA, Canada, and Europe, what trends and changes have been observed in special needs education concepts and terms e.g., “mental handicapped”, “mental deficiency”, “intellectual disability” and so on. 
When comparing this review paper to the LDA result of 52 articles, we may encounter with some identical patterns within cross-cultural discipline and usage of various terminology, although the LDA results mainly describe the situation in Special needs education in Europe, whereas the review paper gives more overall picture since the establishment of Special needs education legislation/concepts around the world. 
The parental involvement that has been detected a lot in LDA result, has not been noticed in this revies paper as a main subject of topic. Instead, different models of disability (disability mode, medical model, social model. )have been categorized accordingly, which can easily be interpreted and applied to the situation of children in Special needs education. 
In the section of Pedagogy of Special needs education, the key factors have been touched upon in the review paper which most of them could be observed within the  LDA result as well. 
The individual education plan is the most discussed topic according to both LDA result, and the review paper. It has been one of the most effective learning strategies based on individual learning which a teacher can employ in the planning of educational procedures to meet the difficulties faced by students with intellectual disabilities (Hawsawi, 2002).  







 
