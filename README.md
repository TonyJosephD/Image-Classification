# Predicting Animal Crossing and Doom

## Introduction

### As social media continues to grow as leading platform for communication, the style of communication is rapidly evolving. Memes are a popular form of communication on social media and often involve the combinationn of both text and images to encapsulate an idea or aspect in relation to a topic. Taken out of context, the text and images by themselves may never be enough to refer to the original idea. However, there may be trends present with the text and images paired together that refer to the original idea. Our goal is to try and predict one of two categories using a combination of trends for both image and text attributes associated with memes. We want to know:
- Can we train a model to classify a meme as Animal Crossing or Doom?
- Will the combination of Natural Language Processing and Image Recognition work together to understand topics out of context?


## Data

### We utilize a Kaggle dataset found [here](https://www.kaggle.com/datasets/andrewmvd/doom-crossing), which is a collection of Reddit posts containing images that relate to either Animal Crossing or Doom. Both are video games, but Animal Crossing is Fun and Doom is ridiculous. 
### Looking at how our data is broken down, we can summarize the data into 3 basic categories, outside of what subreddit its from: there is a title for each post, a list of user engagement metrics, and an attached url and filename for the actual image posted to Reddit.
# As such, as we began approaching how to handle our problem, we saw the opportunity to try and process *all* this data, as opposed to just making predictions off of the engagement metrics.

## Methods

### In order to effectively process all our data, we had to do a bit of preprocessing across all our variables to get them in a suitable format. The user engagement metrics were already in usable form, given they were just numbers representing the total number of things like comments, upvotes, downvotes, or a ratio representing the upvote to downvote ratio. If we wanted a single model that processed all our information at once, however, we realized that we would have to find some sort of way to process our other data formats.

### For our text, we decided to try and use a Tfidf text vectorizer. While this sounds scary, the actual idea behind what it does is relatively simple to graph. Tfidf essentially takes all the words inside of the sample "documents," which in this case were the titles of each reddit post, and assign them a score based on how often they were used in what number of documents. This, in essence, is attempting to extract the most relevant words in the text, words that appear often. We split our data between the two games' subreddits, and ran Tfidf to extract the most relevant words for each game. We got the follows: <img src="https://cdn.discordapp.com/attachments/1044291148560224339/1047592757037895730/image.png" alt="drawing"/>

### Looking at this, there are clear differences in the types of words referenced in both games. For Animal Crossing, we have words like "island" and "villagers," as well as character names like "nook" and "blathers." For Doom, the words were a little more violent (as is the game), with words like "rip," "tear," and "slayer" pulled out as important. Now that we had an idea of what words were common (and common to see!) in our titles, we looped through the titles to make binary variables that represented whether or not a word was present in that title. From this, we had distilled the titles down into a series of variables that could give us (or a model) a rough idea of what game the title was about.

### For our image, we used a convolutional neural network, a type of machine learning algorithm that can essentially take any type of numerical input and process it through a series of linear transformations until you're left with whatever output you train the network to give. In our case, we wanted to pass in images, and have the model predict which game the image was about. We've create a small diagram, seen below, which roughly demonstarates how exactly stuff goes from the input of a neural network, gets transformed, and is eventually output as a decision. <img src="https://cdn.discordapp.com/attachments/1044291148560224339/1051967123783696496/Untitled_5.png" alt="drawing"/>

### For the case of images, there are a few things that need to be mentioned as how they are processed. As stated above, a neural network takes a series of numbers and does some math on them, so how do we process an image through that? Actually pretty easily, as we just pass in the individual red, blue, and green color values for each pixel. Modern computers draw images via pixels, each of which are made up of 3 subpixels, which are the above colors. It passes how bright each pixel should be, on a range of 0 (off) to 255 (fully on), and the blending of these three colors creates the whole spectrum of colors you can see on your computer! But how does the network know how to train on every image, when they could all be different sizes? Simple! We shrink all our images down to 300x300, which means that the same algorithm and training can be applied to all of them, since we're analyzing the same number of pixels, in the same shape, in the same order.

### Now how do our outputs work? We trained the algorithm to predict based off the image given whether it thought the image was from Animal Crossing or Doom. Why are we predicting the game now, when we said we wanted to include all the variables? The output of model isn't actually just a binary "one or the other" assessment, but rather two weights representing its belief that the image is from each, and it picks the one it has stronger confidence in to be our "decision." We decided to take these weights for the predictions for each model, and actually use that as a way to "process" what the image was about. That way, we now have variables representing what game the image (which was before just a sea of pixels!) is likely to be, and likely not to be, which could be passed into a traditional model.

### we've talked a lot about passing these assessments into a "traditional model" so what do we mean by that? We visualized the avenues the different data take to get processed in this chart below, which covers a lot of what we've discussed here: <img src="https://cdn.discordapp.com/attachments/1044291148560224339/1051966682408681492/Untitled_4.png" alt="drawing"/>

### Essentially, we're taking the now much simply outputs of the two previously unstructured data formats, and using them in tandem with the already-structured user metrics to make a decision on what games' subreddit the post is from, in a manner that includes *all* information on the post, structured or unstrucred, text or image or number.

## Evaluation

## Conclusions and Takeaways

## Impact

