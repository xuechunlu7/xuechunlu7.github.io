## Natural Language Processing - Sentence Ordering 

**Task:** Given a set of unordered sentences, can you re-arrange them and make them a story?

An example:
Given a set of sentences like below... Can you predict the correct order of these sentences?

0. In 2016, Bailey ran for mayor of Portland, losing to Ted Wheeler.
1. Jules Bailey (born November, 1979) is a former Multnomah County Commissioner who now works at the Oregon Beverage Recycling Cooperative.
2. He served in the Oregon House of Representatives from 2009 to 2014, representing inner Southeast and Northeast Portland, and on the County Commission for Multnomah County, Oregon from June 2014 to December 2016.
3. He earned a bachelor's degree in  Lewis & Clark College and received MPA/URP from Princeton University\n\nBailey studied in a dual-degree graduate program at Princeton University's Woodrow Wilson School of Public and International Affairs.
4. In January 2017, he began working for the Oregon Beverage Recycling Cooperative as the chief stewardship officer.
5. Early life and education \nBailey was raised in Portland, Oregon and graduated from Lincoln High School.

**Answer:** [2, 0, 1, 5, 3, 4]

Did you get it right? Maybe yes, maybe no, but hmm, it's not so easy, right? 

Well, yea, but that's exactly why we want to build a model, so the work can be done by a computer! 

### 1. Brain Storming
When I was first introduced to the problem, I was thinking about if I can build my very own model like a very complicated LSTM and I began to work on the embeddings of the sentences using GLOVE embeddings. I spent a whole week implementing such a thing, but in the end, my model didn't learn anything becuase the predictions for 590,026 sets of sentences are all the same?! How come! Well, I guess it's because my model isn't deep enough.

Then what if I use some existed, pretrained, state of the art model... like BERT from Google? It's trained intensively on a much larger text data sets and it's very accurate!

However, BERT model, for example, BertForSequenceClassification model, can only take two sentences (embedded together) as inputs and predict which one comes first. But in our case, we have six sentences per set. How are we gonna feed them into BertForSequenceClassification??

### 2. Our Model!
Finally, I figured it out. 

For each set of six sentences, generate 6 chooses 2 (15) sentence pairs. Each pair is fed into a BertForSequenceClassification model and a binary label (1 if sentence 1 comes first, 0 otherwise) is produced in the end. For each set, we are gonna have a sequence of 15 binary labels. With this sequence, we can figure it out the correct order of these sentences by using a topological sort. 

Topological sort is a type of graph sort. We first create a graph with six vertices and then add directed edges between the edges according to the binary label. Once all the 15 labels are entered, the graph becomes a fully connected and directed graph. With this graph, we can sort out the order eventually. 

The example code of how to use [BERT embeddings for sentences]('notebook/Bert Data Embedding and Implementation of Topological Sort.ipynb'):

```
def bert_tokenization(dataset):
  # for each set
  input_ids = []
  labels = []
  attention_masks = []
  token_type_ids = []

  counter = 0
  for instance in dataset:
    if counter % 1000 == 0: print('current step is =: ', counter)
    counter = counter + 1 

    indexes = instance['indexes']
    inputs_set = []
    labels_set = []
    attention_mask_set = []
    token_type_id_set = []
    for i in range(0,6):
      for j in range(i+1,6):
        s1 = instance['sentences'][i]
        s2 = instance['sentences'][j]
        encoded_sentences = tokenizer.encode_plus(s1, s2, 
                          max_length = 100, 
                          truncation= True,
                          add_special_tokens = True, # Add '[CLS]' and '[SEP]'
                          return_attention_mask = True,
                          return_token_type_ids = True,
                          pad_to_max_length = True,
                          return_tensors = 'pt')
        input = encoded_sentences['input_ids']
        attention_mask = encoded_sentences['attention_mask']
        token_type_id = encoded_sentences['token_type_ids']

        inputs_set.append(input)
        attention_mask_set.append(attention_mask)
        token_type_id_set.append(token_type_id)
        # if s1 is before s2, label = 1, else label = 0
        # index1 = indexes.index(i)
        # index2 = indexes.index(j)
        index1 = indexes[i]
        index2 = indexes[j]
        if (index1 < index2):
          label = 1
        else: 
          label = 0
        labels_set.append(label)
    input_ids.append(inputs_set)
    labels.append(labels_set)
    attention_masks.append(attention_mask_set)
    token_type_ids.append(token_type_id_set)
  return input_ids, attention_masks, token_type_ids, labels
```

The example code of [topological sort]('notebook/bert_sequence.ipynb'):


```
# inputs: data should be n*15 array
def topSort(data):
  orders = []
  for instance in data:
    g = Graph(6)
    for j in range(0,15):
      pred = instance[j]
      pos_s1, pos_s2 = get_pos(j)

      if pred == 1: 
        g.addEdge(pos_s1, pos_s2)
      if pred == 0: 
        g.addEdge(pos_s2, pos_s1)
    while g.isCyclic():
      g.isCyclic()

    sorted = g.topologicalSort()
    arr = []
    for i in range(0,6):
      arr.append(sorted.index(i))
  
    orders.append(arr)
  return orders

```


In the kaggle leaderboard, I achieved an accuracy of 76%, which gives me a good ranking! We made it!